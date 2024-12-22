---
tags:
  - Linux/コマンド
---
- ファイルの内容を表示
- conCATenate
```bash
cat ~/.bashrc
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi
...
```

- `-n`
	- 行番号をつけて表示