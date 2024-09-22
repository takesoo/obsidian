---
tags:
  - react/hooks
---
### effect
### deps
この配列内の値が変更されるとeffectが実行される。

## 無限ループに注意
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