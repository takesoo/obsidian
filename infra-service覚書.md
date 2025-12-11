docker-compose.yml, Dockerfileをコピー

.devcontainer/をコピー

package.jsonをコピー

yarn installでパッケージのインストール。.huskyが生成された

一度devcontainerを閉じてDockerfileに`COPY package.json`を追加してrebuild and reopen

prettier.jsonとかをコピー

cdk init

cdk synth

cdk bootstrap

cdk deploy

cdk deploy —hotswap

cdk diff

npx(yarn) cdk deploy

cdk用のS3バケットをコンソールで削除した場合は、同名のバケットを手動で作成する必要がある。バケット名はcdk deployなどのエラーメッセージで表示される。

  

githubのソースコードをCodePipelineに使う方法？CodePipelineSource.githubでは取れない？infra-serviceだとcodestarを使ってる？なにそれ？