---
Tags:
  - CS50
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
[

CS50 2021 - Lecture 3 - Algorithms (日本語字幕付)

この動画は、次の動画に日本語の字幕をつけたものです。  
https://www.youtube.com/watch?v=yb0PY3LX2x8  
  
LICENSE  
CC BY-NC-SA 4.0  
Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International Public License

![](https://www.youtube.com/s/desktop/066935b0/img/favicon_144x144.png)https://www.youtube.com/watch?v=s3oA2qdt_I0

![](https://i.ytimg.com/vi/s3oA2qdt_I0/maxresdefault.jpg)](https://www.youtube.com/watch?v=s3oA2qdt_I0)

効率の良い実行処理（アルゴリズム）を考える

  

数字の配列から任意の数字があるか探す場合

先頭から一つ一つ確認して探す

線形探索

配列を昇順にソートして、真ん中の数字を確認し、大小によって右半分か左半分に分けて探索する

二分探索

線形探索はθ(n)なのに対して二分探索はθ(log n)なので二分探索の方が効率がいいと言える

O(n): 実行時間の上限

Ω(n): 実行時間の下限

θ(n): O(n)=Ω(n)の時の表記

nはアルゴリズムのステップ数

ソート処理をする場合

選択ソート

1. 最小の数字を先頭から順に確認する [4, 2, 3, **1**]
2. 最小の数字を先頭にスワップする[1, 4, 2, 3]
3. 1, 2を繰り返す

バブルソート

1. n番目とn+1番目を確認する [**4**, **2**, 3, 1]
2. 順序が逆ならスワップする [2, 4, 3, 1]
3. 1, 2を繰り返す
4. 配列の最後まで処理したらもう一度先頭から繰り返す

マージソート

1. [4, 2, 3, 1]を[[4, 2], [3, 1]]に分割する
2. それぞれを比較して[[2, 4], [1. 3]]にする
3. 右の配列と左の配列の先頭を比較して新しい配列に配置する
    1. [[2, 4], [3]] ⇒ [1]
    2. [[4], [3]] ⇒ [1, 2]
    3. [[4]] ⇒ [1, 2, 3]
    4. [] ⇒ [1, 2, 3, 4]

選択ソート: O(n^2), Ω(n^2)

バブルソート: O(n^2), Ω(n)

マージソート: O(n * logn), Ω(n * logn))

マージソートが圧倒的に早い

カプセル化：関連する情報の断片を封じ込めること