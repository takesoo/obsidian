---
tags:
  - Ruby_on_Rails
  - ActiveStorage
aliases:
  - ActiveStorage::Blob::Analyzable
---
- 関連するアナライザーを使用して、このブロブに関連付けられたファイルからメタデータを抽出して保存します。Active Storageには、画像と動画用のアナライザが組み込まれています。
	- ActiveStorage::ImageAnalyzer
	- ActiveStorage::VideoAnalyzer
- アナライザーが処理に失敗してもファイル自体はストレージに保存され、active_storage_blobsテーブルとactive_storage_attachmentsテーブルへのレコード作成される。