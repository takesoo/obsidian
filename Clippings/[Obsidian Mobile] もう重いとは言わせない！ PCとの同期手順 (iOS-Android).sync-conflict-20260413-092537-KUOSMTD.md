---
category: "[[Clippings]]"
author: "[[創造性原理]]"
title: "[Obsidian Mobile] もう重いとは言わせない！ PCとの同期手順 (iOS/Android)"
source: https://pouhon.net/obsidian-mobile/6320/#toc10
clipped: 2023-09-05
published: 2021-11-16
topics: 
tags: [clippings ソフトウェア]
---

※ この記事はObsidianのVaultを「iCloudやGoogle Driveで同期する」方に向けた記事となっています。しかし現在、Obsidianの同期方法は他にもいくつかの方法が発見され、実際に運用されています。  
特に**WindowsとMacを含む同期環境**を構築したい方、Obsidianのデータ同期に関してより詳しく知りたい方はこちらの記事を先にご覧ください。

[

![](https://pouhon.net/wp-content/uploads/2020/10/obsidian_logo-320x180.jpg)

Obsidianの複数端末同期方法まとめ (Mac/Windows/iOS/Android)

現在、ノートアプリObsidianのデータ(Vault)をiOSやAndroid...





](https://pouhon.net/obsidian-sync/6796/ "Obsidianの複数端末同期方法まとめ (Mac/Windows/iOS/Android)")

---

2021年7月、遂にiOS / Android対応のObsidianモバイルアプリが一般公開されました。この瞬間を待ち望んでいたのは僕だけではないはずです。

しかしいざ使おうと思うと、こんな問題に直面する方も多いのではないでしょうか。

-   **iCloudで同期したら固まって使えない！**
-   **Androidデバイスとクラウドストレージの同期が上手くいかない！**

これ、あなたが気付かぬうちに生成されたフォルダが原因かもしれません。**GitHubを使ったことは？**

そうでなくとも、こんなエラーもあり得ます。

-   **横幅が画面と合ってなくて使いづらい！**

これもありがち。もしかして初回起動時、**セーフモードをOFF**にしませんでした？

今回はObsidianモバイルアプリ、特にパソコンとモバイルのデータ同期に焦点を絞り、

-   「重い」問題の回避方法
-   iOS – PCの同期手順
-   Android – PCの同期手順

をお伝えします。この手順通り進めれば多くの場合、同期に関する問題は回避できるはずです。

## 「重い」を回避するための事前チェック

この後パソコンとモバイルアプリのデータ同期を行っていきますが、実際の手順に入る前に1つだけ確認していただきたいことがあります。

パソコン上で、今までObsidianで使っていたVaultフォルダを開いてみてください。また念のため、隠しファイルを表示する設定になっているかチェックしておきましょう。

-   Windowsなら… エクスプローラ上部『表示』> 『隠しファイル』にチェックを入れる

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210720_041-e1626713911699.png)

-   Macなら… Finderのショートカットキー Command+Shift+.(ピリオド) で、隠しファイルの表示/非表示を切り替え

開いたVaultフォルダ内に、こんなフォルダは無いでしょうか？

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210720_023-e1626714815649.png)

『.git』という名前の隠しフォルダ。これはGitHubを使ったときに自動的に作られるフォルダです。なので「GitHubを全く知らない」という方は見当たらないはず。

このフォルダ、GitHubを利用する際に必要なものが詰まっていますが、**非常にファイル数が多い**。  
しかも今のところ、**モバイルアプリ単体ではGitHubが使えません**。つまりモバイルにとっては全くの無駄フォルダです。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210720_031-e1626715425980.png)

この「.git」が入ったままのVaultフォルダを丸ごとiCloudにコピーしたり、クラウドストレージに入れたままにしておくと、

-   iOS – **初回起動時に長時間固まる (ように見える)**
-   Android – **クラウド上のファイルをデバイスにダウンロードする段階で失敗する**

こんなエラーが高確率で起こります。.gitフォルダが見つかった方はこの後の手順で、iCloud Driveやクラウドストレージから除外してください。GitHubにこだわりが無ければ普通に削除してもOKです。

