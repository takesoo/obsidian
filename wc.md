---
tags:
  - Linux/コマンド
overview: 文字をカウントする
---
```bash
wc [options] [file]

$ wc /etc/services
 11473  63129 692252 /etc/services # 行数 単語数 文字数
 
# -c 文字数をカウントする

# -l 行をカウントする

# -w 単語をカウントする
```