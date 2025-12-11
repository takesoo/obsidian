---
category: "[[Clippings]]"
author: "[[Nな人のWeb示録]]"
title: DBeaverでMySQL Dumpができない
source: https://nnahito.com/articles/63
clipped: 2023-10-02
published:
tags:
  - clippings
  - dbeaver
---

日本語対応で無料のOpenSource Universal Database Tool「[DBeaver](https://dbeaver.io/)」を最近使い始めました。

## 事象

MySQL Dumpをしようとすると、エラーが発生する。

```
IO error: Utility 'mysqldump.exe' not found in client home 'MySQL Router 8.0'
```

とかとか…

## 解決方法

私はscoopでmysqlをインストールしていたので、  
`C:\Program Files\MySQL\MySQL Router 8.0\`に`mysqldump.exe`と`mysqldump.shim`のシンボリックリンクを貼ってあげます。

パスは適宜、ご自身の環境に合わせてください。

```
mklink "C:\Program Files\MySQL\MySQL Router 8.0\mysqldump.exe" "C:\Users\USER_NAME\scoop\shims\mysqldump.exe"
mklink "C:\Program Files\MySQL\MySQL Router 8.0\mysqldump.shim" "C:\Users\USER_NAME\scoop\shims\mysqldump.shim"
```

そしてさらにJavaのエラー（`java.io.IOException: Process failed (exit code = 2).`）が出ると思うので、「Extra command args」に以下を追記

```
--column-statistics=0
```

## 参考

-   [https://github.com/dbeaver/dbeaver/issues/2765](https://github.com/dbeaver/dbeaver/issues/2765)