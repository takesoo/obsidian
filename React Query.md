---
tags:
  - npm
  - hooks
  - React
  - データフェッチングライブラリ
---
データフェッチングやキャッシング、[[Server State]]の管理を簡単に行うための[[Custom Hook|カスタムフック]]を提供している。
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
## useQuery
### queryKey
このキーを参考にキャッシュを管理する。[[LocalStorage]]にこのキーでキャッシュする。
### queryFn
データフェッチする関数。[[Promise]]を返ることを期待している。

## staleTime
キャッシュがstale(古い状態)になったとみなす時間
## cacheTime
キャッシュを[[ガベージコレクション]]するまでの時間

