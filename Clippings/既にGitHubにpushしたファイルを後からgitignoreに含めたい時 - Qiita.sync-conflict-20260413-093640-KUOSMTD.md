---
category: "[[Clippings]]"
author: "[[by smallpalace]]"
title: "既にGitHubにpushしたファイルを後からgitignoreに含めたい時 - Qiita"
source: https://qiita.com/YotaHamasaki/items/9f674f2a56381eb9888e
clipped: 2023-09-18
published: 2021-05-21
topics: 
tags: [clippings Git GitHub]
---

git pushしたファイルの中に一般公開してはセキュリティ上良くないファイルがあったりすると思います。  
僕の場合、railsで開発しているアプリにおいて、seeds.rbファイルに管理者ユーザーの情報を記載しており、一般公開されると良くないことに後から気づきました。  
ファイルをgitignoreに含める方法は段階によって色々ありますが、ここではgit pushまで実行してしまっている場合を想定しています。

## [](#%E5%AE%9F%E8%A1%8C%E6%89%8B%E9%A0%86)実行手順

①「git rm --cached ファイル名」コマンドにて該当ファイルの削除。(ディレクトリ毎削除する場合は-rをつける。)  
②.gitignoreファイルにプッシュしたくないファイル名を追記。  
③再度git addからpushまでを実行する。

※①のコマンド入力時に「fatal: pathspec 'ファイル名' did not match any files」と表示される場合は、「git rm --cached --ignore-unmatch .」コマンドを入力してみてください。  
このエラーはgitの管理対象外のファイルが含まれていることが原因みたいです。

## [](#%E3%81%8A%E3%82%8F%E3%82%8A%E3%81%AB)おわりに

「該当ファイルをGitHubから削除→ローカルでgitignoreに追記→再度push」という簡単な流れで実行できてよかったです。  
この手順だとコミット履歴が残るという点だけ注意が必要です。