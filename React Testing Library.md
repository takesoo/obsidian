---
tags:
  - npm
  - testing-library
aliases:
  - RTL
---
https://testing-library.com/docs/react-testing-library/intro/
Reactコンポーネントをテストするためのテストライブラリの一つ。
[[Jest]]と一緒に使用する

```jsx
// src/app.js
import React from 'react';

const title = 'Hello React';

function App() {
  return <div>{title}</div>;
}

export default App;
```
```js
// src/app.test.js
import React from 'react';
import { render } from '@testing-library/react';

import App from './App';

describe('App', () => {
  test('renders App component', () => {
    render(<App />);
    
	expect(screen.getByText('Search:')).toBeInTheDocument();
  });
});

```
