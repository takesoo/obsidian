---
tags:
  - React
  - react/hooks
aliases:
  - ref
---
## what
- ミュータブルなオブジェクト(ref)を提供するhooks
- refはコンポーネントの再レンダリング間で値を保持できる
- refを変更しても再レンダリングは発生しない
## why
- コンポーネントにミュータブルなグローバル変数を提供する
- Reactがクラスコンポーネントだったころのインスタンス変数の役割を関数コンポーネントで受け継いだのがuseRef
## how
- useRefでrefを宣言
- ref.currentで値を参照
- ref.currentの値を更新しても再レンダリングは発生しない
- refの活用方法は2つ
	- refでDOMを操作する（フォーカス、スクロール）
	- 値の再生成を防ぐ（パフォーマンス改善）
### refでDOMを操作する
- フォームのフォーカス操作
- ページトップへ移動
- etc
```js
// ボタンクリックで入力欄にフォーカスが当たる
import { useRef } from 'react';

function TextInputWithFocusButton() {
  const inputEl = useRef(null); // 初期値nullの場合はDOM要素がセットされる

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