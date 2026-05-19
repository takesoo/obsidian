## what
- `Result`
- `ResultAsync`: `Promise<Result<T, E>>`をラップした型。非同期処理の結果型
## why
- TypeScriptで[[関数型プログラミング]]のエラーハンドリングを楽に実装するため
## how
### Result
```ts
import { ok, err, Result } from 'neverthrow';

/** 
 * 同期的なバリデーション
 * ok()/err()を返す
 */
function validateAge(age: number): Result<number, string> {
  return age >= 0 ? ok(age) : err("年齢は0以上で入力してください");
}

/** 
 * その場で結果が決まるので、スムーズに次の処理へ流せる
 * isOk()/isErr()で判定する
 */
const result = validateAge(25)
  .map(age => age + 1); 

if (result.isOk()) {
  console.log(result.value); // 26
}
```

### ResultAsync
```ts
import { ResultAsync } from 'neverthrow';

// 非同期でユーザーを取得する（Promiseを返すので ResultAsync に変換）
function fetchUser(id: string): ResultAsync<User, Error> {
  return ResultAsync.fromPromise(
    fetch(`/api/users/${id}`).then(res => res.json()),
    () => new Error("通信に失敗しました")
  );
}

// メソッドチェーンの途中では await 不要！同期処理っぽく書ける
const userNameAsync = fetchUser("user-123")
  .map(user => user.name); // ここではまだ非同期の「予約」状態

// 最後に値を取り出したいタイミングで await する
const nameResult = await userNameAsync;
```