---
tags:
  - npm
aliases:
  - NextAuth.js
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
signIn
- 後述の`components/sign-in.tsx`のようにsignIn関数を呼び出すことで認証プロセスを開始できる
auth
- 現在のリクエストにおける認証状況（セッションなど）を確認する関数
- sessionをカスタマイズすることで独自の認証オブジェクトを作成できる
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

### useSession
- サインイン状態やセッション情報を取得するhooks
- [[クライアントコンポーネント]]でのみ動作、[[サーバーコンポーネント]]では`getServerSession`を使用する
```ts
// app.tsx
import { SessionProvider } from "next-auth/react"

export default function App({ Component, pageProps: { session, ...pageProps } }) {
  return (
    <SessionProvider session={session}>
      <Component {...pageProps} />
    </SessionProvider>
  )
}

// components/component.ts
import { useSession } from "next-auth/react"

export default function Component() {
  const { data: session, status } = useSession()
  if (status === "authenticated") {
    return <p>Signed in as {session.user.email}</p>
  }
  return <a href="/api/auth/signin">Sign in</a>
}

// API Routes
import { authOptions } from "pages/api/auth/[...nextauth]"
import { getServerSession } from "next-auth/next"

export default async function handler(req, res) {
  const session = await getServerSession(req, res, authOptions)

  if (!session) {
    res.status(401).json({ message: "You must be logged in." })
    return
  }

  return res.json({
    message: "Success",
  })
}

// App Routes
import { getServerSession } from "next-auth/next"
import { authOptions } from "pages/api/auth/[...nextauth]"

export default async function Page() {
  const session = await getServerSession(authOptions)
  return <pre>{JSON.stringify(session, null, 2)}</pre>
}
```
