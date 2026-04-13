---
Created: Invalid date
URL: https://dev.classmethod.jp/articles/glacier-instructions-001/
---
[![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/05/amazon-s3-glacier.png)](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/05/amazon-s3-glacier.png)

---

## はじめに

S3 Glacierに直接アップロードやダウンロードをする機会が普段あまり無いため試してみたいと思います。

今回実施する内容は以下の通りです。

1. ボールトの作成
2. アーカイブのアップロード
3. ボールトインベントリのダウンロード
4. アーカイブのダウンロード
5. アーカイブの削除
6. ボールトの削除

ボールト及びアーカイブの説明については、公式ドキュメントから引用します[1](https://dev.classmethod.jp/articles/glacier-instructions-001/#fn-785824-1) [2](https://dev.classmethod.jp/articles/glacier-instructions-001/#fn-785824-2)

> ボールトは、アーカイブを格納するコンテナです。
> 
> アーカイブとは、写真、動画、ドキュメントなど、ボールトに格納するオブジェクトを指します。

管理コンソールからはボールトの作成と削除しか行えないため、 CLI での操作が主となります。[3](https://dev.classmethod.jp/articles/glacier-instructions-001/#fn-785824-3)

## ボールトの作成

管理コンソールでも作成は可能ですが、今回は CLI での作成を実施したいと思います。

ボールトの作成は下記コマンドを実行します。

```
aws glacier create-vault --vault-name ボールト名 --account-id アカウント名
```

今回は、 examplevault という名前でボールトを作成してみます。

```
$ aws glacier create-vault --vault-name examplevault --account-id ************{    "location": "/************/vaults/examplevault"}
```

ボールトが作成されているか確認します。

ボールトの状態確認は下記コマンドを実行します。

```
aws glacier describe-vault --vault-name ボールト名 --account-id アカウント名
```

```
$ aws glacier describe-vault --vault-name examplevault --account-id ************{    "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",    "VaultName": "examplevault",    "CreationDate": "2021-10-14T01:05:33.264Z",    "NumberOfArchives": 0,    "SizeInBytes": 0}
```

コンソールでも確認してみましょう。

無事作成されていました。

## アーカイブのアップロード

それではGlacierにアーカイブをアップロードしていきたいと思います。

```
$ aws glacier upload-archive --account-id ************ --vault-name examplevault --body samplearchive.txt{    "location": "/************/vaults/examplevault/archives/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",    "checksum": "f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2",    "archiveId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"}
```

ここで出力される **archiveId** はダウロードの際にも利用しますので、控えるようにしてください。

アーカイブが作成されているか確認します。

```
$ aws glacier describe-vault --vault-name examplevault --account-id ************{    "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",    "VaultName": "examplevault",    "CreationDate": "2021-10-14T01:05:33.264Z",    "NumberOfArchives": 0,    "SizeInBytes": 0}
```

```
"NumberOfArchives": 0
```

作成されていませんでした。

Glacierではアーカイブのダウンロードだけではなく、アップロードにも時間がかかります。

数時間かかるので、気長に待ちましょう。

== 翌日 ==

もう一度確認してみます。

```
$ aws glacier describe-vault --vault-name examplevault --account-id ************{    "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",    "VaultName": "examplevault",    "CreationDate": "2021-10-14T01:05:33.264Z",    "LastInventoryDate": "2021-10-14T06:16:08.344Z",    "NumberOfArchives": 1,    "SizeInBytes": 32773}
```

無事作成されていました。

同様の内容はコンソール上でも確認できます。

## ボールトインベントリのダウンロード

**archiveId**を失念した場合は、ボールトインベントリから取得する必要があります。

ボールトインベントリの説明については、公式ドキュメントから引用します[4](https://dev.classmethod.jp/articles/glacier-instructions-001/#fn-785824-4)

> ボールトインベントリとは、ボールト内のアーカイブのリストを指します。インベントリではリスト内の各アーカイブに、アーカイブ ID、作成日、サイズなど、アーカイブに関する情報が記載されています。

まずはインベントリの取得ジョブを開始します

```
$ aws glacier initiate-job   --account-id ************ --vault-name examplevault --job-parameters '{"Type": "inventory-retrieval"}'{    "location": "/************/vaults/examplevault/jobs/yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy",    "jobId": "yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy"}
```

取得したjobIdを指定して、インベントリのダウンロードを実施します。

```
$ aws glacier get-job-output --account-id ************ --vault-name examplevault --job-id yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy archive.jsonAn error occurred (ResourceNotFoundException) when calling the GetJobOutput operation: The job ID was not found: yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

jobIdが無いとエラーになりました。

ジョブリストを確認してみます

```
$ aws glacier list-jobs --account-id ************ --vault-name examplevault{    "JobList": [        {            "JobId": "yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy",            "Action": "InventoryRetrieval",            "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",            "CreationDate": "2021-10-15T06:14:35.697Z",            "Completed": false,            "StatusCode": "InProgress",            "InventoryRetrievalParameters": {                "Format": "JSON"            }        }    ]}
```

取得ジョブがまだ完了していないようです。これも時間がかかるようなので少し待ってみます。

```
"Completed": false,"StatusCode": "InProgress",
```

== 後日 ==

さて、前回の作業から休日を挟んだので、3日ぶりに確認してみます。流石に終わっているでしょう。

```
$ aws glacier list-jobs --account-id ************ --vault-name examplevault{    "JobList": []}
```

あれ…？

ドキュメント[5](https://dev.classmethod.jp/articles/glacier-instructions-001/#fn-785824-5)の内容を確認してみます。

> S3 Glacier は、出力を取得する前にジョブを完了している必要があります。ジョブは、完了から少なくとも 24 時間は有効です。つまり、ジョブが完了してから 24 時間は、出力をダウンロードできます

ということです。24時間以上経過しているので、ジョブの開始からやり直します。

== 翌日 ==

```
$ aws glacier list-jobs --account-id ************ --vault-name examplevault{    "JobList": [        {            "JobId": "yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy",            "Action": "InventoryRetrieval",            "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",            "CreationDate": "2021-10-18T06:11:45.766Z",            "Completed": true,            "StatusCode": "Succeeded",            "StatusMessage": "Succeeded",            "InventorySizeInBytes": 443,            "CompletionDate": "2021-10-18T09:53:31.993Z",            "InventoryRetrievalParameters": {                "Format": "JSON"            }        }    ]}
```

成功しました。ジョブの完了までは4時間程かかっていたようです。

```
"Completed": true,"StatusCode": "Succeeded",
```

さて、ではお待ちかね。インベントリをダウンロードします。

```
$ aws glacier get-job-output --account-id ************ --vault-name examplevault --job-id yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy archive.json{    "status": 200,    "acceptRanges": "bytes",    "contentType": "application/json"}
```

成功しました。

無事、 ArchiveId も確認出来ました。

```
$ cat archive.json{    "VaultARN":"arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",    "InventoryDate":"2021-10-14T06:16:08Z",    "ArchiveList":[        {            "ArchiveId":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",            "ArchiveDescription":"",            "CreationDate":"2021-10-14T01:31:13Z",            "Size":5,            "SHA256TreeHash":"**************************************"        }    ]}
```

## アーカイブのダウンロード

それでは、アーカイブをダウンロードしていきたいと思います。

まずはアーカイブの取得ジョブを開始します

事前に取得ジョブコマンドへ渡すjsonファイルを作成します。

```
$ cat retrieval.json{    "Type": "archive-retrieval",    "ArchiveId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",    "Description": "archive-retrieval_20211021_001"}
```

先ほど作成したjsonファイルを --job-parameters として指定し、アーカイブの取得ジョブコマンドを実行します。

```
$ aws glacier initiate-job --account-id ************ --vault-name examplevault --job-parameters file://retrieval.json{    "location": "/************/vaults/examplevault/jobs/zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz",    "jobId": "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"}
```

インベントリのダウンロードの時と同様に list-jobs で "Completed": True になるのを待ちます。※数時間かかります

```
$ aws glacier list-jobs --account-id ************ --vault-name examplevault{    "JobList": [        {            "JobId": "yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy",            "JobDescription": "archive-retrieval_20211021_001",            "Action": "ArchiveRetrieval",            "ArchiveId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",            "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",            "CreationDate": "2021-10-21T06:12:07.953Z",            "Completed": true,            "StatusCode": "Succeeded",            "StatusMessage": "Succeeded",            "ArchiveSizeInBytes": 5,            "CompletionDate": "2021-10-21T09:53:15.201Z",            "SHA256TreeHash": "**************************************",            "ArchiveSHA256TreeHash": "**************************************",            "RetrievalByteRange": "0-4",            "Tier": "Standard"        }    ]}
```

完了したら、取得したjobIdを指定して、アーカイブのダウンロードを実施します。

```
$ aws glacier get-job-output --account-id ************ --vault-name examplevault --job-id zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz retrieval_samplearchive.txt{    "checksum": "**************************************",    "status": 200,    "acceptRanges": "bytes",    "contentType": "application/octet-stream"}
```

無事ダウンロード完了しました。

## アーカイブの削除

アーカイブの削除をしていきます。

その前に…Glacierではアーカイブは3カ月保管が前提となっています。そのため、3カ月未満の状態でアーカイブを削除した場合、残りの請求分（3ヶ月分）がまとめて請求されます。ご注意ください。

```
$ aws glacier delete-archive --account-id ************ --vault-name examplevault --archive-id xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

アーカイブが削除されたか確認します。

```
$ aws glacier describe-vault --vault-name examplevault --account-id ************{    "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",    "VaultName": "examplevault",    "CreationDate": "2021-10-14T01:05:33.264Z",    "LastInventoryDate": "2021-10-14T06:16:08.344Z",    "NumberOfArchives": 1,    "SizeInBytes": 32773}
```

インベントリの更新は約１日１回となりますので、インベントリに反映されるまで時間がかかるようです。しばらく待ちます。[6](https://dev.classmethod.jp/articles/glacier-instructions-001/#fn-785824-6)

> 最初のアーカイブをボールトにアップロードすると、Amazon S3 Glacier（S3 Glacier）により、ボールトインベントリが自動的に作成され、インベントリが約 1 日 1 回のペースで更新されます。S3 Glacier が最初のインベントリを作成した後、そのインベントリを取得できるようになるまで、通常半日から最大 1 日かかります。

```
$ aws glacier describe-vault --vault-name examplevault --account-id ************{    "VaultARN": "arn:aws:glacier:ap-northeast-1:************:vaults/examplevault",    "VaultName": "examplevault",    "CreationDate": "2021-10-14T01:05:33.264Z",    "LastInventoryDate": "2021-10-22T10:18:28.838Z",    "NumberOfArchives": 0,    "SizeInBytes": 0}
```

アーカイブが消えていることを確認しました。

```
"NumberOfArchives": 0
```

コンソールでも確認してみましょう。

消えていますね。

## ボールトの削除

それでは、最後にボールトを削除します。

こちらもボールトの作成同様管理コンソールでも操作は可能ですが、今回は CLI での操作を実施したいきます。

ボールトの作成は下記コマンドを実行します。

```
aws glacier delete-vault --vault-name ボールト名 --account-id アカウント名
```

```
$ aws glacier delete-vault --vault-name examplevault --account-id ************$
```

実行結果は何も返って来ませんでした。

本当にボールトが消えたのか確認してみましょう。

```
$ aws glacier describe-vault --vault-name examplevault --account-id ************An error occurred (ResourceNotFoundException) when calling the DescribeVault operation: Vault not found for ARN: arn:aws:glacier:ap-northeast-1:************:vaults/examplevault
```

ボールトが存在しないためエラーが返ってきました。削除は成功したようです。

コンソールも確認してみましょう。

ボールトが存在しないため、作成前の初期画面が表示されました。

ボールトの作成、削除は即刻反映されるようですね。

## 最後に

今回、ボールトの作成からアーカイブの操作、ボールトの削除まで試してみて分かりましたが、Glacier を扱う上でアーカイブやボールト、インベントリ、ジョブ等の概念も理解する必要があり、S3 のストレージクラスの一つ。という程度の理解だといざ利用しよう！となった時に混乱することが分かりました。

また、理解していたつもりでしたが、実際に触ってみると想像以上にアーカイブ関連の各操作の反映までに時間がかかりました。

直接 Glacier を操作する機会はあまり多くは無いかもしれませんが、利用する際は S3 と同様の感覚では操作出来ないこと。アーカイブ操作には時間がかかること。ここを理解しておきましょう。

## アノテーション株式会社について

アノテーション株式会社は、クラスメソッド社のグループ企業として「オペレーション・エクセレンス」を担える企業を目指してチャレンジを続けています。「らしく働く、らしく生きる」のスローガンを掲げ、様々な背景をもつ多様なメンバーが自由度の高い働き方を通してお客様へサービスを提供し続けてきました。現在当社では一緒に会社を盛り上げていただけるメンバーを募集中です。少しでもご興味あれば、[アノテーション株式会社WEBサイト](https://annotation.co.jp/)をご覧ください。

## 参考資料