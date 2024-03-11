---
tags:
  - AWS/CDK
aliases:
  - cdk command
---
- cdkコマンドを使えるようにするツールキット

| Command                    | Function                                                                                                                |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `cdk list` (`ls`)          | Lists the stacks in the app                                                                                             |
| `cdk synthesize` (`synth`) | Synthesizes and prints the CloudFormation template for one or more specified stacks                                     |
| `cdk bootstrap`            | Deploys the CDK Toolkit staging stack; see [Bootstrapping](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html) |
| `cdk deploy`               | Deploys one or more specified stacks                                                                                    |
| `cdk destroy`              | Destroys one or more specified stacks                                                                                   |
| `cdk diff`                 | Compares the specified stack and its dependencies with the deployed stacks or a local CloudFormation template           |
| `cdk import`               | Uses CloudFormation resource imports to bring existing resources into a stack managed by CDK                            |
| `cdk metadata`             | Displays metadata about the specified stack                                                                             |
| `cdk init`                 | Creates a new CDK project in the current directory from a specified template                                            |
| `cdk context`              | Manages cached context values                                                                                           |
| `cdk docs` (`doc`)         | Opens the CDK API Reference in your browser                                                                             |
| `cdk doctor`               | Checks your CDK project for potential problems                                                                          |