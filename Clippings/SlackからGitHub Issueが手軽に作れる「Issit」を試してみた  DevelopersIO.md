---
Created: Invalid date
URL: https://dev.classmethod.jp/articles/make-github-issue-with-slack-by-issit/
---
[![](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/07/github-eyecatch.png)](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/07/github-eyecatch.png)

---

GitHub IssueをSlackのメッセージから手軽に作成できるIssitを触ってみました。

社内slackの書き込みを見ていたところ、SlackのメッセージからGitHub Issueを作成する「issit」というサービスが公開されたことを知り、試してみました。

## 設定

必要なものは以下の通り。

- Slack Workspace
- GitHubアカウント

Slack Workspaceへのインストールに管理者権限での制約がある場合は申請が必要になります。

### 手続き

今回はプライベートのSlack Workspaceを利用しました。

1. [issit](https://issit.plaid.co.jp/)にアクセスし、「Slackに追加する」をクリックします。
    
2. Slack Workspaceへのアクセス権限を許可します。
    
3. Appsからissitを選択します。/li>
    
4. 表示されるアナウンスに従い、まずはGitHub appをIssue作成に利用するGitHubアカウントへインストールします。
5. 完了するとissitのアナウンスが以下の内容に切り替わります。
    

## Issueを作る

まずはSlack上で適当にメッセージを投稿してみます。

次に、該当メッセージ上で「GitHubのIssueにする」を選択します。

以下のダイアログが表示されるので、対象のリポジトリを選びます。

Issueタイトルを入力します。（ここはSlackメッセージが直接入ってもよさそうなものですが。）その他、必要に応じて色々設定し作成します。

完了するとissitがレスをつけてきます。

GitHub Issues側では以下のような内容にて作成されました。

## あとがき

Slackメッセージを基にIssueを作成する、というプロセスは割と頑張って自作されている方も多いのではと思われます。Issitを使うとシンプルなワークフローにて構成できるため、手軽にやってみたいけど仕組み作りが大変、という場合にはまずはIssitでやってみるとよいかもしれません。現在、全ての機能を無料にて提供されているので、色々試すには特におすすめです。