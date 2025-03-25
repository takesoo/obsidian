---
finished reading: true
favorite: true
tags:
  - clippings
---
# まだPCにIAMアクセスキー保存してるの？ ベスプラに沿って、AWS CLIやSDKから安全に開発しよう！ - Qiita
  #ReadItLater 
 #ReadableArticle

## articleURL
https://qiita.com/minorun365/items/d504c73014056ea5a485

## siteName
Qiita

## date
2025-03-22

## articleContent
AWSで開発をする際に必須のわりには、CLIやSDKからAWSアカウントへのアクセス設定ってかなり複雑ですよね。

私も未だによく混乱しては調べ直しているので、改めてベスプラを整理してまとめておきます。

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#aws-organizations--iam-identity-center%E3%81%8C%E4%BD%BF%E3%81%88%E3%82%8B%E5%A0%B4%E5%90%88)AWS Organizations & IAM Identity Centerが使える場合

理由がなければこちらを採用しましょう。  
個人検証用のAWSアカウントでも、用途ごとにアカウントをつど発行して使い捨てにできるので、安全ですし便利です。

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E7%90%86%E6%83%B3%E7%9A%84%E3%81%AA%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E6%96%B9%E5%BC%8F)理想的なアクセス方式

個人用IAM IICユーザー（Assume Role権限のみ） -> Assume Roleを実施（MFA認証必須） -> 作業対象AWSアカウント（作業に必要なIAMロールを利用）

[![スクリーンショット 2025-03-16 17.24.57.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F019fa12a-1482-457d-a14b-de6935c8314e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0fe3b2c72904b775dd67c7ff2d782214)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2F019fa12a-1482-457d-a14b-de6935c8314e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0fe3b2c72904b775dd67c7ff2d782214)

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E8%A3%9C%E8%B6%B3-iam-iic%E3%81%A8%E3%81%AF)補足： IAM IICとは？

IAMユーザーとは別に、IIC自体にユーザーアカウントを作成することができます。このIICユーザーには、IAMポリシーの代わりにIICの「許可セット」を設定することができ、これによってOrganization内の他のAWSアカウントへのアクセス権限を付与します。

このIICユーザーを、同じOrganization配下の各AWSアカウントにAssume Roleするためのスイッチ元アカウントとして使うことができます。

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E8%A8%AD%E5%AE%9A%E6%89%8B%E9%A0%86)設定手順

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E7%AE%A1%E7%90%86%E7%94%A8aws%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%81%A7aws-organizations%E3%82%92%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)管理用AWSアカウントでAWS Organizationsをセットアップ

-   AWS Organizationsを有効にする
-   作業用AWSアカウントを組織に追加する

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E7%AE%A1%E7%90%86%E7%94%A8aws%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%81%A7iam-iic%E3%82%92%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)管理用AWSアカウントでIAM IICをセットアップ

-   IAM IICを有効化する
-   IAM IICユーザーを作成する
-   IICユーザーに許可セットを設定する

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#iic%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E3%81%ABmfa%E3%82%92%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)IICユーザーにMFAをセットアップ

-   AWSアクセスポータルでIICユーザーにMFAデバイスを設定する

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E4%BD%9C%E6%A5%AD%E7%94%A8pc%E3%81%AB%E8%AA%8D%E8%A8%BC%E6%83%85%E5%A0%B1%E3%82%92%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)作業用PCに認証情報をセットアップ

-   AWS CLI v2をインストールする
-   aws configure ssoでSSO認証情報を設定する

~/.aws/config（自動作成）

```
[sso-session my-sso]
sso_start_url = https://xxxxxx.awsapps.com/start
sso_region = ap-northeast-1
sso_registration_scopes = sso:account:access

[profile admin]
sso_session = my-sso
sso_account_id = XXXXXXXXXX
sso_role_name = AdministratorAccess
region = ap-northeast-1
```

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E6%AF%8E%E5%9B%9E%E3%81%AE%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E6%89%8B%E9%A0%86)毎回のアクセス手順

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#gui%E3%83%9E%E3%83%8D%E3%82%B8%E3%83%A1%E3%83%B3%E3%83%88%E3%82%B3%E3%83%B3%E3%82%BD%E3%83%BC%E3%83%AB%E3%81%AE%E5%A0%B4%E5%90%88)GUI（マネジメントコンソール）の場合

