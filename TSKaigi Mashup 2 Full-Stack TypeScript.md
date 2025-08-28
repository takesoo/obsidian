## T3-Turboで新規開発してみた
- T3-Turbo
	- Next.jsとExpoで型安全に開発すること
	- T3-Stackだとモバイル対応するときにAPI Routesがボトルネックになる
	- ちゃんとバックエンドを用意する
	- フロントとバックエンドで型を共有できる
- Turborepo
	- Vercel製ものレポツール
- tRPC
	- tRPC pannel: swagger風画面
- 生成AIとFull Stack Typescriptは相性がいい
## Event Notification で TypeScript バックエンドの可能性を拡げる
- Event Notificationとは、データ変更をトリガーに通知すること
- イベントの設計にはFact EventとDelta Eventがある
- TypeScriptでの実装
	- State TableとEvent Tableの整合性をとる課題
	- Nest.jsのばあい@Transactionalでトランザクション管理
- Event Notificationのスケール
	- CDC + Kafka
## TypeScriptでモバイル開発はいいぞ！（ほぼ）1人でショートドラマのWeb/モバイルアプリを爆速開発した話
- React Native + Next.js
- Expo SDK 53, Tamagui, Turborepo
- ネイティブ部分はkotrin, swift
## Full Stackな型安全を支えるGraphQLのFragment活用
- Fragment Colocation
	- 親子の依存関係が減る
	- スキーマが変更されたら型エラーが発生してくれる
## フレームワークに縛られない技術選定：気づけば Full-Stack TypeScriptになった話
- 言語仕様として評価
- 言語エコシステムがシステムの中核的な関心ごとをサポートしてくれるか
- 他の使用技術との相性はどうか