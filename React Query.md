---
tags:
  - npm
  - hooks
  - React
  - データフェッチングライブラリ
---
データフェッチングやキャッシング、[[Server State]]の管理を簡単に行うための[[Custom Hook|カスタムフック]]を提供している。
```js
import {
  useQuery,
  useMutation,
  useQueryClient,
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'
import { getTodos, postTodo } from '../my-api'

// app.tsxで、QueryClientProviderを使ってqueryClientを提供する
// Create a client
const queryClient = new QueryClient()

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  )
}

// コンポーネントで、useQueryClientでqueryClientにアクセスする
function Todos() {
  // Access the client
  const queryClient = useQueryClient()

  // Queries
  // queryKey: このキーでキャッシュが管理される
  // queryFn: データフェッチする関数
  const query = useQuery({ queryKey: ['todos'], queryFn: getTodos }) 

  // Mutations
  // mutationFn: ミューテーションする関数
  // onSuccess: ミューテーションに成功した時の処理。キャッシュを取得し直すなど。
  const mutation = useMutation({
    mutationFn: postTodo,
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })

  return (
    <div>
      <ul>{query.data?.map((todo) => <li key={todo.id}>{todo.title}</li>)}</ul>

      <button
        onClick={() => {
          mutation.mutate({
            id: Date.now(),
            title: 'Do Laundry',
          })
        }}
      >
        Add Todo
      </button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
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

