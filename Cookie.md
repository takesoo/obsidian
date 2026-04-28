## what
- webサイトがユーザーのブラウザに一時的に保存するテキストデータ
- HTTPリクエストごとに自動送信されるため、セッション管理やユーザー追跡に使用される。
- **保存期間**: 開発者が指定（セッション終了、または指定された期間）
- **容量**: 約4KB
- **スコープ**: ドメインとパスで制御可能
- **セキュリティ**: HTTPOnlyオプションを使用することでJavaScriptからのアクセスを制限可能
## why
- セッション管理
- トラッキング
## how
```ts
// Cookiesの設定
document.cookie = "username=John Doe; expires=Fri, 31 Dec 9999 23:59:59 GMT; path=/";

// Cookiesの取得
const cookies = document.cookie;

// Cookiesの削除
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";

```