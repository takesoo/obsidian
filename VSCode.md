## 設定
### 設定レベル
- **ユーザー設定**: VSCode全体で使用される設定。すべてのプロジェクトで共通です。
- **ワークスペース設定**: 特定のプロジェクトフォルダに対して適用される設定。プロジェクトごとに異なる設定が可能です。
- **フォルダー設定**: ワークスペース内の特定のフォルダごとにカスタマイズされた設定。
### 設定画面
`cmd + ,`
### settings.json
```json
{
	"editor.defaultFormatter": "biomejs.biome",
	"editor.formatOnSave": true,
	"editor.codeActionsOnSave": {
		"quickfix.biome": "explicit",
		"source.organizeImports.biome": "explicit"
	},
	"[javascript]": {
		"editor.maxTokenizationLineLength": 2500,
		"editor.defaultFormatter": "biomejs.biome"
	},
	"cSpell.words": ["Chakra", "todos"]
}
```