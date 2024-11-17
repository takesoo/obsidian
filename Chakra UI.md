---
tags:
  - ComponentLiblary
---
## Getting Start
https://v2.chakra-ui.com/getting-started
```zsh
pnpm add @chakra-ui/react @emotion/react @emotion/styled framer-motion
```
```tsx
import * as React from 'react' 
// 1. import `ChakraProvider` component 
import { ChakraProvider } from '@chakra-ui/react' 

function App() { // 2. Wrap ChakraProvider at the root of your app 
	return (
		<ChakraProvider>
			<TheRestOfYourApplication />
		</ChakraProvider>
	)
}
```
`npx create-next-app`で作成されるstylesディレクトリ以下のglobals.cssとかは基本的に不要になる。併用も可能。