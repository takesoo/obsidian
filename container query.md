---
aliases:
  - コンテナクエリ
tags:
  - CSS
---
## what
- 要素のサイズに応じてスタイルを切り替える仕組み
## how
```css
.container {
  container-type: inline-size; %% この要素をcontainer queryの基準とする %%
  width: 500px
}

%% containerクラスの要素のwidthが300px以上ならdisplay: flex;にする %%
@container (min-width: 300px) {
  ul {
    display: flex;
    justify-content: space-between;
  }
  li {
    width: 33%
  }
}

.named-container {
  container-name: hoge;
  container-type: inline-size;
  width: 500px
}

%% container-nameで指定することもできる %%
@container hoge (min-width: 300px) {
  ul {
    display: flex;
    justify-content: space-between;
  }
  li {
    width: 33%
  }
}

.bulkset-container {
  container: hoge / inline-size; %% 一括設定 %%
}
```