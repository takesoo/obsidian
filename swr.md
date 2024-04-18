---
tags:
  - npm
---
APIリクエストのラッパー

```javascript
import useSWR from 'swr'
 
function Profile() {
  // fetcherにはAxiosやaspidaなどのAPIクライアントを渡す
  const { data, error, isLoading } = useSWR('/api/user', fetcher)
 
  if (error) return <div>failed to load</div>
  if (isLoading) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```