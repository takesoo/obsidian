---
Created: Invalid date
URL: https://aws.amazon.com/jp/blogs/news/running-wordpress-amazon-ecs-fargate-ecs/
---
[![](https://d2908q01vomqb2.cloudfront.net/b3f0c7f6bb763af1be91d9e74eabfeb199dc1f1f/2021/06/22/image-2021-05-20T153609.196-557x630.png)](https://d2908q01vomqb2.cloudfront.net/b3f0c7f6bb763af1be91d9e74eabfeb199dc1f1f/2021/06/22/image-2021-05-20T153609.196-557x630.png)

---

この記事は、[Running WordPress on Amazon ECS on AWS Fargate with Amazon EFS](https://aws.amazon.com/blogs/containers/running-wordpress-amazon-ecs-fargate-ecs/) を翻訳したものです。

私が初めてウェブサイトを作ったのは 1997 年のことでした。それは当時好きだったミュージシャンのファンサイトでした。ウェブサイトの作り方はよく知らなかったのですが、自分の音楽の好みを (誰が聞いているかわかりませんが) _World Wide Web_ に伝えたいという熱い思いがありました。学校のコンピューターラボにあった_フロッピーディスク付きの PC_ は [MS-DOS](https://en.wikipedia.org/wiki/MS-DOS) を搭載しており、ラボの先生は Basic なトレーニングしか受けていなかったので、私の「Web 開発」の知識のほとんどは、_クールな_ウェブサイトを見つけて、そのコードを恥ずかしげもなくコピーしたものでした (「ソースを見る」ボタンを考えた人には特に感謝します)。ウェブページを作るのに必要な最低限の HTML を学んでいきましたが、数時間後には自分の限られた経験では価値のあるものを作ることはできないと痛感しました。インターネットの達人によると、CSS、PHP、JavaScript、Java アプレット、そして言うまでもなく Macromedia Shockwave の洗練されたアニメーションがないウェブサイトはジョークのようなものだと言っていました。そして最終的にはサイトを作ったものの、時間は掛かり、自分の好みに合うほど洗練されたものにはなりませんでした。

ありがたいことに、この 20 年間で多くのことが変わりました。今日では、誰でも数回クリックするだけで、プロ並みのウェブサイトを作成することができます。WordPress のようなコンテンツ管理システム (CMS) のおかげで、最小限の技術経験でサイト、ブログ、フォーラムなどを構築できるようになりました。プログラミングのスキルがなくてもウェブサイトを作ることができるのです。驚くことではありませんが、W3ctechs によると [Web サイトの 40 ％](https://w3techs.com/blog/entry/40_percent_of_the_web_uses_wordpress) が WordPress を使用しており、[最もトラフィックの多い Web サイト](https://wordpress.com/notable-users/)の一部を支えています

大規模な機関では数百もの WordPress サイトを運営していることも珍しくありません。例えば保険会社では、提供するプランごとに専用のサイトを作成します。また証券会社では、販売する投資商品ごとにパンフレットのようなサイトを作ることもあります。企業は WordPress を使って数百万件のリクエストに対応できるサイトを素早く作成しています。

コンテナ化された環境で WordPress サイトをスケールさせることが難しいのは、パフォーマンスと耐久性に優れた共有ストレージレイヤーが必要だからです。これまでは、サーバー間で WordPress のレプリカを実行するためにネットワーク上のファイル共有やファイルレプリケーションサービスに頼っていました。そのため、ファイル共有自体が単一障害点になったり、拡張性がないためにボトルネックになったりと別の問題が発生することも少なくありませんでした。そこでコンテナの採用について AWS のお客様と話をしたところ、「Amazon Elastic Container Service (Amazon ECS) を使えば WordPress サイトを大規模に運用できるのではないか？」という質問をよく受けました。

## Amazon EFS との統合

WordPress をコンテナで大規模にスケールすることが可能になったのは、[Amazon ECS が Amazon Elastic File System (Amazon EFS) をサポート](https://aws.amazon.com/jp/about-aws/whats-new/2020/04/amazon-ecs-aws-fargate-support-amazon-efs-filesystems-generally-available/)してからです。EFS はファイルの追加と削除に応じて自動的に拡張と縮小を行う超並列の共有アクセスを提供します。何千もの ECS タスクが共有された EFS ファイルシステムに対して同時に読み取りと書き込みの操作を行うことができます。

EFS が共有ストレージの問題に対する解決策を提供したことで、多くのお客様が ECS を使用した WordPress のワークロードをオーケストレーションするようになりました。最近、お客様とお会いすると「AWS Fargate を使うことで、運用の責任範囲をさらに減らせるのではないか？」という質問をよく受けます。

AWS Fargate は、[Amazon ECS](https://aws.amazon.com/jp/ecs/) と [Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/jp/eks/) の両方で動作するコンテナ用のサーバーレスコンピューティングエンジンです。サーバーのプロビジョニングと管理の必要性を排除し、アプリケーションごとにリソースを指定して支払いを行うことができ、意図的にアプリケーションを隔離することによってセキュリティを向上させています。適切なコンピューティング容量を割り当てることで、インスタンスの選択やクラスターの容量を調整する必要がありません。また、コンテナの実行に必要なリソースに対してのみ支払いを行うため、オーバープロビジョニングや追加のサーバーへの支払いは発生しません。Fargate はそれぞれのタスクや Pod を独自のカーネルで実行し、タスクや Pod に独自の隔離されたコンピュート環境を提供します。これによりアプリケーションはワークロードの分離が可能となり、Security by Design が改善します。これらの理由により、[Vanguard、Accenture、Foursquare、Ancestry](https://aws.amazon.com/fargate/customers/) などの AWS のお客様はミッションクリティカルなアプリケーションを Fargate 上で実行することを選択しました。

## ソリューション

このブログでは AWS Fargate 上で Amazon ECS を使用して、サーバーレスに WordPress を実行する方法を紹介します。VPC と ECS クラスターを作成し、その中で WordPress のタスクを実行します。RDS の MySQL インスタンスは WordPress が必要とするデータベースです。WordPress は共有された Amazon EFS ファイルシステムにデータを保存します。

### 前提条件

このチュートリアルを完了するには [AWS CLI バージョン 2](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/install-cliv2.html) が必要です。手順は Amazon Linux 2 でテストされています。

いくつかの環境変数を設定することから始めましょう。

```
export WOF_AWS_REGION=us-west-2 <-- 利用するリージョンに合わせて変更してくださいexport WOF_ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)export WOF_ECS_CLUSTER_NAME=ecs-fargate-wordpressexport WOF_CFN_STACK_NAME=WordPress-on-Fargat
```

### ベースとなるインフラストラクチャの作成

次に、2 つのアベイラビリティーゾーン (AZ) にまたがる 2 つのパブリックサブネットとプライベートサブネットを持つ VPC を作成し、各 AZ に NAT ゲートウェイを設置します。VPC のリソース、RDS データベース、EFS ファイルシステム、ALB (Application Load Balancer)、リスナー、ターゲットグループ、関連するセキュリティグループをプロビジョニングする CloudFormation テンプレートを作成しました。  
CloudFormation を使用してスタックを作成します。

```
wget https://raw.githubusercontent.com/aws-samples/containers-blog-maelstrom/main/CloudFormation/wordpress-ecs-fargate.yamlaws cloudformation create-stack \  --stack-name $WOF_CFN_STACK_NAME \  --region $WOF_AWS_REGION \  --template-body file://wordpress-ecs-fargate.yaml
```

スタックの展開が完了するまで、およそ 5 分かかります。

以下のコマンドはスタックのデプロイが完了するまで待機します。このコマンドでスタックのデプロイが完了したかを判断するか、[AWS マネジメントコンソール](https://console.aws.amazon.com/cloudformation/home)でデプロイの進捗状況を確認することができます。

```
aws cloudformation wait stack-create-complete \  --stack-name $(aws cloudformation describe-stacks  \    --region $WOF_AWS_REGION \    --stack-name $WOF_CFN_STACK_NAME \    --query 'Stacks[0].StackId' --output text) \  --region $WOF_AWS_REGION
```

CloudFormation スタックの出力から環境変数を読み込みます。

```
export WOF_EFS_FS_ID=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='EFSFSId'].OutputValue" \  --output text)export WOF_EFS_AP=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='EFSAccessPoint'].OutputValue" \  --output text)export WOF_RDS_ENDPOINT=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='RDSEndpointAddress'].OutputValue" \  --output text) export WOF_VPC_ID=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='VPCId'].OutputValue" \  --output text)export WOF_PUBLIC_SUBNET0=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet0'].OutputValue" \  --output text)export WOF_PUBLIC_SUBNET1=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet1'].OutputValue" \  --output text)export WOF_PRIVATE_SUBNET0=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='PrivateSubnet0'].OutputValue" \  --output text)export WOF_PRIVATE_SUBNET1=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='PrivateSubnet1'].OutputValue" \  --output text)export WOF_ALB_SG_ID=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='ALBSecurityGroup'].OutputValue" \  --output text)export WOF_TG_ARN=$(aws cloudformation describe-stacks  \  --region $WOF_AWS_REGION \  --stack-name $WOF_CFN_STACK_NAME \  --query "Stacks[0].Outputs[?OutputKey=='WordPressTargetGroup'].OutputValue" \  --output text)
```

このスタックは 2 つの AZ にマウントターゲットを持つ EFS ファイルシステムを作成します。各 AZ の WordPress タスクはその AZ のローカル EFS マウントターゲットを使用して EFS ファイルシステムをマウントします。

このスタックが作成する RDS MySQL インスタンスは、単一障害点であることに注意してください。本番環境では [Amazon RDS マルチ AZ 配置](https://aws.amazon.com/jp/rds/features/multi-az/)を使用して WordPress データベースの耐障害性を向上させることをお勧めします。また、RDS MySQL の代わりに [Amazon Aurora Serverless](https://aws.amazon.com/jp/rds/aurora/serverless/) を使用することもできます。これはアプリケーションのニーズに基づいて自動的に起動、シャットダウン、およびデータベース容量のスケーリングを行います。サーバーを管理することなくデータベースを稼働させることができます。

キャッシング層 ([Amazon ElastiCache for Memcached](https://aws.amazon.com/jp/elasticache/memcached/) など) や、[Amazon CloudFront](https://aws.amazon.com/jp/blogs/startups/how-to-accelerate-your-wordpress-site-with-amazon-cloudfront/) のようなコンテンツ配信ネットワークを利用することで、WordPress サイトのパフォーマンスをさらに高めることができます。[このガイド](https://aws.amazon.com/jp/elasticache/memcached/wordpress-with-memcached/)に従って、Amazon ElastiCache for Memcached を使った WordPress の高速化について学びましょう。

### タスク定義の作成

ネットワーク、データベース、共有ストレージの作成が完了したので WordPress をデプロイする準備が整いました。[bitnami/wordpress](https://hub.docker.com/r/bitnami/wordpress/dockerfile/) のコンテナイメージを使ってタスクを作成します。タスク定義にはデータベースの認証情報が含まれています。実世界のシナリオでは、データベースのパスワードを変更してください。

タスク定義を作成してみましょう。

```
cat > wp-task-definition.json << EOF{   "networkMode": "awsvpc",    "containerDefinitions": [        {            "portMappings": [                {                    "containerPort": 8080,                    "protocol": "tcp"                }            ],            "essential": true,            "mountPoints": [                {                    "containerPath": "/bitnami/wordpress",                    "sourceVolume": "wordpress"                }            ],            "name": "wordpress",            "image": "bitnami/wordpress",            "environment": [                {                    "name": "MARIADB_HOST",                    "value": "${WOF_RDS_ENDPOINT}"                },                {                    "name": "WORDPRESS_DATABASE_USER",                    "value": "admin"                },                {                    "name": "WORDPRESS_DATABASE_PASSWORD",                    "value": "supersecretpassword"                },                {                    "name": "WORDPRESS_DATABASE_NAME",                    "value": "wordpress"                },                {                    "name": "PHP_MEMORY_LIMIT",                    "value": "512M"                },                {                    "name": "enabled",                    "value": "false"                },                {                    "name": "ALLOW_EMPTY_PASSWORD",                    "value": "yes"                }            ]        }    ],    "requiresCompatibilities": [        "FARGATE"    ],    "cpu": "1024",    "memory": "3072",    "volumes": [        {            "name": "wordpress",            "efsVolumeConfiguration": {                "fileSystemId": "${WOF_EFS_FS_ID}",                "transitEncryption": "ENABLED",                "authorizationConfig": {                    "accessPointId": "${WOF_EFS_AP}",                    "iam": "DISABLED"                }            }        }    ],    "family": "wof-tutorial"}EOF
```

WordPress のコンテナイメージは `/bitnami/wordpress` にデータを保存するように設定されています。上記のタスク定義で明らかなように、このコンテナはコンテナのファイルシステムに `/bitnami/wordpress` の EFS ファイルシステムをマウントします。

また、EFS ファイルシステムへのアクセスにはアクセスポイントを使用します。EFS アクセスポイントは EFS ファイルシステムへのアプリケーション固有のエントリーポイントで、共有データセットへのアプリケーションのアクセスを容易に管理することができます。アクセスポイントは、そのアクセスポイントを経由して行われるすべてのファイルシステムへの要求に対して、ユーザー ID (ユーザーの POSIX グループなど) を強制することができます。また、ファイルシステムのルートディレクトリを指定して、クライアントが指定されたディレクトリやそのサブディレクトリのデータにのみアクセスできるようにすることもできます。EFS のセキュリティモデルとコンテナでの動作をさらに理解するには、Massimo Re Ferre の [Developers Guide to using Amazon EFS with Amazon ECS and AWS Fargate – Part 2](https://aws.amazon.com/blogs/containers/developers-guide-to-using-amazon-efs-with-amazon-ecs-and-aws-fargate-part-2/) をお読みください。

複数の WordPress を運用する場合、単一の EFS ファイルシステムを使用して複数サイトのデータを永続化し、サイトごとにアクセスポイントを使用してデータを分離することができます。

タスク定義を登録します。

```
WOF_TASK_DEFINITION_ARN=$(aws ecs register-task-definition \--cli-input-json file://wp-task-definition.json \--region $WOF_AWS_REGION \--query taskDefinition.taskDefinitionArn --output text)
```

### WordPress の実行

このセクションでは WordPress を実行する ECS クラスターを作成します。高可用性のために 2 つの WordPress のレプリカを維持する [ECS サービス](https://docs.aws.amazon.com/ja_jp/AmazonECS/latest/developerguide/ecs_services.html)を作成します。また ALB と統合し、WordPress のタスクが作成・破棄されると ALB のターゲットを自動的に更新します。

ECS クラスターを作成します。

```
aws ecs create-cluster \  --cluster-name $WOF_ECS_CLUSTER_NAME \  --region $WOF_AWS_REGION
```

ALB のセキュリティグループから 8080 ポートのトラフィックを受け付けるセキュリティグループを作成します。

```
WOF_SVC_SG_ID=$(aws ec2 create-security-group \  --description Svc-WordPress-on-Fargate \  --group-name Svc-WordPress-on-Fargate \  --vpc-id $WOF_VPC_ID --region $WOF_AWS_REGION \  --query 'GroupId' --output text)## 8080 ポートのトラフィックを受け付けるaws ec2 authorize-security-group-ingress \  --group-id $WOF_SVC_SG_ID --protocol tcp \  --port 8080 --source-group $WOF_ALB_SG_ID \  --region $WOF_AWS_REGION
```

ECS サービスを作成します。

```
aws ecs create-service \  --cluster $WOF_ECS_CLUSTER_NAME \  --service-name wof-efs-rw-service \  --task-definition "${WOF_TASK_DEFINITION_ARN}" \  --load-balancers targetGroupArn="${WOF_TG_ARN}",containerName=wordpress,containerPort=8080 \  --desired-count 2 \  --platform-version 1.4.0 \  --launch-type FARGATE \  --deployment-configuration maximumPercent=100,minimumHealthyPercent=0 \  --network-configuration "awsvpcConfiguration={subnets=["$WOF_PRIVATE_SUBNET0,$WOF_PRIVATE_SUBNET1"],securityGroups=["$WOF_SVC_SG_ID"],assignPublicIp=DISABLED}"\  --region $WOF_AWS_REGION# 2 つのタスクが実行されるまで待ちますwatch aws ecs describe-services \  --services wof-efs-rw-service \  --cluster $WOF_ECS_CLUSTER_NAME \  --region $WOF_AWS_REGION \  --query 'services[].runningCount'
```

2 つのタスクが実行されたら 次の手順に進みます。

### WordPress の管理者用ダッシュボードにアクセス

Fargate タスクが実行されたら、WordPress の管理者用ダッシュボードにログインします。ダッシュボードのアドレスを取得します。

```
echo "http://$(aws elbv2 describe-load-balancers \  --names wof-load-balancer --region $WOF_AWS_REGION \  --query 'LoadBalancers[].DNSName' --output text)/wp-admin/"
```

管理者のユーザー名は _‘user’_ で、パスワードは _‘bitnami’_ です。すぐに[パスワードを変更](https://wordpress.org/support/article/resetting-your-password/#to-change-your-password)しましょう。

### データの永続性をテスト

WordPress のインストールが完了したらデータの永続性をテストます。WordPress 管理画面で[新しいサイトテーマを有効にする](https://wordpress.com/support/themes/#activate-a-theme)などのサイト構成の変更を行います。変更が完了したらすべての WordPress コンテナを終了します。

```
aws ecs update-service \  --cluster $WOF_ECS_CLUSTER_NAME \  --region $WOF_AWS_REGION \  --service wof-efs-rw-service \  --task-definition "$WOF_TASK_DEFINITION_ARN" \  --desired-count 0
```

アクティブなレプリカが無くなるまで待ちます。watch を使用してレプリカの数を取得できます。

```
watch aws ecs describe-services \  --services wof-efs-rw-service \  --cluster $WOF_ECS_CLUSTER_NAME \  --region $WOF_AWS_REGION \  --query 'services[].runningCount'
```

すべてのタスクが終了したら、サービスを少なくとも 3 つのレプリカにスケールバックします。

```
aws ecs update-service \  --cluster $WOF_ECS_CLUSTER_NAME \  --region $WOF_AWS_REGION \  --service wof-efs-rw-service \  --task-definition "$WOF_TASK_DEFINITION_ARN" \  --desired-count 2
```

タスクが実行されたら WordPress の管理ダッシュボードに移動し、変更が維持されていることを確認します。

### サービスの Auto Scaling

人気のあるウェブサイトのトラフィックを分析してみると、頻繁に変動していることに気づくでしょう。トラフィックは周期的なパターンに沿っているため予測可能な場合もありますが、多くの場合、マーケティングキャンペーンやその他のビジネスプロセスなどの多くの外部要因に左右され、予測は非常に難しいものです。以前は、チームは必要な計算能力を予測しピーク時の需要を満たすためにインフラストラクチャをプロビジョニングする必要がありました。クラウドではアプリケーションとインフラストラクチャはビジネスニーズに応じて拡張することが期待されます。[ECS サービスの Auto Scaling](https://docs.aws.amazon.com/ja_jp/AmazonECS/latest/developerguide/service-auto-scaling.html) は、アプリケーションの負荷の増減に応じてパフォーマンスレベルを維持するのに役立ちます。

ここでは、WordPress に サービスの Auto Scaling を設定する例をご紹介します。まず、WordPress の ECS サービスを [アプリケーションの Auto Scaling](https://docs.aws.amazon.com/ja_jp/autoscaling/application/userguide/what-is-application-auto-scaling.html) に登録し、サービスの平均 CPU 使用率に基づいてタスクレプリカの数を自動調整するポリシーを作成します。

ECS サービスを Application Auto Scaling に登録します。

```
aws application-autoscaling \  register-scalable-target \  --region $WOF_AWS_REGION \  --service-namespace ecs \  --resource-id service/${WOF_ECS_CLUSTER_NAME}/wof-efs-rw-service \  --scalable-dimension ecs:service:DesiredCount \  --min-capacity 2 \  --max-capacity 4
```

> _ECS Service Auto Scaling を使用したことがない場合、_[_サービスの Auto Scaling に必要な IAM ロール_](https://docs.aws.amazon.com/ja_jp/AmazonECS/latest/developerguide/service-auto-scaling.html#auto-scaling-IAM)_も作成します。_

サービスの Auto Scaling のポリシードキュメントを作成します。

```
cat > scaling.config.json << EOF{     "TargetValue": 75.0,     "PredefinedMetricSpecification": {         "PredefinedMetricType": "ECSServiceAverageCPUUtilization"     },     "ScaleOutCooldown": 60,    "ScaleInCooldown": 60}EOF
```

アプリケーションの Auto Scaling のポリシーを設定します。

```
aws application-autoscaling put-scaling-policy \  --service-namespace ecs \  --scalable-dimension ecs:service:DesiredCount \  --resource-id service/${WOF_ECS_CLUSTER_NAME}/wof-efs-rw-service \  --policy-name cpu75-target-tracking-scaling-policy \  --policy-type TargetTrackingScaling \  --region $WOF_AWS_REGION \  --target-tracking-scaling-policy-configuration file://scaling.config.json
```

WordPress サービスの平均 CPU 使用率が 75 ％ 以上になると、スケールアウトアラームによりサービスの Auto Scaling が作動します。WordPress サービスに別のタスクを追加することで実行中のタスクの負荷を減らし、ユーザーがサービスの中断を意識しないようにします。逆に、平均 CPU 使用率が 75 ％ 以下になるとスケールインアラームが作動して、サービスの希望するタスク数が減少します。

ここで [hey](https://github.com/rakyll/hey) のようなユーティリティを使って負荷を生成し、Auto Scaling の設定をテストをします。

```
./hey_linux_amd64 -z 20m  <WordPress URL>
```

Auto Scaling を設定すると ECS は CPU 使用率に基づいてタスクを自動的に追加または削除します。サイトのトラフィックパターンを分析することで、ベースラインのパフォーマンスに必要な最小限のタスク数を見極めることができ、ECS はトラフィックの急増に対応してタスクを追加します。また、不要になったタスクを削除 (スケールイン) することでインフラストラクチャの支出を削減することができます。

### Fargate Spot

インフラストラクチャのコストについて言うと、WordPress のようなアプリケーションは Fargate Spot の理想的な候補です。ECS クラスターで動作する WordPress は、サービスに支障をきたすことなく Spot の中断に耐えることができます。これは、ECS サービスがタスクのレプリカの数を維持しているためです。タスクが Spot によって終了した場合、ECS は代わりのタスクを作成します。ECS キャパシティープロバイダーを使用して Spot と オンデマンドキャパシティを混在させることで、Spot キャパシティが一時的に利用できない場合でも、アプリケーションがオフラインにならないようにすることができます。Fargate Spot の詳細については、[こちらのブログ](https://aws.amazon.com/jp/blogs/compute/deep-dive-into-fargate-spot-to-run-your-ecs-tasks-for-up-to-70-less/)をご覧ください。

また、このトピックに触れたので、[Compute Savings Plans](https://aws.amazon.com/jp/savingsplans/compute-pricing/) を利用することで Fargate や EC2 への支出をさらに減らすことができることをお伝えしたいと思います。

### 後片付け

このブログ内で作成したリソースを削除するには次のコマンドを使用します。

```
aws application-autoscaling delete-scaling-policy --policy-name cpu75-target-tracking-scaling-policy --service-namespace ecs --resource-id service/${WOF_ECS_CLUSTER_NAME}/wof-efs-rw-service  --scalable-dimension ecs:service:DesiredCount --region $WOF_AWS_REGIONaws application-autoscaling deregister-scalable-target --service-namespace ecs --resource-id service/${WOF_ECS_CLUSTER_NAME}/wof-efs-rw-service  --scalable-dimension ecs:service:DesiredCount --region $WOF_AWS_REGIONaws ecs delete-service --service wof-efs-rw-service --cluster $WOF_ECS_CLUSTER_NAME --region $WOF_AWS_REGION --forceaws ec2 revoke-security-group-ingress --group-id $WOF_SVC_SG_ID --region $WOF_AWS_REGION --protocol tcp --port 8080 --source-group $WOF_ALB_SG_IDaws ec2 delete-security-group --group-id $WOF_SVC_SG_ID --region $WOF_AWS_REGIONaws ecs delete-cluster --cluster $WOF_ECS_CLUSTER_NAME --region $WOF_AWS_REGIONaws cloudformation delete-stack --stack-name $WOF_CFN_STACK_NAME --region $WOF_AWS_REGION
```

### まとめ

技術の進歩のおかげで、スケーラブルなウェブサイトを構築・運用することは、20 年前の私のような衝撃的な経験ではなくなりました。誰でも WordPress のようなコンテンツ管理システムを使ってコンテンツを作成し、公開することができます。このブログでは、Amazon ECS on AWS Fargate でサーバー管理の責任を負うことなくスケーラブルな WordPress の構築を支援する方法を紹介しました。Amazon ECS は Amazon EFS と統合して Web サイトを自動的にスケーリングできるパフォーマンスレイヤーを提供し、アクセスが落ち着いた時間帯にはコストを節約し、ピーク時には安定したパフォーマンスを確保します。

翻訳はソリューションアーキテクト加治が担当しました。原文は[こちら](https://aws.amazon.com/blogs/containers/running-wordpress-amazon-ecs-fargate-ecs/)です。