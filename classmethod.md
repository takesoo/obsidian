---
Tags:
  - AWS
---
## 技術選定

- 全体構成
    
    - Next.js
        - モダン
        - Reactエンジニアが多い
        - SEOの要望が強い
        - ReactでSSRといえばNext.js
    - ECS Fargate
        - 既存のRDSへの接続
        - 既存システムとの並行かどうしながら移行
        - SSR
    - 内部通信経路にRoute53
    - API RoutesをBFFに
    - リポジトリ構成
        - プロジェクトメンバーがバックエンドに偏ってた
        - 型情報を共通化
        - モノリポ
            - turborepo
                - ビルドシステム
            - Zod
                - スキーマ共有
    - コンテンツ管理
        - MicroCMS
            
            - 開発への変更依頼をなくしたい
            - 操作が簡単
            - 拡張性
            - APIの使いやすさ
            
              
            
    
    ---
    

## Graviton2

- Intelより高性能なCPU
- arm
- AWS製プロセッサ
- ドキュメント（？）
    - AWS Graviton getting-started(Github)
    - AWS Graviton2 for independent Softwear
- lambda
    - lambdaのarm64=Graviton2
    - インタプリタは最新バージョンならOK
    - コンパイル型ならサポートラインタイムを使う
    - Porting Advisor for Graviton
- Fargate
    - アーキテクチャごとのイメージビルドが必要
    - マルチアーキテクチャコンテナイメージ
        - docker buildx hogehoge

---

## ログ収集・監視アーキテクチャパターン

- メトリクスとログとトレース
    - メトリクスで当たりをつけて、ログとトレースで詳細を調べる
- AWSサービスのみでのログ収集監視
- VPCエンドポイントがお金かかるようならFluentbit
- さらに問題が出てきたらSaaSを利用
    - SaaSによってコストのポイントが違うので注意
    - SaaSが高機能すぎて浸透しないことも
- より最適化するならOSSで
    - Grafana Loki, Tempo