---
tags:
  - react/hooks
aliases:
  - effect
  - エフェクト
---
## what
- コンポーネントのレンダリング時に実行される[[hooks]]
- エフェクト：
	- レンダー自体によって引き起こされる副作用を指定するためのもの。
	- DOMへのコミットの後に実行される
	- つまりDOMへの変更（副作用）を記述する時にエフェクトを使用する
## why
- Reactコンポーネントはレンダーコードとイベントハンドラによって構成されるが、実際にはレンダー自体によって引き起こされる計算がもあり、それを記述するため
- 外部システムと同期するために使用される
## how
```ts
useEffect(setup, dependencies?)

useEffect(() => {
  let ignore = false;
  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  /**
   * cleanup関数
   * コンポーネントがアンマウントされる時に実行される
   */
  return () => {
    ignore = true;
  };

  /**
   * dependencies
   * これらの値が変更されると再レンダリングされる
   */
}, [userId]);
```
### useEffectはできるだけ避ける
#### 予期せぬ再実行
```ts
// ❌ 問題のあるコード
function Component({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // userIdが変わるたびに実行される
  
  return <div>{user?.name}</div>;
}
```
#### 無限ループに注意
```tsx
const Component1: React.FC = () => {
  const [count, setCount] = useState(0);

  // レンダリングされるたびにhogeを生成する
  const hoge = {};

  // 前回レンダリング時のhogeと今回ので参照先が違うので無限ループする
  useEffect(() => {
    setCount(n => n + 1);
    console.log(hoge);
  }, [hoge]);

  return <div>count: {count}</div>;
};

```
```tsx
const Component1: React.FC = () => {
  const [count, setCount] = useState(0);

  // useMemoでキャッシュすることで前回レンダリング時と同じhogeを使う
  const hoge = useMemo(() => {
	  return {};
  }, []);

  // hoge == hogeとなり無限ループにならない
  useEffect(() => {
    setCount(n => n + 1);
    console.log(hoge);
  }, [hoge]);

  return <div>count: {count}</div>;
};

```
#### 複雑な依存関係
```ts
// ❌ 管理が困難
useEffect(() => {
  // 複雑な処理
}, [a, b, c, d, e, f]); // 依存関係が多すぎて管理困難
```