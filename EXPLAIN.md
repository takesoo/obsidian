---
tags:
  - mysql
---
- [[mysql-index|index]]が効いているかを確認するコマンド

## type
| type   | 意味                                                                         |
| ------ | ------------------------------------------------------------------------ |
| const  | PRIMARY KEYまたはUNIQUEインデックスのルックアップによるアクセス。最速    |
| eq_ref | JOINにおいてPRIMARY KEYまたはUNIQUE KEYが利用される時のアクセスタイプ    |
| ref    | ユニークでないインデックスを使って等価検索(WHERE key = value)            |
| range  | インデックスを用いた範囲検索                                             |
| index  | フルインデックススキャン。インデックス全体を好かんする必要があるので遅い |
| ALL    | フルテーブルスキャン。インデックスがまったく利用されていない             |
- indexまたはALLをみたらチューニングが必要。
## possible_keys
- 使用できるインデックス

## key
- 実際に使用したインデックス

## rows
- 実行計画上で、このクエリで検査する行数
