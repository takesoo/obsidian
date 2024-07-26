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

const ThemeContext = createContext<>()
function Button() {
	const theme = useContext(ThemeContext) // "dark"
}
```

コンテキストはグローバルな状態管理や深いコンポーネントツリーでの状態共有に適している。局所的な状態管理であればpropsで十分

## createContext
コンテキストの作成
引数：コンテキストの初期値
