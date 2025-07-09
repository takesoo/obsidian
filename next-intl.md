---
tags:
  - npm
---
## what
- next.jsの多言語対応を楽に実装できるライブラリ
## how
### パスで言語を特定する場合
- `example.com/en/about`, `en.example.com/about`とかで切り替えるならこっち
- ファイル構成を以下にする
```
├── messages                言語ごとの辞書ファイル
│   ├── en.json
│   └── ...
├── next.config.ts          プラグインをインポートして、nextConfigをラップしてエクスポート
└── src
    ├── i18n
    │   ├── routing.ts      言語によるルーティングの設定ファイル
    │   ├── navigation.ts   言語によるルーティングを提供するファイル
    │   └── request.ts      サーバーコンポーネントでnext-intlを使用する場合に必要になる設定ファイル
    ├── middleware.ts       言語によるリダイレクトなどを提供するファイル
    └── app
        └── [locale]
            ├── layout.tsx  middlewareでマッチしたlocaleをparamsで受け取る
            └── page.tsx
```
### パスで言語を特定しない場合
- ユーザー設定、または１言語だけサポートする時はこっち
- ファイル構成を以下にする
```
├── messages            言語ごとの辞書ファイル
│   ├── en.json
│   └── ...
├── next.config.ts      プラグインをインポートして、nextConfigをラップしてエクスポート
└── src
    ├── i18n
    │   └── request.ts  リクエストに基づいて多言語設定オブジェクトを生成し、翻訳メッセージをサーバーコンポーネントに提供する。
    └── app
        ├── layout.tsx  getLocaleによってlocaleを取得
        └── page.tsx
```

### [[サーバーコンポーネント]]を多言語化する場合
```tsx
// in async components
// use getter
import {getTranslations} from 'next-intl/server';
 
export default async function ProfilePage() {
  const user = await fetchUser();
  const t = await getTranslations('ProfilePage');
 
  return (
    <PageLayout title={t('title', {username: user.name})}>
      <UserDetails user={user} />
    </PageLayout>
  );
}

// in non-async componets
// use hooks
import {useTranslations} from 'next-intl';
 
export default function UserDetails({user}) {
  const t = useTranslations('UserProfile');
 
  // This component will execute as a Server Component by default.
  // However, if it is imported from a Client Component, it will
  // execute as a Client Component.
  return (
    <section>
      <h2>{t('title')}</h2>
      <p>{t('followers', {count: user.numFollowers})}</p>
    </section>
  );
}
```
### [[クライアントコンポーネント]]を多言語化する場合
- `NextIntlClientProvider`を使用する（簡単だがパフォーマンスはよくない）
- サーバーコンポーネントからmessageをpropsかchildrenで渡す
- stateをサーバーコンポーネントに移動させる
- 