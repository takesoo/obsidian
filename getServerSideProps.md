---
tags:
  - nextjs
---
[[Next.js]]で[[サーバーサイドレンダリング|Server Side Rendering]]でデータフェッチする時に使うメソッド。
このメソッドを呼び出すことでNext.jsは自動的にSSR形式でデータフェッチするようになる。
```jsx
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    },
  };
}
```
