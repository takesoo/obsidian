## what
- TypeScriptによる[[Remote Procedure Call|RPC]]
## why
- REST APIの場合は、サーバーサイドとクライアントサイドで型を共有する方法が課題だったが、tRPCではサーバーサイドで定義した型をフロントエンドに推論されるため、型共有問題が解決する
- IDEの支援をフルで得られる
- [[T3 Stack]]のようにTypeScriptで完結するプロジェクトならわざわざHTTPの作法に則る必要がない