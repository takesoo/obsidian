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


```typescript
import { Controller、 } from '@nestjs/common'；

// @xxxはデコレーター
@Controller('cats')uid="873">) // @Controllerで/catsにルーティングを指定。
export class CatsController {
	@Get() findAll() { // @Getでハンドラーを指定。GET /catsリクエストをマップする
		return『このアクションはすべての猫を返します』；
	}
} 
```

