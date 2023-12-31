---
tags:
  - コンピュータ
---
## 物理構成
- アクセスアーム
- 磁気ヘッド
- [[プラッタ]]
![[スクリーンショット 2023-09-10 15.58.40.png]]
### 記憶領域の構成
- [[セクタ]]：データを書き込む最小単位
- [[トラック]]：プラッタ１周の単位。セクタの集合
- [[シリンダ]]：プラッタの同じ位置にあるトラックの集合のこと
![[スクリーンショット 2023-09-10 16.00.06.png]]
## 記憶容量の計算
![[スクリーンショット 2023-09-10 16.04.41.png]]
![[スクリーンショット 2023-09-10 16.04.56.png]]

## アクセス時間
- 位置決め時間（シーク時間）：磁気ヘッドの位置を調整する時間
- 回転待ち時間（サーチ時間）：読み書きするセクタが磁気ヘッドの下に来るのを待つ時間。正確な時間を算出するのは難しいので平均を使う。
- データ転送時間：データを読み書きする時間
![[スクリーンショット 2023-09-10 16.16.01.png]]
$$
\begin{align}
アクセス時間 &= 待ち時間 + データ転送時間 \\
&= (位置決め時間 + 回転待ち時間) + データ転送時間
\end{align}
$$
## データの管理
アクセスを繰り返しているとHDDはフラグメンテーション（データがバラバラのセクタに保存される）を起こすことがある。フラグメンテーションを起こすとアクセス時間が増大してしまうので、デフラグしてアクセス時間を改善させる。

↓あくまでイメージ
![[スクリーンショット 2023-09-10 16.20.09.png]]

## ハードディスクの性能向上
[[Redundant Arrays of Independent Disks|RAID]]：複数のハードディスクを組み合わせることで、処理速度と信頼性を向上させること
- [[RAID0]]
- [[RAID1]]
- [[RAID5]]
