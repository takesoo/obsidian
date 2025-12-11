---
Tags:
  - AWS
---
# 参考資料

[[【CloudFormation入門】5分と6行で始めるAWS CloudFormationテンプレートによるインフラ構築 DevelopersIO]]

[[CloudFormationの全てを味わいつくせ！「AWSの全てをコードで管理する方法〜その理想と現実〜」 cmdevio DevelopersIO]]

# 概要

cloudformationは設定ファイルを設計図として、作成されるリソースを「stack」という括りで管理する

stackは設定ファイルを変更、反映すれば更新できる

IaCできる

stackを削除すればリソースが全て消えるので、消し忘れによる料金請求を防げる

# 設定ファイル

```
AWSTemplateFormatVersion: "2010-09-09"Resources:  FirstVPC:    Type: AWS::EC2::VPC    Properties:      CidrBlock: 10.0.0.0/16      Tags:        - Key: Name          Value: first-VPC  InternetGateway:    Type: AWS::EC2::InternetGateway    Properties:      Tags:        - Key: Name          Value: FirstVPC-IGW  AttachGateway:    Type: AWS::EC2::VPCGatewayAttachment    Properties:      InternetGatewayId: !Ref InternetGateway      VpcId: !Ref FirstVPC  FrontendSubnet:    Type: AWS::EC2::Subnet    Properties:      CidrBlock: 10.0.0.0/24      MapPublicIpOnLaunch: 'true'      VpcId: !Ref FirstVPC      Tags:      - Key: Name        Value: FirstVPC-FrontendSubnet    DependsOn: AttachGateway　# DependsOn設定しなくていい気がする  FrontendRouteTable:    Type: AWS::EC2::RouteTable    Properties:      VpcId: !Ref FirstVPC      Tags:      - Key: Name        Value: FirstVPC-FrontendRoute    DependsOn: AttachGateway  FrontendRoute:    Type: AWS::EC2::Route    Properties:      RouteTableId: !Ref FrontendRouteTable      DestinationCidrBlock: 0.0.0.0/0      GatewayId: !Ref InternetGateway    DependsOn: AttachGateway  FrontendSubnetRouteTableAssociation:    Type: AWS::EC2::SubnetRouteTableAssociation    Properties:      SubnetId: !Ref FrontendSubnet      RouteTableId: !Ref FrontendRouteTable
```

1. VPCを作成
    1. CIDRブロックを定義する
2. インターネットゲートウェイの作成
    1. IGWの定義
    2. AWS::EC2::VPCGatewayAttachmentでVPCと紐づける
3. サブネットの作成
    1. CIDRブロックを定義する
4. VPC内のルーティングを設定
    1. ルートテーブルの定義
    2. 0.0.0.0/0をIGWにつなぐルートをルートテーブルに追加
    3. ルートテーブルをサブネットに紐付け

# コンソールからデプロイ

「スタックの作成」または「スタックの更新」でファイルをアップロード

# AWS CLIからデプロイ

```
$ aws cloudformation deploy --profile **** --template-file ****.yaml --stack-name ****Waiting for changeset to be created..Waiting for stack create/update to completeSuccessfully created/updated stack - tuto-vpc
```