---
aliases:
  - shadcn/ui
---
## what
- [[Radix UI]]のプリミティブをベースに、[[Tailwind CSS]]でスタイル済みの再利用可能なコンポーネント集
	- 現在は、[[Radix UI]]、[[Base UI]]、[[React Aria]] の3種類のプリミティブから選べるようになっている。
- [[npm]]パッケージではなく、レジストリ+CLI。`add`コマンドをうちとコンポーネントのソースコードそのものがリポジトリにコピーされる。（土台となるヘッドレスライブラリだけはnpmでインストールされる。）
## how
- components.json: CLI設定ファイル
- `pnpm dlx shadcn@latest add tooltip`