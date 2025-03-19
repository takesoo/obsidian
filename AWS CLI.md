---
tags:
  - AWS
  - AWS/CLI
---
## マニュアル表示
```shell
aws help
aws ec2 help
aws ec2 describe-instances help
```
## profile
- `~/.aws/config`のprofile設定に基づいて実行する
```shell
aws ec2 start --profile special-profile
```
