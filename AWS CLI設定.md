---
Tags:
  - AWS
  - CLI
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
```
# 設定$ aws configureAWS Access Key ID [None]: <Access Key ID>AWS Secret Access Key [None]: <Secret Access Key>Default region name [ap-northeast-1]:Default output format [None]: json# 設定の確認$ aws configure list      Name                    Value             Type    Location      ----                    -----             ----    --------   profile                <not set>             None    Noneaccess_key     ****************Y4H2 shared-credentials-filesecret_key     ****************0DZ5 shared-credentials-file    region           ap-northeast-1      config-file    ~/.aws/config# 設定ファイル$ less ~/.aws/config[default]region = ap-northeast-1output = json# クレデンシャルファイル$ less ~/.aws/credintials[default]aws_access_key_id = <aws_access_key_id>aws_secret_access_key = <aws_secret_access_key>
```

[

設定の基本 - AWS Command Line Interface

AWS CLI は、AWS のサービスとやり取りするためのコマンドを提供する AWS SDK for Python (Boto) を使用して構築されたオープンソースツールです。最小限の設定で、使い慣れたターミナルプログラムから、AWS Management Consoleで提供されるすべての機能の使用を開始できます。 このガイドでは、Windows、macOS、および Linux で AWS CLI をインストール、設定、使用するための手順を示します。AWS CLI を使用して任意の AWS のサービスのパブリック API にアクセスし、スクリプトを記述して AWS リソースを管理する方法を説明します。

![](https://docs.aws.amazon.com/assets/images/favicon.ico)https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-configure-quickstart.html

![](https://a0.awsstatic.com/libra-css/images/logos/aws_logo_smile_179x109.png)](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-configure-quickstart.html)