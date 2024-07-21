---
tags:
  - react/hooks
---
状態管理のフック
複数のstateを同時に取り扱うことができる。[[useState]]では扱いにくい複雑な状態管理の場合に使用する。
```js
import { useReducer } from 'react';

function reducer(state, action) {
	switch(action){
		case 'increment':
			return state.age + 1
		// ...
	}
}

function MyComponent() {
	// stateとdispatch関数を返す
	const [state, dispatch] = useReducer(
		reducer, // reducer function
		{ age: 42 } // 初期値
	);

	function handleClick() {
		// dispatchを実行。typeプロパティ(actionの識別子)を引数に渡す。
		dispatch({ type: 'increment' })
	}
}
```

## dispatch関数
stateの更新とリレンダリングをトリガーする関数。