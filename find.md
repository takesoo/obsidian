---
tags:
  - Linux/コマンド
overview: ファイルの検索
---
```bash
find ディレクトリ -name ファイル名

find /etc/ -name hosts
/etc/hosts # /etc/直下に存在することがわかる
find: ‘/etc/ssh/sshd_config.d’: Permission denied # read権限がない場合はエラーが変える
find: ‘/etc/sudoers.d’: Permission denied
```
