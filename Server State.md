---
tags:
  - フロントエンド
---
## what
- DBや外部APIなどの永続化サーバーで保持する状態
- 特徴
	- [[Client State]]と違って、複数人で共有されるため、頻繁に状態が変化する
	- 必ず取得・キャッシュ・再検証（stale/refresh）・エラーハンドリングが伴う
## how
ライブラリ
- [[tanstack query]]
- [[swr]]
- [[redux|Redux Toolkit Query]]
- [[Apollo]]