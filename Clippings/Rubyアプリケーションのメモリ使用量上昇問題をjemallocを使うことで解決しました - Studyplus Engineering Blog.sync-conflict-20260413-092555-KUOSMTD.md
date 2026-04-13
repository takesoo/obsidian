---
category: "[[Clippings]]"
author: "[[Studyplus Engineering Blog]]"
title: "Rubyアプリケーションのメモリ使用量上昇問題をjemallocを使うことで解決しました - Studyplus Engineering Blog"
source: https://tech.studyplus.co.jp/entry/2019/09/09/094140
clipped: 2023-08-17
published: 2019-09-09
topics: 
tags: [clippings]
---

こんにちは、スタディプラスの栗山([id:shepherdMaster](http://blog.hatena.ne.jp/shepherdMaster/))です。  
今回はRubyアプリケーションのメモリ使用量上昇問題をjemallocを使うことで解決した話です。

弊社ではRuby on Railsをメインで使っていますが、Rubyアプリケーションを長時間稼働させていると、次第にメモリ使用量が増えていく問題に悩まされていました。  
これは、Rubyのメモリ領域の断片化によって引き起こされるそうです([参考](https://techracho.bpsinc.jp/hachi8833/2017_12_28/50109))

この問題に対応するため、 [puma\_worker\_killer](https://github.com/schneems/puma_worker_killer)という定期的にpumaのworkerをrestartさせるためのgemを使用し、メモリ使用量が上昇するのを抑えていました。  
しかし、puma worker killerの実行のタイミングでたまに`No connection pool`エラーが発生することがあり、調査や修正を試みていたのですが解決にはいたらない状態でした。

そこでpuma worker killer以外の解決方法を探すことになり、候補として上がったのが[jemalloc](http://jemalloc.net/)です。  
jemallocを使うとメモリ領域の断片化を減らしてくれるということで実際に導入してみました。

EC2インスタンス上で運用しているサービスに関しては、Rubyビルド時にjemallocを有効化するオプション(`RUBY_CONFIGURE_OPTS=--with-jemalloc`)をつけてjemallocを有効化しました。

AWS Elastic Beanstalk[1](#fn:1)上で動いているサービスに関しては、Rubyのビルドオプションを指定することが難しそうなので、[LD\_PRELOADを使った方法](https://github.com/jemalloc/jemalloc/wiki/Getting-Started)を取りました。  
(LD\_PRELOADを使った方法は手軽ですが、そのサーバーのすべてのアプリケーションに影響がでるので注意が必要です。幸い私達の場合、特に問題は出ませんでした。)

## jemallocが有効になっているかの確認方法

### Rubyビルド時にjemallocを有効にした場合

$ ruby -r rbconfig -e 'puts RbConfig::CONFIG\["MAINLIBS"\]'
-lz -lpthread -lrt -lrt -ljemalloc -ldl -lcrypt -lm

`-ljemalloc`が表示されれば有効化されています。

### LD\_PRELOADを使った場合

Rubyアプリケーションのpid(pumaで動かしていればpumaのpid)を指定してメモリマップを確認し、jemallocへのパスが表示されれば有効化されています。

$ sudo strings /proc/10289/maps  | grep jemalloc
7f59e5ff8000-7f59e6028000 r-xp 00000000 103:03 12712                     /usr/lib64/libjemalloc.so.1
7f59e6028000-7f59e6227000 ---p 00030000 103:03 12712                     /usr/lib64/libjemalloc.so.1
7f59e6227000-7f59e6229000 rw-p 0002f000 103:03 12712                     /usr/lib64/libjemalloc.so.1

### メモリ使用量

![f:id:shepherdMaster:20190828124434p:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/s/shepherdMaster/20190828/20190828124434.png "f:id:shepherdMaster:20190828124434p:plain")

12:00あたりまではpuma worker killerを15分起きに実行していたのでグラフが小刻みに上下しています。 その後12:00過ぎからpuma worker killerを2時間起きに実行するように変更しました。これによりメモリ上昇が起こっているのが分かります。  
そして15:00あたりからjemallocを有効にした結果メモリ使用量が大きく下がりました。約3割減です 🎉 またその後メモリ使用量も上がることなく安定しています。  
jemallocによりメモリ使用量の上昇を抑えられることが分かったのでpuma worker killerをその後取り除きました。取り除いた後もメモリ使用量は安定しています。

ありがとう、jemalloc 🙏

### レスポンスタイム

Load AverageとCPU使用率に関しては変化がありませんでしたが、レスポンスタイムは気持ち速くなったようにみえます 😊

![f:id:shepherdMaster:20190828124458p:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/s/shepherdMaster/20190828/20190828124458.png "f:id:shepherdMaster:20190828124458p:plain")

jemallocを導入することでRubyアプリケーションのメモリ使用量が上昇しつづける問題を解消することができました。 これほど効果がでるならもっと早く導入しておけばよかったなと思ってます。  
Rubyアプリケーションのメモリ使用量上昇にお困りでしたら、ぜひjemallocをお試し下さい。