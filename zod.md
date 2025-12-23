---
tags:
  - npm
---
## what
- 実行時バリデーション＋型推論・型生成ライブラリ
- 
## how
```js
import { z } from "zod";

// creating a schema for strings
const mySchema = z.string();

// parsing
mySchema.parse("tuna"); // => "tuna"
mySchema.parse(12); // => throws ZodError

// "safe" parsing (doesn't throw error if validation fails)
mySchema.safeParse("tuna"); // => { success: true; data: "tuna" }
mySchema.safeParse(12); // => { success: false; error: ZodError }

// creating a object schema
const User = z.object({
  username: z.string(),
});

User.parse({ username: "Ludwig" });

// extract the inferred type
type User = z.infer<typeof User>;
// { username: string }
```
### サーバーサイドのZodとのE2E型安全
- サーバーサイドでZodを使った型生成をし、[[tRPC]]でクライアントサイドへ型を共有し、サーバーサイドとクライアントサイド間で型を共有できる。[[React Hook Form]]などに型を利用でき、型の差異をなくすことができる。
```ts
// server/routers/inventory.ts  （サーバー側: Zod + tRPC）
import { z } from 'zod';
import { router, publicProcedure } from '../trpc'; // tRPC側のヘルパー

// Zodスキーマ（単一の真実源）
export const InventoryItemSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1),
  unit: z.enum(['kg', 'L', 'pcs']),
  quantity: z.number().int().nonnegative(),
});

// 入力クエリのスキーマ
const ListInputSchema = z.object({
  page: z.number().int().min(1).default(1),
  limit: z.number().int().min(1).max(100).default(10),
});

// 出力（レスポンス）スキーマ
const ListOutputSchema = z.object({
  items: z.array(InventoryItemSchema),
  total: z.number().int().nonnegative(),
});

export const inventoryRouter = router({
  list: publicProcedure
    .input(ListInputSchema)
    .output(ListOutputSchema) // 出力もZodで宣言（重要）
    .query(async ({ input }) => {
      // 実データ取得（DB等）
      const { page, limit } = input;

      const total = 2;
      const items = [
        { id: 'a3f1b9e8-9c1d-4c3c-9c4c-111111111111', name: 'Raw Milk', unit: 'L', quantity: 1200 },
        { id: 'b4e2c0f9-8d2e-4d2f-8d5e-222222222222', name: 'Sugar', unit: 'kg', quantity: 300 },
      ].slice((page - 1) * limit, page * limit);

      // tRPCはoutputで検証されるため、ここで型・構造が保証される
      return { items, total };
    }),
});

// server/index.ts  （AppRouterを定義してクライアントへ型共有）
import { router } from './trpc';
import { inventoryRouter } from './routers/inventory';

export const appRouter = router({
  inventory: inventoryRouter,
});

export type AppRouter = typeof appRouter;

// shared/lib/trpc/client.ts  （クライアント: tRPC + React Query）
import { createTRPCReact } from '@trpc/react-query';
import type { AppRouter } from '../../../server'; // サーバーの型を直接共有

export const trpc = createTRPCReact<AppRouter>();

// shared/lib/trpc/provider.tsx  （プロバイダ設定：React QueryとtRPC）
'use client';

import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { httpBatchLink } from '@trpc/client';
import { trpc } from './client';
import React from 'react';

const queryClient = new QueryClient();
const trpcClient = trpc.createClient({
  links: [
    httpBatchLink({
      url: '/trpc', // APIゲートウェイへのエンドポイント
    }),
  ],
});

export function TrpcProvider({ children }: { children: React.ReactNode }) {
  return (
    <trpc.Provider client={trpcClient} queryClient={queryClient}>
      <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
    </trpc.Provider>
  );
}

// features/inventory/logic/hooks/use-inventory-list.ts  （Logic: 型安全な取得フック）
import { trpc } from '@/shared/lib/trpc/client';

export function useInventoryList(page: number = 1, limit: number = 10) {
  // 引数・戻り値の型が自動補完・検証される
  return trpc.inventory.list.useQuery({ page, limit });
}

```