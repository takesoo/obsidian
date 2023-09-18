---
Created: Invalid date
URL: https://dev.classmethod.jp/articles/cloudformation-beginner01/
---
[![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2014/05/AWS_CloudFormation.png)](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2014/05/AWS_CloudFormation.png)

---

**「設定ファイルでインフラ構築とか、オシャレだよね〜」**

AWSの構築をオートメーション化するサービスの代表格である、AWS CloudFormation。

AWSに普段から触れている方であれば、なんとなくはその存在を知りつつも、「設定ファイルとか、300行ぐらい書くのでは？」「余計めんどくさそう」「覚えること多そう」なんて敬遠している人も多いかと思います。

というか、自分も昔はそうでした。

この記事ではそんな方に向けて**「5分と6行で始めるCloudFormationテンプレートによるインフラ構築」**と題して、yamlファイルを利用したCloudFormationによるインフラ構築の手順や、設定ファイルの書き方、拡張の仕方などの**超基本的**な部分を解説します。

これをきっかけに、膨大な仕様をもつ奥深く趣深いCloudFormationワールドに足を踏み入れていただける方が少しでも増えれば、筆者としては望外の喜びです。

- **想定読者**

普段からある程度AWSを触っているが、CloudFormationは敬遠していた人

- **この記事のゴール**

自分でテンプレートファイルを書いて試すことができる 世の中に多数出回っているテンプレートファイルやスニペットを自分用にカスタマイズできる

ほな、いってみましょ。

## CloudFormationとは？

まずは基本のBlackBeltから。

90ページにわたる資料なのですが、ここでは基本的なCloudFormationの特徴（p4〜p19）あたりをみていただければ十分です。

他のAWSのデプロイ＆マネージメント関連サービスとの兼ね合いで言えば、CloudFormationは、Provisioningを担当します。山のようにあるAWSサービスの中で、サービスごとの位置づけが俯瞰できるこういうスライドは、頭が整理されて非常にありがたい。

また、テンプレートとスタックの概念も把握しておきましょう。

