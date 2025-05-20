## what
- テーブルをロックせずに（読み書きを止めずに）実行できる[[Data Definition Language|DDL]]

## how
```sql
ALTER TABLE users ADD COLUMN age INT, ALGORITHM=INPLACE, LOCK=NONE;
```

|モード名|概要|対応バージョン|備考|
|---|---|---|---|
|`INSTANT`|メタデータのみ変更、一瞬で完了|8.0以降|カラム追加（末尾）のみ対応|
|`INPLACE`|バックグラウンド処理、非ロック|5.6以降|インデックス追加など|
|`COPY`|テーブルをコピーして差し替える|全バージョン|**ロックが発生し、時間もかかる**|