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
```ts
// auth.ts
import NextAuth from "next-auth"
import GitHub from "next-auth/providers/github"
export const { auth, handlers } = NextAuth({ providers: [GitHub] })

// middleware.ts
// ミドルウェアはオプショナル
export { auth as middleware } from "@/auth"

// app/api/auth/[...nextauth]/route.ts
import { handlers } from "@/auth"
export const { GET, POST } = handlers

// components/sign-in.tsx
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
