---
aliases:
  - Cyclic Redundancy Check
  - 巡回冗長検査
tags:
  - 誤り検出
---
データの誤り検出方法の一つ。
データをビット列の値とみなし、それより短いビット列である定数（生成多項式）で割った余りをチェック用の値として付加する方法。
[[チェックサム]]や[[パリティチェック]]と比べ、1カ所に連続して集中的に発生するバースト誤りの検知に強い