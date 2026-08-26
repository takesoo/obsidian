---
tags:
  - React
  - npm
  - nextjs
---
フォームを簡単に扱うことができるライブラリ
- 状態管理
- バリデーション
- useStateをたくさん書く必要がない
## 使い方
useFormフックを使用する
```tsx
// useFormでフォームのステートを作る
const form = useForm<Values>({ resolver: zodResolver(schema) })

<form onSubmit={form.handleSubmit(onSubmit)}>
  <label htmlFor="name">名前</label>
  <input id="name" {...form.register('name')} />
  {form.formState.errors.name && <p>{form.formState.errors.name.message}</p>}
</form>
```
### registar
入力要素をフォームの状態に関連づける
### controller
- registarの機能に加えて、初期値やバリデーションなどを定義できる
- [[Chakra UI]]などの外部ライブラリのコンポーネントがrefを直接公開してない場合、Controllerを使って結合することが一般的。