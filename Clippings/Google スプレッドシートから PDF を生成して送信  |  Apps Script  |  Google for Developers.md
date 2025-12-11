---
category: "[[Clippings]]"
author:
title: Google スプレッドシートから PDF を生成して送信  |  Apps Script  |  Google for Developers
source: https://developers.google.com/apps-script/samples/automations/generate-pdfs?hl=ja
clipped: 2023-11-10
published:
tags:
  - clippings
---

bookmark\_border コレクションでコンテンツを整理 必要に応じて、コンテンツの保存と分類を行います。

-   このページの内容
-   [目標](#objectives)
-   [このソリューションについて](#about_this_solution)
    -   [仕組み](#how_it_works)
    -   [Apps Script サービス](#services)
-   [Prerequisites](#prerequisites)
-   [スクリプトを設定する](#set_up_the_script)
-   [スクリプトを実行する](#run_the_script)
-   [コードを確認する](#review_the_code)
-   [作成・変更者](#contributors)
-   [次のステップ](#next_steps)

**コーディング レベル**: 初級  
**所要時間**: 15 分  
**プロジェクトの種類**: [カスタム メニュー](https://developers.google.com/apps-script/guides/menus?hl=ja)による自動化

## 目標

-   ソリューションの機能を理解します。
-   Apps Script サービスがソリューション内でどのように機能するかを理解します。
-   スクリプトを設定します。
-   スクリプトを実行します。

## このソリューションについて

Google スプレッドシートのスプレッドシートのシートの情報を使用して、PDF を自動的に作成します。PDF が生成されたら、スプレッドシートから直接メールで送信できます。このソリューションはカスタム請求書の作成に重点を置いていますが、ニーズに合わせてテンプレートとスクリプトを更新できます。

![請求書テンプレートのスクリーンショット](https://developers.google.com/static/apps-script/samples/images/generate-send-pdfs.png?hl=ja)

![](https://developers.google.com/static/apps-script/samples/images/generate-send-pdfs.png?hl=ja)

### 仕組み

このスクリプトは、**Invoice template** シートをテンプレートとして使用し、PDF を生成します。テンプレートの特定のセルに入力する情報は、他のシートから取得されます。PDF をメールで送信するには、スクリプトで \[**Invoices**\] シートを繰り返し処理し、PDF のリンクと関連付けられたメールアドレスを取得します。このスクリプトは、一般的なメールの件名と本文を作成し、送信前に PDF を添付します。

### Apps Script サービス

このソリューションでは、次のサービスを使用します。

-   [スプレッドシート サービス](https://developers.google.com/apps-script/reference/spreadsheet?hl=ja) - 請求書の PDF やメールの作成に必要なすべての情報を提供します。ユーザーがカスタム メニューの \[**テンプレートをリセット**\] をクリックすると、テンプレートからデータが消去されます。
-   [ユーティリティ サービス](https://developers.google.com/apps-script/reference/utilities?hl=ja) - 各請求書に正しい情報が追加されるよう、お客様ごとに反復処理を行う間、`sleep()` メソッドでスクリプトを一時停止します。
-   [URL 取得サービス](https://developers.google.com/apps-script/reference/url-fetch?hl=ja) - 請求書テンプレート シートを PDF にエクスポートします。
-   [スクリプト サービス](https://developers.google.com/apps-script/reference/script?hl=ja) - URL 取得サービスによるスプレッドシートへのアクセスを承認します。
-   [ドライブ サービス](https://developers.google.com/apps-script/reference/drive?hl=ja) - エクスポートされた PDF 用のフォルダを作成します。PDF ファイルをメールに添付します。
-   [Gmail サービス](https://developers.google.com/apps-script/reference/gmail?hl=ja) - メールを作成して送信します。

## Prerequisites

このサンプルを使用するには、次の前提条件を満たす必要があります。

-   Google アカウント（Google Workspace アカウントの場合、管理者の承認が必要になる場合があります）。
-   インターネットにアクセスできるウェブブラウザ。

## スクリプトを設定する

1.  次のボタンをクリックして、「**Generate and send PDFs from Google Sheets**」スプレッドシートをコピーします。このソリューションの Apps Script プロジェクトはスプレッドシートに添付されています。  
    [コピーを作成](https://docs.google.com/spreadsheets/d/1kT3jCJcjQZVr0JQtEev5exCKhc6VqFhD3UKaBBV6Lvo/copy?resourcekey=0-3CcEz63Pa_yYyf216zbcaw&hl=ja#gid=1382003151)
    
2.  \[**拡張機能**\] \> \[**Apps Script**\] の順にクリックします。
    
3.  `Code.gs` ファイルで、次の変数を更新します。
    
    1.  `EMAIL_OVERRIDE` を `true` に設定します。
    2.  `EMAIL_ADDRESS_OVERRIDE` をメールアドレスに設定します。
4.  ![保存アイコン](https://fonts.gstatic.com/s/i/short-term/release/googlesymbols/save/default/24px.svg)\[保存\] をクリックします。
    

## スクリプトを実行する

1.  スプレッドシートに戻り、\[**PDF を生成して送信**\] \> \[**請求書を処理**\] をクリックします。
2.  メッセージが表示されたら、スクリプトを承認します。OAuth 同意画面に「**このアプリは確認されていません**」という警告が表示された場合は、\[**詳細設定**\] \> \[**{プロジェクト名} に移動（安全でない）**\] を選択します。
    
3.  もう一度 \[**PDF を生成して送信**\] \> \[**請求書を処理**\] をクリックします。
    
4.  PDF を表示するには、\[**Invoices**\] シートに切り替えて、\[**Invoice link**\] 列のリンクをクリックします。
    
5.  \[**PDF を生成して送信**\] \> \[**メールを送信**\] をクリックします。
    
6.  メールをチェックして、メールおよび添付の PDF を確認します。前のセクションで `EMAIL_OVERRIDE` を `true` に設定したため、スクリプトは `EMAIL_ADDRESS_OVERRIDE` に指定したメールアドレスにすべてのメールを送信します。`EMAIL_OVERRIDE` を false に設定すると、スクリプトは \[**Customers**\] シートにあるメールアドレスにメールを送信します。
    
7.  （省略可）\[**請求書テンプレート**\] シートからデータを削除するには、\[**PDF を生成して送信する**\] \> \[**テンプレートをリセット**\] をクリックします。
    

## コードを確認する

このソリューションの Apps Script コードを確認するには、下の \[**ソースコードを表示**\] をクリックします。

## 作成・変更者

このサンプルは、Google Developer Experts の支援により Google が保守しています。

## 次のステップ

-   [Google Workspace のカスタム メニュー](https://developers.google.com/apps-script/guides/menus?hl=ja)
-   [Google スプレッドシートを拡張する](https://developers.google.com/apps-script/guides/sheets?hl=ja)

特に記載のない限り、このページのコンテンツは[クリエイティブ・コモンズの表示 4.0 ライセンス](https://creativecommons.org/licenses/by/4.0/)により使用許諾されます。コードサンプルは [Apache 2.0 ライセンス](https://www.apache.org/licenses/LICENSE-2.0)により使用許諾されます。詳しくは、[Google Developers サイトのポリシー](https://developers.google.com/site-policies?hl=ja)をご覧ください。Java は Oracle および関連会社の登録商標です。

最終更新日 2023-10-10 UTC。

新しいページが読み込まれました。