[![](https://image.slidesharecdn.com/20161124-aws-blackbelt-cloudformationv2-161206105655/95/aws-black-belt-online-seminar-2016-aws-cloudformation-20-1024.jpg?cb=1481021881)](https://image.slidesharecdn.com/20161124-aws-blackbelt-cloudformationv2-161206105655/95/aws-black-belt-online-seminar-2016-aws-cloudformation-20-1024.jpg?cb=1481021881)

JSON/YAML形式のテンプレートテキストから、CloudFormationを経由してスタックを作成。そのスタックの単位でリソースの集合を管理する流れですね。

こういうふうにスタックでリソースを管理しておくことで、テンプレートを流したあと、「やっぱりちょっと設定変えてやり直したい」といったときに、スタックを削除すれば、簡単に作ったリソースを削除できます。最初のトライアンドエラー時期には、非常に便利です。

今回の記事では、テンプレートファイル例として人間に優しいYAMLを利用します。一昔前は、テンプレートはJSONのみ対応で人間が編集するには辛いものがあったのですが、YAML対応により、簡単に人間の手でも設定ファイルを編集できるようになりました。素晴らしい！

## 入門1 「まずは5分と6行でVPCを作ってみよう」

一番最初にVPCだけを単純に作成するテンプレートファイルを用意しました。合わせて作成されるVPCの構成図も貼っておきます。

```
AWSTemplateFormatVersion: '2010-09-09'Resources:  FirstVPC:    Type: AWS::EC2::VPC    Properties:      CidrBlock: 10.0.0.0/16
```

適当なファイル名（ここでは、01_create_vpc.yaml）で保存しておきましょう。

VPC作るだけで、中のリソースは全くありません。なので課金もありません。この記事で提示しているテンプレートは、全て無料なので、お気軽に試しくださいませ。ちなみに、CloudFormationの利用自体にも課金は発生しません。

CloudFormationのWebコンソールを開きます。

今回は、新規でスタックを作成するので、「新しいスタックの作成」をクリックします。

テンプレートの選択画面が表示されます。既存のテンプレートファイルを利用するので、「テンプレートをAmazon S3にアップロード」を選択し、テンプレートファイルを選択し、次へ行きます。

スタックの名前を入力します。あとでスタックの一覧で見たときに、「これ何を作るためのスタックやったっけ・・・」と微妙な気持ちにならないためにも、名前はきちんと入れておきましょう。

とりあえず、ここでは、端的に「VPC」とだけ入れておきます。

オプションは、ひとまずそのままスルーして次へ行きます。

確認画面が表示されたら、最後に作成をクリック。

作成がはじまり、最後に状況が「CREATE_COMPLETE」になれば、OKです。VPCの一覧などをみて、実際にVPCが作成されていることを確認してみてください。

作成したスタック（およびそれに紐づくリソース）の削除は、この画面から「アクション」→「スタックの削除」で可能です。

## 入門2 「テンプレートの超基本を理解する」

先程の設定ファイルを解説していきます。

```
AWSTemplateFormatVersion: '2010-09-09'Resources:  FirstVPC:    Type: AWS::EC2::VPC    Properties:      CidrBlock: 10.0.0.0/16
```

テンプレートは、下記のようにいくつかのセッションに分かれています。

公式ドキュメント：[AWS CloudFormation テンプレートの使用 » テンプレートの分析](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/template-anatomy.html#w2ab2c17c15b9)

- Format Version (任意)
- Description (任意)
- Metadata (任意)
- Parameters (任意)
- Mappings (任意)
- Resources (必須)
- Outputs (任意)

今回のテンプレートで利用しているのは、AWSTemplateFormatVersionと、Resourcesなので、その２つを解説します。

### AWSTemplateFormatVersion

テンプレートの形式を指定します。現在は2010-09-09のみ利用できます。テンプレートファイルのお作法として、とりあえず指定しておくようにしましょう。

参考：[形式バージョン](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/format-version-structure.html)

### Resources

テンプレートにおいて唯一必須のセッション。スタックに含める、VPCやEC2インスタンスやS3バケットなどのリソースを宣言します。

公式ドキュメント：[リソース](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/resources-section-structure.html)

リソース部分の構文はこちら。

```
Resources:  <Logical ID>:    Type: <Resource type>    Properties:      <Set of properties...>
```

### Logical ID

テンプレート内で一意なID。テンプレートの中で、他のリソースを参照する場合などは、このIDを利用します。また、スタックのリソース一覧にも、このIDでリソースが表示されます。

### Resource type

実際に作成するリソースのタイプです。対応しているリソースタイプは[AWS リソースプロパティタイプのリファレンス](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)に一覧があります。

### Resource properties

各リソースの作成時に指定するプロパティです。リソースタイプによって利用できるプロパティは異なるので、公式ドキュメントとにらめっこしながら、指定していきます。

### リソースにタグを指定したテンプレートファイル例

例えば、リソースに命名するときは、タグのキーにNameを指定して、任意の名前を入力します。

```
AWSTemplateFormatVersion: '2010-09-09'Resources:  hamadaVPC:    Type: AWS::EC2::VPC    Properties:      CidrBlock: 10.0.0.0/16      Tags:      - Key: Name        Value: first-VPC
```

どうでしょうか。最小構成であれば、設定ファイルは非常にシンプルであることがお分かりいただけたんじゃないでしょうか。

## 入門3 「VPCにサブネットやルーティングテーブルやインターネットゲートウェイを構築する」

さて、入門2では、非常にシンプルな設定ファイルでVPCを作成しましたが、VPCには他にも設定が必要なリソースがあります。

- インターネットゲートウェイ
- サブネット
- ルーティングテーブル

これらを、VPCと合わせて作成するテンプレート例はこうなります。構成図も合わせて貼っておきますので、見比べてみてください。先ほどと同じ手順で実行して、うまく関連リソース作成されたら良ござんす。

```
AWSTemplateFormatVersion: '2010-09-09'Resources:  FirstVPC:    Type: AWS::EC2::VPC    Properties:      CidrBlock: 10.0.0.0/16      Tags:      - Key: Name        Value: FirstVPC  InternetGateway:    Type: AWS::EC2::InternetGateway    Properties:      Tags:      - Key: Name        Value: FirstVPC-IGW  AttachGateway:    Type: AWS::EC2::VPCGatewayAttachment    Properties:      VpcId: !Ref FirstVPC      InternetGatewayId: !Ref InternetGateway  FrontendRouteTable:    Type: AWS::EC2::RouteTable    DependsOn: AttachGateway    Properties:      VpcId: !Ref FirstVPC      Tags:      - Key: Name        Value: FirstVPC-FrontendRoute  FrontendRoute:    Type: AWS::EC2::Route    DependsOn: AttachGateway    Properties:      RouteTableId: !Ref FrontendRouteTable      DestinationCidrBlock: 0.0.0.0/0      GatewayId: !Ref InternetGateway  FrontendSubnet:    Type: AWS::EC2::Subnet    DependsOn: AttachGateway    Properties:      CidrBlock: 10.0.1.0/24      MapPublicIpOnLaunch: 'true'      VpcId: !Ref FirstVPC      Tags:      - Key: Name        Value: FirstVPC-FrontendSubnet  FrontendSubnetRouteTableAssociation:    Type: AWS::EC2::SubnetRouteTableAssociation    Properties:      SubnetId: !Ref FrontendSubnet      RouteTableId: !Ref FrontendRouteTable
```

いきなりなが～くなりましたが、作成されるリソースの内容を順に追っていけば、よくある一般的なVPCの設定になっているのがおぼろげながらわかるのではないでしょうか。

ここでは、テンプレートファイルに対して、新しく２つの概念が登場してます。

### !Ref（組み込み関数）

CloudFormationのテンプレートファイルには組み込み関数を利用することが可能で、これもその一種。

!Refでテンプレートファイル内で指定したリソースの値を参照します。上の例では、AttachGatewayのところで、アタッチ対象のインターネットゲートウェイのIDを!Ref InternetGateWayで指定しています。実際に構築するまでIDがわからないリソースを参照するためには、必須です。

こういうふうに関連するリソースが多々あるものについては、一つのテンプレートファイルにまとめて、リソースIDを!Refで参照させるのが、CloudFormationテンプレートを作成するときの重要なお作法。

CloudFormationには、他にも組み込み関数が多数用意されているので、事前に一通り目を通しておくことをオススメします。

公式ドキュメント：[組み込み関数リファレンス](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html)

### DependsOn（リソース依存関係の定義）

特定リソースが他のリソースに続けて作成されるように指定します。通常、CloudFormationは、上で紹介した!Refを使用する場合、暗黙的に、参照先のリソースを事前に作成するルールが適用されますが、DependsOn属性を利用することで、任意のリソースに対しても、その作成する依存関係を定義することができます。

上のテンプレートファイル例では、FrontendRouteTableの作成時、DependsOnで、AttachGatewayリソースに対して、依存関係を設定しています。

公式ドキュメント：[DependsOn属性](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-attribute-dependson.html)

以上で、CloudFormationテンプレートの基礎解説は、終了となります。

## 記事のまとめと、今後の学習指針

この記事で見てきたとおり、CloudFormationのテンプレートファイルを理解するには、いきなり長大なテンプレートファイルを読み込むよりも、最小構成のテンプレートを利用して以下の順番で学習するのが効率が良いです。

1. CloudFormationの基礎（テンプレートとスタックとリソースの関係）
2. テンプレートファイルの各セクション
3. 各種リソースの作成方法
4. 組み込み関数の存在
5. DependsOn（リソース依存関係）

概ねこれぐらいを理解しておけば、いろんな公式のテンプレートファイルやスニペットを見ているときに「ななな、なんのこっちゃ？」と迷うこともなくなると思います。

あとは、文中紹介した、各種公式リファレンスや[テンプレートスニペット](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/CHAP_TemplateQuickRef.html)などを参考に、頑張ってみてください！ 健闘を祈ります。

## もっと実践的なCloudFormationの活用について

先日、2019年11月のDevelopers.IO Tokyoにて、CloudFormationの活用についてより実践的な内容で発表しました。

実際にいろんなAWSのサービスをCloudFormationで定義し、そして運用していく上でのハマリポイントや、スタックのわけかたのベストプラクティスなどを惜しげもなく詰め込んでいるので、CloudFormationに慣れてきたら、是非こちらの内容も参考にしてください。

それでは、今日はこのへんで。[濱田](https://dev.classmethod.jp/author/hamada-koji/)でした。

## Developers.IOの他の参考記事

Developers.IOには、他にもCloudFormationの記事が多数挙げられています。これらも参考にしてみてください！