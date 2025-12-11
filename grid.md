---
tags:
  - CSS
---
## what
- グリッドレイアウトの作成
## how
```html
<div class="container">
  <div></div>
  <div></div>
  <div></div>
  <div></div>
</div>
```
```css
.container {
  display: grid;
  grid: repeat(2, 60px) / auto-flow 80px;
}

.container > div {
  background-color: blue;
  width: 50px;
  height: 50px;
}
```
### grid-template
- grid-template-columns: 
- grid-template-rows:
- grid-template-areas:


