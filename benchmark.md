```ruby
require 'benchmark'

Benchmark.bm do |x|
	x.report("sleep 1:") { sleep(1) }
	x.report("sleep 2:") { sleep(2) }
end

#=>
#user     system      total        real
#sleep 1: 0.000000   0.000000   0.000000 (  1.001234)
#sleep 2: 0.000000   0.000000   0.000000 (  2.002345)
```
- `user`：ユーザーCPU時間。アプリケーションが直接CPUを使って処理してる時間。計算処理やデータ加工など。
- `system`：システムCPU時間。OSのカーネルがCPUを使って処理している時間。ファイル読み書き、メモリ管理など。
- `total`：合計CPU時間
- `real`：実際の経過時間（実時間）