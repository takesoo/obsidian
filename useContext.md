---
tags:
  - React
  - react/hooks
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
function useAuthUserStateDispatch(
	state: AuthUserState,
	{
		useAuthUserLocalStorageRepository: useRepository,
	}: {useAuthUserLocalStorageRepository: UseAuthUserLocalStorageRepository},
) {
	const repo = useRepository();
	function save(authUser: AuthUser) {
		repo.save(authUser);
	}

	function update(authUser: Partial<Omit<AuthUser, 'id'>>) {
		if (!state) {
			return;
		}
		const newAuthUser: AuthUser = {...state, ...authUser};
		repo.save(newAuthUser);
	}

	function destroy() {
		repo.remove();
	}

	return {save, state, update, destroy};
}

const MyContext = createContext({
	destroy: () => {},
	save: () => {},
	update: () => {},
})

export function useAuthUserDispatchContext() {
	return useContext(AuthUserDispatchContext);
}

export const MyContextProvider = ({
	children,
	dependencies
}) => {
	const dispatches = useStateDispatch(state, {
		useRepo
	});
	return (
		<MyContext.Provider value={dispatches}>
			{childern}
		</MyContext.Provider>
	)
}
```

