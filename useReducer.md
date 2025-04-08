---
tags:
  - react/hooks
---
- 状態管理のフック
- 複数のstateを同時に取り扱うことができる。[[useState]]では扱いにくい複雑な状態管理の場合に使用する。
	- useStateのset関数が条件分岐などで複雑化したらuseReducerの使用を検討する
```js
import { useReducer } from 'react';

/*
 * reducer関数
 * @params
 * state 現在のstate
 * action　actionオブジェクト
 * @return
 * 変更後のstate
 */
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
		// dispatch関数にactionオブジェクトを引数で渡すと、reducer関数が実行される
		dispatch({ type: 'increment' })
	}
}
```

- dispatch: stateの更新とリレンダリングをトリガーする関数。ユーザーアクションをリデューサーに「派遣」する
- reducer: dispatchから受け取ったactionと、store内のstateをもとに、変更されたstateを返す[[純粋関数]]
	- [[immer]]を使用することで元のstateを不変にしたままstate更新ができるようになる。reducerを純粋関数として実装できる
```js
// reducer
// 元のstateは残したまま、draft作成して差し替える。
function reducer(draft, acrion) {
	switch(action) {
		case 'increment':
			return draft.age + 1
	}
}

// component
import { useImmerReducer } from 'use-immer';

export default function MyComponent() {
	const [state, dispatch] = useImmerReducer(
		reducer,
		{ age: 42 }
	);
}
```
- [[useContext]]と組み合わせることで巨大な状態管理に対応することができる
	- https://ja.react.dev/learn/scaling-up-with-reducer-and-context