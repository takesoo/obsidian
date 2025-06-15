---
tags:
  - react/hooks
---
## what
- 再レンダー間で関数定義をキャッシュできるようにする。
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
## why
- 関数の再作成を防ぐことでパフォーマンスの向上につながる
```ts
// この例は間違ってるかもしれない

// ❌ 毎回新しい関数が作られる
function Parent({ items }) {
  const handleClick = (id) => {
    console.log('クリック:', id);
  };
  
  return (
    <div>
      {items.map(item => 
        <Child key={item.id} onClick={handleClick} /> // 毎回新しい関数
      )}
    </div>
  );
}

// ✅ 関数を再利用
function Parent({ items }) {
  const handleClick = useCallback((id) => {
    console.log('クリック:', id);
  }, []); // 依存関係が変わらない限り同じ関数を再利用
  
  return (
    <div>
      {items.map(item => 
        <Child key={item.id} onClick={handleClick} />
      )}
    </div>
  );
}
```
## how
逆にパフォーマンスが悪くなる時もあるので注意
[[いつuseCallbackを使うべきなのか - asoview-engineering - Medium-2024-09-18 09-58-33]]