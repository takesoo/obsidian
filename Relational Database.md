---
aliases:
  - リレーショナルデータベース
  - 関係型データベース
  - RDB
tags:
  - データベース
---
データ量が多いとマイグレーションが困難になる
- テーブルロックが発生してシステムを停止させてしまうため
- カラムの型変更やインデックスの再作成などは、レコードの全権処理が必要になり時間がかかる
- トランザクションログが急激に増えてディスクを圧迫し、最悪の場合はログ領域の枯渇による障害が発生する可能性がある
- ロールバックにもコスト（時間とリソース）がかかる

| 操作内容                 | 実行コスト   | 特記事項・注意点                                                      |
| -------------------- | ------- | ------------------------------------------------------------- |
| **カラムの追加（末尾）**       | **低**   | たいていノンブロッキング（即時完了）。MySQL InnoDBでは内部的にメタデータだけの変更。              |
| **カラムの追加（途中）**       | **中〜高** | PostgreSQLでは簡単でも、MySQL InnoDBではテーブル再構築（全レコードコピー）になる。          |
| **カラムの型変更**          | **中〜高** | 型によっては全レコード更新が発生（`VARCHAR → TEXT` などは軽いが、`INT → BIGINT` は重い）。 |
| **カラムの削除**           | **中〜高** | 実データの再構築が必要。MySQLではほぼ全レコードのコピーが起きる。                           |
| **NOT NULL 制約の追加**   | **中**   | 既存データの全スキャンとバリデーションが必要になる。大量データでは時間がかかる。                      |
| **インデックスの追加**        | **中〜高** | 全レコードをスキャンしてインデックス構築。レコード数が多いと時間がかかる。                         |
| **インデックスの削除**        | **低〜中** | インデックス構造の削除のみなので比較的軽いが、レプリケーション遅延には注意。                        |
| **既存データの変換（UPDATE）** | **高**   | すべてのレコードを更新するので、最も重い操作の一つ。トランザクションログ肥大にも注意。                   |
| **テーブルのリネーム**        | **低**   | ほぼメタデータ変更のみ。即時に完了する。                                          |
| **主キーや外部キー制約の追加・変更** | **中〜高** | 全データの整合性チェックが走る。失敗リスクもあるので事前バリデーションが必要。                       |

動作
- 高速な[[主記憶装置|メモリ]]で処理を実施し、[[ディスク]]への書き込みを後回しにしている
- トランザクションの途中で異常終了してもリカバリできるように細かくログ出力している。
- 
