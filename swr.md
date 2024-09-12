---
tags:
  - npm
  - hooks
aliases:
  - Stale-While-Revalidate
---
データフェッチングライブラリ。
クライアントサイドでのデータ取得やキャッシング、再検証を簡単に行うためのフックを提供している。

```javascript
import useSWR from 'swr'

const fetcher = url => fetch(url).then(res => res.json());
 
function Profile() {
  // fetcherにはAxiosやaspidaなどのAPIクライアントを渡す
  const { data, error, isLoading } = useSWR('/api/user', fetcher)
 
  if (error) return <div>failed to load</div>
  if (isLoading) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```

APIクライアントで直接データフェッチするのと違い、キャッシュ管理、自動再フェッチ、エラーハンドリングとリトライもしてくれる。

シンプルで軽量なライブラリであり、複雑な処理が必要な場合は[[React Query]]を使う