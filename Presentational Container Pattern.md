---
aliases:
  - Presentational/Container Pattern
  - Container/Presentational Pattern
tags:
  - フロントエンド
---
## what
- UIを表示専用の「Presentational」と、データ取得や状態管理を担う「Container」に分けて責務を分離する設計
- [[サーバーコンポーネント|RSC]]においては、Container Componentsはデータフェッチなどのサーバーサイド処理のみを担い、Presentaional Componentsはデータフェッチを含まない[[Shared Components]]もしくは[[クライアントコンポーネント|Client Components]]を指す
## why
- UIとビジネスロジックの結合を弱め、テスト容易性と再利用性が向上する
## how

- Presentational Componentsには[[React Testing Library|RTL]]や[[Storybook]]によるテストを使用できる
```ts
test("`post`として渡された値がタイトルとして表示される", () => {
  const post = {
    title: "test post",
  };
  render(<PostPresentation post={post} />);

  expect(
    screen.getByRole("heading", {
      name: "test post",
      level: 1,
    }),
  ).toBeInTheDocument();
});
```

- Container Componentsは単純な関数として実行して戻り値を検証する
```ts
describe("PostAPIよりデータ取得成功時", () => {
  test("PostPresentationにAPIより取得した値が渡される", async () => {
    // mswの設定
    server.use(
      http.get("https://dummyjson.com/posts/postId", () => {
        return HttpResponse.json(post);
      }),
    );

    const { type, props } = await PostContainer({ postId: "1" });

    expect(type).toBe(PostPresentation);
    expect(props.post).toEqual(post);
  });
});
```