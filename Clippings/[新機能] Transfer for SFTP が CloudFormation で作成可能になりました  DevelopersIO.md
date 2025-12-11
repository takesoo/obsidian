---
URL: https://dev.classmethod.jp/articles/transfer-for-sftp-with-cloudformation/
---
[![](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/04/aws-transfer-for-sftp.png)](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/04/aws-transfer-for-sftp.png)

---

この記事は公開されてから1年以上経過しています。情報が古い可能性がありますので、ご注意ください。

こんにちは、菊池です。

地味なアップデートを紹介していくコーナーです。先日のアップデートにて、AWS Transfer for SFTP が CloudFormation に対応しました。

## Transfer for SFTP を CloudFormation で作成

早速やってみました。

ServerとUser、両方ともCloudFormationに対応しています。 Transfer for SFTPを利用するのに必要な、IAM Role、S3バケットも併せて、まとめて作成してみました。

```
AWSTemplateFormatVersion: '2010-09-09'Description: SFTP RESOURCESResources:\#S3アクセス用のIAM Roleの作成  SftpRole:    Type: AWS::IAM::Role    Properties:      AssumeRolePolicyDocument:        Version: "2012-10-17"        Statement:          - Effect: "Allow"            Principal:              Service:                - "transfer.amazonaws.com"            Action:              - "sts:AssumeRole"      ManagedPolicyArns:        - arn:aws:iam::aws:policy/AmazonS3FullAccess\#SFTP Serverの作成  SftpServer:    Type: AWS::Transfer::Server    Properties:      EndpointType: PUBLIC\#Home Directory用S3バケットの作成  HomeS3bucket:    Type: AWS::S3::Bucket    Properties:      BucketName: !Join        - '-'        - - 'sftp'          - !Ref 'AWS::AccountId'#SFTP Userの作成  SftpUser:    Type: AWS::Transfer::User    Properties:      UserName: SFTPUSER      HomeDirectory: !Sub /${HomeS3bucket}/SFTPUSER      Role: !GetAtt SftpRole.Arn      ServerId: !GetAtt SftpServer.ServerId      SshPublicKeys:        - ssh-rsa XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXaB40RRVx4bHrdiuGbTBLp3qXwNf4QPSAylHPFCPBbdi/d+t9ox4NWtG3snuSV2ghRF8WBSj96DOQ7a/bRXaSayZnugmckoAi2LQPEBh7a9hcnCml/xGAPmq2riXo3dUgIlNTxS6RiCSuOX3HRzd1uMjtQX+tRqlt6Wnj/IY4fpIVY4Ra3g67lmZVBtGvh+uNnJX8cs7FnN6Y3KaFqelQMl9lLVYIGURfSrhwyZRSKF3b1Hgqo1uzX5SCugwb0IjNGcPU3x5p1fYUdrAv8mhk0nw/qPW47SVyXBaN9EkAk3AltF
```

SFTP Userの接続に利用するパブリックキーは、公開鍵の内容をそのまま記載することで設定可能です。

スタックを作成すれば、以下のようにまとめてリソースが設定されました。SFTPのマネジメントコンソール上で、CloudFormationで管理さている旨が表示されています。

## 最後に

以上です。Transfer for SFTPは、サーバー側の設定は非常にシンプルですが、CloudFormationでユーザーをポリシーなど含めて一括で管理できるのがうれしいところかもしれません。