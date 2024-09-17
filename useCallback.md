---
tags:
  - react/hooks
---
再レンダー間で関数定義をキャッシュできるようにする。
```tsx
import { useCallback } from 'react';

export const ProductPage = () => {
	/*
	初回レンダリングでfnを返しつつキャッシュする。
	2回目以降はキャッシュしたfnを返す。
	*/
	const handleSubmit = useCallback(
		/*
		fn: キャッシュしたい関数。
		*/
		(orderDetails) => {
			post('/product/' + productId + '/buy', {
				referrer,
				orderDetails
			})
		},
		/*
		dependencies: 依存配列。
		fnのコード内で参照されるすべてのリアクティブな値のリスト。
		*/
		[productId, referrer]
	);
};
```
