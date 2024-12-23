---
tags:
  - Linux/コマンド
overview: マニュアルの表示
---
```bash
$ man ls
# セクション1に分類されている
LS(1P)                                              POSIX Programmer's Manual                                             LS(1P)

PROLOG
       This  manual  page is part of the POSIX Programmer's Manual.  The Linux implementation of this interface may differ (con‐
       sult the corresponding Linux manual page for details of Linux behavior), or the  interface  may  not  be  implemented  on
       Linux.

NAME
       ls — list directory contents

SYNOPSIS # 書式
       ls [-ikqrs] [-glno] [-A|-a] [-C|-m|-x|-1] \
           [-F|-p] [-H|-L] [-R|-d] [-S|-f|-t] [-c|-u] [file...]

DESCRIPTION
...
OPTIONS
...
SEE ALSO # 関連事項
```
## セクション
マニュアルは種類毎に分類されている

| セクション | 項目            |
| ----- | ------------- |
| 1     | ユーザーコマンド      |
| 2     | システムコール       |
| 3     | システムライブラリや関数  |
| 4     | デバイスやデバイスドライバ |
| 5     | ファイルの形式       |
| 6     | ゲームやデモなど      |
| 7     | その他           |
| 8     | システム管理系のコマンド  |
| 9     | カーネルなどの情報     |
```bash
$ man 5 passwd
# passwdファイルの書式についてのマニュアル
passwd(5)                                              File Formats Manual                                             passwd(5)

NAME
       passwd - password file

DESCRIPTION
```