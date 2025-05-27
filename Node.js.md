---
tags:
  - nodejs
  - ランタイム
---
## what
- JavaScriptをサーバー上で動かせるようにしてくれるしくみ（実行環境）
- JavaScriptでOSの機能（file systemなど）を使えるようになる
- もともとJavaScriptをサーバーサイドで実行できるようにするしくみだったが、現在ではクライアントサイドの開発にも利用されている
- [[V8]]エンジンを使用している
- シングルスレッド
- [[イベントループ]]を使ったノンブロッキング（非同期）処理を採用
---
[Node.jsとはなにか？なぜみんな使っているのか？ #JavaScript - Qiita](https://qiita.com/non_cal/items/a8fee0b7ad96e67713eb)

```dataview
table
FROM #nodejs/module 
```
## why
- web開発者の慣れ親しんだJavaScriptをサーバーサイドでも使えるようになると、フロントとバックの両方を開発できるフルスタックJavaScriptのモメンタムが高まった