---
tags:
  - Rails
---
- レコードの同時編集を許可する
- `lock_version`カラムを用意し、「レコードが読まれた以降で変更があったかどうか」でチェックする
- 取得時と更新時で`lock_version`が一致しない場合は`ActiveRecord::StaleObjectError`が発生する