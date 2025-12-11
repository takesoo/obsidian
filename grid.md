---
tags:
  - CSS
---
## what
- グリッドレイアウトの作成
- 可変サイズのグリッドには`fr`を使う。1fr=グリッドコンテナー内の利用可能な空間の比。
## how
```css
.container {
  display: grid;　/* またはinline-grid */
  grid-template-columns: repeat(3, 1fr); /* 横方向に3等分のグリッドを作成する。 1fr 1fr 1frと同じ。 */
  grid-auto-rows: 200px; /* グリッドトラックの高さ。任意。 */
  gap: 1rem; /* グリッドの間隔 */
}

.container > div {
  background-color: skyblue;
  border: solid;
  width: 100%;
  height: 100%;
}
```
```html
<div class="container">
  <div>one</div>
  <div>two</div>
  <div>three</div>
  <div>four</div>
  <div>five</div>
</div>
```


