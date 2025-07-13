---
aliases:
  - GA
  - GA4
---
## what
- Webサイトとアプリからデータを収集し、ビジネスに関する分析情報を提供するレポートを作成するプラットフォーム。
- JavaScript測定コードを設置することで、ユーザー情報とページ操作情報が収集される
	- 言語設定、ブラウザの種類、デバイスおよびOS、トラフィックソース
	- GA4タグを直接HTMLに記述する方法と、[[Google Tag Manager|GTM]]でscriptタグを生成する方法がある
- 収集された情報はGAでレポートに反映される
- イベントベースの計測
## why
- [[カスタマージャーニー]]の把握
- [[Conversion|コンバージョン]]測定・[[ファネルレポート]]作成
## how
- ユーザー数
- イベント数
	- クリックやスクロールなどユーザーが起こしたイベント
- コンバージョン
- 新規ユーザー数
	- ユーザー数のうちの新規のユーザー数
- セッション：流入元
- ユーザー属性
	- ロケーションなど
- テクノロジー：デバイスやOS、ブラウザ
- 集客・トラフィック獲得：流入経路
	- Organic Search: 検索エンジンから
	- Direct: ブックマークなど
	- Organic Videos: youtubeなどから
	- Referral: 他のサイトから紹介されたリンクから
	- Organic Social: SNS
	- Organic Search 70%, Organic Social 30%くらいがバランス取れてていい感じ
- エンゲージメント: ユーザー行動
	- page_view: ページ閲覧
	- session_start: セッションの開始
	- first_visit: ページに初めて訪問したユーザー
	- scroll: スクロール
	- アクティブユーザー
	- ページとスクリーン: 各ページのイベントを確認できる
		- 各ページでのユーザーの行動を分析できる
		- 経路データ探索で流入/流出を把握する
- Next.jsの場合、v14から`GoocleAnalytics`コンポーネントが用意されている
	- https://support.google.com/analytics/answer/9304153?hl=ja
	- https://nextjs.org/docs/app/guides/third-party-libraries
- イベントトラッキングの方法
	1. gtag(): GTMを経由しないシンプルな実装
	2. dataLayer.push(): GTMによるデータ送信
### Next.js
https://nextjs.org/docs/app/guides/third-party-libraries
```tsx
// app/lauout.tsx
import { GoogleAnalytics } from '@next/third-parties/google'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}

// app/page.tsx
import { GoogleAnalytics } from '@next/third-parties/google'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}
```