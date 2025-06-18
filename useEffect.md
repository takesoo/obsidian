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
### アンチパターン
- 既存のpropsやstateからstateを計算してはいけない（だいたいエフェクトを使って計算してる）
- イベントハンドラでできることをエフェクトにやらせない
#### propsまたはstateに基づいてstateを更新する
```js
function Form() {  
  const [firstName, setFirstName] = useState('Taylor');  
  const [lastName, setLastName] = useState('Swift');  

  // 🔴 Avoid: redundant state and unnecessary Effect  
  const [fullName, setFullName] = useState('');  
  useEffect(() => {  
    setFullName(firstName + ' ' + lastName);  
  }, [firstName, lastName]);  

  // ✅ Good: calculated during rendering  
  const fullName = firstName + ' ' + lastName;
  // ...  
}
```
#### propsが変更された時に全てのstateをリセットする
```js
// 🔴 userIdを変更した時にcommentが残ってしまうのでuseEffectのdependenciesにuserIdを追加した
// 効率が悪い
// userIdが変更されて再レンダーが発生（commentは古いまま）→DOM更新→エフェクト実行→再レンダー発生
export default function ProfilePage({ userId }) {  
  const [comment, setComment] = useState('');  

  // 🔴 Avoid: Resetting state on prop change in an Effect  
  useEffect(() => {  
    setComment('');  
  }, [userId]);  
  // ...  
}

// userIdが変更されると初回レンダーが実行されるため、stateが初期化される
// ✅ Divide Components, Give key
export default function ProfilePage({ userId }) {  
  return (  
    <Profile  
      userId={userId}  
      key={userId}  
    />  
  );  
}  

function Profile({ userId }) {  
  // ✅ This and any other state below will reset on key change automatically  
  const [comment, setComment] = useState('');  
  // ...  
}
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