---
tags:
  - ruby/class
---
文字列にIOと同じインターフェースを持たせるためのクラス

```ruby
require "stringio"
sio = StringIO.new("hoge", 'r+')
p sio.read                 #=> "hoge"
sio.rewind
p sio.read(1)              #=> "h"
sio.write("OGE")
sio.rewind
p sio.read                 #=> "hOGE"
```

### rewind
自身のファイルポインタの位置(pos)と現在の行番号(lineno)をそれぞれ0にする