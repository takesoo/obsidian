## what
- webブラウザにデータを永続的に保存するHTML5のWeb API機能
- **保存期間**: 永続的（ユーザーが削除するまで）
- **容量**: 約5MB
- **スコープ**: オリジン（ドメイン + プロトコル + ポート）
- **セキュリティ**: JavaScriptからアクセス可能
## why
- ユーザーデータの一時的な保存のため
- ユーザー再訪時にも保持したいデータ
- ダークモード設定や表示言語設定など
- アプリケーションデータのキャッシュとして
## how
```ts
// データの保存
localStorage.setItem('key', 'value');

// データの取得
const value = localStorage.getItem('key');

// データの削除
localStorage.removeItem('key');

```