---
tags:
  - React
---
[[関数コンポーネント]]を型定義するための[[ユーティリティ型]]。関数コンポーネントの型を簡単に定義できる。[[JSX.Element]]を使う方がいいかも。
## `React.FC`を指定するべき時
- `children`を受け取るコンポーネントの場合
- シンプルなプロパティと子要素を扱うだけで、特に型制約を気にしない場合
```tsx
export const SomeComponent = ({
	props
}: SomeComponentProps): React.FC<SomeComponentProps> => {
	return (
		<SomeElement>
			{ children }
		</SomeElement>
	);
} ;
```
