---
URL: https://speakerdeck.com/smt7174/gitcao-zuo-bian
---
[![](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_0.jpg?22282251)](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_0.jpg?22282251)

---

[ES2022の新機能](https://speakerdeck.com/smt7174/es2022no)

[serverless.pdf](https://speakerdeck.com/smt7174/serverless)

[Isar勉強会](https://speakerdeck.com/hoddy3190/isarmian-qiang-hui)

[回帰分析ではlm()ではなくestimatr::lm_robust()を使おう / TokyoR100](https://speakerdeck.com/dropout009/tokyor100)

[kintone × LINE Bot で餃子検定Botを作った話](https://speakerdeck.com/naberina/kintone-x-line-bot-dejiao-zi-jian-ding-botwozuo-tutahua)

[Help! I've created a serverless monolith - Azure Lowlands](https://speakerdeck.com/marcduiker/help-ive-created-a-serverless-monolith-azure-lowlands)

[FargateとAthenaで作る、機械学習システム](https://speakerdeck.com/nayuts/fargatetoathenatezuo-ru-ji-jie-xue-xi-sisutemu)

[Fantastic passwords and where to find them - at NoRuKo](https://speakerdeck.com/philnash/fantastic-passwords-and-where-to-find-them-at-noruko)

[Build your cross-platform service in a week with App Engine](https://speakerdeck.com/jlugia/build-your-cross-platform-service-in-a-week-with-app-engine)

[Fireside Chat](https://speakerdeck.com/paigeccino/fireside-chat)

[Design and Strategy: How to Deal with People Who Don’t "Get" Design](https://speakerdeck.com/morganepeng/design-and-strategy-how-to-deal-with-people-who-dont-get-design)

[For a Future-Friendly Web](https://speakerdeck.com/brad_frost/for-a-future-friendly-web)

[Debugging Ruby Performance](https://speakerdeck.com/tmm1/debugging-ruby-performance)

[StorybookのUI Testing Handbookを読んだ](https://speakerdeck.com/zakiyama/ui-testing-handbook-by-storybook)

## Transcript

1. プルリクエスト 5. 便利な拡張機能 6. まとめ 2
    
    ### [アジェンダ 1. 自己紹介＆注意事項 2. 基本的な操作 3. その他の操作 • リポジトリ作成/ステージング・コミット・プッシュ/コンフリクト 4.](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_1.jpg)
    
2. 業務：クラウド全般(主にサーバーレス、及びバックエンド全般が好き) ◼ 技術： • AWS/Serverless Framework/AWS CDK/Terraform/その他サーバーレスバックエンド全般 • TypeScript/JavaScript/Jest/Vue/React(Next.js)/Haxe • CI/CD(GitHub Actions), コンテナ(Docker/ECR/ECS Fargate) etc. ◼ SNS： • @makky12 (SUZUKI Masaki＠クラウドエンジニア) • https://github.com/smt7174/ • http://makky12.hatenablog.com/ 3
    
    ### [自己紹介 ◼ 名前：鈴木 正樹(Masaki Suzuki) ◼ 在住：愛知県 ◼ 職業：フリーランスエンジニア(ただし今月で一旦活動終了) ◼](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_2.jpg)
    
3. • （例）「Gitってなに？」「コミットって何？」など • 必要に応じて、各自調べてください ◼ 本資料は、下記URLで公開しています • https://hogehoge.fugafuga.com 4
    
    ### [注意事項 ◼ 今回の発表は、本当に「基礎編」の内容になります • VS CodeでのGit操作の本当に基礎的な部分のみの発表になります • その旨、ご了承ください ◼ Gitの仕様については、時間の都合上、説明は省略します](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_3.jpg)
    
4. VS Codeで実行できるGit機能は全てここから実施可能 • Gitでよく使う機能は、ほとんどが実施可能（※ただしプルリクエストは除く） ◼ チェックアウトと変更の同期(push&pull)は、ステータスバー左下から も実施可能 6 チェックアウト 変更の同期
    
    ### [基本的な操作 ◼ VSCodeのGit機能は、「ソース管理」(赤丸)アイコンから実施する • アイコンをクリックすると、左側に「ソース管理」が表示される • [表示]-[ソース管理]やCtrl+Shift+G→Gでも表示できる ◼ 「…」(青丸)をクリックすると、一覧メニューが表示される •](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_5.jpg)
    
5. ◼ 一部の拡張機能は、GitHubへのサインインが必要 • GitHub Pull Request And Issues(後述)、GitHub Copilotなど 7
    
    ### [GitHubへのサインイン ◼ VSCodeから、GitHubにサインインすることができる • 左下のサインインアイコンから実施 ◼ GitHubにサインインすることで、GitHubとの連携が可能 • リポジトリに対するpush, pullなど](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_6.jpg)
    
6. public/privateどちらも作成可能 ◼ 「GitHubに公開」をクリックすれば、直接GitHub にリモートリポジトリを作成することもできます
    
    ### [リポジトリ作成 9 ◼ 「リポジトリを初期化する」でローカルリポジトリの 初期化を行えます(git init) ◼ 初期化後「ブランチの発行」をクリックすることで、 リモートリポジトリを作成できます •](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_8.jpg)
    
7. ◼ 上のテキストボックスにコミットメッセージを入力 して「コミット」をクリックすると、コミットされる ◼ コミット後に「変更の同期」をクリックすれば、コミッ トのプッシュ＆プルを行うことができる • プッシュのみしたい場合、一覧メニューの「プッシュ」を選択
    
    ### [ステージング・コミット・プッシュ 10 ◼ ファイルの変更を行うと、自動で「変更」欄に表示 される ◼ 右の「+」をクリックすると、該当ファイルがステー ジ状態になる • 「-」をクリックするとステージ状態を解除できます](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_9.jpg)
    
8. マージ対象指定の他にも、差分をファイル比較の 形で確認したり、 Live Shareセッションを起動(※) することも可能 ※ Live Share(拡張機能)のインストールが必要
    
    ### [コンフリクト解消 11 ◼ コンフリクトが発生した場合、ツリーの「変更のマー ジ」項目に該当ファイルが表示される ◼ 該当ファイルをクリックすると、コンフリクト発生個 所が色付きで表示されるので、差分を確認して、 マージ対象を指定する(詳細は次ページ) ◼](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_10.jpg)
    
9. 現在の変更を有効にする Accept Incoming Change 取り込もうとしている変更を有効にする Accept Both Changes 現在の変更と取り込もうとしている変更を 両方有効にする Compare Changes 差分を比較する（ファイル比較の形で閲覧 できる） 標準表示よりだいぶ差分を確認しやす い。(前スライド参照) Start Live Share Session Live Shareセッションを開く Live Shareのインストールが必要
    
    ### [コンフリクト解消 12 Conflict発生時の表示 マージ対象の指定の説明 項目名 説明 備考 Accept Current Change](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_11.jpg)
    
10. And Issues」という拡張 機能をインストールすると、VS Code上でプルリク エスト(やIssue)を作成できる ◼ プルリクコメントも表示できる(GitHubなどで追加 されたコメントも閲覧可能) ◼ GitHubでファイル管理を行っている場合のみ有 効（？） • Azure DevOpsで管理している場合、作成ができなかった… ◼ https://marketplace.visualstudio.com/items?i temName=GitHub.vscode-pull-request- github プルリク作成 Issue作成 このアイコンをク リックする
    
    ### [プルリクエスト作成 14 ◼ デフォルトでは、プルリクエストはVS Codeから作 成することはできない。 ◼ 「GitHub Pull Request](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_13.jpg)
    
11. ファイルの変更履歴の確認、及びファイル・ブランチ・コミットなど 様々な単位で内容の比較を行うことができる ◼ https://marketplace.visualstudio.com/items?itemName=ea modio.gitlens
    
    ### [GitLens 17 ◼ ソース管理を便利にしてくれる、非常に便利な拡張機能 ◼ カーソル行やファイル、コードブロック(クラスや構造体など)の コードについて、誰がいつ書いたのかを表示できる • ポップアップから、各種情報にアクセスすることもできる ◼](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_16.jpg)
    
12. 一部の操作は、ツリーから直接行うことができる • チェックアウト、マージ、タグ作成、Cherry Pickなど ◼ 検索する際「Git」と「Graph」の間に半角スペースを入れないと ヒットしないので注意 ◼ https://marketplace.visualstudio.com/items?itemName=m hutchie.git-graph
    
    ### [Git Graph 18 ◼ リポジトリの変更履歴を、ブランチツリーで表示する拡張機能 • ブランチの変更内容やチェックアウト・マージ状態を容易に確認できる • ブランチツリーは赤丸のアイコンをクリックすると表示される ◼](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_17.jpg)
    
13. ◼ 拡張機能でさらに便利になる • 標準で出来ない機能も、拡張機能で出来る場合がある(プルリクエストなど) • それ以外にも、拡張機能を使うことでファイル管理がやりやすくなる ◼ CLIコマンドを知るのは大事 • 細かいオプション項目は、VS Codeでは対応しきれない場合がある • CLIコマンドを直接実行するケースも結構あったりする… • CLIコマンドに慣れるのも大事(最近はCLIベースで実施するツールも多いですし) 20
    
    ### [まとめ ◼ VS Codeは、Git機能がめちゃくちゃ充実している • Gitのほとんどの操作は、VS Code上で実行可能 • Gitとの連携という意味では、VS Codeはかなりおススメ](https://files.speakerdeck.com/presentations/fb83e0bb866a48d2998f48e6c9e6d4a3/slide_19.jpg)