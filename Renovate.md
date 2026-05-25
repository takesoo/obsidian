## what
- 依存関係の自動更新ツール
	- [[node package]]が主。[[Github Actions]]のバージョン管理もできる。
## how
### getting start
1. github apps インストール
2. `Settings`>`Applications`で設定へ移動
3. `renovate.json`を編集
```json
{
  // ベースとなる設定を読み込む
  "extends": [
    "config:recomended"
  ],

  // Renovate の更新を毎日 00:00-23:59 の間に１時間ごとに行う
  "schedule": [
    "before 9am on monday"
  ],

  // パッケージごとのルール決め
  "packageRules": [
    {
      // `minor` アップデートはコミットプレフィックスをつける
      "matchUpdateTypes": ["minor"],
      "commitMessagePrefix": "chore(deps): [minor]"
    },
    {
      "description": "React関連をまとめて更新",
      "groupName": "react",
      "matchPackageNames": ["react", "react-dom", "@types/react", "@types/react-dom"]
    }
  ]
}

```