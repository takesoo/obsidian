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
- コンポーネントのレンダー：コンポーネントの呼び出し
- [[Document Object Model|DOM]]へのコミット：レンダリングしたコンポーネントを（[[仮想DOM]]を介して）DOMに反映する
![[Pasted image 20260428083312.png]]

## ⚙ フックの実行順とタイミング（初回マウント時）
1. 関数コンポーネント実行（[[useState]], [[useRef]], JSX返す）
2. [[仮想DOM]]の比較（Diff）
3. 必要なら[[Document Object Model|リアルDOM]]を変更
4. 画面が描画される（ユーザーに見える）
5. [[useEffect]]のコールバック実行

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

## フォルダ構成
- [Reactはフォルダ構成に意見を持たない](https://legacy.reactjs.org/docs/faq-structure.html#is-there-a-recommended-way-to-structure-react-projects)
- 一般的な構成は以下
	- [[type-based architecture]]
	- [[feature-based architecture]]
	- [[layer-based architecture]]
- type-basedとfeature-basedを組み合わせることも多い
```
src
├── features
    ├── users           # feature
    │   ├── api         # type
    │   ├── components  # type
    │   └── hooks       # type
    └── posts           # feature
        ├── api         # type
        ├── components  # type
        └── hooks       # type
...
```
- 考え方
	- ネストは3~4階層に抑える
	- テストしやすさを意識する
		- type-basedを取り入れて技術的な関心の分離をするとテスト対象がわかりやすくなる
	- [[Colocation|コロケーション]]を意識する
		- type-basedはコロケーションがうまくいかないデメリットがあるので、feature-basedを取り入れる
		- コロケーションのためにフォルダを追加するとネストが深くなるトレードオフ
	- [[Screaming Architecture]]を意識する
		- フォルダ構成はドメイン→技術的詳細の順になるべき
		- feature-basedを最初にする
	- 依存関係を考慮する
		- feature→sharedへと依存させる
		- 逆はリンターなどでルールを設けて制限する
		- `features`内は複雑化するもの。必要なら別featureからインポートできないように制限を設ける
	- 目的を意識した名前をつける
		- `lib`フォルダ内に`utils`や`helpers`といった曖昧なフォルダ名は避ける
		- `datetime`や`url`など目的を意識した具体的な命名をする
	- 以上を踏まえて、feature-basedをベースにして、各feature内はtype-basedで構成する
