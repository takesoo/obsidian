---
tags:
  - React
---
Reactコンポーネントの型。[[React.FC]]よりも素直？なのでこちらを推奨する（らしい）。
## `JSX.Element`を指定するべき時
- `children`を受け取らないコンポーネントの場合
- 戻り値の型が[[JSX]]であることを明示したい時
- パフォーマンスと厳密な型付けを重視するとき。