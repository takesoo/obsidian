---
URL: https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/
---
Raspberry Pi(ラズパイ)を動かすには、ストレージとなるmicroSDカードへOSをインストールする必要があります。Raspberry Pi公式のSDカード作成ツールのImagerを用いて、Raspberry Pi OS (Raspbian)をインストールする手順をWindows/Mac/Ubuntuそれぞれについて解説しています。

本記事の内容は、最新のRaspberry Pi 4を含め、SDカードから起動する全てのRaspberry Piに対応しています。2021年11月に最新版のRaspberry Pi OS 「Bullseye」がリリースされました。それ以前のバージョン「Buster」をインストールしたい場合は、[過去バージョンRaspberry Pi OSのSDカード作成](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/#make-old-version)を参照して下さい。

### Raspberry Pi起動の仕組み

最初にRaspberry Piの起動の仕組みについて説明しておきます。

PC(パソコン)の場合はSSDやハードディスクといったストレージにWindowsやMacなどのOSがインストールされており、電源を入れるとそれらのOSが起動します。一方、Raspberry Piでは、microSDカードがストレージの役割をしており、そこに書き込まれたOSが起動する仕組みになっています。

そのため、PC上でRaspberry Pi用のOSが書き込まれたmicroSDカードを準備必要があるのです。Raspberry PiのOSのインストールと起動手順は以下の通りです。

1. PCでmicroSDカードにRaspberry Pi用のOSを書き込む。
2. 準備したmicroSDカードをRaspberry Piのスロットに挿入して電源を入れる。

8GB以上の容量のmicroSDカード、およびPC用のmicroSDカードリーダーが必要になりますので、無い場合は用意しましょう。

### Raspberry Pi Imagerとは

Raspberry Pi Imagerとは、Raspberry Pi財団が公式で用意しているSDカード作成ツールです。Windows, Mac, Ubuntu用があり、様々な環境で使えます。また、最新版のOSを自動でダウンロードしてくれる機能もあるので、便利です。

サードパーティー製のソフトでSDカードを作成することもできますが、本記事では公式のRaspberry Pi Imagerを使う方法を解説します。まずはPCにインストールしましょう。お使いのOSに応じて、次のいずれかに進んで下さい。

- [Imagerのインストール(Windows)](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/#install-windows)

### Imagerのインストール (Windows)

Raspberry Pi公式[ダウンロードページ](https://www.raspberrypi.org/downloads/)から、Windows版をダウンロードしましょう。

ダウンロードしたファイルをダブルクリックすると、インストーラーが起動しますので、「Install」をクリックします。

少し待つと、終了画面が表示されるので、「Finish」をクリックしてインストール完了です。スタートメニューから起動できるようになります。

インストールが完了したら、[Raspberry Pi OSについて](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/#about-raspberry-pi-os)に進んで下さい。

### Imagerのインストール (Mac)

Raspberry Pi公式[ダウンロードページ](https://www.raspberrypi.org/downloads/)から、Mac版をダウンロードしましょう。

ダウンロードした「imager.dmg」ファイルをダブルクリックすると、インストール画面になりますので、Raspberry Pi ImagerのappアイコンをApplicationsにドラッグ＆ドロップしてインストール完了です。Launchpadから起動できるようになります。

インストールが完了したら、[Raspberry Pi OSについて](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/#about-raspberry-pi-os)に進んで下さい。

### Imagerのインストール (Ubuntu)

Raspberry Pi公式[ダウンロードページ](https://www.raspberrypi.org/downloads/)から、Ubuntu版をダウンロードしましょう。

ダウンロードしたファイルと同じディレクトリでターミナルを開き、以下のコマンドでインストールします。「1.5」の部分にはバージョン番号が入るので、ダウンロードしたファイル名に合わせて下さい。

```
sudo apt install ./imager_1.5_amd64.deb
```

インストール後はランチャー画面から起動できます。

SDカードを消去(Erase)する場合は権限がないためエラーとなる場合があります。その場合は sudo rpi-imager コマンドで起動して下さい。

また、Linux MintなどのUbuntuベースのディストリビューションでも、権限確認のダイアログが出ないためにエラーとなる場合があります。その際も同様に sudo rpi-imager コマンドで起動することでエラーを回避できます。

```
sudo rpi-imager
```

### Raspberry Pi OSについて

Raspberry Piで動作するOSには、用途によって様々なものが開発されています。その中でも最も標準的なものがRaspberry財団が公式にリリースしている「Raspberry Pi OS」(2020年5月に「Raspbian」から名称が変更されました)です。

Raspberry Pi OSは、人気のあるLinuxディストリビューションのDebianをベースに、Raspberry Pi向けにカスタマイズしたものとなっています。Raspberry Piの特徴である、GPIO端子を通じてセンサーや拡張基板を制御することもできます。Indoor CorgiのRaspberry Pi用拡張基板も、Raspberry Pi OSで動作させることができます。

温度/湿度/気圧/明るさ/赤外線 ホームIoT拡張ボード「RPZ-IR-Sensor」

はじめてRaspberry Piを使う方は、Raspberry Pi OSを選んでおけば間違いないでしょう。また、microSDカードにOSをインストールする方式なので、カードを入れ替えるだけで別のOSを動作させることも可能です。

### Raspberry Pi OSをSDカードにインストール

Imagerの準備ができたので、Raspberry Pi OSをSDカードにインストールしていきましょう。8GB以上のmicroSDカードをPCに接続して下さい。

Raspberry Pi Imagerの使い方は各OSで共通ですが、環境によっては途中で確認のダイアログボックスや権限許可のパスワード入力画面が表示されます。適宜許可、入力するようにして下さい。

Imagerを起動すると、以下のような画面が表示されます。「CHOOSE OS」でインストールするOSを選択します。

1番上にあるRaspberry Pi OSを選択することで、自動的に最新版をダウンロードしてインストールしてくれます。

次に「CHOOSE SD CARD」をクリックして、書き込み先のデバイスを選択します。画面は環境によって変わりますが、ここでは16GBのmicroSDカードを選択しました。

**書き込み先のmicroSDカードは消去されますので、間違えないように注意して下さい。**余計な外付けストレージなどはあらかじめ外しておくと良いと思います。

「WRITE」ボタンをクリックします。

確認ダイアログが表示されるので、「YES」をクリックすると書き込みが始まります。

書き込みにはしばらく時間がかかります。以下のような画面が表示されたら成功です。「CONTINUE」をクリックして画面を閉じ、microSDカードをPCから取り外します。

エラーが出てうまくいかない時は、以下を確認してみて下さい。

- 別のSDカードスロットや、SDカードリーダーにするとうまくいく場合があります。
- Ubuntuおよび互換OSの場合は、「sudo rpi-imager」コマンドで起動してスーパーユーザー権限があるとうまくいく場合があります。

### 過去バージョンRaspberry Pi OSのSDカード作成

Imagerを使うことで、最新版のRaspberry Pi OSをインストールする手順を説明しました。ただ、状況によっては特定のバージョンをインストールしたい場合もあるでしょう。ここでは最新版以外のインストール方法を解説します。

### Buster版(Legacy)をインストール

現在Raspberry Pi OSの最新版はBullseyeとなっています。ただ、前のバージョンであるBusterからの変更点が多く、互換性目的でBuster版も引き続きサポートされています。Busterをインストールする場合はImagerで「CHOOSE OS」をクリック後、「Raspberry Pi OS (other)」をクリックします。

下にスクロールして、「Raspberry Pi OS (Legacy)」をクリックします。

これ以降は、[通常のインストール手順](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/#install)と同様です。対象のmicroSDカードを選択して「WRITE」をクリックして書き込みを開始します。Write successfulダイアログが表示されれば成功です。

### 特定のイメージをダウンロードしてインストール

特定のバージョンをインストールするには、まずインストールしたいバージョンのイメージファイルをダウンロードする必要があります。以下のページの各バージョンのディレクトリ内にあるzipイメージをダウンロードして下さい。

準備ができたら、Imagerを起動して「CHOOSE OS」をクリックします。

次に、「Use custom」をクリックします。

その後、ダウンロードしたイメージファイルを選択します。Imagerはimg/zipどちらの形式にも対応しています。(zipの場合は自動的に解凍して中のimgファイルを書き込みます)

これ以降は、[通常のインストール手順](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-os%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/#install)と同様です。対象のmicroSDカードを選択して「WRITE」をクリックして書き込みを開始します。Write successfulダイアログが表示されれば成功です。

### 注意点

ハードウェアのリリース日よりも古い日付のOSを使用すると、ハードウェアに対応していないために起動しない場合があります。Raspberry Piシリーズのハードウェアリリース日は[wikipedia](https://ja.wikipedia.org/wiki/Raspberry_Pi)を参照して下さい。

### SDカードのOSを消去する

インストールしたOSを消去して、microSDカードを別の用途に使いたいケースはどうしたら良いでしょうか？

Raspberry Pi OSをインストールしてあるSDカードは、特殊なファイルシステムを使っているため、ファイルを削除するだけでは別の用途(カメラやスマートフォンなど)に使えません。そこで、Imagerを使って消去する手順を説明します。

Imagerを起動したら、「CHOOSE OS」をクリックします。

下にスクロールして「Erase」を選択します。

これ以降は、上で説明したOSインストールと同じ手順です。対象のmicroSDカードを選択して「WRITE」をクリックします。

以下のような完了画面が表示されれば成功です。一般的にSDカードで使われるFAT32形式でフォーマットされるので、カメラやスマートフォンなどで使えるようになります。

### まとめ

Raspberry Pi公式のSDカード作成ツールのImagerを用いて、Raspberry Pi OS (Raspbian)をインストールする手順は以上です。これでRaspberry Piを起動する準備が整いました。

次のステップとして、[周辺機器の接続と初期設定の解説をした記事](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-setup/)も用意しているので、ぜひ参考にして下さい。