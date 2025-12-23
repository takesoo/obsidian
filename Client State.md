---
tags:
  - フロントエンド
---
## what
- フロントエンド側、ブラウザ内だけで完結する一時的でUI駆動の状態
- Reactなら[[useState]], [[useContext]], [[useReducer]]のこと
	- useStateはコンポーネント内で完結する一時的な状態
		- モーダルの開閉、タブ選択、フォームの入力途中、トースト表示、ドラフトの並び替えなど
	- useContext, useReducerはコンポーネント間で共有したいアプリ状態
		- 認証情報、テーマ（ライト/ダーク）、言語設定、通知、etc...

## how
ライブラリ
- [[zustand]]
- [[jotai]]
- [[Recoil]]