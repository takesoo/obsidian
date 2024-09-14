---
tags:
  - React
aliases:
  - カスタムフック
---
データの取得やユーザーのオンライン状態の監視、チャットルームへの接続など、より特化した目的のために独自で実装された[[hooks]]。

before:
```tsx
// App.js
// StatusBarとSaveButtonの両方でオンライン状態を管理している
import { useState, useEffect } from 'react';

export default function StatusBar() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }
    function handleOffline() {
      setIsOnline(false);
    }
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}

export default function SaveButton() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }
    function handleOffline() {
      setIsOnline(false);
    }
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  function handleSaveClick() {
    console.log('✅ Progress saved');
  }

  return (
    <button disabled={!isOnline} onClick={handleSaveClick}>
      {isOnline ? 'Save progress' : 'Reconnecting...'}
    </button>
  );
}

```

after:
```js
// useOnlineStatus.js
// オンライン状態管理ロジックをカスタムフックで共通化
function useOnlineStatus() {  
	const [isOnline, setIsOnline] = useState(true);  
	useEffect(() => {  
		function handleOnline() {  
			setIsOnline(true);  
		}  
		function handleOffline() {  
			setIsOnline(false);  
		}  
		window.addEventListener('online', handleOnline);  
		window.addEventListener('offline', handleOffline);  
		return () => {  
			window.removeEventListener('online', handleOnline);  
			window.removeEventListener('offline', handleOffline);  
		};  
	}, []);  
	return isOnline;  
}
```
```jsx
// App.js
import { useOnlineStatus } from './useOnlineStatus.js';

function StatusBar() {
  const isOnline = useOnlineStatus();
  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}

function SaveButton() {
  const isOnline = useOnlineStatus();

  function handleSaveClick() {
    console.log('✅ Progress saved');
  }

  return (
    <button disabled={!isOnline} onClick={handleSaveClick}>
      {isOnline ? 'Save progress' : 'Reconnecting...'}
    </button>
  );
}

export default function App() {
  return (
    <>
      <SaveButton />
      <StatusBar />
    </>
  );
}

```