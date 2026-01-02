## what
- TypeScriptによる[[Remote Procedure Call|RPC]]
- プロシージャ：クライアントサイドにエクスポートされる関数。`query`, `mutation`, `subscription`の3種類
## why
- REST APIの場合は、サーバーサイドとクライアントサイドで型を共有する方法が課題だったが、tRPCではサーバーサイドで定義した型をフロントエンドに推論されるため、型共有問題が解決する
- IDEの支援をフルで得られる
- [[T3 Stack]]のようにTypeScriptで完結するプロジェクトならわざわざHTTPの作法に則る必要がない
## how
1. サーバーサイドでrouterの定義
	```ts
	// 1. tRPCの初期化
	// server/trpc.ts
	import { initTRPC } from '@trpc/server';
	
	const t = initTRPC.create();
	 
	export const router = t.router; // routerをエクスポート
	export const publicProcedure = t.procedure; // プロシージャをエクスポート
	
	// 2. routerを定義
	// sever/_app.ts
	import { publicProcedure, router } from './trpc';
	
	// procedure that asserts that the user is logged in
	export const authedProcedure = publicProcedure.use(async function isAuthed(opts) {
		const { ctx } = opts;
		// `ctx.user` is nullable
		if (!ctx.user) {
			throw new TRPCError({ code: 'UNAUTHORIZED' });
		}
		return opts.next({
			ctx: {
				// ✅ user value is known to be non-null now
				user: ctx.user,
			},
		});
	});
	
	// procedure that a user is a member of a specific organization
	export const organizationProcedure = authedProcedure
		.input(z.object({ organizationId: z.string() }))
		.use(function isMemberOfOrganization(opts) {
			const membership = opts.ctx.user.memberships.find(
				(m) => m.Organization.id === opts.input.organizationId,
			);
			if (!membership) {
				throw new TRPCError({
					code: 'FORBIDDEN',
				});
			}
			return opts.next({
				ctx: {
					Organization: membership.Organization,
				},
			});
		});
	 
	const appRouter = router({
	  greeting: publicProcedure.query(() => 'hello tRPC v10!'), // プロシージャを定義
	  whoami: authProcedure.query(async (opts) => {
		  const { ctx } = opts;
		  return ctx.user;
	  }),
	  addMember: organizationProcedure
		  .input(
			  z.object({
				  email: z.string().email(),
			  })
		  )
		  .mutation((opts) => {
			  const { ctx } = opts; // const ctx: { user: User; Organization: Organization; }
			  const { input } = opts; // const input: { organizationId: string; email: string; }
			  return '...'
		  })
	});
	 
	// Export only the type of a router!
	// This prevents us from importing server code on the client.
	export type AppRouter = typeof appRouter;
	
	// 3. Routes Handlerを定義
	// app/api/trpc/[trpc]/route.ts
	import { fetchRequestHandler } from '@trpc/server/adapters/fetch';
	import { createTRPCContext } from '~/trpc/init';
	import { appRouter } from '~/trpc/routers/_app';
	const handler = (req: Request) =>
	  fetchRequestHandler({
	    endpoint: '/api/trpc',
	    req,
	    router: appRouter,
	    createContext: createTRPCContext,
	  });
	export { handler as GET, handler as POST };
	```
2. Query Clientのファクトリを作成
	```ts
	// trpc/query-client.ts
	import {
	  defaultShouldDehydrateQuery,
	  QueryClient,
	} from '@tanstack/react-query';
	import superjson from 'superjson';
	export function makeQueryClient() {
	  return new QueryClient({
	    defaultOptions: {
	      queries: {
	        staleTime: 30 * 1000,
	      },
	      dehydrate: {
	        // serializeData: superjson.serialize,
	        shouldDehydrateQuery: (query) =>
	          defaultShouldDehydrateQuery(query) ||
	          query.state.status === 'pending',
	      },
	      hydrate: {
	        // deserializeData: superjson.deserialize,
	      },
	    },
	  });
	}
	```
3. tRPCクライアントを作成してlayout.tsxにセット
	```ts
	// trpc/client.tsx
	'use client';
	// ^-- to make sure we can mount the Provider from a server component
	import type { QueryClient } from '@tanstack/react-query';
	import { QueryClientProvider } from '@tanstack/react-query';
	import { createTRPCClient, httpBatchLink } from '@trpc/client';
	import { createTRPCContext } from '@trpc/tanstack-react-query';
	import { useState } from 'react';
	import { makeQueryClient } from './query-client';
	import type { AppRouter } from './routers/_app';
	export const { TRPCProvider, useTRPC } = createTRPCContext<AppRouter>();
	let browserQueryClient: QueryClient;
	function getQueryClient() {
	  if (typeof window === 'undefined') {
	    // Server: always make a new query client
	    return makeQueryClient();
	  }
	  // Browser: make a new query client if we don't already have one
	  // This is very important, so we don't re-make a new client if React
	  // suspends during the initial render. This may not be needed if we
	  // have a suspense boundary BELOW the creation of the query client
	  if (!browserQueryClient) browserQueryClient = makeQueryClient();
	  return browserQueryClient;
	}
	function getUrl() {
	  const base = (() => {
	    if (typeof window !== 'undefined') return '';
	    if (process.env.VERCEL_URL) return `https://${process.env.VERCEL_URL}`;
	    return 'http://localhost:3000';
	  })();
	  return `${base}/api/trpc`;
	}
	export function TRPCReactProvider(
	  props: Readonly<{
	    children: React.ReactNode;
	  }>,
	) {
	  // NOTE: Avoid useState when initializing the query client if you don't
	  //       have a suspense boundary between this and the code that may
	  //       suspend because React will throw away the client on the initial
	  //       render if it suspends and there is no boundary
	  const queryClient = getQueryClient();
	  const [trpcClient] = useState(() =>
	    createTRPCClient<AppRouter>({
	      links: [
	        httpBatchLink({
	          // transformer: superjson, <-- if you use a data transformer
	          url: getUrl(),
	        }),
	      ],
	    }),
	  );
	  return (
	    <QueryClientProvider client={queryClient}>
	      <TRPCProvider trpcClient={trpcClient} queryClient={queryClient}>
	        {props.children}
	      </TRPCProvider>
	    </QueryClientProvider>
	  );
	}
	```
4. コンポーネントでデータフェッチ
	```ts
	'use client'
	// <-- hooks can only be used in client components
	import { useTRPC } from '@/trpc/client'
	import { useQuery } from '@tanstack/react-query'
	export function ClientGreeting() {
		const trpc = useTRPC()
		const greeting = useQuery(trpc.hello.queryOptions({ text: 'world' }))
		console.log('greeting:', greeting)
		if (!greeting.data) return <div>Loading...</div>
		return <div>{greeting.data.greeting}</div>
	}
	```