---
tags:
  - ruby/gems
---
zipファイルを操作するためのgem
[[ストリーム|ストリーム処理]]が使える
## OutputStream
zipへの書き込みをするクラス。
```ruby
stream = Zip::OutputStream.new('zip_filename') # OutputStreamを生成
stream.put_next_entry('contained_filename') # 新しいエントリーを開く
stream.write(data) # データを書き込み
stream.close
```