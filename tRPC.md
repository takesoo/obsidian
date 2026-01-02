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
	  addMember: organizationProcedure.input()
	});
	 
	// Export only the type of a router!
	// This prevents us from importing server code on the client.
	export type AppRouter = typeof appRouter;
	```
2. 