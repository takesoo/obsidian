---
tags:
aliases:
  - 複合コンポーネントパターン
---
## what
- 複数のサブコンポーネントを組み合わせて1つのUIを構築するパターン
- 各サブコンポーネントはReact [[useContext|context]]を介して親と状態を共有する。
- 利用側はHTMLのように直感的な構造で記述できる
```tsx
<ParentComponent
  props1={props1}
  props2={props2}
>
  <ParentComponent.Header
    headerProps={headerProps}
  />
  <ParentComponent.Content
    ContentProps={contentProps}
  />
  <ParentComponent.Footer
    FooterProps={footerProps}
  />
</ ParentComponent>
```
## why
- コンポーネントの柔軟性向上：サブコンポーネントの省略、差し替え、並び替えが利用側で自由
- 可読性向上：HTML風にかけるため構造がわかりやすい
- 拡張性向上：新しいサブコンポーネント追加が破壊的変更にならない