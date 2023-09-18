---
Tags:
  - ssh
  - 公開鍵
Status: Draft
TechArticles:
  - "[[SSH]]"
Last edited time: Invalid date
Created time: Invalid date
---
## Gitの設定ファイルは３種類

|   |   |   |
|---|---|---|
|system|システム全体|/etc/gitconfig|
|global|該当ユーザーの全リポジトリ|~/gitconfig|
|local|リポジトリ|.git/config|

system→global→localの順に参照する

## .git/configの中身

```
[core]        repositoryformatversion = 0        filemode = true        bare = false        logallrefupdates = true        ignorecase = true        precomposeunicode = true[remote "origin"]        url = https://github.com/hoge/fuga.git        fetch = +refs/heads/*:refs/remotes/origin/*[user]        name = hoge        email = hoge@gmail.com
```

### core

リポジトリの設定

### remote “origin”

リモートリポジトリの設定

### user

ユーザーの設定

  

## SSH key

```
$ ssh-keygen -t rsaGenerating public/private rsa key pair.Enter file in which to save the key (/Users/(ここはユーザーの名前が入ります)/.ssh/id_rsa): <----- ここで鍵ファイルを保存するフォルダの名前を設定できますEnter passphrase (empty for no passphrase): <----- パスフレーズEnter same passphrase again: <----- パスフレーズ（確認用）Your identification has been saved in /Users/(ここはユーザーの名前が入ります)/.ssh/id_rsa.Your public key has been saved in /Users/(ここはユーザーの名前が入ります)/.ssh/id_rsa.pub.The key fingerprint is:99:24:u0:8i:2d:34:e6:82:r5:89:41:p8:a7:58:1p:n2（ここにメールアドレスが入ります）.localThe key's randomart image is:+---[RSA 2048]----+|      . =+.+.    ||     . + .+      ||  . .   .. .     || o + o  o + .    ||o . o o.SB ..+   ||..     .+ +oo+o  ||o .    . ..*=.o. ||.o      . ..E+.o+||.           .+=o+|+-----------------+# keyが生成される$ lsid_rsa     #秘密鍵id_rsa.pub #公開鍵
```

  

## 接続先ごとにアカウントを変える

```
# ssh configをアカウントごとに設定する$ less ~/.ssh/configHOST accountA	HostName github.com	IdentifyFile ~/.ssh/github-A	User gitHOST accountB	HostName github.com	IdentifyFile ~/.ssh/github-B	User git# 現在のリモートリポジトリ URLgit remote -vorigin  git@github.com:takesoo/private-repository.git (fetch)origin  git@github.com:takesoo/private-repository.git (push)# リモートリポジトリ URL を# git@github.com:takesoo/private-repository.git から# accountA:takesoo/private-repository.git に変更する# (ユーザ名の git は変更先から取り除いている)git remote set-url origin accountA:takesoo/private-repository.git# 新たに設定したリモートリポジトリ URLgit remote -vorigin  accountA:takesoo/private-repository.git (fetch)origin  accountA:takesoo/private-repository.git (push)
```

