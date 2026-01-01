---
tags:
  - nodejs
---
## what
- node.jsの[[パッケージ管理システム|パッケージマネージャ]]
## why
- [[npm]]の欠点の解消
	- ディスク容量の節約：プロジェクト間で依存関係を共有する
	- インストール速度の向上：依存関係の解決、ディレクトリ構造の計算、リンクの３ステップを並列実行する
	- フラットでないnode_modules：プロジェクトの直接の依存関係のみをモジュールディレクトリのルートに追加する。

## how
```shell
# 初期化
pnpm init

# 依存関係のインストール
pnpm install
pnpm i

# パッケージの追加
pnpm add <package>
pnpm add <package> -D

# script実行
pnpm <script>
```
### npmから移行
```shell
# pnpm-lock.yaml生成
pnpm import

# node_modulesとpackage-lock.json削除
rm -rf node_modules package-lock.json

# install packages
pnpm install
```
### [[モノレポ]]定義
```
my-monorepo/
├── pnpm-workspace.yaml  <-- 【必須】ここがワークスペースだと宣言するファイル
├── package.json         <-- ルートの設定
├── pnpm-lock.yaml
│
├── apps/                <-- アプリケーションを入れるフォルダ
│   ├── web/             <-- Webサイト (Next.jsなど)
│   │   └── package.json
│   └── docs/            <-- ドキュメントサイト
│       └── package.json
│
└── packages/            <-- 共通部品を入れるフォルダ
    ├── ui/              <-- 共通UIライブラリ
    │   └── package.json
    └── utils/           <-- 共通ユーティリティ
        └── package.json
```
```yaml
# pnpm-workspace.yaml
packages:
  - 'packages/*'
  - 'apps/*'
```
```shell
# apps/webにpackages/uiをインストールする
pnpm add @my-monorepo/ui --filter web
```