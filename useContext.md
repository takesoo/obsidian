---
tags:
  - React
  - react/hooks
aliases:
  - context
---
コンポーネントでコンテキストの読み取りとサブスクライブを行うためのフック。
親コンポーネントでプロバイダーを記述し、子コンポーネントで`useContext`を使用して受け取る。
```js
// pages/my_page.tsx
function MyPage() {
	return (
		<ThemeContext.Provider value="dark">  
			<Form />  
		</ThemeContext.Provider>  
	);
}  

// components/form.tsx
function Form() {  
// ... renders buttons inside ...  
}

// components/button.tsx
import { useContext } from 'react';

const ThemeContext = createContext('light') // 初期値
function Button() {
	const theme = useContext(ThemeContext) // "dark". Providerで渡した値が取得できる
}
```

コンテキストはグローバルな状態管理や深いコンポーネントツリーでの状態共有に適している。局所的な状態管理であればpropsで十分

```js
// contextファイル
import { useRepository } from './somewhere' // saveメソッドはこちらで定義している

function useStateDispatch(
	state
	{
		useRepository: useRepository,
	},
) {
	const repo = useRepository();
	function save(authUser) {
		repo.save(authUser); // saveメソッドの本体はrepository
	}

	return {save, state};
}

const MyContext = createContext({
	save: () => {}, // saveメソッドの初期値をとりあえず定義
})

export function useMyContext() {
	return useContext(AuthUserDispatchContext);
}

export const MyContextProvider = ({
	children,
}) => {
	const state = useRepository().data
	const dispatches = useStateDispatch(state, { // dispatches(stateとsaveメソッド)を取得
		useRepository: useRepository;
	});
	return (
		<MyContext.Provider value={dispatches}> // providerでdispatchesを注入。
			{childern}
		</MyContext.Provider>
	)
}

// 呼び出し元
const { save } = useMyContext(); // saveメソッドを呼び出せるようになる
```
==Contextの中身が知りたければ、createContextとProviderで何を渡しているかを調べる==

