---
Tags:
  - AWS
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
## AutoScalingグループ

EC2インスタンスの最小、最大数などのスケーリングの設定をする。

## 起動テンプレート

起動するEC2インスタンスの設定をテンプレートとして保存する。AutoScaleの際にはこのテンプレートをもとに新しいEC2インスタンスが起動する。

  

  

[[Amazon EC2 Auto Scaling とは - Amazon EC2 Auto Scaling (日本語)]]

[

Amazon EC2 Auto Scaling とは

Amazon EC2 Auto Scaling は、アプリケーションの負荷を処理するために適切な数の Amazon EC2 インスタンスを利用できるようにします。 Auto Scaling グループ と呼ばれる EC2 インスタンスの集合を作成します。各 Auto Scaling グループ内のインスタンスの最小数を指定することができ、Amazon EC2 Auto Scaling グループはこのサイズよりも小さくなることはありません。各 Auto Scaling グループ内のインスタンスの最大数を指定することができ、Amazon EC2 Auto Scaling グループはこのサイズよりも大きくなることはありません。グループの作成時、またはそれ以降の任意の時点で、希望するキャパシティーを指定した場合、Amazon EC2 Auto Scaling によって、グループのインスタンス数はこの数に設定されます。スケーリングポリシーを指定する場合、Amazon EC2 Auto Scaling でアプリケーションに対する需要の増減に応じて、インスタンスを起動または終了できます。 たとえば、次の Auto Scaling グループで、インスタンス数の最小サイズが 1、希望するキャパシティーが 2、最大サイズが 4 であるとします。定義するスケーリングポリシーによって、指定した条件に基づいて、インスタンスの最小数と最大数の間でインスタンス数が調整されます。 Amazon EC2 Auto Scaling のメリットの詳細については、「 Amazon

![](https://docs.aws.amazon.com/assets/images/favicon.ico)https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html

![](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/images/as-basic-diagram.png)](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)