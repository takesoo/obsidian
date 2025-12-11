---
URL: https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html
---
Amazon EC2 Auto Scaling は、アプリケーションの負荷を処理するために適切な数の Amazon EC2 インスタンスを利用できるようにします。_Auto Scaling グループ_と呼ばれる EC2 インスタンスの集合を作成します。各 Auto Scaling グループ内のインスタンスの最小数を指定することができ、Amazon EC2 Auto Scaling グループはこのサイズよりも小さくなることはありません。各 Auto Scaling グループ内のインスタンスの最大数を指定することができ、Amazon EC2 Auto Scaling グループはこのサイズよりも大きくなることはありません。グループの作成時、またはそれ以降の任意の時点で、希望するキャパシティーを指定した場合、Amazon EC2 Auto Scaling によって、グループのインスタンス数はこの数に設定されます。スケーリングポリシーを指定する場合、Amazon EC2 Auto Scaling でアプリケーションに対する需要の増減に応じて、インスタンスを起動または終了できます。

たとえば、次の Auto Scaling グループで、インスタンス数の最小サイズが 1、希望するキャパシティーが 2、最大サイズが 4 であるとします。定義するスケーリングポリシーによって、指定した条件に基づいて、インスタンスの最小数と最大数の間でインスタンス数が調整されます。

[![](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/images/as-basic-diagram.png)](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/images/as-basic-diagram.png)

Amazon EC2 Auto Scaling のメリットの詳細については、「[Amazon EC2 Auto Scaling のメリット](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/auto-scaling-benefits.html)」を参照してください。

## Auto Scaling コンポーネント

次の表は、Amazon EC2 Auto Scaling の主要コンポーネントを示しています。

|プロパティ|設定テンプレート グループでは、起動テンプレート、または起動設定 (推奨されません。機能は少なくなります) を EC2 インスタンスの設定テンプレートとして使用します。インスタンスの AMI ID、インスタンスタイプ、キーペア、セキュリティグループ、ブロックデバイスマッピングなどの情報を指定できます。詳細については、「複数のテンプレートを起動する」および「起動設定」を参照してください。|
|---|---|
|[[Clippings/Amazon EC2 Auto Scaling とは - Amazon EC2 Auto Scaling (日本語)/無題のデータベース/無題|無題]]|**スケーリングのオプション**  <br>Amazon EC2 Auto Scaling では、Auto Scaling グループをスケーリングする方法がいくつか用意されています。たとえば、特定の条件の発生に基づいて、またはスケジュールに基づいて、グループがスケールされるように設定できます。詳細については、「[スケーリングのオプション](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/scaling_plan.html#scaling_typesof)」を参照してください。|

  
  

## 使用開始方法

使用を開始するには、「[Amazon EC2 Auto Scaling の使用を開始する](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/GettingStartedTutorial.html)」チュートリアルを最後まで行って Auto Scaling グループを作成し、そのグループ内のインスタンスが終了するときにどのように応答するかを確認してください。

## Amazon EC2 Auto Scaling の料金

Amazon EC2 Auto Scaling では追加料金は発生しません。AWS アーキテクチャにどのようなメリットがあるかをお気軽に試し、確認してください。お客様の料金は、AWS リソース (EC2 インスタンス、EBS ボリューム、CloudWatch アラームなど) のみです。

## Amazon EC2 Auto Scaling へのアクセス

Amazon Web Services アカウントにサインアップ済みの場合は、AWS Management Consoleにサインして Amazon EC2 Auto Scaling にアクセスできます。コンソールのホームページで [**EC2**] を選択し、ナビゲーションペインで [**Auto Scaling グループ**] を選択します。

Amazon EC2 Auto Scaling にアクセスするには、[Amazon EC2 Auto Scaling API](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/) を使用します。Amazon EC2 Auto Scaling はクエリ API を提供します。このリクエストは、HTTP 動詞である GET または POST と、`Action` という名前の Query パラメータを使用する HTTP または HTTPS リクエストです。Amazon EC2 Auto Scaling の API アクションの詳細については、_Amazon EC2 Auto Scaling API リファレンス_の「[アクション](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_Operations.html)」を参照してください。

HTTP または HTTPS を介してリクエストを送信する代わりに、言語固有の API を使用してアプリケーションを構築することを希望する場合に備えて、AWS には、ソフトウェアデベロッパー向けのライブラリ、サンプルコード、チュートリアル、その他のリソースが用意されています。これらのライブラリには、リクエストの暗号化署名、リクエストの再試行、エラーレスポンスの処理などのタスクを自動化する基本機能が用意されているので、開発を簡単に始められます。詳細については、[AWS の SDK およびツール](http://aws.amazon.com/tools/)を参照してください。

コマンドラインインターフェイスを使用する場合は、以下の選択肢があります。

**AWS コマンドラインインターフェイス（CLI）** 一連のさまざまな AWS 製品用のコマンドを提供し、Windows、macOS、および Linux でサポートされています。開始するには、「[AWS Command Line Interface ユーザーガイド](https://docs.aws.amazon.com/cli/latest/userguide/)」を参照してください。詳細については、_AWS CLI コマンドリファレンス_の「[autoscaling](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/index.html)」を参照してください。 **AWS Tools for Windows PowerShell** PowerShell 環境でスクリプトを記述するユーザー向けに、さまざまな AWS 製品用のコマンドが用意されています。開始するには、「[AWS Tools for Windows PowerShell ユーザーガイド](https://docs.aws.amazon.com/powershell/latest/userguide/)」を参照してください。詳細については、「[AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/Index.html)」を参照してください。

AWSにアクセスするための認証情報の詳細については、_Amazon Web Services 全般のリファレンス_の「[AWSセキュリティ認証情報](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)」を参照してください。Amazon EC2 Auto Scaling を呼び出すためのリージョンとエンドポイントの詳細については、_AWS全般のリファレンス_の「[リージョンとエンドポイント](https://docs.aws.amazon.com/general/latest/gr/as.html)」テーブルを参照してください。

## 関連サービス

受信アプリケーショントラフィックを Auto Scaling グループの複数のインスタンスに自動的に分散するには、Elastic Load Balancing を使用します。詳細については、[Elastic Load Balancing ユーザーガイド](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)を参照してください。

インスタンスと Amazon EBS ボリュームの基本的な統計情報をモニタリングするには、Amazon CloudWatch を使用します。詳細については、[Amazon CloudWatch ユーザーガイド](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)を参照してください。

Amazon EC2 以外の他のAmazon Web Services 用にスケーラブルなリソースの Auto Scaling を設定するには、[アプリケーション Auto Scaling ユーザーガイド](https://docs.aws.amazon.com/autoscaling/application/userguide/)を参照してください。