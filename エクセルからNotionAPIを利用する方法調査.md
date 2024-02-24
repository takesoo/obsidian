## Python in Excel 🙅
- [Python in Excel の概要 - Microsoft サポート](https://support.microsoft.com/ja-jp/office/python-in-excel-%E3%81%AE%E6%A6%82%E8%A6%81-55643c2e-ff56-4168-b1ce-9428c8308545)
- [ExcelでPythonを実行できる「Python in Excel」を使ってみた。導入方法とできることを詳しく解説。 – trends](https://trends.codecamp.jp/blogs/media/python-in-excel)
- できること
	- エクセル上のデータでグラフ作成、データスクリーニング、機械学習
	- ==外部APIを叩けるかは不明==
	- よしんばAPI叩けても１セル１リクエストなのでアクセス制限でアウト
- 使用できる環境
	- Excel for Windowsのみ
	- Microsoft 365 insiderのベータチャンネルへの参加が必要

## WEBSERVICE 関数 + ExcelAPI 🙅
- [個人開発で「Excel専用のWebAPI」を作りました](https://zenn.dev/ryuden/articles/1161c6bee032c4)
- [Excelにインターネットからデータを取り込むサイト | ExcelAPI](https://excelapi.org/)
- 1万件/1日までは無料
- 同じエンドポイントへのアクセスはキャッシュが使用されるのでアクセス上限も大丈夫そう
- google splead sheetでも動作する
- ==認証が必要なAPIへの利用はできない==

## Power Query
- [How to GET data from an API using POWER QUERY in Power BI | Food Bank & Job Search - YouTube](https://www.youtube.com/watch?v=WK1poTLbG5o)
- エクセルの機能のため実装コストとメンテナンスコストは小さい
- 事務がPowerQueryの操作を覚える必要がある？

## ExcelマクロVBA 
- [Excelマクロ（VBA）からREST APIを呼び出して値を取得 - タムコム](https://tamcom.jp/blog/call-rest-api-from-vba)
- なんでもできるが実装コストとメンテナンスコストも大きいか

## PythonとExcelの組み合わせ
- xlwingsなどを利用する
- lambda functionでエクセル作成→S3にアップロードしてダウンロードurlをクライアントに返すとか？
- これも実装コストとメンテナンスコストが大きい（VBAよりは小さい？）
- マクロと比べると実行環境の影響を受けにくいのは利点


