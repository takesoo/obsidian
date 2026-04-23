---
tags:
  - React
---
## what
- 子コンポーネントがまだ準備できてない間、代わりの表示（fallback）を出す仕組み
	- 非同期データ取得（[[サーバーコンポーネント|Server Components]]のasync fetch, use()フックなど）
	- コード分割された遅延ロード（React.lazy()）
	- Next.jsのuseSearchParams()
```tsx
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```
## why
## how