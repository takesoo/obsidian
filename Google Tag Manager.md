---
aliases:
  - GTM
---
## what
- タグ管理ツール
	- [[Google Analytics|GA4]], [[Google Ads]], SNSなどのJavaScriptタグを、ノーコードで管理設定できる
- イベントのトリガー設定
- [[Google Analytics|GA4]]へのデータ送信設定
	- 特定のイベントにタグを設定しておくと、ユーザーアクションに応じてGA4にイベントが送信される
	- GA4で収集されたデータを分析する
- `dataLayer`
	- GTMが提供するグローバルJavaScriptオブジェクト
	- ウェブサイトからマーケティングツールおよび分析ツールへのデータ送信を抽象化する仕組み
	- イベントや変数を動的にプッシュし、GTMがそれらをショルするためのキュー
```js
// dataLayerの初期化
window.dataLayer = window.dataLayer || [];

// イベント発生時にデータをプッシュ
window.dataLayer.push({
  'event': 'button_click',      // イベント名
  'custom': {                   // カスタムデータ
    'button_name': 'submit',
    'page_section': 'header'
  }
});
```
## how
### Next.js
```tsx
// app/layout.tsx
import { GoogleTagManager } from '@next/third-parties/google'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <GoogleTagManager gtmId="GTM-XYZ" />
      <body>{children}</body>
    </html>
  )
}

// app/page.tsx
'use client'
 
import { sendGTMEvent } from '@next/third-parties/google'
 
export function EventButton() {
  return (
    <div>
      <button
        onClick={() => sendGTMEvent({ event: 'buttonClicked', value: 'xyz' })}
      >
        Send Event
      </button>
    </div>
  )
}
```