---
tags:
  - react/hooks
---
関数の計算結果をキャッシュ（メモ化）するためのフック。パフォーマンスの向上が期待できる。
```js
import { useMemo } from 'react';  

function TodoList({ todos, tab }) {  
	/**
	calculateValueの戻り値を返す
	*/
	const visibleTodos = useMemo(  
		/** 
		calculateValue: キャッシュしたい値を計算する関数。
		初回レンダリングでこの関数を呼び出す。
		*/
		() => filterTodos(todos, tab),
		/**
		dependencies: 依存配列。
		この中の値が変わったときはcalculateValue関数が再実行される。
		*/
		[todos, tab]  
	);  
	// ...  
}
```

## メモ化
同じ結果を返す処理について、初回のみ処理を実行してキャッシュしておくこと。