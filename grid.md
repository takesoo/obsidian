---
tags:
  - CSS
---
## what
- グリッドレイアウトの作成
- 可変サイズのグリッドには`fr`を使う
## how
```css
.container {
  display: grid;　/* またはinline-grid */
  grid: repeat(2, 60px) / auto-flow 80px;
}

.container > div {
  background-color: blue;
  width: 50px;
  height: 50px;
}
```
```html
<div class="container">　<!-- グリッドコンテナー -->
  <div></div> <!-- グリッドトラック -->
  <div></div>
  <div></div>
  <div></div>
</div>
```

### grid-template
- grid-template-columns: 
- grid-template-rows:
- grid-template-areas:


