---
aliases:
  - flex
  - フレックス
  - フレックスボックス
tags:
  - CSS
---
## what
- コンテナの子要素を横並びにする
> CSS フレックスボックスレイアウトでは、以下のことができるようになります。
>
> - コンテンツのブロックを、親コンテンツの中で上下中央に配置すること。
> - 利用できる幅や高さに関係なく、コンテナーのすべての子が利用できる幅や高さを等しくすること。
> - 段組みのレイアウトで、コンテンツの量が異なっていても、すべての段の高さが同じになるようにすること。
>
> フレックスボックス機能は、1 次元レイアウトのニーズに最適なソリューションでしょう。早速みてみましょう。
> [フレックスボックス - ウェブ開発の学習 | MDN](https://developer.mozilla.org/ja/docs/Learn_web_development/Core/CSS_layout/Flexbox#%E3%81%AA%E3%81%9C%E3%83%95%E3%83%AC%E3%83%83%E3%82%AF%E3%82%B9%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9%E3%81%AA%E3%81%AE%E3%81%8B)
- 
## how
```html
<div class="container">
  <div class="content">Item 1</div>
  <div class="content">Item 2</div>
  <div class="content">Item 3</div>
</div>
```
```css
.container {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.content {
  background-color: skyblue;
  width: 100%;
  border: solid;
}
```

- justify-content: 水平方向の揃え
	- 
- align-items: 垂直方向の揃え