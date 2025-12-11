---
tags:
  - CSS
---
## what
- 親グリッドのサイズを子グリッドに引き継ぐ
## why
- ネストしたグリッドの柔軟な制御が可能になる
## how
```html
<div class="parent-grid">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item subgrid">
      <div class="subitem">A</div>
      <div class="subitem">B</div>
      <div class="subitem">C</div>
  </div>
</div>

<div class="parent-grid">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">
      <div class="subitem">A</div>
      <div class="subitem">B</div>
      <div class="subitem">C</div>
  </div>
</div>
```
```css
.parent-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 10px;
    margin-bottom: 12px;
}

.subgrid {
    display: grid;
    grid-template-columns: subgrid; /* 子グリッドの列を親グリッドに合わせる */
    grid-template-rows: subgrid; /* 子グリッドの行を親グリッドに合わせる */
    background-color: #e0e0e0;
}
```