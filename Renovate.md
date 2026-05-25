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
    "config:base"
  ],

  // Dependency Dashboard を無効化する
  "dependencyDashboard": false,

  // ステータスチェックを行わない
  "requiredStatusChecks": null,

  // Renovate の更新を毎日 00:00-23:59 の間に１時間ごとに行う
  "schedule": [
    "every 1 hour after 00:00 and before 23:59 every day"
  ],

  // PR の生成と自動マージを毎日 00:00-23:59 の間に１時間ごとに行う
  "automergeSchedule": [
    "every 1 hour after 00:00 and before 23:59 every day"
  ],

  // major の更新は対象外とする
  "major": {
    "enabled": false
  },

  // パッケージごとのルール決め
  "packageRules": [
    {
      // `major` 以外の PR は自動マージとする
      "matchUpdateTypes": ["minor", "patch", "pin", "digest"],
      "automerge": true
    },
    {
      // `major` 以外の PR は自動マージとする
      // 加えて `eslint` という文字列にマッチした場合はグループ化する
      "matchUpdateTypes": ["minor", "patch", "pin", "digest"],
      "matchPackagePatterns": ["eslint"],
      "groupName": "eslint",
      "automerge": true
    }
  ]
}

```