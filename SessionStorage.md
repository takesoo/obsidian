## what
- webブラウザにデータを永続的に保存する仕組み
- 保存期間：ブラウザのタブが閉じるまで
- 容量：約5MB
- スコープ：オリジン（ドメイン＋プロトコル＋ポート）
- セキュリティ：JavaScriptからアクセス可能
## why
- ユーザー入力の一時的な保存
	- フォームの入力データをタブが閉じられるまでは保持する
	- 複数ステップにまたがるフォームでのデータ保持
## how
```ts
// データの保存
sessionStorage.setItem('key', 'value');

// データの取得
const value = sessionStorage.getItem('key');

// データの削除
sessionStorage.removeItem('key');
```