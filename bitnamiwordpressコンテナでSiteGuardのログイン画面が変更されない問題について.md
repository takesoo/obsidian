---
Tags:
  - Docker
  - Wordpress
---
# 現象

SiteGuardはwordpressのログイン画面のurlを変更してセキュリティを上げてくれるプラグイン。bitnami/wordpressコンテナ環境でインストールしたがログイン画面urlが変更されなかった。

# 原因

SiteGuardの設定をすると内部的には.htaccessにリダイレクト処理を追加するみたい

.htaccessには`AllowOverride`で追記の許可を設定する。デフォルトはNone、不許可になっているためこれをAllに変更する必要がある。

# 対処法

WORDPRESS_HTACCESS_OVERRIDE_NONE: noを設定する

下記ファイルの`AllowOverride`がAllになってるはずです

- /opt/bitnami/apache2/conf/vhosts/wordpress-https-vhost.conf
- /opt/bitnami/apache2/conf/vhosts/wordpress-vhost.conf

  

# 引用

[

SiteGuard WP Plugin

You can find docs, FAQ and more detailed information on English Page Japanese Page. Simply install the SiteGuard WP Plugin, WordPress security is improved. This plugin is a security plugin that specializes in the login attack of brute force, such as protection and management capabilities. 注 It does not support the multisite function of WordPress.

![](https://s.w.org/images/wmark.png)https://ja.wordpress.org/plugins/siteguard/

![](https://ps.w.org/siteguard/assets/banner-772x250.png?rev=1011863)](https://ja.wordpress.org/plugins/siteguard/)

[

【AWS】WordPressでSite Guardの設定が効かない対処法 - Qiita

AWSのEC2で構築したWordPressでログインページをデフォルトから変更するためにSite Guard WP Pluginを導入しました。 画面上では有効になるものの、実際にアクセスしてみると以下のような挙動になりました。 wp-adminにアクセスするとwp-login.phpにリダイレクトされる ログインページ変更後のURLでアクセスしようとすると404エラーが返ってくる ...

![](https://cdn.qiita.com/assets/favicons/public/production-c620d3e403342b1022967ba5e3db1aaa.ico)https://qiita.com/pike3/items/4ac1d2ae13c0d8103964

![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgwJTkwQVdTJUUzJTgwJTkxV29yZFByZXNzJUUzJTgxJUE3U2l0ZSUyMEd1YXJkJUUzJTgxJUFFJUU4JUE4JUFEJUU1JUFFJTlBJUUzJTgxJThDJUU1JThBJUI5JUUzJTgxJThCJUUzJTgxJUFBJUUzJTgxJTg0JUU1JUFGJUJFJUU1JTg3JUE2JUU2JUIzJTk1JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz00YzhiZDYyOTFiYTI2MWExZTRkYjgxN2E2ODlkMzNiZg&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwcGlrZTMmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWMyMWQwMTc0ZTU3NzUxNzVjNzQ3ZTlmMjZmZWU3ODdk&blend-x=142&blend-y=491&blend-mode=normal&s=75db5249aa59a384b94018a8b8c0c5ad)](https://qiita.com/pike3/items/4ac1d2ae13c0d8103964)

[

Docker

![](https://hub.docker.com/favicon.ico)https://hub.docker.com/r/bitnami/wordpress/



](https://hub.docker.com/r/bitnami/wordpress/)