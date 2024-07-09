---
tags:
  - nextjs
---
[[Next.js]]の画像表示タグ
以下の最適化を自動でやってくれる
- 画像ロード時のレイアウト崩れを防ぐ
- [[viewport]]の小さいデバイスに大きな画像がロードされないようにリサイズする
- デフォルトで遅延ロードする
- [[WebP]]や[[AVIF]]などのモダンな画像フォーマットを提供

```jsx
<Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
```

