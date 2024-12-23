---
tags:
  - Linux/コマンド
overview: ファイルの内容を表示
---
- con==cat==enate
```bash
cat ~/.bashrc
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi
...

# -n 行番号をつけて表示
```

- 引数を指定しない場合、[[標準入力]]をそのまま[[標準出力]]する
```bash
cat > cat-output
Hello, World! # 標準入力でHello, World!と入力するとcat-outputにリダイレクトされる
cat cat-output
Hello, World!
```
