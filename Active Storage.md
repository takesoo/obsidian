---
tags:
  - Ruby_on_Rails
  - ActiveStorage
Link:
  - https://railsguides.jp/active_storage_overview.html
---
## What
> Active Storageは、Amazon S3、Google Cloud Storage、Microsoft Azure Storageなどのクラウドストレージサービスへのファイルのアップロードや、ファイルをActive Recordオブジェクトにアタッチする機能を提供します。

Ruby on Railsにおいてファイルを扱うための機能。ファイルはレコードに紐づく。

> Active Storageは、アプリケーションのデータベースで `active_storage_blobs`、`active_storage_variant_records`、`active_storage_attachments`という名前の3つのテーブルを使います。

### active_storage_blobs
[[ActiveStorageBlob|ActiveStorage::Blob]]モデルのテーブル
[[BLOB]]の情報（識別キー、ファイル名、Content-Type、メタデータ、サイズなど）を保存するテーブル

### active_storage_attachments
[[ActiveStorageAttachments|ActiveStorage::Attachments]]モデルのテーブル
[[ActiveStorageBlob|ActiveStorage::Blob]]とアプリ内のモデルをポリモーフィックで関連づける中間テーブル
これによってモデルとblobはn対nになる

### actve_storage_variant_records
[[ActiveStorageVariant|ActiveStorage::Variant]]モデルのテーブル
画像のサイズ変更や回転などの加工情報を保存する


## How
### 基本的な使い方
```ruby
# has_one_attached
class User < ApplicationRecord
  has_one_attached :avatar
end

user.create(avatar: ...)
user.avatar.attach(params[:avatar])
user.avatar.purge
user.avatar.purge_later

class Message < ApplicationRecord
  has_many_attached :images
end

message.create({images: [...]})
message.images.attach(params[:images])
```
### url生成
```ruby
if file.image?
	url_for(file.variant(resize_to_limit: [3456, 3018]))
else
	rails_blob_url(file)
end

```
### ダイレクトアップロード
クライアントサイドからストレージにアップロードして、後からActiveRecordと紐づける方法。
クロスオリジンリクエストを許可する必要がある。
### 設定
config/storage.yml
config/environments/development.rbでvariand_processorを設定できる
```ruby
Rails.application.configure do
config.active_storage.variant_processor = :vips | :mini_magick
```

## 内部的な処理の流れ
1. insert into active_storage_blobs
2. insert into active_storage_attributes
3. after_commitでファイルアップロード
4. [[ActiveStorageBlobAnalyzable]]

## signed_id
ファイルを安全に参照するために使用される一意の識別子。
署名されているので改竄されていないことを保証する。
使用用途
	- ファイルのセキュアな参照
	- 一時的なアクセス許可
	- フォームデータとの統合