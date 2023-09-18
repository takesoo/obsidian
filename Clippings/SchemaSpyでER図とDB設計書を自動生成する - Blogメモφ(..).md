---
Created: Invalid date
URL: https://blog.nightonly.com/2021/07/23/schemaspy%E3%81%A7er%E5%9B%B3%E3%81%A8db%E8%A8%AD%E8%A8%88%E6%9B%B8%E3%82%92%E8%87%AA%E5%8B%95%E7%94%9F%E6%88%90%E3%81%99%E3%82%8B/
---
[![](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88-2021-07-23-11.29.41-1024x148.png)](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88-2021-07-23-11.29.41-1024x148.png)

---

Rails ERDも便利なのですが、テーブル単位のリレーションでカラム単位で見たいし、テーブル数が多くなると見難いので、SchemaSpyを試してみました。

やってみた解った事は、ERDではモデルを見てリレーションを判断しているが、SchemaSpyはDBのみなので、DBにforeign_keyがないとER図が出ない。  
-railsオプションがありますが、あくまでもActive Recordのルールに従っている場合しか対応できません。

## SchemaSpyの挙動

SchemaSpyはJavaで動いて、DBにJDBCドライバで接続できればOKなので、必要な時に手動で叩いても良いし、migrate時やCircleCI等で自動生成するでも良さそう。

### 前提

### Javaがインストールされている

```
% java -versionjava version "1.8.0_281"Java(TM) SE Runtime Environment (build 1.8.0_281-b09)Java HotSpot(TM) 64-Bit Server VM (build 25.281-b09, mixed mode)
```

### Graphvizがインストールされている（任意）

SchemaSpyに組み込まれているモジュールを使うオプションもあるので任意。

```
% dot -Vdot - graphviz version 2.47.0 (20210316.0004)
```

### DBに接続できる環境から実行

