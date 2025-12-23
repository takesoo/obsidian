---
tags:
  - npm
---
## what
- 実行時バリデーション＋型推論・型生成ライブラリ
- 
## how
```js
import { z } from "zod";

// creating a schema for strings
const mySchema = z.string();

// parsing
mySchema.parse("tuna"); // => "tuna"
mySchema.parse(12); // => throws ZodError

// "safe" parsing (doesn't throw error if validation fails)
mySchema.safeParse("tuna"); // => { success: true; data: "tuna" }
mySchema.safeParse(12); // => { success: false; error: ZodError }

// creating a object schema
const User = z.object({
  username: z.string(),
});

User.parse({ username: "Ludwig" });

// extract the inferred type
type User = z.infer<typeof User>;
// { username: string }
```
### サーバーサイドのZodとのE2E型安全
- サーバーサイドでZodを使った型生成をし、[[tRPC]]でクライアントサイドへ型を共有し、サーバーサイドとクライアントサイド間で型を共有できる。[[React Hook Form]]などに型を利用でき、型の差異をなくすことができる。