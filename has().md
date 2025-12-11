---
aliases:
  - :has()
tags:
  - CSS
  - CSS/擬似クラス
---
## what
- 引数に渡したセレクタを持っている親要素を取得してスタイル適用できる
## why
- 子要素に基づいて親要素を取得する時のため
## how
```css
.box:has(.yellow-text) {
  bg-color: black;
}
```
```html
<div class="box"> %% このboxだけbg-color: black;が適用される %%
  <p class="yellow-text">黄色い文字</p>
</div>
<div class="box">
  <p>黒い文字</p>
</div>

```