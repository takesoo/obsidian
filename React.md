---
tags:
  - React
---
[[hooks]]
[[Custom Hook]]

- イベントハンドラ：クリック、ホバー、フォーム入力などに応答する関数
	- `handleClick`, `handleHover`, etc
	- propsとして渡す場合は`onClick`, `onHover`
- レンダーのトリガー：レンダリングが発生するきっかけ
	- 初回レンダー
	- コンポーネント（またはその祖先のいずれか）の[[useState|state]]の更新
	- (propsが変更された時も？)
- コンポーネントのレンダー：コンポーネントを作成
- DOMへのコミット：レンダリングしたコンポーネントをDOMに反映する
