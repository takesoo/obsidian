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

## ⚙ フックの実行順とタイミング（初回マウント時）
1. 関数コンポーネント実行（[[useState]], [[useRef]], JSX返す）
2. 仮想DOMの比較（Diff）
3. 必要ならリアルDOMを変更
4. [[useLayoutEffect]] のコールバック実行
5. 画面が描画される（ユーザーに見える）
6. [[useEffect]]のコールバック実行

---

## 🔁 更新時（stateやpropsの変更）

同じ順番で再びライフサイクルが回ります：
1. state/propsの変更
2. 再レンダリング（関数の再実行）
3. DOM差分を検出し、必要な変更を適用
4. 前回の `useEffect` / `useLayoutEffect` のクリーンアップ（必要なら）
5. 新しい `useLayoutEffect` 実行
6. 画面再描画
7. 新しい `useEffect` 実行