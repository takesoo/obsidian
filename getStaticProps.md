---
tags:
  - nextjs
---
[[Next.js]]で[[スタティックサイトジェネレーション|Static Site Generation]]でデータフェッチする時に使うメソッド。
このメソッドを呼び出すことでNext.jsは自動的にSSG形式でデータフェッチするようになる。
```js
import { getSortedPostsData } from '../lib/posts';

export async function getStaticProps() {
  const allPostsData = getSortedPostsData();
  return {
    props: {
      allPostsData,
    },
  };
}
```
