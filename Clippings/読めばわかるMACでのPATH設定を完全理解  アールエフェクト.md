---
URL: https://reffect.co.jp/windows/full_understanding_mac
---
[![](https://reffect.co.jp/wp-content/uploads/2019/01/mac_path.png)](https://reffect.co.jp/wp-content/uploads/2019/01/mac_path.png)

---

目次

## 環境変数PATHとは

環境変数PATHは、コマンドを実行するプログラムが保存されているディレクトリの場所を設定する変数です。

## なぜpwdコマンドが実行できるのか

環境変数PATHの説明を聞いても最初理解することは非常に難しいです。環境？変数？さっぱり意味がわかりません。理解できたとしてもなぜ環境という言葉を使うのかはわからないかもしれません。

言葉の意味を聞いて理解するよりも動作させながら少しずつ理解を深めていくのが最善の方法だと思いますのでコマンドを使いながら説明を行っていきます。

現在ユーザがいるディレクトリを確認するpwdコマンド(print woking directory)を通して、環境変数PATHを理解していきましょう。

MACでターミナルを起動し、pwdコマンドを実行すると実行時にユーザがいるディレクトリの場所が表示されます。この/Users/macのことをPATH(パス)と呼びます。

ターミナルは上部のメニューの移動→ユーティリティを選択すると表示されるフォルダの中で見つけることができます。実行すると下記のような画面が表示されます。下記の画面の中でコマンドを入力していきます。

ターミナルを起動

```
 % pwd/Users/mac
```

ユーザがいる場所といってもピント来ない人はWindowsでフォルダを開いてその中にあるファイルをクリックする操作を思い浮かべてください。ファイルをクリックしたフォルダが現在ユーザがいる場所を表します。そのフォルダ内にさらにフォルダがあり、そのフォルダに移動するとユーザのいる場所はそのクリックしたフォルダになります。また場所というからにはそこがどこなのかを示す必要がありますその情報を表すのがパスになります。

では、なぜpwdコマンドを実行できるのでしょうか？

コンピューターで何かの処理を行うためにはプログラミング言語で記述されたファイルを実行する必要があります。pwdコマンドが実行できるということはこのファイルの保存場所がどこにあるのかを知っておく必要があります。

lsコマンドを実行して現在のディレクトリに存在するファイルを確認してみましょう。

lsコマンドは指定したディレクトリ内のファイルの一覧を表示するコマンドです。ディレクトリを指定しない場合は、実行したディレクトリ内のファイル一覧が表示されます。

lsコマンドを実行しても現在いるディレクトリにはpwdの実行ファイル（pwdと同じ名前のファイル）はありません。

```
mac$ lsApplicationsDesktopDocumentsDownloadsLibraryMoviesMusic PicturesPublic
```

先ほどの説明では必ずコマンドに対応するファイルが存在すると言ったのにpwdといったファイルもlsといったファイルも存在しません。しかし、pwdコマンドもlsコマンドも実行できました。

では、pwdの実行ファイルはどこにあるのでしょうか？

コマンドがどこにあるのかを教えてくれるコマンドも存在します。それがwhichコマンドです。

whichコマンドは実体はプログラムです。pwdコマンドと同様にwhichファイルがないに実行できます。実行できる理由はpwdコマンドと同じです。

whichコマンドを実行するとpwdの実行ファイルの場所を確認することができ/binディレクトリの下に存在することがわかります。

whichコマンドに空白を１つ開けてコマンドを指定すると指定したコマンドの実行ファイルが保存されている場所を見つけることができるコマンドです。そのコマンドがMacにインストールされて存在するのかチェックする時に実行します。

```
 % which pwd /bin/pwd % which ls /bin/ls
```

whichコマンドを利用することでpwdコマンドは、/binディレクトリの下にあることはわかりましたが、なぜbinの下にあるpwdコマンドを/Users/macから実行することができるのでしょう。

pwdコマンドが存在する場所は/bin/lsということがわかりました。ではなぜlsコマンドを実行することができるのでしょう。

ユーザがいる場所は存在しないpwdコマンドを実行するために重要な役割をするのが環境変数PATHです。

## printenvコマンドにより環境変数一覧を表示

環境変数PATHというものがコマンドを実行するために重要な役割をすると説明をしました。では環境変数とは一体なんなのでしょう。環境変数は何なのか確認するために環境変数を表示してみましょう。

Macには環境変数を一覧表示するprintenvというコマンドが備わっています。

printenvを実行するとさまざまな情報が表示されますが、表示されている中からPATHという文字列を探してください。

```
 % printenvTERM_PROGRAM=Apple_TerminalSHELL=/bin/bashTERM=xterm-256colorTMPDIR=/var/folders/5h/bxvtcnj978dfmqwdkr59xvj40000gn/T/Apple_PubSub_Socket_Render=/private/tmp/com.apple.launchd.FWsThb4MSd/RenderTERM_PROGRAM_VERSION=421.1TERM_SESSION_ID=A16EEA5A-E4A8-4F62-99B9-2CE967439E34USER=macSSH_AUTH_SOCK=/private/tmp/com.apple.launchd.SI0Id5ua6b/ListenersPATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbinPWD=/Users/macLANG=ja_JP.UTF-8XPC_FLAGS=0x0XPC_SERVICE_NAME=0SHLVL=1HOME=/Users/macLOGNAME=macSECURITYSESSIONID=186a8_=/usr/bin/printenv
```

上から順番に見ていくと真ん中あたりにPATHという文字列があるかと思います。環境変数PATHには、/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbinが設定されていることがわかります。

このPATHの設定値の中にpwdの実行ファイルが存在した/binディレクトリがあることが確認できます。先ほど実行したpwdコマンドは/binの下にあったことを思い出してください。環境変数PATHがこの情報を持っているからこそ/binディレクトリに存在するpwdコマンドが実行できるのです。

## echoコマンドによるPATHの確認

先ほどはPATHを含め、設定されている環境変数すべてが表示されましたが、echoコマンドにより個別の環境変数の設定値を確認することができます。

```
％ echo $PATH/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

## pwdコマンドの実行時の流れ

ここまでの説明でpwdコマンドは/bin/ディレクトリの下に存在することがわかりました。また環境変数PATHというものが存在し、その環境変数にはpwdコマンドが存在する/binディレクトリの情報を持っていることがわかりました。

環境変数PATHの力を借りてどのようにpwdコマンドが実行されるのか流れをみていきましょう。

1. ターミナルを起動してpwdコマンドを実行
2. 環境PATHの中身を確認(/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin)。先頭に記述されているディレクトリから順番にそのディレクトリ下にpwdの実行ファイルがあるかを確認していきます。
3. 最初に環境変数PATHの先頭に表示されている/usr/local/binの下にpwdの実行ファイルがあるか確認します。存在しない場合は、次に記述されているディレクトリを確認します。
4. /usr/binの下にpwdの実行ファイルがあるか確認するが存在しない場合は、次に記述されているディレクトリを確認します。
5. /binの下にpwdの実行ファイルがあるか確認し、pwdコマンドが存在することを確認。
6. /binが存在することが確認できたのでようやくpwdの実行ファイルを実行することができます。

ここまでの読み進めてもらえれば環境変数PATHがどのように利用されるのかを理解することができます。

## PATHを通すとは

購入直後のMacには存在しないコマンドを例にしてPATHを通すとは何かを説明していきます。特にサーバなどのインフラ系で働くとすぐに耳にするのがこのPATHを通すという言葉です。何かコマンドを実行して実行できない場合に同僚からそのコマンドPATHがちゃんと通っているか確認しておいてといった使われ方をします。この文書の残りを読み進めればPATHを通すの意味とPATHの通し方を理解することができます。

PATHの理解のためPHPのパッケージ管理のcomposerをインストールした場合に作成される実行ファイルを利用します。

composerをインストールするとインストールを実行したディレクトリにcomposer.pharという実行ファイルが作成されます。インストールディレクトリは、/Users/mac/Documentsで行っています。

ここではcomposer.pharという実行ファイルを通して説明しますが、実行ファイルについては自分の作ったものでも他のパッケージのファイルでも構いません。方法はすべて同じです。

composer.pharを実行してみましょう。

```
mac$ pwd/Users/mac/Documentsmac$ lscomposer.pharmac$ ./composer.phar -VComposer version 1.8.0 2018-12-03 10:31:16
```

“./composer.phar”の”./”は現在のディレクトリの中にあるcomposer.pharを実行することを意味しています。

しかし、/Users/macディレクトリに移動して実行するとcommand not found(コマンドが見つかりません)のエラーが表示されます。

```
mac$ composer.phar-bash: composer.phar: command not found
```

ではcomposer.pharをインストールを実施したディレクトリ(/Users/mac/Documents)以外で実行するにはどうすればいいのでしょう。

新たに環境変数のPATHにcomposer.pharのPATH(パス)を追加することでどこのディレクトリからもcomposer.pharを実行できるようになります。

この設定作業をcomposer.pharの”PATHを通す”といいます。

設定方法はexportコマンドを利用して環境変数PATHに新たなPATHを追加します。追加後、/Users/macからでもcomposer.pharを実行することができます。

$PATHには現在設定されているPATHの情報が含まれています。その値に追加したい場合は:(コロン)の値に追加したいパスを指定します。

```
mac$ export PATH=$PATH:/Users/mac/Documentsmac$ echo $PATH/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/mac/Documentsmac$ pwd/Users/macmac$ composer.phar -VComposer version 1.8.0 2018-12-03 10:31:16
```

既存のPATHにcomposer.pharが存在する場所の/Users/mac/Documentsを追加することでcomposer.pharが存在する場所(/Users/mac)とは異なる場所からもコマンドを実行できることがわかりました。しかし、exportコマンドで実行した設定は一時的なもので、別のターミナルを起動するとその設定は解除されてしまいます。

## .bash_profileファイルに環境変数PATH設定

exportコマンドでは環境変数の値を永続的に保持することができません。そのため、.bash_profileファイル(使用しているシェルがzshシェルの場合は.zshrcになります。bashシェルの場合は.bash_profileです。)を使用して、環境変数PATHの値を保持させます。

.bash_profile（.zshrc）はユーザがターミナルを新規で開いた場合に読み込まれるファイルです。このファイルに設定を加えれば、ターミナルを開いた時にその中に記述した設定を反映することができます。シェルの確認はこれも環境変数から確認できます。echo $SHELLを実行し、/bin/zshならzshシェルです。/bin/bashならbashシェルです。

MACインストール直後では.bash_profileは存在しないので、下記のコマンドを使用して.bash_profileを作成し、そのファイルの中にexportコマンドの設定を記述します。

```
mac$ echo 'export PATH=$PATH:/Users/mac/Documents' >> .bash_profile
```

.bash_profileファイルを作成後、再度ターミナルを開いて、composer.pharを実行するとPATHが通っているため、実行することできます。

```
mac$ composer.phar -VComposer version 1.8.0 2018-12-03 10:31:16
```

## /usr/local/binへの実行ファイルの保存

ここからの作業は管理者権限が必要となるため、管理者権限がない場合は実行できません。

exportコマンドを使用して、環境変数PATHの追加を行いましたが、環境変数PATHに追加するのではなく、事前に環境変数PATHに設定されている/usr/local/binに実行ファイルを保存するという方法もあります。その場合は、.bash_profileの作成も必要ありません。

MACではインストール直後では、/usr/localにbinディレクトリは存在しないので作成を行います。作成完了後に実行ファイルを保存します。

環境変数PATHにはいくつかのディレクトリが設定されています。/usr/local/binはユーザが追加のパッケージをインストールした場合の実行ファイルを保存します。/usr/bin, /binにはユーザが使用するOS標準のコマンドが保存されています。sbinには、管理者が使用するコマンドが主に保存されます。

通常ユーザで/user/localにbinディレクトリを作成しようとするとアクセス権の問題で管理者のパスワードが必要となるため、通常のユーザでは作成できません。

```
mac$ mkdir /usr/local/binmkdir: /usr/local/bin: Permission denied
```

管理者権限で作成するためにsudoコマンドを利用します。binディレクトリを作成後、composer.pharをbinディレクトリに保存します。その際も管理者権限が必要となります。

```
mac$ sudo mkdir /usr/local/binPassword:mac$ sudo mv composer.phar /usr/local/bin/composer
```

/user/local/binに保存できれば、どこのディレクトリからも実行が可能です。

```
mac$ pwd/Users/mac/Musicmac$ composer -VComposer version 1.8.0 2018-12-03 10:31:16
```

## まとめ

ここまで読んでもらえれば、冒頭に記載した、”環境変数PATHは、コマンドを実行するプログラムが保存されているディレクトリの場所を設定する変数”という意味が理解できたかと思います。環境変数というものはMacだけに限らず、Windowsにも存在するもので、ここで環境変数PATHを理解することができれば今後PATHの設定について困ることはほとんどなくなります。PATHの設定はOSによって異なるので設定方法はその都度確認する必要はあります。

同じカテゴリーのおすすめ記事