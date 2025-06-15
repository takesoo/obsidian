---
tags:
  - React
  - react/hooks
aliases:
  - ref
---
## what
- レンダリング時には不要な値を参照するためのhooks
- refとは、レンダリングされたDOM要素やReactコンポーネントへの参照を保持するオブジェクト
	- 特定のDOM要素への直接アクセスを可能にする。フォーカス設定やスクロール位置の調整などに使われる
	- コンポーネントの再レンダリング間で値を保持できる
	- refを変更しても再レンダリングは発生しない
## why
- refの活用方法は2つ
	- refでDOMを操作するため
	- 値の再生成を防ぐため（パフォーマンス改善）
## how
- useRefでrefを宣言
- ref.currentで値を参照
- ref.currentの値を更新しても再レンダリングは発生しない
### refでDOMを操作する
- フォームのフォーカス操作
- ページトップへ移動
- etc
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

### 値の再生成を防ぐ
```ts
// 普通の変数（再レンダリングで消える）
let normalVariable = 0;

// useRef（再レンダリングでも値が保持される）
const refVariable = useRef(0);

function Example() {
  const intervalRef = useRef(null);
  
  const startTimer = () => {
    // タイマーのIDを保存
    intervalRef.current = setInterval(() => {
      console.log('タイマー実行中');
    }, 1000);
  };
  
  const stopTimer = () => {
    // 保存したIDでタイマーを停止
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
      intervalRef.current = null;
    }
  };
}
```