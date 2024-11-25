---
tags:
  - react/hooks
---
- 状態管理のフック
- 複数のstateを同時に取り扱うことができる。[[useState]]では扱いにくい複雑な状態管理の場合に使用する。
	- useStateのset関数が条件分岐などで複雑化したらuseReducerの使用を検討する
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
		// dispatchを実行。actionを引数に渡す。
		dispatch({ type: 'increment' })
	}
}
```

- dispatch: stateの更新とリレンダリングをトリガーする関数。
- reducer: dispatchから受け取ったactionと、store内のstateをもとに、変更されたstateを返す純粋関数