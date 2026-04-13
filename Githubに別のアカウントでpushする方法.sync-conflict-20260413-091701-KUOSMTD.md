---
Tags:
  - Git
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
アカウントA：リポジトリのオーナー

アカウントB：作業アカウント

1. githubでリポジトリを作成する
2. リポジトリのコラボレーターにアカウントBを追加する
3. アカウントBに招待メールが届くのでacceptする
4. `git init`
5. `**git remote add origin git@使用する公開鍵:アカウントA名/リポジトリ名.git**`
6. `git remote -v`
7. `git push -u origin main`

[

【git】既存のフォルダをgit管理にしてリモートリポジトリと紐づける

既存のフォルダを、git管理下にしてリモートリポジトリと紐づけする方法を解説します。 物（ソース）を先に作成してしまってから、後からgitに登録したいなと思った時に、便利な方法です。 ターミナルを開いて、対象のフォルダに移動後、 git init コマンドを実行して、フォルダをgit管理下にします。 cd /path/to/dir git init git管理化にしたフォルダに、リモートリポジトリを設定します。 ・ SSH版（GitHub） $ git remote add origin git@github.com:ユーザ名/リポジトリ名.git ・ SSH版（HTTPS） $ git remote add origin https://github.com/ユーザ名/リポジトリ名.git フォルダ内のすべてのファイルを、Git管理するために git addでステージングします。git管理外にしたいファイルがある場合は、個別に git add してください。 次に、 git commit でローカルのgitリポジトリをコミットします git add * git commit -m 'first Commit.'

![](https://www.sukerou.com/favicon.ico)https://www.sukerou.com/2019/12/git-tipsgit.html

![](https://lh3.googleusercontent.com/pw/AM-JKLUIsYK3OnmlldTkzyVB01ca1DGCWciw5GGuBoGCoxSn_2ROWPm204a_3W2e0AdyeiDtUZq1lVzsv9JiMQi0n6rdFFDerBkEnwlgSt36eKVjw-NG7CPH-dyF4sB2aZ-E7o4QSbP9pxbKq-q3f40-5HtG=w1200-h630-p-k-no-nu)](https://www.sukerou.com/2019/12/git-tipsgit.html)

[

Error: Permission to user/repo denied to other-user - GitHub Docs

このエラーは、プッシュしているキーが、リポジトリへのアクセス権を持たないアカウントに添付されていることを示します。

https://docs.github.com/ja/authentication/troubleshooting-ssh/error-permission-to-userrepo-denied-to-other-user

![](https://github.githubassets.com/images/modules/open_graph/github-logo.png)](https://docs.github.com/ja/authentication/troubleshooting-ssh/error-permission-to-userrepo-denied-to-other-user)

[

複数のGitHubアカウントでそれぞれのSSH Keyを分けて使いたい。

2つのGitHubアカウントがあります。 例えば、プライベート用と仕事用を使い分けるときなどでしょうか、 別々の公開鍵をそれぞれのGitHubアカウントに登録済み。 リポジトリ毎にディレクトリを分けてssh keyを保管。 ~/.ssh/github_private/id_rsa ~/.ssh/github_job/id_rsa configファイルはこんな感じ。 ... Host github_private User git HostName github.com IdentityFile ~/.ssh/github_private/id_rsa Host github_job User git HostName github.com IdentityFile ~/.ssh/github_job/id_rsa ... こんな感じでリポジトリが登録されていると思います。 git remote -v origin git@github.com:アカウント名/リポジトリ名.git (fetch) こんな感じでホスト(@以下の文字列)をconfigファイルで設定しているHostにしてやればOK git remote set-url origin git@github_job:アカウント名/リポジトリ名.git (fetch) ⇣ git remote -v origin git@github_job:アカウント名/リポジトリ名.git (fetch) 私はconfigファイルを整理していたら、こんな権限エラーを吐くようになり、上記の方法で解消しました。 仕事用アカウントにpushしたいのに、プライベート用のssh keyを参照していた様子。 $ git push origin master git@github.com: Permission denied (publickey).

![](https://zenn.dev/images/logo-transparent.png)https://zenn.dev/komiya/articles/multiple-github-account-sshkey

![](https://res.cloudinary.com/zenn/image/upload/s--OaGW5HdQ--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:komiya%2520komiya%2520k...%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:%25E8%25A4%2587%25E6%2595%25B0%25E3%2581%25AEGitHub%25E3%2582%25A2%25E3%2582%25AB%25E3%2582%25A6%25E3%2583%25B3%25E3%2583%2588%25E3%2581%25A7%25E3%2581%259D%25E3%2582%258C%25E3%2581%259E%25E3%2582%258C%25E3%2581%25AESSH%2520Key%25E3%2582%2592%25E5%2588%2586%25E3%2581%2591%25E3%2581%25A6%25E4%25BD%25BF%25E3%2581%2584%25E3%2581%259F%25E3%2581%2584%25E3%2580%2582%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2d1QlJpbzMyb1hrMlRzdTB3Q0QzTnMyWmVhNUo5Ml9SaTlfRTJ3c1E9czgwLWM=%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png)](https://zenn.dev/komiya/articles/multiple-github-account-sshkey)