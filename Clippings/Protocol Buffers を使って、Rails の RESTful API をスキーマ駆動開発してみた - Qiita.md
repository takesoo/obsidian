---
category: "[[Clippings]]"
author: "[[by moonstruckdrops@github]]"
title: Protocol Buffers を使って、Rails の RESTful API をスキーマ駆動開発してみた - Qiita
source: https://qiita.com/t0yohei/items/8b04bd427767c8df7465
clipped: 2023-10-04
published: 2019-12-20
tags:
  - clippings Ruby Rails protobuf ProtocolBuffers
---

こんにちは、クラウドワークスの [@t0yohei](https://qiita.com/t0yohei "t0yohei") です。普段は Rails や Vue.js などを使って crowdworks.jp の開発をしています。  
この記事は クラウドワークス Advent Calendar 2019 の20日目の記事です。

昨日は [@juntetsu\_tei](https://qiita.com/juntetsu_tei "juntetsu_tei") による、[Rails更新は最低限Controllerのテストが欲しいというお話](https://qiita.com/juntetsu_tei/items/3bf793ae84191b86d6ac)でした。

今回は、Protocol Buffers を使った、 Rails の RESTful API はこんな感じに開発できるんじゃない？って内容です。実務で実際に使ったわけではないので、不足している点があると思いますが悪しからず。

## [](#%E7%94%A8%E8%AA%9E%E3%81%AE%E6%95%B4%E7%90%86)用語の整理

### [](#protocol-buffers-%E3%81%A3%E3%81%A6)Protocol Buffers って？

Google が社内向けに開発を始めたツールです。2008年以降 OSS として公開されており、誰でも閲覧、 contribute することができます。[https://github.com/protocolbuffers/protobuf](https://github.com/protocolbuffers/protobuf)

Protocol Buffers は protobuf と略称して呼ばれており、この記事でも今後 protobuf という呼び方を基本的に使いたいと思います。

[GitHub の Overview](https://github.com/protocolbuffers/protobuf#overview) によると、

> Protocol Buffers (a.k.a., protobuf) are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data.

Protocol Buffers（別名、protobuf）は、構造化データをシリアライズするための Google の言語中立、プラットフォーム中立な拡張可能なメカニズムです。(著者訳)

と書かれていますが、正直難しい。個人的な理解としては、「プロセス間(システム間)でデータをやりとりする際のシリアライズ形式と、シリアライズに必要な各種ライブラリ及びスキーマ定義のためのDSL」という認識です(わかりにくい)。

### [](#protocol-buffers-%E7%BE%8E%E3%81%97%E3%81%84%E4%B8%80%E7%95%AA%E5%A5%BD%E3%81%8D%E3%81%AA%E3%82%B9%E3%82%AD%E3%83%BC%E3%83%9E%E5%AE%9A%E7%BE%A9%E8%A8%80%E8%AA%9E%E3%81%A7%E3%81%99)Protocol Buffers 美しい！一番好きなスキーマ定義言語です！

先ほどの説明の最後に、 `スキーマ定義のためのDSL` というワードが出てきましたが、この DSL がとても美しく、今でも protobuf が生き残っている由縁と言われています。  
サンプルとして、公式ガイドに載っているスキーマ定義を載せておこうと思います。 protobuf のことを知らなくても、なんとなくどんな内容が書かれているのか想像することができるかと思います。

```
message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}
```

スキーマ定義言語とは何か、なぜそれが重要かといったことは、[今さらProtocol Buffersと、手に馴染む道具の話](https://qiita.com/yugui/items/160737021d25d761b353)という記事に全てまとめられているので、こちらをご参照いただければと思います。

自分のようにリンク先を読むのが面倒だという人のために、スキーマ定義言語とは何かということについて、自分なりの説明を試みてみようと思います。

スキーマと聞くと、多くの方はDBスキーマを想像するかと思いますが、スキーマ本来の意味としてはDBに限らない「構造」となります。 protobuf で定義するものは、この「構造」の中でも、プロセス間(システム間)でやりとりされるデータの構造(形式・内容)となります。  
protobuf と同種のスキーマを定義する言語としては、他にも [Open API(旧Swagger)](https://www.openapis.org/) や [GraphQL](https://graphql.org/) が上げられるかと思います(GraphQLについては、GraphQLのDSLと読み替えた方が正しいかもしれません)。 ([注釈1](https://qiita.com/t0yohei/private/8b04bd427767c8df7465#%E6%B3%A8%E9%87%881))

protobuf は [gRPC のスキーマ定義の標準言語に位置付けられており](https://grpc.io/docs/guides/)、 gRPC の発達とともに最近再度注目を集めている印象です。

protobuf の詳細について知りたい方は、 [Google の protobuf に関する公式ガイド](https://developers.google.com/protocol-buffers/docs/overview)をお読みください。  
自分のように、なんやかんや日本語訳が読みやすいよねって方は、上記を翻訳してみた[こちらの記事](https://qiita.com/t0yohei/items/f77659749de0217a8ec5)をご参照ください。

### [](#%E3%82%B9%E3%82%AD%E3%83%BC%E3%83%9E%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA)スキーマ駆動開発

プロセス間(システム間)でやりとりされるデータの構造(スキーマ)を一番最初に決めて、開発を進める手法です。テスト駆動開発がテストを書くことから始まり、テストに駆動された実装になるように、スキーマ駆動開発でもスキーマを先に定義して、スキーマに駆動される開発を行います。

protobuf では、定義したスキーマから実装に使用するコードを自動生成します。この点から、開発がスキーマに駆動されている感覚があり、またスキーマから自動生成されたコードを元に実装を組み上げるため、スキーマ定義と実装間の完全性が保たれます。

オレオレスキーマ定義(ドキュメントにプロセス間でやりとりされるデータをいい感じにまとめる手法)や、 Open API などを使ったスキーマ定義との違いが現れるのがこの部分で、実装とドキュメントが解離しているといった不幸をなくすことができます。 protobuf 素晴らしい。

## [](#%E5%AE%9F%E8%A3%85%E3%82%A4%E3%83%A1%E3%83%BC%E3%82%B8)実装イメージ

前置きが長くなりましたが、 protobuf を使った、Rails の RESTful API がどんな感じになるのかを書いていきます。今回は雰囲気を掴んでもらうことを主眼に置いているので、チュートリアル形式にはなっていません。実際に実装を試したいと思った方は、[付録1に簡単なチュートリアルっぽいもの](https://qiita.com/t0yohei/private/8b04bd427767c8df7465#%E4%BB%98%E9%8C%B21-%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB%E3%81%AE%E3%82%88%E3%81%86%E3%81%AA%E3%82%82%E3%81%AE)を用意しているのでご参照ください。

### [](#proto-%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%A7%E3%81%AE%E3%82%B9%E3%82%AD%E3%83%BC%E3%83%9E%E5%AE%9A%E7%BE%A9)proto ファイルでのスキーマ定義

まずは proto ファイルでのスキーマ定義です。 今回は、フロントエンドをSPAで実装することを想定した TODO リストにおける Rails の RESTful API のうち、 index, show, create の各アクションについて定義していきます。各アクションではそれぞれ、 Request/Response の形式・内容を定義しています。

task.proto

```
syntax = "proto3";
option ruby_package = "Protos::";

/*
GET proto/tasks タスク一覧の取得
*/
// message FetchTasksRequest {}

message FetchTasksResponse {
  Tasks tasks = 1;
}

/*
GET proto/task/:id タスク一件の取得
*/
// message FetchTaskRequest {}

message FetchTaskResponse {
  Task task = 1;
}

/*
POST proto/tasks タスクの新規追加
*/
message CreateTaskRequest {
  string title = 1;
  string description = 2;
}

/*
タスクの新規追加成功・失敗時のレスポンス
Scuccess Status: 201
Error Status: 400...
*/
message CreateTaskResponse {
  Status status = 1;
}

message Tasks {
  repeated Task task = 1;
}

message Task {
  int32 id = 1;
  string title = 2;
  string description = 3;
}

message Status {
  int32 code = 1;
  string message = 2;
}
```

細かく見ていきます。まずは `GET proto/tasks タスク一覧の取得` の部分。

task.proto

```
/*
GET proto/tasks タスク一覧の取得
*/
// message FetchTasksRequest {}

message FetchTasksResponse {
  Tasks tasks = 1;
}

message Tasks {
  repeated Task task = 1;
}

message Task {
  int32 id = 1;
  string title = 2;
  string description = 3;
}
```

この部分では、タスク一覧取得の index アクションについて定義しています。  
リクエスト時は、エンドポイントをそのまま叩くだけなので、データの内容を定義しません(定義しないことを明確にするためにコメントアウトしています)。  
レスポンスとして、 id, title, description を持つ、 Task 型のデータを複数持つ Tasks 型の値を返します(ややこしい)。

次に、`GET proto/task/:id タスク一件の取得` の部分。

task.proto

```
/*
GET proto/task/:id タスク一件の取得
*/
// message FetchTaskRequest {}

message FetchTaskResponse {
  Task task = 1;
}
```

指定した id を持つタスク一件を取得する show アクションに対する定義です。  
リクエスト時は、 id に対するエンドポイントをそのまま叩くだけなので、データの内容を定義しません。 ([注釈2](https://qiita.com/t0yohei/private/8b04bd427767c8df7465#%E6%B3%A8%E9%87%882))  
レスポンスとして、 id, title, description を持つ、 Task 型の値を返します。

最後に、`POST proto/tasks タスクの新規追加` の部分。

task.proto

```
/*
POST proto/tasks タスクの新規追加
*/
message CreateTaskRequest {
  string title = 1;
  string description = 2;
}

/*
タスクの新規追加成功・失敗時のレスポンス
Scuccess Status: 201
Error Status: 400...
*/
message CreateTaskResponse {
  Status status = 1;
}

message Status {
  int32 code = 1;
  string message = 2;
}
```

タスクの新規追加の create アクションの定義です。  
リクエストのデータとして、 title と description を送るように定義します。  
また、レスポンスとしては、成功・失敗などのステータスを返却するように定義します。

### [](#%E3%82%B9%E3%82%AD%E3%83%BC%E3%83%9E%E5%AE%9A%E7%BE%A9%E3%82%92%E5%85%83%E3%81%AB%E5%AE%9F%E8%A3%85%E3%81%A7%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E8%87%AA%E5%8B%95%E7%94%9F%E6%88%90)スキーマ定義を元に、実装で使用するコードを自動生成

上記で作成した proto ファイルの定義を元に、実際の実装で使用するコードを自動生成します。

```
$  protoc --ruby_out=./lib proto/task.proto
```

自動生成される内容がこちら。

lib/proto/task\_pb.rb

```
# Generated by the protocol buffer compiler.  DO NOT EDIT!
# source: proto/task.proto

require 'google/protobuf'

Google::Protobuf::DescriptorPool.generated_pool.build do
  add_file("proto/task.proto", :syntax => :proto3) do
    add_message "FetchTasksResponse" do
      optional :tasks, :message, 1, "Tasks"
    end
    add_message "FetchTaskResponse" do
      optional :task, :message, 1, "Task"
    end
    add_message "CreateTaskRequest" do
      optional :title, :string, 1
      optional :description, :string, 2
    end
    add_message "CreateTaskResponse" do
      optional :status, :message, 1, "Status"
    end
    add_message "Tasks" do
      repeated :task, :message, 1, "Task"
    end
    add_message "Task" do
      optional :id, :int32, 1
      optional :title, :string, 2
      optional :description, :string, 3
    end
    add_message "Status" do
      optional :code, :int32, 1
      optional :message, :string, 2
    end
  end
end

module Protos
  FetchTasksResponse = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("FetchTasksResponse").msgclass
  FetchTaskResponse = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("FetchTaskResponse").msgclass
  CreateTaskRequest = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("CreateTaskRequest").msgclass
  CreateTaskResponse = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("CreateTaskResponse").msgclass
  Tasks = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("Tasks").msgclass
  Task = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("Task").msgclass
  Status = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("Status").msgclass
end
```

クラス定義わかりにくい...って思うかもしれませんが、その話はここではしない。

### [](#%E8%87%AA%E5%8B%95%E7%94%9F%E6%88%90%E3%81%95%E3%82%8C%E3%81%9F%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%9F%E5%90%84%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E5%AE%9F%E8%A3%85)自動生成されたコードを使用した各アクションの実装

次に、自動生成されたコードを活用してコントローラーを実装していきます。

app/controllers/proto/tasks\_controller.rb

```
class Proto::TasksController < ApplicationController
  def index
    tasks = Task.all
    tasks_proto = Protos::Tasks.new
    tasks.each do |each_task|
      task_proto = Protos::Task.new(
        id: each_task.id,
        title: each_task.title,
        description: each_task.description
      )
      tasks_proto.task.push(task_proto)
    end

    response = Protos::FetchTasksResponse.new(tasks: tasks_proto)
    response_encoded_data = Protos::FetchTasksResponse.encode(response)
    render plain: response_encoded_data
  end

  def show
    task = Task.find(params[:id])
    task_proto = Protos::Task.new(
      id: task.id,
      title: task.title,
      description: task.description
    )
    response = Protos::FetchTaskResponse.new(task: task_proto)
    response_encoded_data = Protos::FetchTaskResponse.encode(response)
    render plain: response_encoded_data
  end

  def create
    decoded_data = Protos::CreateTaskRequest.decode(request.raw_post)
    task = Task.new(title: decoded_data.title, description: decoded_data.description)
    if task.save
      status = Protos::Status.new(code: 201, message: "#{task.title}を作成しました。")
      render plain: build_create_message_encoded(status: status)
    else
      status = Protos::Status.new(code: 400, message: "#{task.title}の作成に失敗しました。")
      render plain: build_create_message_encoded(status: status)
    end
  end

  private

  def build_create_message_encoded(status:)
    message = Protos::CreateTaskResponse.new(status: status)
    Protos::CreateTaskResponse.encode(message)
  end
end
```

こちらも細かく見ていきます。

#### [](#show)show

順番が少し前後しますが、まずは簡単な show アクションから。

app/controllers/proto/tasks\_controller.rb

```
  def show
    task = Task.find(params[:id])
    task_proto = Protos::Task.new(
      id: task.id,
      title: task.title,
      description: task.description
    )
    response = Protos::FetchTaskResponse.new(task: task_proto)
    response_encoded_data = Protos::FetchTaskResponse.encode(response)
    render plain: response_encoded_data
  end
```

指定された id に合致するタスクを一件取得して、 自動生成で作成された `Protos::Task` クラスのインスタンスオブジェクトにタスクのデータを詰め替えます。  
詰め替え後のデータを元に、レスポンスオブジェクトを作成し、 `encode` メソッドでレスポンスオブジェクトをシリアライズします。そして最後に、シリアライズされたデータを返却します。  
せっかくなので、各部分でデータがどう変化していくかをコメントに書いてみました。

```
task = Task.create(id: 1, title: 'title', description: 'description') # => #<Task id: 1, title: "title", created_at: "2019-12-17 08:30:09", updated_at: "2019-12-17 08:30:09", description: "description">
task_proto = Protos::Task.new(
  id: task.id,
  title: task.title,
  description: task.description
) # => <Protos::Task: id: 1, title: "title", description: "description">
response = Protos::FetchTaskResponse.new(task: task_proto) # => <Protos::FetchTaskResponse: task: <Protos::Task: id: 1, title: "title", description: "description">>
response_encoded_data = Protos::FetchTaskResponse.encode(response) # => "\n\x16\b\x01\x12\x05title\x1A\vdescription"
```

最終的には、 `"\n\x16\b\x01\x12\x05title\x1A\vdescription"` のような文字列がレスポンスとして返されることになります。  
このデータの呼び方は特に定義されていないのですが、 (protobuf の)シリアライズデータと呼ぶことにします。  
このシリアライズデータには、型情報や実際の値が含まれているので、同じ proto ファイルのスキーマ定義を元に自動生成されたクラス定義(多言語でも可)を使用することで、受け取り先で decode することができます。

もちろん、 encode したシリアライズデータをそのまま decode することもできます(実際の開発でやることは少ないと思いますが)。

```
response_encoded_data = Protos::FetchTaskResponse.encode(response)
decoded_data = Protos::FetchTaskResponse.decode(response_encoded_data) # =><Protos::FetchTaskResponse: task: <Protos::Task: id: 1, title: "title", description: "description">>
```

また、それでも json が好きだ！という場合は、 json に変換することも可能です。

```
response = Protos::FetchTaskResponse.new(task: task_proto)
Protos::FetchTaskResponse.encode_json(response) # => "{\"task\":{\"id\":1,\"title\":\"title\",\"description\":\"description\"}}"
```

#### [](#index)index

次に index アクションについて見ていきます。

app/controllers/proto/tasks\_controller.rb

```
  def index
    tasks = Task.all
    tasks_proto = Protos::Tasks.new
    tasks.each do |each_task|
      task_proto = Protos::Task.new(
        id: each_task.id,
        title: each_task.title,
        description: each_task.description
      )
      tasks_proto.task.push(task_proto)
    end

    response = Protos::FetchTasksResponse.new(tasks: tasks_proto)
    response_encoded_data = Protos::FetchTasksResponse.encode(response)
    render plain: response_encoded_data
  end
```

show アクションとの違いは特にないのですが、データの詰め替えの部分の処理が長ったらしくなってしまっています... この辺は要リファクタリングです。

#### [](#create)create

最後に、 create アクションです。

app/controllers/proto/tasks\_controller.rb

```
  def create
    decoded_data = Protos::CreateTaskRequest.decode(request.raw_post)
    task = Task.new(title: decoded_data.title, description: decoded_data.description)
    if task.save
      status = Protos::Status.new(code: 201, message: "#{task.title}を作成しました。")
      render plain: build_create_message_encoded(status: status)
    else
      status = Protos::Status.new(code: 400, message: "#{task.title}の作成に失敗しました。")
      render plain: build_create_message_encoded(status: status)
    end
  end

  private

  def build_create_message_encoded(status:)
    message = Protos::CreateTaskResponse.new(status: status)
    Protos::CreateTaskResponse.encode(message)
  end
```

`request.raw_post` で post で送られてくる(protobuf の)シリアライズデータを取得して、デコードします。デコード後は通常の create 処理と同様に save してあげて、レスポンスデータを show メソッドなどと同様の方法で作成し、レスポンスを返してあげます。

## [](#%E3%81%BE%E3%81%A8%E3%82%81)まとめ

ひとまず、 proto ファイルでスキーマを定義してからレスポンスを返す実装を一通り紹介できたかなと思います。最後に、 protobuf を使ったスキーマ駆動開発って何がいいの？または何が悪いの？って点について個人的な感想を書きたいと思います。

### [](#%E8%89%AF%E3%81%84%E7%82%B9)良い点

-   簡素で明瞭なスキーマ定義が書ける。
-   自動生成するコードを元に実装を進めるので、スキーマ定義と実装が基本的に一致する。
-   スキーマを先に決めて開発するので、フロントエンド・バックエンドなどの実装で認識の齟齬が起きにくい。
-   スキーマ定義とそれを元に自動生成されるコードがあるので、バックエンドの実装を待たずに、フロントエンドでモックを作ったりテストを書いたりできる。

### [](#%E6%82%AA%E3%81%84%E7%82%B9)悪い点

-   自動生成されるコードに依存する実装になるので、やめたい時に引き剥がずのが一手間かかる。
-   protobuf のドキュメントや実装サンプルが豊富ではないので、学習がちょっとやりにくい。

実際に業務に適用できるかどうかは、チームや組織の状況によりそうです。個人的には JSON や YAML を使ったスキーマ定義よりも protobuf の方が見やすいと思うので、スキーマ駆動開発をする際は是非とも使いたいという気持ちです。

ひとまず本編は以上になります。最後まで読んでいただきありがとうございました。

---

## [](#%E4%BB%98%E9%8C%B21-%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB%E3%81%AE%E3%82%88%E3%81%86%E3%81%AA%E3%82%82%E3%81%AE)付録1 チュートリアルのようなもの

この付録1では、実際に protobuf を使った rails の RESTful API を作成していく手順について書いていこうと思います。  
本編と同様に、フロントエンドをSPAで実装することを想定した TODO リストを想定して開発を進めていきます。railsの基本的な環境構築は省略しますが、自分は下記の手順で構築しましたので必要に応じて参考にしてください。  
[https://qiita.com/t0yohei/items/9f3a418d3ea61e090be3](https://qiita.com/t0yohei/items/9f3a418d3ea61e090be3)

また、実装全体やテスト、 Nuxt.js の SPA への適用法などの詳細が知りたい場合は、下記のリポジトリを参照いただければと思います。  
[https://github.com/t0yohei/rails-nuxt-protobuf-todo](https://github.com/t0yohei/rails-nuxt-protobuf-todo)

では、早速進めていきましょう。

### [](#%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89)環境構築

今回は下記の環境で動作することを確認しています。

#### [](#protoc-%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)protoc のインストール

proto ファイルから実装コードを自動生成する際に、 `protoc` というライブラリが必要になるのでインストールします。

#### [](#protobuf-%E7%94%A8%E3%81%AE-gem-%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)protobuf 用の gem のインストール

今回は、 google に開発が主導されている [google-protobuf](https://github.com/protocolbuffers/protobuf/tree/master/ruby) という gem を使用します。  
gem の選定に関しては、[付録2 gem の選定](https://qiita.com/t0yohei/private/8b04bd427767c8df7465#%E4%BB%98%E9%8C%B22-gem-%E3%81%AE%E9%81%B8%E5%AE%9A)の章で話そうと思います。

Gemfile に `gem 'google-protobuf'` を追記して、 `bundle install` を実行してください。

#### [](#model-migration-%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E4%BD%9C%E6%88%90)model, migration ファイルの作成

まずは model, migration ファイルを作成して、タイトルと詳細だけを保持するシンプルなタスクテーブルを作成します。

```
$ bundle exec rails g model Task title:string description:string
```

validation の追加

app/models/task.rb

```
class Task < ApplicationRecord
  validates :title, presence: true, length: { in: 1..25 }
  validates :description, length: { maximum: 100 }
end
```

各種制約の追加

db/migrate/20190000000000\_create\_tasks.rb

```
class CreateTasks < ActiveRecord::Migration[6.0]
  def change
    create_table :tasks do |t|
      t.string :title, :string, null: false, default: '', limit: 25
      t.string :description, :string, limit: 100

      t.timestamps
    end
  end
end
```

migration を実行しましょう。

```
$ bundle exec rails db:migrate
```

続いて、開発がしやすいように seed データを準備します。

db/seeds.rb

```
Task.create(title: '大掃除', description: '年末なので気だるいけど大掃除をする')
Task.create(title: 'advent calendar を書く', description: 'そろそろ書き始めないとヤバイ')
```

忘れずデータを投入しておきましょう。

```
$ bundle exec rails db:seed
```

#### [](#proto-%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%A7%E3%81%AE-%E3%82%B9%E3%82%AD%E3%83%BC%E3%83%9E%E3%81%AE%E5%AE%9A%E7%BE%A9)proto ファイルでの スキーマの定義

次に、システム間でどういうデータ形式でデータをやりとりするのかを、 proto ファイルを使用して定義します。  
今回は RESTful な API なので、 Request/Response をそれぞれ定義しています。

proto/task.proto

```
/*
command: protoc --ruby_out=./lib proto/task.proto
*/

syntax = "proto3";
option ruby_package = "Protos::";

/*
GET proto/tasks タスク一覧の取得
*/
// message FetchTasksRequest {}

message FetchTasksResponse {
  Tasks tasks = 1;
}

/*
GET proto/task/:id タスク一件の取得
*/
// message FetchTaskRequest {}

message FetchTaskResponse {
  Task task = 1;
}

/*
POST proto/tasks タスクの新規追加
*/
message CreateTaskRequest {
  string title = 1;
  string description = 2;
}

/*
タスクの新規追加成功・失敗時のレスポンス
Scuccess Status: 201
Error Status: 400...
*/
message CreateTaskResponse {
  Status status = 1;
}

message Tasks {
  repeated Task task = 1;
}

message Task {
  int32 id = 1;
  string title = 2;
  string description = 3;
}

message Status {
  int32 code = 1;
  string message = 2;
}
```

`option ruby_package = "Protos::";` の部分で、自動生成するクラスのモジュールを定義しています。こうしておくことで、 ActiveRecord の Task クラスとの名前衝突を回避することができます。

続いて、実装で使用するコードの自動生成します。

```
$  protoc --ruby_out=./lib proto/task.proto
```

下記のようなコードが生成されているかと思います。

lib/proto/task\_pb.rb

```
# Generated by the protocol buffer compiler.  DO NOT EDIT!
# source: proto/task.proto

require 'google/protobuf'

Google::Protobuf::DescriptorPool.generated_pool.build do
  add_file("proto/task.proto", :syntax => :proto3) do
    add_message "FetchTasksResponse" do
      optional :tasks, :message, 1, "Tasks"
    end
    add_message "FetchTaskResponse" do
      optional :task, :message, 1, "Task"
    end
    add_message "CreateTaskRequest" do
      optional :title, :string, 1
      optional :description, :string, 2
    end
    add_message "CreateTaskResponse" do
      optional :status, :message, 1, "Status"
    end
    add_message "Tasks" do
      repeated :task, :message, 1, "Task"
    end
    add_message "Task" do
      optional :id, :int32, 1
      optional :title, :string, 2
      optional :description, :string, 3
    end
    add_message "Status" do
      optional :code, :int32, 1
      optional :message, :string, 2
    end
  end
end

module Protos
  FetchTasksResponse = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("FetchTasksResponse").msgclass
  FetchTaskResponse = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("FetchTaskResponse").msgclass
  CreateTaskRequest = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("CreateTaskRequest").msgclass
  CreateTaskResponse = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("CreateTaskResponse").msgclass
  Tasks = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("Tasks").msgclass
  Task = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("Task").msgclass
  Status = ::Google::Protobuf::DescriptorPool.generated_pool.lookup("Status").msgclass
end
```

lib 配下に 自動生成されるファイルを置くようにしたので、下記を追記してちゃんと読み込まれるようにします。

config/application.rb

```
Dir["#{Rails.root}/lib/proto/*.rb"].each { |file| require file }
```

#### [](#routerb-%E3%81%A7-api-%E3%81%AE%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%82%92%E5%AE%9A%E7%BE%A9)route.rb で API のエンドポイントを定義

route.rb

```
Rails.application.routes.draw do
  namespace :proto do
    resources :tasks
  end
end
```

### [](#controller-%E3%81%AE%E5%AE%9F%E8%A3%85)controller の実装

app/controllers/proto/tasks\_controller.rb

```
class Proto::TasksController < ApplicationController
  def index
    tasks = Task.all
    tasks_proto = Protos::Tasks.new
    tasks.each do |each_task|
      task_proto = Protos::Task.new(
        id: each_task.id,
        title: each_task.title,
        description: each_task.description
      )
      tasks_proto.task.push(task_proto)
    end

    response = Protos::FetchTasksResponse.new(tasks: tasks_proto)
    response_encoded_data = Protos::FetchTasksResponse.encode(response)
    render plain: response_encoded_data
  end

  def show
    task = Task.find(params[:id])
    task_proto = Protos::Task.new(
      id: task.id,
      title: task.title,
      description: task.description
    )
    response = Protos::FetchTaskResponse.new(task: task_proto)
    response_encoded_data = Protos::FetchTaskResponse.encode(response)
    render plain: response_encoded_data
  end

  def create
    decoded_data = Protos::CreateTaskRequest.decode(request.raw_post)
    task = Task.new(title: decoded_data.title, description: decoded_data.description)
    if task.save
      status = Protos::Status.new(code: 201, message: "#{task.title}を作成しました。")
      render plain: build_create_message_encoded(status: status)
    else
      status = Protos::Status.new(code: 400, message: "#{task.title}の作成に失敗しました。")
      render plain: build_create_message_encoded(status: status)
    end
  end

  private

  def build_create_message_encoded(status:)
    message = Protos::CreateTaskResponse.new(status: status)
    Protos::CreateTaskResponse.encode(message)
  end
end
```

ちゃんと動くか動作確認をします。

`http://localhost:3000/proto/tasks` を見てみると、文字化けしたものが出力されていると思います。(自分の環境ではゴミデータも混ざっているので、写真の通りには出てこないと思いますが悪しからず)

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F152779%2F15992384-96bc-7cdf-8ade-f2f577df5a8c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7896438d58e775e98d6436e267c2afcb)](https://camo.qiitausercontent.com/5df60d3527bce2cce9cc3ea84fe4168c3d3431b1/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f3135323737392f31353939323338342d393662632d376364662d386164652d6632663537376466356138632e706e67)

#### [](#%E3%83%86%E3%82%B9%E3%83%88%E3%81%AE%E4%BD%9C%E6%88%90)テストの作成

長くなりそうだったので、 index アクションのテストだけを記載しました。他のアクションのテストについては、[こちらのリポジトリ](https://github.com/t0yohei/rails-nuxt-protobuf-todo/blob/master/backend/spec/requests/proto/tasks_spec.rb)をご確認ください。

spec/requests/proto/tasks\_spec.rb

```
require 'rails_helper'

RSpec.describe "Proto::Tasks", type: :request do
  describe "GET /proto/tasks" do
    subject { get proto_tasks_path }
    it "特に条件を指定しなくても、200を返すこと" do
      subject
      expect(response).to have_http_status(200)
    end

    context "taskが1件も存在しないとき" do
      it "空の encoded_data が返却されること" do
        subject
        expect(response).to have_http_status(200)
        decoded_response = Protos::FetchTasksResponse.decode(response.body)
        expect(decoded_response.tasks.task).to be_empty
      end
    end

    context "taskが1件存在するとき" do
      let!(:task) { Task.create(id: 1, title: 'title', description: 'description') }
      it "task1件分の encoded_data が返却されること" do
        subject
        decoded_response = Protos::FetchTasksResponse.decode(response.body)
        expect(decoded_response.tasks.task.count).to eq(1)
        expect(decoded_response.tasks.task.first.id).to eq(task.id)
        expect(decoded_response.tasks.task.first.title).to eq(task.title)
        expect(decoded_response.tasks.task.first.description).to eq(task.description)
      end
    end

    context "taskが2件存在するとき" do
      let!(:task1) { Task.create(id: 1, title: 'title1', description: 'description1') }
      let!(:task2) { Task.create(id: 2, title: 'title2', description: 'description2') }
      it "task2件分の encoded_data が返却されること" do
        subject
        decoded_response = Protos::FetchTasksResponse.decode(response.body)
        expect(decoded_response.tasks.task.count).to eq(2)
        expect(decoded_response.tasks.task.first.id).to eq(task1.id)
        expect(decoded_response.tasks.task.first.title).to eq(task1.title)
        expect(decoded_response.tasks.task.first.description).to eq(task1.description)
      end
    end
  end
end
```

### [](#index-show-%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%82%92%E3%83%AA%E3%83%95%E3%82%A1%E3%82%AF%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0)index, show メソッドをリファクタリング

index アクションにコードが長々とあるのでリファクタリングをします。

app/controllers/proto/tasks\_controller.rb

```
  def index
    response = Protos::FetchTasksResponse.new(tasks: Task.convert_all_to_message_object)
    response_encoded_data = Protos::FetchTasksResponse.encode(response)
    render plain: response_encoded_data, status: :ok
  end

  def show
    task = Task.find(params[:id])
    response = Protos::FetchTaskResponse.new(task: task.convert_to_message_object)
    response_encoded_data = Protos::FetchTaskResponse.encode(response)
    render plain: response_encoded_data, status: :ok
  end
```

app/models/task.rb

```
class Task < ApplicationRecord

  def self.convert_all_to_message_object
    Protos::Tasks.new(
      task: Task.all.map {
        |task| task.convert_to_message_object
      }
    )
  end

  def convert_to_message_object
    Protos::Task.new(
      id: self.id,
      title: self.title,
      description: self.description
    )
  end
end
```

リファクタリングについては、こちらの記事を参考にさせてもらいました。  
[Building APIs with Rails + Protocol Buffers](http://leohetsch.com/using-protocol-buffers-in-your-apis/)

これで、一通りの実装が作成できたかと思います。お疲れ様でした。  
update, destroy アクションの実装が気になる場合は、こちらをご覧ください。  
[https://github.com/t0yohei/rails-nuxt-protobuf-todo/blob/master/backend/app/controllers/proto/tasks\_controller.rb](https://github.com/t0yohei/rails-nuxt-protobuf-todo/blob/master/backend/app/controllers/proto/tasks_controller.rb)

## [](#%E4%BB%98%E9%8C%B22-gem-%E3%81%AE%E9%81%B8%E5%AE%9A)付録2 gem の選定

今回の実装では、 google が公式に出している、 [google-protobuf](https://github.com/protocolbuffers/protobuf/tree/master/ruby) を使用しました。  
ruby で protobuf を使用する際に導入できそうな gem としては他にも、 [protobuf](https://github.com/ruby-protobuf/protobuf) などがあります。(ややこしいですがgoogleの公式の方に `google` という prefix がわざわざついています。ややこしい。)  
後者の gem も今でもメンテナンスされており、使用しても問題ないように思います。  
(なぜ複数の active な gem が存在しているのかということはちゃんとは調べていないのですが、公式の protobuf の ruby ライブラリ(gem)より先に、有志の gem が作られたのかなー？と勝手に予想しています。現に [proto2 の時点では ruby で protobuf を扱うことができなかった](https://developers.google.com/protocol-buffers/docs/proto#whats-generated-from-your-.proto-)ようです(現在は proto3 がデフォルトなので1つ前のバージョンの時)。)

どちらの gem を選択するかは難しいところですが、自分が知っている限りでのそれぞれの特徴を書いておこうと思います。

### [](#google-protobuf)google-protobuf

リポジトリ: [google-protobuf](https://github.com/protocolbuffers/protobuf/tree/master/ruby)

-   google が公開しているものなので、ちゃんとメンテナンスされ続けそう。
-   gem の実装に Java とか C などのコードが使われている。
-   自動生成される ruby のコードがわかりにくい。

### [](#protobuf)protobuf

リポジトリ: [protobuf](https://github.com/ruby-protobuf/protobuf)

-   メンテナンスは今でもされていそう(2019年12月時点)。
-   ruby で実装されている(重要)。
-   自動生成される ruby のコードが[シンプルでわかりやすい](https://github.com/ruby-protobuf/protobuf/wiki/Messages-&-Enums#messages)。
-   [protobuf-activerecord](https://github.com/liveh2o/protobuf-activerecord) を使う際に必須になる。

### [](#%E3%81%BE%E3%81%A8%E3%82%81-1)まとめ

個人で趣味として使うのであれば [protobuf](https://github.com/ruby-protobuf/protobuf) の方が、使いやすそうだなーと個人的に思っています。 `protobuf-activerecord` を使って、実装をシンプルにできそうだし。業務で使う場合にどちらを使用するかは、皆さんでご判断ください。

## [](#%E4%BB%98%E9%8C%B23-%E4%BE%8B%E5%A4%96%E5%87%A6%E7%90%86)付録3 例外処理

protobuf で自動生成されたコードを使って実装を進める場合、レスポンスは proto ファイルで定義した response の型で返してあげる必要があります。そのため、 `ApplicationController` に全メソッド共通の例外処理などを簡単には書くことはできません。そのため、個人的には下記のような形式が `まだ` 綺麗な方じゃないかなーと思っています。

app/controllers/proto/tasks\_controller.rb

```
  def show
    task = Task.find(params[:id])
    response = Protos::FetchTaskResponse.new(task: task.convert_to_message_object)
  rescue => e
    response = Protos::Status.new(code: 500, message: e.message)
  ensure
    response_encoded_data = Protos::FetchTaskResponse.encode(response)
    render plain: response_encoded_data, status: :ok
  end
```

## [](#%E6%B3%A8%E9%87%88)注釈

### [](#%E6%B3%A8%E9%87%881)注釈1

[protoc-gen-swagger](https://github.com/grpc-ecosystem/grpc-gateway/tree/master/protoc-gen-swagger) というライブラリを介して、 protobuf の定義から OpenAPI 定義ファイルを生成することも可能なようです。

### [](#%E6%B3%A8%E9%87%882)注釈2

show アクションのリクエスト関しては、

```
/*
GET proto/tasks タスク一件の取得
*/
message FetchTaskRequest {
  int32 id = 1;
}
```

と表現することもできるのですが、rails の show アクションのデフォルト定義に合わせて、今回は `GET proto/task/:id` で取得することにしました。

## [](#%E5%8F%82%E8%80%83%E6%96%87%E7%8C%AE%E3%81%AA%E3%81%A9)参考文献など

[今さらProtocol Buffersと、手に馴染む道具の話](https://qiita.com/yugui/items/160737021d25d761b353)  
[WEB+DB PRESS Vol.108 スキーマ駆動Web API開発](https://gihyo.jp/magazine/wdpress/archive/2019/vol108)  
[Web API に秩序を与える Protocol Buffers](https://speakerdeck.com/south37/protocol-buffers-for-web-api-number-builderscon)  
[Protocol Buffers | Google Developers](https://developers.google.com/protocol-buffers)