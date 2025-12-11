---
Tags:
  - AWS
  - Wordpress
---
## Simple File Manager for Amazon EFS

[

Simple File Manager for Amazon EFS を使ってみたら結構良かった | DevelopersIO

いわさです。 先日、AWSソリューション実装にEFS向けのファイル管理用テンプレートがリリースされました。 試してみたのですが、ソリューションの完成度が高くデプロイも簡単で費用面の心配も無いとのことで紹介したいと思います。 これはAWSの新サービスではなく、AWSソリューション実装と呼ばれる、AWSから提供されているソリューションテンプレートです。 ...

![](https://dev.classmethod.jp/_nuxt/icon_64x64.459039.png)https://dev.classmethod.jp/articles/efs-simple-file-manager-kekkou-yoi/

![](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/05/amazon-efs.png)](https://dev.classmethod.jp/articles/efs-simple-file-manager-kekkou-yoi/)

[

Automated deployment

Before you launch the solution, review the architecture, components, network security, and other considerations discussed in this guide. Follow the step-by-step instructions in this section to configure and deploy the solution into your account. Time to deploy: Approximately 15 minutes. Use the following steps to deploy this solution on AWS.

![](https://docs.aws.amazon.com/assets/images/favicon.ico)https://docs.aws.amazon.com/solutions/latest/simple-file-manager-for-amazon-efs/automated-deployment.html



](https://docs.aws.amazon.com/solutions/latest/simple-file-manager-for-amazon-efs/automated-deployment.html)

ログインユーザーを増やすにはcognitoに追加する

ディレクトリごとダウンロードはできないっぽい

  

## AWS Transfer Family for EFS

[

[アップデート] AWS Transfer Family の対象ストレージに Amazon EFS が追加サポートされました | DevelopersIO

本日のアップデートで AWS Transfer Family から EFS への接続が可能となりました。 従来、AWS Transfer Family は S3 を対象とした FTP/FTPS/SFTP が可能でしたが、今回のアップデートで EFS への FTP アクセスが可能となりました。 EFS へのアクセスは FTP/FTPS/SFTP 3 つのプロトコルで利用可能です。 EFS は VPC を経由して NFS マウントする共有ファイルシステムのマネージドサービスです。VPC 内の EC2 からは勿論のこと、Direct Connect や VPN を利用することでオンプレ環境からマウントすることが出来ます。 一方でパブリックな環境から EFS へ直接アクセスすることは出来ませんでしたが、 AWS Transfer Family を介することでパブリックな環境からも EFS 内のファイルへのアクセスが可能になります。 また従来であれば EFS をマウントするサーバー側で FTP

![](https://dev.classmethod.jp/_nuxt/icon_64x64.459039.png)https://dev.classmethod.jp/articles/aws-transfer-family-supported-amazon-efs/\#toc-1

![](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2020/06/aws-transfer-family.png)](https://dev.classmethod.jp/articles/aws-transfer-family-supported-amazon-efs/\#toc-1)

ディレクトリ丸ごとダウンロードも可能

bitnamiディレクトリはroot権限なのでユーザーIDとグループIDを0にするのがポイント