---
tags:
  - React
---
- https://github.com/alan2207/bulletproof-react
- Reactアプリケーションのアーキテクチャの一例。

## Application Oberview
サンプルアプリの概要
## Project Standards
- コーディング規約の遵守のために[[eslint]]を導入すること
- コードフォーマットのために[[prettier]]を導入すること
- [[TypeScript]]の導入を推奨する
- コミット前のコード検証ワークフローのために[[husky]]を導入すること
- 絶対パスでインポートすること
	- 🙅`import something from "../../../component"`
	- 🙆 `import something from "@/component"`
	- `"compilerOptions: { "baseUrl": ".", "paths": { "@/*": ["./src/*"] } }"`
- ファイル命名規則もeslintで強制する
## Project Structure
ソースコードはsrcディレクトリ以下に配置することで、フレームワークのコードと分離させる
```bash
src
|
+-- app               # application layer containing:
|   |                 # this folder might differ based on the meta framework used
|   +-- routes        # application routes / can also be pages
|   +-- app.tsx       # main application component
|   +-- provider.tsx  # application provider that wraps the entire application with different global providers - this might also differ based on meta framework used
|   +-- router.tsx    # application router configuration
+-- assets            # assets folder can contain all the static files such as images, fonts, etc.
|
+-- components        # shared components used across the entire application
|
+-- config            # global configurations, exported env variables etc.
|
+-- features          # feature based modules
|
+-- hooks             # shared hooks used across the entire application
|
+-- lib               # reusable libraries preconfigured for the application
|
+-- stores            # global state stores
|
+-- testing           # test utilities and mocks
|
+-- types             # shared types used across the application
|
+-- utils             # shared utility functions
```
機能関連のコードはfeaturesディレクトリ内に整理することで、共通コンポーネントとの混在を防いで管理と保守を容易にする。
```bash
src/features/awesome-feature
|
+-- api         # exported API request declarations and api hooks related to a specific feature
|
+-- assets      # assets folder can contain all the static files for a specific feature
|
+-- components  # components scoped to a specific feature
|
+-- hooks       # hooks scoped to a specific feature
|
+-- stores      # state stores for a specific feature
|
+-- types       # typescript types used within the feature
|
+-- utils       # utility functions for a specific feature
```
- apiディレクトリの処理が機能間で共有されるなら、apiディレクトリをfeaturesの外に配置するのもOK
- [[バレルファイル]]は[[vite]]だとパフォーマンスの低下につながるので推奨しない。
- [[import/no-restricted-paths]]でimportできるパスを制限することで、各機能の独立性を確保する。コードの依存関係も制限することができる。
## Components And Styling

## API Layer
## State Management
## Testing
## Error Handling
## Security
## Performance
## Deployment

---
[Reactベストプラクティスの宝庫！「bulletproof-react」が勉強になりすぎる件](https://zenn.dev/manalink_dev/articles/bulletproof-react-is-best-architecture)
[本気で考えるReactのベストプラクティス！bulletproof-react2022](https://zenn.dev/t_keshi/articles/bulletproof-react-2022)