僕の環境ではこのフォルダを退避させるだけで上記の問題を回避できましたが、他にも作った覚えのない隠しフォルダが見つかった場合、一度サイズやファイル数を確認しておくことをおすすめします。

(**『.obsidian』と『.trash』フォルダはそのまま残しておいてください**)

## パソコンとの同期方法3種

前置きが長くなってしまいましたが、ここから本題。パソコンとモバイルアプリの同期方法です。

公式アナウンスされているのでご存知の方も多いと思いますが、ObsidianモバイルアプリはそのOSによって、選択できる同期方法が異なります。

この中で『Obsidian Sync』はObsidianが提供するサブスク型有料サービス。ほとんどの方は無料で事足りるので、今回はここには触れません。

以降はiOS → Androidの順で同期手順をご紹介していきます。

## iOSの場合: iCloud Driveで同期する

iPhoneやiPadはiCloud Drive (Appleのオンラインストレージ) を使って同期するのが一般的。  
Macをお使いの方は問題無いはずですが、Windowsを使っていて**iCloudがインストールされていない**という方は、まずiCloudアプリをWindows Storeからインストールしましょう。

### iCloudのインストール

[

![](https://microsoft-store.azurewebsites.net/store/detail/9PKTQ5699M62/image)

Get iCloud from the Microsoft Store

iCloudforWindowsでは、写真、ファイル、カレンダー、連絡先、パスワード、およびその他の重要な情報が常にiPhoneとWindowsPC間で自動的に同期されます。iCloud写真•すべてのデバイスおよびPCで写真を最新の状態に保つことができます。•共有アルバムを作成して、ほかの人に写真やビデオやコメントを追...





](https://www.microsoft.com/ja-jp/p/icloud/9pktq5699m62?rtc=1&activetab=pivot:overviewtab "Get iCloud from the Microsoft Store")

インストール後、設定画面が開きます。ここではiCloudに関して詳しく触れませんが、『iCloud Drive』にチェックが入っているかどうか確認してください。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210718_031-e1626615002759.png)

正しくインストールされていれば、パソコン内にiCloud Driveフォルダが作られます。エクスプローラで探しても見当たらない場合は、`C:\Users\ユーザー名`をチェックしましょう。

ちなみにこのフォルダの中身はこんな感じ。iOSアプリ名のフォルダが並んでいます。(実際にはObsidianフォルダはまだ無くてOK)

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210718_041-e1626615423650.png)

### Obsidianモバイルアプリのインストール

次にiPhoneやiPadでObsidianモバイルアプリをインストール。

