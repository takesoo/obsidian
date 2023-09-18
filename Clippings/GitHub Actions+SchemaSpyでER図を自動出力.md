---
Created: Invalid date
URL: https://zenn.dev/tmf16/articles/75e4de8eeeed63
---
[![](https://res.cloudinary.com/zenn/image/upload/s--XMeqFOO6--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:tmf16%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:GitHub%2520Actions%252BSchemaSpy%25E3%2581%25A7ER%25E5%259B%25B3%25E3%2582%2592%25E8%2587%25AA%25E5%258B%2595%25E5%2587%25BA%25E5%258A%259B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2dNNVUxdzZfY2g4OFpmYTVhbFBqUlo5eWNydmg4SXZ5QlpoLVNVT3c9czgwLWM=%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png)](https://res.cloudinary.com/zenn/image/upload/s--XMeqFOO6--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:tmf16%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:GitHub%2520Actions%252BSchemaSpy%25E3%2581%25A7ER%25E5%259B%25B3%25E3%2582%2592%25E8%2587%25AA%25E5%258B%2595%25E5%2587%25BA%25E5%258A%259B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2dNNVUxdzZfY2g4OFpmYTVhbFBqUlo5eWNydmg4SXZ5QlpoLVNVT3c9czgwLWM=%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png)

---

ER図を自動で出力する機会あり、GitHub Actions+SchemaSpyを試してみた。

Railsのアプリケーションで、データベースはmysqlを利用している。  
ただし外部キー制約がなく、リレーションの表示は難しいかなと思っていたのだが、世の中便利なことになんとかなった。

SchemaSpyには、Railsベースの命名規則を使って外部キーを推測してくれる`-rails` オプションがある。

リレーションの表示がいまいちだったら外部キー制約の追加クエリを出力してくれるやつhakagiも試してみようかと思っていたが `-rails` オプションで十分だった。

## ER図を生成するところまで

出力結果はs3にアップするなり、社内のwebサーバにアップルするなり好きにして下さい。

```
name: eron:  push:    branches:      - main    paths:      - "db/migrate/**"jobs:  run:    runs-on: ubuntu-latest    env:      RAILS_ENV: development      DB_NAME: hoge    services:      db:        image: mysql:5.7        ports:          - 3306:3306        env:          MYSQL_ALLOW_EMPTY_PASSWORD: 'yes'    steps:      - uses: actions/checkout@v2      - uses: ruby/setup-ruby@v1        with:          ruby-version: 3.0.0          bundler-cache: true          cache-version: 1      - name: mysql set character_set        run: |          mysql -u root -h 127.0.0.1 -e "set global character_set_server='utf8mb4';"          mysql -u root -h 127.0.0.1 -e "set global collation_server='utf8mb4_unicode_ci';"      - name: db migrate        run: |          cp config/database.ci.yml config/database.yml          bundle exec rails db:create          bundle exec rails db:migrate      - name: make schemaspy.properties        run: echo -e "          schemaspy.t=mysql\n          schemaspy.dp=\n          schemaspy.host=db\n          schemaspy.port=3306\n          schemaspy.db=$DB_NAME\n          schemaspy.u=root\n          schemaspy.p=\n          schemaspy.o=\n          schemaspy.s=$DB_NAME\n          " > /tmp/schemaspy.properties      - name: Prepare a dir for schemaspy output        run: mkdir -m 777 /tmp/output      - name: run schemaspy        run: docker run --net=${{ job.container.network }} -v "/tmp/output:/output" -v "/tmp/schemaspy.properties:/schemaspy.properties" schemaspy/schemaspy:latest -rails      - name: upload er        run: echo "/tmp/outputにER図が出力されているのでs3とかにアップロードしてね"
```

```
development:  adapter: mysql2  encoding: utf8mb4  charset: utf8mb4  collation: utf8mb4_general_ci  database: hoge  username: root  password:  host: 127.0.0.1
```

## おそらく

ruby/setup-rubyの部分をphpに、  
db:migrateしてる部分を `php artisan migrate` に置き換えればlaravelでも同じようなことができるはず