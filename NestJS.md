---
tags:
  - JavaScript
  - nodejs
---
効率的でスケーラブルな[[Node.js]]サーバーサイドアプリケーションを構築するためのフレームワーク
https://docs.nestjs.com/

- src
	- app.controller.spec.ts: コントローラテスト
	- app.controller.ts: ルートコントローラ
	- app.module.ts: ルートモジュール
	- app.service.ts: サービス
	- main.ts: エントリ

## コントローラ
入力されたリクエストを処理し、レスポンスをクライアントに返す責任を持つ
```ts
// src/cats/cats.controller.ts
import { Controller, Get, Post, Body, Bind, Dependencies } from '@nestjs/common';
import { CatsService } from './cats.service';

// @xxxはデコレーター
@Controller('cats') // @Controllerで/catsにルーティングを指定。
@Dependencies(CatsService)
export class CatsController {
  constructor(catsService) {
    this.catsService = catsService;
  }

  @Post()
  @Bind(Body())
  async create(createCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get() // @Getでハンドラーを指定。GET /catsリクエストをマップす
  async findAll() {
    return this.catsService.findAll();
  }
}
```
## プロバイダー
サービス、リポジトリ、ファクトリ、ヘルパーなどはプロバイダーをして扱われ、依存関係としてインジェクトできる。`@Injectable()`デコレーターを使用して宣言する。依存性を注入される側で`@Dependencies()`デコレーターを宣言する。
```typescript
// src/cats/cats.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  constructor() {
    this.cats = [];
  }

  create(cat) {
    this.cats.push(cat);
  }

  findAll() {
    return this.cats;
  }
}
```
インジェクションを実行できるようにするために、`app.module.ts`にプロバイダーを登録する
```ts
// app.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```
## モジュール
`@Module()`デコレーターでアノテーションされたクラス。`@Module()`はNestがアプリケーションの構造を整理するために利用するメタデータを提供する。
```typescript
// src/cats/cats.module.ts
// feature moduleとして定義する
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService] // CatsModuleをインポートしたモジュールはCatsServiceにアクセスできるようになり、同様にインポートしている他のモジュールと同じCatsServiceインスタンスを共有できるようになる
})
export class CatsModule {}

```
```typescript
// src/app.module.ts
// app.module.tsでインポートして登録する
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```
## ミドルウェア
ルートハンドラの前に呼び出される関数。リクエストとレスポンスオブジェクト、およびアプリケーションのリクエスト-レスポンスサイクルの`next()`ミドルウェア関数にアクセスできる。
## [[Data Transfer Object|DTO]]
NestではDTOにバリデーションデコレーター(`@IsString()`や`@IsInt()`など)を使用してリクエストのバリデーションを実装できる
```ts
import { IsString, IsNotEmpty } from 'class-validator';

export class CreateTodoDto {
  @IsString()
  @IsNotEmpty()
  title: string;

  @IsString()
  description: string;
}

```
ビジネスロジック層と分離することで依存関係を分離できる。
```ts
// todos.controller.ts
@Post()
createTodo(@Body() createTodoDto: CreateTodoDto) {
	return this.todosService.create(createTodoDto);
}
```
また、DTOクラスに型情報を持たせることで型安全性の確保になる。