-   AWSアクセスポータルにアクセスする
-   作業用AWSアカウントの、必要な許可セットを選択してアクセスする

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#aws-cli%E3%81%AE%E5%A0%B4%E5%90%88)AWS CLIの場合

-   `aws sso login --profile <プロファイル名>` で作業用AWSアカウントの一時権限を取得する
-   実施したいコマンドに `--profile <プロファイル名>` を付けて実行する

S3バケットを一覧表示するコマンドの例

```
aws s3 ls --profile admin
```

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#aws-sdk%E3%81%AE%E5%A0%B4%E5%90%88boto3%E3%81%AE%E4%BE%8B)AWS SDKの場合（Boto3の例）

-   Boto3の `client` メソッドを実行する前に、プロファイル名を指定してAssumeRoleを行う

test.py

```
import boto3

# AssumeRoleを実施
session = boto3.Session(profile_name="admin")

# 目的の処理を実行
s3 = session.client("s3")
response = s3.list_buckets()
print(response)
```

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#aws-organizations--iam-identity-center%E3%81%8C%E4%BD%BF%E3%81%88%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88)AWS Organizations & IAM Identity Centerが使えない場合

お仕事でリセラー制約下のAWSアカウントを使う場合など、IAM IICが利用できない場合のベスプラも紹介します。

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E7%90%86%E6%83%B3%E7%9A%84%E3%81%AA%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E6%96%B9%E5%BC%8F-1)理想的なアクセス方式

個人用IAMユーザー（Assume Role権限のみ） -> Assume Roleを実施（MFA認証必須） -> 作業対象AWSアカウント（作業に必要なIAMロールを利用）

[![スクリーンショット 2025-03-16 17.25.04.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fbf55e384-c99d-4b61-8ad5-403909c18229.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e55afa70c33a90a28ce4184cd28dc6fb)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1633856%2Fbf55e384-c99d-4b61-8ad5-403909c18229.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e55afa70c33a90a28ce4184cd28dc6fb)

※AWSアカウントを分けているのは、個人用IAMユーザーを作業先アカウントごとに複数持たなくてもいいよう、1箇所に統一するためです。

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E8%A8%AD%E5%AE%9A%E6%89%8B%E9%A0%86-1)設定手順

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E7%AE%A1%E7%90%86%E7%94%A8aws%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%81%AB%E5%80%8B%E4%BA%BA%E7%94%A8iam%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E3%82%92%E4%BD%9C%E6%88%90)管理用AWSアカウントに個人用IAMユーザーを作成

-   個人用IAMユーザーを作成する（下記2ポリシーを適用）

AllowAssuumeRole

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::<アカウントID>:role/<遷移先ロール名>",
            "Condition": {
                "Bool": {
                    "aws:MultiFactorAuthPresent": "true"
                }
            }
        }
    ]
}
```

AllowManageOwnMFA

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowListActions",
            "Effect": "Allow",
            "Action": [
                "iam:ListUsers",
                "iam:ListVirtualMFADevices"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowUserToCreateVirtualMFADevice",
            "Effect": "Allow",
            "Action": [
                "iam:CreateVirtualMFADevice"
            ],
            "Resource": "arn:aws:iam::*:mfa/*"
        },
        {
            "Sid": "AllowUserToManageTheirOwnMFA",
            "Effect": "Allow",
            "Action": [
                "iam:EnableMFADevice",
                "iam:GetMFADevice",
                "iam:ListMFADevices",
                "iam:ResyncMFADevice"
            ],
            "Resource": "arn:aws:iam::*:user/${aws:username}"
        },
        {
            "Sid": "AllowUserToDeactivateTheirOwnMFAOnlyWhenUsingMFA",
            "Effect": "Allow",
            "Action": [
                "iam:DeactivateMFADevice"
            ],
            "Resource": [
                "arn:aws:iam::*:user/${aws:username}"
            ],
            "Condition": {
                "Bool": {
                    "aws:MultiFactorAuthPresent": "true"
                }
            }
        },
        {
            "Sid": "BlockMostAccessUnlessSignedInWithMFA",
            "Effect": "Deny",
            "NotAction": [
                "iam:CreateVirtualMFADevice",
                "iam:EnableMFADevice",
                "iam:ListMFADevices",
                "iam:ListUsers",
                "iam:ListVirtualMFADevices",
                "iam:ResyncMFADevice"
            ],
            "Resource": "*",
            "Condition": {
                "BoolIfExists": {
                    "aws:MultiFactorAuthPresent": "false"
                }
            }
        }
    ]
}
```

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E4%BD%9C%E6%A5%AD%E7%94%A8aws%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%81%ABiam%E3%83%AD%E3%83%BC%E3%83%AB%E3%82%92%E4%BD%9C%E6%88%90)作業用AWSアカウントにIAMロールを作成

