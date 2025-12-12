---
tags:
  - CSS
---
## what
- 要素の配置設定
## how
```css
.static {
  position: static; /* 通常 */
}
.relative {
  /* 初期位置のtopから40px、leftから40pxずれて配置 */
  position: relative;
  top: 40px;
  left: 40px;
}
.absolute {
  /* ページの左上またはposition: relative;指定された祖先からインライン方向に40px、ブロック方向に40pxの位置で配置 */
  position: absolute;
  inset-inline-start: 40px;
  inset-block-start: 40px;
}
.sticky {
  /* スクロールした時にtop 20pxの位置で固定する */
  position: sticky;
  top: 20px;
}
```