[

GitHub 接続時の ~/.ssh/config の書き方 | Kadinche Corporation: VR AR MR Deep Learning

ssh/config の書き方 SSH キーが未登録の場合は 公式サイトの手順 に従って鍵の生成・登録までを事前に行っておきます。 まず GitHub へ接続する ~/.ssh/config の設定を見ていきます。GitHub で認証に使用する SSH キーは登録済みで秘密鍵は ~/.ssh/github へ配置している想定です。 ~/.ssh/config の各種設定項目は下記になります。 設定を行うことで github.com に SSH 接続する際、ユーザ名に git が指定され、~/.ssh/github に存在する秘密鍵を用いて SSH 接続を試みるようになります。 早速 github.com 接続時に正しく認証が通っているか確認するため、適当なプライベートリポジトリを git clone してみます。 プライベートリポジトリを git clone した実行結果 (成功)。無事に git clone できることが確認できたら成功です。 接続先に応じて秘密鍵を使い分けたい GitHub で常に同一の秘密鍵を用いて認証を行う際は問題ないのですが、例えば GitHub アカウントをプライベート用と仕事用で使い分けていて、リポジトリ先に応じて秘密鍵を使い分けたいというケースはあると思います。 その場合は ~/.ssh/config の設定と

![](https://www.kadinche.com/wp-content/uploads/2021/03/cropped-favicon-192x192.png)https://www.kadinche.com/github-接続時の-ssh-config-の書き方/

![](https://www.kadinche.com/wp-content/uploads/2021/04/ogp.png)](https://www.kadinche.com/github-接続時の-ssh-config-の書き方/)

まだできない、、

  

---

  

.ssh/configの仕事の方をコメントアウトすると`ssh git@github.com`はtakesooでログインできる。`ssh takesoo-GitHub`ではpermission denied (Publickey)になる

```
$ ssh git@github.comHi takesoo! You've successfully authenticated, but GitHub does not provide shell access.$ ssh takesoon-GitHubtakesoo@github.com: Permission denied (publickey).$ ssh github.com -i ~/.ssh/takesoo-GitHubkubotahotaka@github.com: Permission denied (publickey).
```

ssh config とssh-agentを調べてみるか

  

## ssh-agent

```
# ssh-agentに鍵を登録$ ssh-add -K path/to/public_key# 確認$ ssh-add -l# 使用# -A: エージェントの使用$ ssh -A public_key_name
```

ssh-agentを使うと踏み台サーバーに公開鍵を置かなくてもよしなにやってくれる

```
# ~/.ssh/configHost github	HostName github.com	User git	ForwardAgent yes # ssh-agentを有効にする# -Aがなくてもssh-agentが鍵を設定してくれる$ ssh -T githubHi user! You've successfully authenticated
```

[

ssh-agentを利用して、安全にSSH認証を行う

![](https://zenn.dev/images/icon.png)https://zenn.dev/naoki_mochizuki/articles/ce381be617cd312ffe7f

![](https://res.cloudinary.com/zenn/image/upload/s--vVp3T9sg--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:ssh-agent%25E3%2582%2592%25E5%2588%25A9%25E7%2594%25A8%25E3%2581%2597%25E3%2581%25A6%25E3%2580%2581%25E5%25AE%2589%25E5%2585%25A8%25E3%2581%25ABSSH%25E8%25AA%258D%25E8%25A8%25BC%25E3%2582%2592%25E8%25A1%258C%25E3%2581%2586%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:Naoki%2520Mochizuki%2Cx_203%2Cy_98/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzNhZmJiZGRkMGUuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/og-base.png)](https://zenn.dev/naoki_mochizuki/articles/ce381be617cd312ffe7f)

  

確認したがssh-agentは使用してなかったので今回の問題とは関係ない

  

---

## known_hosts

> [ssh](https://kaworu.jpn.org/security/ssh)では、2回目のログインからホスト認証が自動的に行われます。この認証では、ホスト鍵が利用されます。[ssh](https://kaworu.jpn.org/security/ssh)の**known_hosts** とは、サーバのホスト鍵(host key)の１つのホスト公開鍵を登録するファイルです。

[

known hosts - セキュリティ

![](https://kaworu.jpn.org/favicon.ico)https://kaworu.jpn.org/security/known_hosts



](https://kaworu.jpn.org/security/known_hosts)

  

  

[[SSH]]

  

[

Error: Permission denied (publickey) - GitHub Docs

A "Permission denied" error means that the server rejected your connection. There could be several reasons why, and the most common examples are explained below.

![](https://docs.github.com/assets/cb-600/images/site/favicon.png)https://docs.github.com/ja/authentication/troubleshooting-ssh/error-permission-denied-publickey\#make-sure-you-have-a-key-that-is-being-used

![](https://github.githubassets.com/images/modules/open_graph/github-logo.png)](https://docs.github.com/ja/authentication/troubleshooting-ssh/error-permission-denied-publickey\#make-sure-you-have-a-key-that-is-being-used)

↑で紹介されている現象と同じみたい

暗号鍵がssh-agentに登録されていなかったのでパスフレーズの入力を求められてしまって認証が失敗していたらしい。

```
# ssh-agentに鍵を登録$ ssh-add --apple-use-keychain ~/.ssh/takesoo-GitHubIdentity added: /Users/kubotahotaka/.ssh/takesoo-GitHub (takesoo-GitHubのために macOS の Sourcetree によって生成された)$ ssh github.com -i ~/.ssh/takesoo-GitHub -l git -vTHi takesoo! You've successfully authenticated, but GitHub does not provide shell access.
```

無事成功！

  

✅

仕組みを理解するの大事

  

### 2/25追記

```
# .ssh/configにgithub.comを追記した- Host *.github.com+ Host *.github.com github.com  AddKeysToAgent yes  UseKeychain yes  IdentityFile ~/.ssh/takesoo-GitHub
```