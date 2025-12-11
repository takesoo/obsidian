---
category: "[[Clippings]]"
author: "[[脱出プロジェクト]]"
title: obsidian：文書内/別文書にある見出しへの内部リンクの書き方
source: https://fukuharahiroshi.com/obsidian_internallink/
clipped: 2023-09-10
published: 2020-09-25
tags:
  - clippings obsidian 技術
---

同じファイル、別ファイルにある見出しへのリンクを貼る方法。

目次

1.  [同一文書内の見出しへのリンク](#toc1)
2.  [別文書にある見出しへのリンク](#toc2)

## 同一文書内の見出しへのリンク

以下のフォーマット。

```
[[#<見出し>]]
```

![](https://fukuharahiroshi.com/dsp/wp-content/uploads/2020/09/Pasted-image-20200925212720.png)

“\[\[#\]\]” まで書くと、文書内にある見出しがオートコンプリートされる。

## 別文書にある見出しへのリンク

別文書内の指定した見出しへのリンクを貼る方法。

```
[[<ファイル名>#<見出し>|<表示文字列>]]
```

記述例：ObsidianのHELPから抜粋

```
[[Obsidian#How we're different|your data is always yours to own and control]]
```

こう書くと、obsidianというファイルの、下の場所へのリンクとなる。

![](https://fukuharahiroshi.com/dsp/wp-content/uploads/2020/09/Pasted-image-20200925211952.png)

内部リンクの記述的にある”#”は仕切りの意味。１つだけで良い。リンク先の見出しの”#”の数とは無関係。

プレビュー時の表示は、”your data is always yours to own and control”のリンク文字列のみになる。

![](https://fukuharahiroshi.com/dsp/wp-content/uploads/2020/09/Pasted-image-20200925212041.png)

Markdownファイルの文字列。

![](https://fukuharahiroshi.com/dsp/wp-content/uploads/2020/09/Pasted-image-20200925212123.png)

こちらはプレビュー。

![](https://fukuharahiroshi.com/dsp/wp-content/uploads/2020/09/Pasted-image-20200925213444.png)

こちらもオートコンプリートされる。