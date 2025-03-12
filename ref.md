---
tags:
  - React
aliases:
  - useRef
---
- レンダリングされたDOM要素やReactコンポーネントへの参照を保持するオブジェクト
- 特定のDOM要素への直接アクセスを可能にする。フォーカス設定やスクロール位置の調整などに使われる
- コンポーネントの再レンダリング間で値を保持できる
- refを変更しても再レンダリングは発生しない

```js
import { useRef } from 'react';

function MyComponent() {
  const myRef = useRef(null);

  return <div ref={myRef}>Hello, World!</div>;
}

```

```js
// ボタンクリックで入力欄にフォーカスが当たる
import { useRef } from 'react';

function TextInputWithFocusButton() {
  const inputEl = useRef(null);

  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>入力欄にフォーカスを当てる</button>
    </>
  );
}

```