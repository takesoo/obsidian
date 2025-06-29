---
tags:
  - npm
---
## what
- [[OAuth]]などの認証を楽にしてくれるライブラリ
- [[NextAuth.js]]のv5以降のこと
- 暗号化した[[JWT]]をsessionに保存するのがデフォルト。アダプターを使ってデータベースに保存させることもできる
- OAuth認証をデフォルトにしていて、80以上のプロバイダーを提供している
## why
- [[NextAuth.js]]は[[Next.js]]に依存していたが、その依存を廃してさまざまな環境に利用できるようにした
## how
### auth.ts
Auth.jsの設定ファイル
設定
- プロバイダー
- リダイレクト
- 認証方法
- サインインのロジック
- ログイン後の遷移先
handlers
- 実態は`{ GET, POST }`という形式のオブジェクト
- 後述の`app/api/auth/[...nextauth]/route.ts`でimportされ、定義されることで、そこに届くGETとPOSTリクエストはAuth.jsが処理するようになる
```ts
import NextAuth from "next-auth"
import GitHub from "next-auth/providers/github"
// handlers: 認証関連の通信を処理するオブジェクト
// auth: ログインユーザー情報を確認するための関数
// signIn, signOut: サインイン、サインアウト処理をする関数
export const { auth, handlers, signIn, signOut } = NextAuth({ providers: [GitHub] })
```
### app/api/auth/\[...nextauth]/route.ts
Auth.jsに関連するすべての通信を引き受けるAPI Routes
ex: /api/auth/singin/google, /api/auth/callback/githubなどもすべてこれ
```ts
import { handlers } from "@/auth"
// handlersのGETプロパティとPOSTプロパティを取り出して定数として宣言
export const { GET, POST } = handlers
```

### components/sign-in.tsx

```tsx
import { signIn } from "@/auth"
 
export default function SignIn() {
  return (
    <form
      action={async () => {
        "use server"
        await signIn("spotify")
      }}
    >
      <button type="submit">Signin with Spotify</button>
    </form>
  )
} 
```

### middleware.ts
ミドルウェアを使用することでユーザーのログイン状態をチェックできる
```ts
export { auth as middleware } from "@/auth"
```
