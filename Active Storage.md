---
tags:
  - Ruby_on_Rails
Link:
  - https://railsguides.jp/active_storage_overview.html
---
## What
> Active Storageは、Amazon S3、Google Cloud Storage、Microsoft Azure Storageなどのクラウドストレージサービスへのファイルのアップロードや、ファイルをActive Recordオブジェクトにアタッチする機能を提供します。

Ruby on Railsにおいて、ファイルを扱うための機能。ファイルはレコードに紐づく。

> Active Storageは、アプリケーションのデータベースで `active_storage_blobs`、`active_storage_variant_records`、`active_storage_attachments`という名前の3つのテーブルを使います。

### active_storage_blobs
[[BLOB]]の情報を保存するテーブル

### active_storage_attachments
モデルのクラス名を保存するポリモーフィックjoinテーブル

### actve_storage_variant_records
[[ActiveStorageVariant|ActiveStorage::Variant]]モデルのテーブル
画像のサイズ変更や回転などの加工情報を保存する
