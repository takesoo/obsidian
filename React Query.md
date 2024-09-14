---
tags:
  - npm
  - hooks
  - React
  - データフェッチングライブラリ
---
データフェッチングやキャッシング、サーバー状態管理を簡単に行うための[[Custom Hook|カスタムフック]]を提供している。
```js
import { useQuery } from 'react-query';

const fetchData = async () => {
  const response = await fetch('/api/data');
  return response.json();
};

const Component = () => {
  const { data, error, isLoading } = useQuery('data', fetchData);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;

  return <div>{data.message}</div>;
};

```