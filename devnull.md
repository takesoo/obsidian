---
tags:
  - Linux
aliases:
  - /dev/null
---
## what
- 何もしないデバイスとして機能する特殊ファイル
- 何を書き込んでもすべて捨てられる
- 読み込んでも空([[EOF]])を返す
- 書き込みがすべて捨てられるのでディスク容量を消費しない
```bash
# 何を書き込んでも消える
echo "hello" > /dev/null

# ファイルサイズは常に0
ls -l /dev/null
# crw-rw-rw- 1 root root 1, 3 Aug 20 10:00 /dev/null

# 読み込むと何も返ってこない 
cat /dev/null
# 何も出力されない
```
