---
tags:
  - Linux/コマンド
overview: 行の重複の消去
---
```bash
uniq [file]

$ cat > uniq-test
A
B
A
C
C
D

uniq uniq-test
A
B
A
C # 前後で重複していたCが1行にまとめられる
D

cat uniq-test | sort | uniq
A # sortの結果Aが並ぶのでuniqで1行にまとめられる
B
C
D
```