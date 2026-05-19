## what
- [[関数型プログラミング]]のResult型とチェーンのライブラリ
## why
- TypeScriptで[[関数型プログラミング]]のエラーハンドリングを楽に実装するため
## how
### Result
同期処理用のResult型
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
非同期処理用のResult型
```ts
import { ResultAsync } from 'neverthrow';

/**
 * 非同期でユーザーを取得する（Promiseを返すので ResultAsync に変換）
 * fromPromise()でラップする
 */
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
### 同期から非同期への連携
```ts
const registerUserWorkflow = (input: string) => {
  // 1. 最初は同期処理 (Result を返す)
  return validateInput(input) 
    // 2. 途中で非同期処理を繋ぐと、自動的にここから「ResultAsync」に化ける
    .asyncAndThen(validData => saveToDatabase(validData)); 
};

// 最終的には非同期処理になるので、呼び出し側は await する
const finalResult = await registerUserWorkflow("raw_input_data");
```
### Result.map
```ts
/**
 * Result<T, E>をResult<U, E>に、成功時の値の型を変換する
 * string型のTをnumber型のUに変換
 */
ok("1").map(v => Number(v))
```
### Result.andThen
```ts
/**
 * andThen<U, F>(callback: (value: T) => Result<U, F>): Result<U, E | F> { ... }
 * okの場合にのみ実行される
 * 引数に渡す関数がResultを返す必要がある
 */

const double = v => ok(v + v)

ok(2).andThen(double).andThen(double) // Ok(8)
err(2).andThen(err).andThen(double) // Err(2)
```
### Result.match
```ts
/**
 * okとerr両方の処理が定義できる
 * 戻り値はResultではなく返した値の型
 * システムの入り口で、okなら200レスポンス、errなら500レスポンスといった分岐に使う？
 */
class Result<T, E> {
  match<A>(
    okCallback: (value: T) => A,
    errorCallback: (error: E) => A,
  ): A => { ... }
}
```
### Result.fromThrowable
例外を投げる可能性がある関数と投げられた例外を変換する関数の二つを引数に取り、結果をResultで返す関数に変換する関数。
例外を投げる外部ライブラリをラップする時に使う。
### ResultAsync.fromPromise
例外を投げる可能性がある`Promise`を`ResultAsync`に変換する関数。