---
tags:
  - AWS/IAM
aliases:
  - Identity and Access Management
---
## what
- [[AWS入門ブログリレー2024〜AWS IAM編〜  DevelopersIO-2025-08-12 08-58-19]]
- 認証認可
- 「誰（何）が」「何に対して」「どのような操作を」「どの条件で」実行できるかを制御するサービス
- 最小権限の原則
### 構成要素
- [[AWS入門ブログリレー2024〜AWS IAM編〜  DevelopersIO-2025-08-12 08-58-19]]を参照。公式ドキュメントでプリンシパルとかIAM IDとかの言葉の正確な定義にこだわると沼にハマるので文脈からなんとなくで読み取ること。
- [[IAMユーザー]]
- [[IAMロール]]
- [[IAMポリシー]]
- [[パーミッションバウンダリー]]
## how
- IAMエンティティにIAMポリシーをアタッチする。
- 権限の許可はIAMポリシーで、制限にはパーミッションバウンダリーで定義する
### ベストプラクティス
- https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html
- IAM Identity Centerのユーザー、フェデレーティッドプリンシパルが推奨。サインイン時にIAMロールを引き受け、一時的な認証情報を使用するため。