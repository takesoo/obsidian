---
tags:
  - ruby/gems
---
zipファイルを操作するためのgem
[[ストリーム|ストリーム処理]]が使える
## OutputStream
zipへの書き込みをするクラス。
```ruby
stream = Zip::OutputStream.new('zip_filename') # OutputStreamを生成。zip_filenameを出力先に指定
stream.put_next_entry('contained_filename') # 新しいエントリーを開く
stream.write(data) # データを書き込み
stream.close

# つぎのように書いても同じ
Zip::OutputStream.open('zip_filename') do |stream|
	stream.put_next_entry('contained_filename') # 新しいエントリーを開く
	stream.write(data) # データを書き込み
end

# ファイルストリームに書き込む場合はこのように書ける
Zip::OutputStream.write_buffer do |stream|
	stream.put_next_entry('contained_filename') # 新しいエントリーを開く
	stream.write(data) # データを書き込み
end
```