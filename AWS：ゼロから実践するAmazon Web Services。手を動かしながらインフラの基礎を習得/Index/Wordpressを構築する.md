---
Created: Invalid date
---
# wordpressを構築する

## DB構築

```
# database作成MySQL [(none)]> create database aws_and_infra default character set utf8 collate utf8_general_ci;Query OK, 1 row affected, 2 warnings (0.05 sec)# user作成MySQL [(none)]> create user 'aws_and_infra'@'%' identified by 'password';Query OK, 0 rows affected (0.02 sec)# userに権限を付与MySQL [(none)]> grant all on aws_and_infra.* to 'aws_and_infra'@'%';Query OK, 0 rows affected (0.01 sec)# 設定を反映MySQL [(none)]> flush privileges;Query OK, 0 rows affected (0.01 sec)# 確認MySQL [(none)]> select user , host from mysql.user;+------------------+-----------+| user             | host      |+------------------+-----------+| aws_and_infra    | %         || root             | %         || mysql.infoschema | localhost || mysql.session    | localhost || mysql.sys        | localhost || rdsadmin         | localhost |+------------------+-----------+6 rows in set (0.01 sec)
```

## wordpressインストール

```
# phpをインストール$ sudo amazon-linux-extras install -y php7.2# wordpressの必要パッケージをインストール$ sudo yum install -y php php-mbstring# wordpressをダウンロード$ wget https://ja.wordpress.org/latest-ja.tar.gz# 解凍$ tar xzvf latest-ja.tar.gz$ sudo cp -r * /var/www/html/$ sudo chown apache:apache /var/www/html/ -R$ sudo systemctl restart httpd.service
```

この時点でドメインを打つとwordpressの初期画面が表示される

## wordpress初期設定

指示されるままデータベース、ユーザーを設定する

完了すると管理画面が作成される

# TCP/IPについて