![Obsidian - Connected Notes](https://is5-ssl.mzstatic.com/image/thumb/Purple115/v4/47/9c/af/479cafd5-84cf-2d0f-3c24-8a93655dd09a/source/512x512bb.jpg)

Obsidian – Connected Notes

Dynalist Inc.無料posted with[アプリーチ](https://mama-hack.com/app-reach/ "アプリーチ")

[![](https://nabettu.github.io/appreach/img/itune_ja.svg)](https://apps.apple.com/jp/app/obsidian-connected-notes/id1557175442?uo=4)

インストールできれば一度起動してください。こんな画面になります。

![](https://pouhon.net/wp-content/uploads/2021/07/IMG_37761-e1626614414369.png)

ここから新たなVaultを作成することもできますが、パソコンのファイルを同期するなら今の時点でやることはありません。ひとまずこのままアプリを閉じましょう。

### Vaultの中身をiCloud Driveにコピーする

次にパソコンのVaultフォルダをiCloud Driveにコピーします。

モバイルアプリをインストール後、再びパソコンでiCloud Driveを開くと**『Obsidian』というフォルダが新たに作成**されているはずです。ここにパソコンで使っていたVaultフォルダをコピーしてください。(安全のために『移動』ではなく『コピー』がおすすめ)

冒頭で触れたとおり、.gitフォルダは除いておきましょう。  
画像ではObsidianフォルダの直下に『Obsidian Vault』という名前のフォルダを新規作成し、その中に.git以外のフォルダやファイルを全てコピーしています。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210720_073-e1626745331936.png)

全てのファイルがiCloudにアップロードされるまで、僕の環境では1, 2分程度。タスクバーでクリックすると**アップデートの状況を確認**できるので、完了してから次に進みましょう。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210718_021-e1626614116749.png)

### モバイルアプリで確認する

アップデートが完了すれば、モバイルアプリをもう一度起動します。

![](https://pouhon.net/wp-content/uploads/2021/07/IMG_37762-e1626616440655.png)

一番下に先ほどVaultをコピーしたフォルダが追加されているはずです。ここをタップ。

![](https://pouhon.net/wp-content/uploads/2021/07/IMG_37791-e1626745571387.png)

「Enter Vault」をタップ。

![](https://pouhon.net/wp-content/uploads/2021/07/IMG_37781-e1626663596618.png)

この画面になります。この時点で「何の操作も受け付けない！ やっぱり固まった！」という方、それは**断じてフリーズではありません**。操作を受け付けないのはインデックス作業中だからです。

**インデックス作業とは:**  
大量のファイルをスムーズに扱うための事前準備。

このインデックス作業は.gitフォルダ対策をしていても10～20秒程度かかりますが、対策していない場合と比べるとその時間は格段に短くなるはず。黙って待ちましょう。

### Safe ModeはON！

インデックス作業が終了するとこの画面に。

![](https://pouhon.net/wp-content/uploads/2021/07/IMG_37801-e1626747629939.png)

パソコンでセーフモードをOFFにしている方、ここでもOFFにしたいですよね？ 気持ちは分かります。僕もそうです。

しかしここだけは**確実にON**にしておくことをおすすめします。ここでセーフモードをOFFにすると、こんな現象が起こるかもしれません。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_md.obsidian005-e1626748060962.jpg)

画面はAndroidですが、下に左右スクロールバーが出ています。これは**ページの横幅が画面の幅と合っていない**証拠。この状態では、はっきり言ってメチャクチャ使いづらいです。

この現象は後からセーフモードを解除しても起こりません。なので初回起動時だけはセーフモードをONにしておき、後からOFFに切り替えることをおすすめします。

### 正常起動後

モバイルアプリが問題無く使えることが確認できたら、最後にパソコンで使用するVaultをiCloudに切り替えます。

パソコンでObsidianを起動し、画面左下『Open another vault』をクリック。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210718_051-e1626620279811.png)

次の画面。『Open folder as vault』の『Open』をクリックし、iCloud Drive内のVaultフォルダを指定しましょう。

![](https://pouhon.net/wp-content/uploads/2021/07/ss_20210718_061-e1626620486135.png)

これでひとまず、iOS – PC間の同期セットアップは完了です。

## Androidの場合: クラウドストレージで同期する

次にAndroidデバイスの場合。こちらはGoogleドライブやDropboxといった一般的なクラウドストレージを利用しますが、ユーザーはいきなり厄介な問題に直面することになります。

### Androidが抱える矛盾と解決策

GoogleドライブやDropboxのモバイルアプリは、基本的にはオンライン上のデータを参照しています。  
オフラインで扱えるようにファイルをダウンロードするオプションもありますが、あくまでオプションでしかありません。  
一方Obsidianが扱えるのは、デバイス本体やSDカードに保存されているローカルファイルのみ。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-android-obsidian03-e1626670193447.png)

クラウド上のファイルをObsidianモバイルで編集したいユーザーは、完全に板挟み状態です。

この状況を打開するためには、なんとかしてクラウド上のフォルダを**丸ごとAndroidデバイスにダウンロード**し、さらにフォルダの内容を**定期的に同期**する必要があるということ。

この隙間を埋めるのがサードパーティ製のアプリです。ここで使うのは公式アナウンスでも紹介されていたFolderSyncというアプリ。

![FolderSync](https://play-lh.googleusercontent.com/yilMq6_MvvcOKncbWzlCHEG_3QPj7Tw5V_U6ul3IIdrue2boE-Ki_w9kIpXpOyQ50rM=s128)

FolderSync

Tacit Dynamics無料posted with[アプリーチ](https://mama-hack.com/app-reach/ "アプリーチ")

[![](https://nabettu.github.io/appreach/img/gplay_ja.png)](https://play.google.com/store/apps/details?id=dk.tacit.android.foldersync.lite)

選択肢は他にもありますが、僕が試してみた中ではこのFolderSyncが一番良かったので、このページではこちらを使います。

### FolderSyncのセットアップ

インストール後、画面に従って進めていくとこんな画面になります。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-foldersync.lite00-e1626752920172.jpg)

『内部ストレージの書き込み』と『バッテリー最適化』のFIXをタップしましょう。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-foldersync.lite01-e1626753204449.jpg)

表示が変わります。この状態で画面下側『OK』をタップ。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-foldersync.lite02-e1626753448996.jpg)

メイン画面が開きます。ここではまずアカウントを設定し、次に同期するフォルダを選択するという流れ。

まず『アカウント』の『追加』をタップして、利用するクラウドストレージを選択しましょう。ここではGoogleドライブを選択。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-android.foldersync.lite03-e1626753665249.jpg)

名前を自由に決めて『アカウント認証』をタップ。Googleアカウントからこのアプリを認証してください。

認証が終わればメイン画面に戻るので、次は『Synchronized folders』の『作成』をタップ。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-foldersync.lite04-e1626753892491.jpg)

ダイアログが開きます。認証済みの『Google Drive』をタップ。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-foldersync.lite15-e1626762934360.jpg)

同期の設定画面が開きます。ここでは、

1.  同期する方向を指定 (Androidでもパソコンでも編集するなら『Upload and Download』)
2.  クラウドストレージ上のVaultフォルダを指定
3.  クラウドストレージと同期したいローカルフォルダを指定 (新規作成も可)
4.  定期的に自動同期するならON → 間隔を選択
5.  『サブフォルダーの同期』をON
6.  『隠しファイルの同期』をON
7.  『削除を同期』は安全性重視ならOFF、同期の精度重視ならON

この状態で『保存』しましょう。

ここまでで設定は終了。メイン画面に戻り、『すべて同期』をタップすると同期が始まりますが、

![](https://pouhon.net/wp-content/uploads/2021/07/ss-foldersync.lite09-e1626762443472.jpg)

タップする前に冒頭で触れた**.gitフォルダ**がクラウドストレージ内に残っていないか確認してください。残っている状態で同期してしまうと、ファイル数が多すぎるのか**ほぼ確実に失敗**します。

### Obsidianのインストールとセットアップ

同期が完了してしまえばもう**勝確**です。Android版のObsidianをインストールしましょう。

![Obsidian](https://play-lh.googleusercontent.com/McJwuNc1Gbs8-XrPCH77Ar-qZMGujN6L0_zb_jv_0oBe2vwnmIboESQjPsTSu1uINbg=s128)

Obsidian

Dynalist Inc.無料posted with[アプリーチ](https://mama-hack.com/app-reach/ "アプリーチ")

[![](https://nabettu.github.io/appreach/img/gplay_ja.png)](https://play.google.com/store/apps/details?id=md.obsidian)

起動すると、最初に出てくるのはこんな画面。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-obsidian-android00-e1626667232912.jpg)

「あなたのデバイスの内部ストレージへのアクセス」を求める文章です。『Grant permission』をタップ。

![](https://pouhon.net/wp-content/uploads/2021/07/ss-obsidian-android01-e1626667575930.jpg)

承認が終わるとこの画面に。『Open folder as vault』で、クラウドストレージと同期させたフォルダを選択。

その後はiOSと全く同じ流れなので、サラッと復習。

1.  インデックス作業を待つ
2.  **セーフモードをONにして**起動する
3.  安定動作を確認後、セーフモードをOFFにして使用する

以上で完了です。お疲れさまでした。