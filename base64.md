---
aliases:
  - base64 encode
  - base64 decode
---
## what
- [[2進数|バイナリデータ]]を64種類のアルファベットと記号（a-z, A-Z, 0-9, +, /）にエンコード・デコードすること
## why
- [[ASCII]]しか送ることができない[[SMTP]]で画像や音声などのデータを送るために、base64 encodeが[[MIME]]で定められた
- 現在ではJSONなどで特殊文字を含まないように画像データをbase64 encodeするなど
## how
```ruby
require 'base64'
Base64.encode64("Now is the time for all good coders\nto learn Ruby")

# => Tm93IGlzIHRoZSB0aW1lIGZvciBhbGwgZ29vZCBjb2RlcnMKdG8gbGVhcm4gUnVieQ==

str = 'VGhpcyBpcyBsaW5lIG9uZQpUaGlzIG' +
      'lzIGxpbmUgdHdvClRoaXMgaXMgbGlu' +
      'ZSB0aHJlZQpBbmQgc28gb24uLi4K'
puts Base64.decode64(str)

# This is line one
# This is line two
# This is line three
# And so on...
```

```ts
import { Buffer } from 'buffer';

Buffer.from("Now is the time for all good coders\nto learn Ruby").toString('vase64');
// => Tm93IGlzIHRoZSB0aW1lIGZvciBhbGwgZ29vZCBjb2RlcnMKdG8gbGVhcm4gUnVieQ==

const str = 'VGhpcyBpcyBsaW5lIG9uZQpUaGlzIG' +
            'lzIGxpbmUgdHdvClRoaXMgaXMgbGlu' +
            'ZSB0aHJlZQpBbmQgc28gb24uLi4K';
Buffer.from(str, 'base64').toString();
// This is line one
// This is line two
// This is line three
// And so on...
```