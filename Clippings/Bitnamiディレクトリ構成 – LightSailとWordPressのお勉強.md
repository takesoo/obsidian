---
Tags:
  - wordpress
URL: http://176.34.40.79/2020/09/30/bitnami%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%88%E3%83%AA%E6%A7%8B%E6%88%90/
---
- Bitnami特有のディレクトリ構成

**一般的なApacheのディレクトリ構成**

/etc/httpd/conf/httpd.conf ←設定ファイル

/etc/httpd/conf.d ←モジュールの設定ファイルなど

/etc/httpd/ ←サーバールート(各種設定の起点)

/usr/sbin/httpd ←httpd本体プログラム

/var/www/html ←ドキュメントルート

Bitnamiディレクトリ構成

- 設定ファイル系

/opt/bitnami/apache2/conf/httpd.conf

/opt/bitnami/apache2/conf/ssi.conf

/opt/bitnami/apache2/conf/bitnami/bitnami.conf

/opt/bitnami/apache2/conf/bitnami/httpd.conf

- MySQLなどの初期パスワードが記載されたファイル

/home/bitnami/bitnami_application_password

- ドキュメントルート

/opt/bitnami/apps/

apps下に配置する。confファイルを修正すればvar/wwwなど好きな場所にもできる(会社移行の時はvar/wwwに設定しなおした)

- ログ /opt/bitnami/apache2/logs/access_log/opt/bitnami/apache2/logs/error_log