---
tags:
  - ruby/gems
---
https://bundler.io/
- ruby gemsのパッケージ管理ライブラリ
	- [[gemfile]]を作成編集して`bundle install`すると[[gemfile.lock]]が作成される
	- `bundle install`はgemfile.lockをもとにgemをインストールする
	- `bundle update`はgemfileをもとにgemをインストールし直して[[gemfile.lock]]を生成しなおす
	- gemを追加する時はgemfileに追加して`bundle install`
	- `bundle update`するとgemのバージョンのズレが起こってクラッシュする可能性がある。本番環境などでは安易にやらずに、ローカル環境で実行して確認する。
- gemの作成もできる

[[Ruby] Bundler 1.15の全コマンド｜TechRacho by BPS株式会社](https://techracho.bpsinc.jp/hachi8833/2017_06_06/40845)