今回は、前回作成したローカルのDockerに接続しました。[既存のRailsアプリにDockerを導入する](https://blog.nightonly.com/2021/07/18/%e6%97%a2%e5%ad%98%e3%81%aerails%e3%82%a2%e3%83%97%e3%83%aa%e3%81%abdocker%e3%82%92%e5%b0%8e%e5%85%a5%e3%81%99%e3%82%8b/)

localhostだとポート番号を指定してもsocket接続になってしまう。

```
% mysql -h localhost -P 3306 -u root -p rails_app_developmentEnter password:ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

IP指定にすればOK

```
% mysql -h 127.0.0.1 -u root -p rails_app_developmentEnter password:Reading table information for completion of table and column namesYou can turn off this feature to get a quicker startup with -AWelcome to the MariaDB monitor.  Commands end with ; or \g.Your MySQL connection id is 19Server version: 8.0.25 MySQL Community Server - GPLCopyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.MySQL [rails_app_development]> \qBye
```

ちなみにPostgreSQLの場合

```
% psql -h localhost -u postgres -d rails_app_development
```

## SchemaSpyの環境作成

適当なディレクトリを作って、その中で

```
% wget https://github.com/schemaspy/schemaspy/releases/download/v6.1.0/schemaspy-6.1.0.jar% mkdir jdbc% cd jdbcMySQLに接続する場合% wget https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.20/mysql-connector-java-8.0.20.jarPostgreSQLに接続する場合% wget https://jdbc.postgresql.org/download/postgresql-42.2.23.jarcd ..vi schemaspy.properties
```

MySQLに接続する場合の例

```
schemaspy.t=mysqlschemaspy.dp=jdbcschemaspy.host=127.0.0.1schemaspy.port=3306schemaspy.s=rails_app_developmentschemaspy.db=rails_app_developmentschemaspy.u=rootschemaspy.p=xyz789schemaspy.o=schemaspy
```

PostgreSQLに接続する場合の例

```
schemaspy.t=pgsqlschemaspy.dp=jdbcschemaspy.host=127.0.0.1schemaspy.port=5432schemaspy.s=publicschemaspy.db=rails_app_developmentschemaspy.u=postgresschemaspy.p=xyz789schemaspy.o=schemaspy
```

## SchemaSpyを実行

```
Graphvizがインストールされている場合% java -jar schemaspy-6.1.0.jarGraphvizがインストールされていない場合% java -jar schemaspy-6.1.0.jar -vizjs
```

```
  ____       _                          ____ / ___|  ___| |__   ___ _ __ ___   __ _/ ___| _ __  _   _ \___ \ / __| '_ \ / _ \ '_ ` _ \ / _` \___ \| '_ \| | | |  ___) | (__| | | |  __/ | | | | | (_| |___) | |_) | |_| | |____/ \___|_| |_|\___|_| |_| |_|\__,_|____/| .__/ \__, |                                             |_|    |___/                                              6.1.0SchemaSpy generates an HTML representation of a database schema's relationships.SchemaSpy comes with ABSOLUTELY NO WARRANTY.SchemaSpy is free software and can be redistributed under the conditions of LGPL version 3 or later.http://www.gnu.org/licenses/INFO  - Starting Main v6.1.0 on xxxx with PID xxxx (/Users/xxxx/workspace/schemaspy/schemaspy-6.1.0.jar started by xxxx in /Users/xxxx/workspace/schemaspy)INFO  - The following profiles are active: defaultINFO  - Found configuration file: schemaspy.propertiesINFO  - Started Main in 2.28 seconds (JVM running for 3.117)INFO  - Loaded configuration from schemaspy.propertiesINFO  - Starting schema analysisINFO  - Connected to MySQL - 8.0.25INFO  - Gathering schema detailsGathering schema details.........(0sec)Connecting relationships.........(0sec)Writing/graphing summary.INFO  - Gathered schema details in 0 secondsINFO  - Writing/graphing summaryINFO  - Graphviz rendered set to ''........(6sec)Writing/diagramming detailsINFO  - Completed summary in 6 secondsINFO  - Writing/diagramming details......(1sec)Wrote relationship details of 6 tables/views to directory 'schemaspy' in 9 seconds.View the results by opening schemaspy/index.htmlINFO  - Wrote table details in 1 secondsINFO  - Wrote relationship details of 6 tables/views to directory 'schemaspy' in 9 seconds.INFO  - View the results by opening schemaspy/index.html% open schemaspy/index.html
```

ER図が出ない  
schemaspy/relationships.html

```
All Relationships× Missed Relationships!No relationships were detected in the schema.
```

[![](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.29.41-2048x296.png)](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.29.41-2048x296.png)

孤立したテーブルに出る  
schemaspy/orphans.html

[![](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.33.47-2048x517.png)](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.33.47-2048x517.png)

### ベターな方法：railsオプション指定

あくまでもActive Recordのルールに従っている場合しか対応できません。  
例えば、「t.references :user」= user_id → usersテーブルのid ならOK

```
Graphvizがインストールされている場合% java -jar schemaspy-6.1.0.jar -railsGraphvizがインストールされていない場合% java -jar schemaspy-6.1.0.jar -vizjs -rails
```

### ベストな方法：foreign_keyを追加

db/migrate/ にforeign_keyを追加

```
-      t.references :user, type: :bigint+      t.references :user, type: :bigint, foreign_key: true
```

db:migrateすると、db/schema.rbに下記が追加される。

```
  add_foreign_key "infomations", "users"
```

Active Recordのルールに従っていない場合は、add_foreign_keyでカラム名や主キーを明示すれば良さそう。[add_foreign_key | Railsドキュメント](https://railsdoc.com/page/add_foreign_key)

[![](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.48.38-2048x975.png)](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.48.38-2048x975.png)

## その他

MySQLでもコメント出るので入れた方が便利。（昔は入らなかったような。。。）

```
      t.string :title, null: false, comment: 'タイトル'      t.string :summary, comment: '概要'      t.text   :body, comment: '本文'
```

[![](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.51.52-2048x1137.png)](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.51.52-2048x1137.png)

その下にインデックス  
更にその下に、このテーブルをメインにしたER図が出そうなので、これは使えそう！

[![](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.58.00-2048x1107.png)](https://blog.nightonly.com/wp-content/uploads/sites/2/2021/07/スクリーンショット-2021-07-23-11.58.00-2048x1107.png)

## 追伸：Meta追加

テーブルのコメントはmetaで設定できます。  
また、foreign_keyを追加できないけど、リレーションを出したい場合にも使える。[Configuration — SchemaSpy 6.0.0 documentation](https://schemaspy.readthedocs.io/en/v6.0.0/configuration.html#schemameta)

schemaspy.properties に追加

```
schemaspy.meta=schemaspymeta.xml
```

schemaspymeta.xml を作成

```
<schemaMeta xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://schemaspy.org/xsd/6/schemameta.xsd" >    <tables>        <table name="users" comments="ユーザー" />        <table name="admin_users" comments="管理者" />        <table name="infomations" comments="お知らせ" />    </tables></schemaMeta>
```

今回のコミット内容  
※[SchemaSpyをSQLite3に使ってみる](https://blog.nightonly.com/2021/08/01/schemaspy%e3%82%92sqlite3%e3%81%ab%e4%bd%bf%e3%81%a3%e3%81%a6%e3%81%bf%e3%82%8b/) で一緒に追加しています。[https://dev.azure.com/nightonly/rails-app-origin/_git/rails-app-origin/commit/44c8267330ca6f3e562115a6063bb1925abb51da](https://dev.azure.com/nightonly/rails-app-origin/_git/rails-app-origin/commit/44c8267330ca6f3e562115a6063bb1925abb51da)