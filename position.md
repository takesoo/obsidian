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
  position: absolute;
  inset-inline-start: 40px;
  inset-block-start: 40px;
}
```