---
tags:
  - React
  - npm
  - nextjs
---
## What
フォームを簡単に扱うことができるライブラリ
- 状態管理
- エラー表示
## Why
- フォームの状態管理を楽にするため
- useStateをたくさん書く必要がない
## How
### useForm, form.register()
```tsx
// useFormでフォームのステートを作る
const form = useForm<Values>({ resolver: zodResolver(schema) })

// form.handleSubmitにonSubmitを渡す。
<form onSubmit={form.handleSubmit(onSubmit)}>
  <label htmlFor="name">名前</label>
  <input id="name" {...form.register('name')} /> // form.register()でフォームの値がステートに登録される
  {form.formState.errors.name && <p>{form.formState.errors.name.message}</p>} // form.formState.errorsにエラー文が格納されている
</form>
```
### Controller
- [[Chakra UI]]などの外部ライブラリのコンポーネントがrefを直接公開してない場合、form.registerで繋げないため、Controllerを使って結合する。
```tsx
<Controller
  control={form.control}
  name="category"
  render={({ field }) => <Select value={field.value} onValueChange={field.onChange} />
/>
```
### FormProvider, useFormContext
- formオブジェクト専用のContext
- ネストの深いフォームではpropsでformオブジェクトをバケツリレーすると複雑になるため。
```tsx
// 親コンポーネント
const form = useForm<Values>({ resolver: zodRezolver(schema) })

return (
  <FormProvider {...form}>
    <form onSubmit={form.handleSubmit(onSave)}>
      ...
    </form>
  </FormProvider>
)

// 子コンポーネント
const { getFieldState, formState } = useFormContext()
```