-   作業用IAMロールを作成する

信頼ポリシーの例

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<遷移元アカウントID>:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
                "Bool": {
                    "aws:MultiFactorAuthPresent": "true"
                }
            }
        }
    ]
}
```

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#iam%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E3%81%ABmfa%E3%82%92%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)IAMユーザーにMFAをセットアップ

-   管理用AWSアカウントのマネジメントコンソールに個人用IAMユーザーでサインインし、MFAデバイスを設定する

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E4%BD%9C%E6%A5%AD%E7%94%A8pc%E3%81%AB%E8%AA%8D%E8%A8%BC%E6%83%85%E5%A0%B1%E3%82%92%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97-1)作業用PCに認証情報をセットアップ

-   AWS CLI v2をインストールする
-   `aws configure --profile <プロファイル名>` でIAM認証情報を設定する

~/.aws/credentialの例（自動作成）

```
[profile assume-only]
aws_access_key_id = <アクセスキーID>
aws_secret_access_key = <シークレットアクセスキー>
```

-   configファイルに作業用IAMロールの情報を追記する

~/.aws/configの例（自動作成＋手動追記）

```
[profile assume-only]
region = ap-northeast-1

[profile admin-role]
role_arn = arn:aws:iam::<アカウントID>:role/<遷移先ロール名>
mfa_serial = arn:aws:iam::<アカウントID>:mfa/<MFAデバイス名>
source_profile = assume-only
region = ap-northeast-1
```

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E6%AF%8E%E5%9B%9E%E3%81%AE%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E6%89%8B%E9%A0%86-1)毎回のアクセス手順

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#gui%E3%83%9E%E3%83%8D%E3%82%B8%E3%83%A1%E3%83%B3%E3%83%88%E3%82%B3%E3%83%B3%E3%82%BD%E3%83%BC%E3%83%AB%E3%81%AE%E5%A0%B4%E5%90%88-1)GUI（マネジメントコンソール）の場合

-   管理用AWSアカウントに個人用IAMユーザーでサインインする
-   作業用AWSアカウントのIAMロールへスイッチロールする

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#aws-cli%E3%81%AE%E5%A0%B4%E5%90%88-1)AWS CLIの場合

-   作業コマンド実行時に `--profile` オプションで作業用IAMロールのプロファイル名を指定するだけ

※スイッチ元の認証情報もcredentialsファイルに記録されているため、CLI利用時に認証コマンドを手動で2段階実施する必要はありません。

### [](https://qiita.com/minorun365/items/d504c73014056ea5a485#aws-sdk%E3%81%AE%E5%A0%B4%E5%90%88boto3%E3%81%AE%E4%BE%8B-1)AWS SDKの場合（Boto3の例）

-   Boto3の `client` メソッドを実行する前に、プロファイル名を指定してAssumeRoleを行う

test.py

```
import boto3

# AssumeRoleを実施
session = boto3.Session(profile_name="admin-role")

# 目的の処理を実行
s3 = session.client("s3")
response = s3.list_buckets()
print(response)
```

※MFA認証が必要な場合は、ターミナル上でMFAコードを尋ねてくれます。

## [](https://qiita.com/minorun365/items/d504c73014056ea5a485#%E6%AC%A1%E3%81%AE%E3%82%B9%E3%83%86%E3%83%83%E3%83%97)次のステップ

このへんの作業を便利にしてくれるOSS「AWSume」があります！

私はこれに1Password用のプラグインを設定して、MFA入力まで自動化しました。

※ちなみに読み方は不明です。脳内ではアウシュームと呼んでます。

[129](https://qiita.com/minorun365/items/d504c73014056ea5a485/likers)

Go to list of users who liked

131

Register as a new user and use Qiita more conveniently

1.  You get articles that match your needs
2.  You can efficiently read back useful information
3.  You can use dark theme

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)