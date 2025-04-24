---
finished reading: false
favorite: false
tags:
  - clippings
---
# 4万行超のopenapi.yamlをTypeSpecに移行した話
  #ReadItLater 
 #ReadableArticle

## articleURL
https://zenn.dev/yuta_takahashi/articles/migrate-to-typespec

## siteName
Zenn

## date
2025-04-24

## articleContent
筆者の所属するチームでは OpenAPI を活用したスキーマ駆動開発を実践しています。  
この記事では、openapi.yaml から TypeSpec へ移行して得られた知見を紹介します。

前提として、今回 TypeSpec へ移行したリポジトリでは以下の技術スタックを利用していました。

| 名称 | バージョン | 役割 |
| --- | --- | --- |
| [OpenAPI](https://github.com/OAI/OpenAPI-Specification) | `3.0.0` | API スキーマの定義 |
| [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) | `6.6.0` | フロントエンドのクライアントコード生成 |
| [Committee](https://github.com/interagent/committee) | `5.5.3` | API レスポンスの実行時スキーマチェック |
| [Committee::Rails](https://github.com/willnet/committee-rails) | `0.8.0` | RSpec による API テスト |

## 4 万行超の openapi.yaml のツラミ

### ファイルが大きすぎて編集しづらい

OpenAPI が肥大化していくにつれて開発体験の低下を招いていました。  
GitHub Copilot や Cursor の Tab 機能で随分楽になりましたが、それでも 4 万行もあるファイルを手作業で入力するのは純粋に辛いです。

### AI Agent のコンテキストを大きく圧迫する

Cursor 等の AI Agent のコンテキストを大きく圧迫することが直近の開発で大きな課題となりました。  
具体的には次のような問題が発生したため、ファイルを分割することが急務となりました。

-   コンテキストが溢れて Cursor Agent が停止する
-   Longer Context を有効化すると処理できるようになるが、トークンを余分に消費してしまう

## TypeSpec とは

> TypeSpec は、API を記述するための Microsoft の強力で柔軟な言語です。 これにより、開発者は拡張可能でわかりやすい方法で API を定義できます。 TypeSpec を使用すると、堅牢なエミッター システムを使用して、API 定義から直接 API 仕様、クライアント コード、およびサーバー側コードを生成できます。  
> [TypeSpec の概要 - TypeSpec とは - TypeSpec | Microsoft Learn](https://learn.microsoft.com/ja-jp/azure/developer/typespec/overview) より引用

TypeSpec はエミッターと呼ばれる標準ライブラリを備えています。エミッターを使うことで、TypeSpec を SSoT として [OpenAPI3](https://typespec.io/docs/emitters/openapi3/reference/), [JSON Schema](https://typespec.io/docs/emitters/json-schema/reference/), [Protobuf](https://typespec.io/docs/emitters/protobuf/reference/) 等の様々なスキーマを自動生成することが可能です。

さらに、2025 年 4 月現在では、JavaScript, Java, Python, CSharp 向けのクライアントエミッターもプレビューとして公開されています。  
近い将来、TypeSpec から直接クライアントコードを生成することがスタンダードになるかもしれません。

## TypeSpec 移行に踏み切った理由

### 強力な DSL によるスケーラブルなスキーマ定義の実現

TypeSpec は、TypeScript によく似た文法で API スキーマを記述することが可能です。

拡張子は`.tsp` です。モジュール分割がサポートされているため、スケーラブルな定義を実現することができます。

address.tsp

```
model Address {
  street: string;
  city: string;
}
```

main.tsp

```
import "@typespec/http";
import "./address.tsp"

using Http;

model Store {
  name: string;
  address: Address;
}

@route("/stores")
interface Stores {
  list(@query filter: string): Store[];
  read(@path id: Store): Store;
}
```

### 撤退のしやすさ

もし 何らかの理由で TypeSpec から撤退することになっても、自動生成された openapi.yaml を直接利用する方針へ容易に転換することができます。  
この撤退のしやすさにより、短期間で移行に踏み切る意思決定ができました。

### TypeSpec 1.0.0-RC のリリース

2025 年 4 月上旬に TypeSpec 1.0.0-RC がリリースされました。

API の安定により、破壊的変更への追従リスクが低下したことで移行に踏み切る判断ができました。

## 移行ステップ 1. TypeSpec のセットアップ

[Installation | TypeSpec](https://typespec.io/docs/) に従ってセットアップします。

```
npm install -g @typespec/compiler
mkdir api-spec
cd api-spec
tsp init
```

今回は OpenAPI 3.0.0 からの移行だったため、`openapi-versions` に `3.0.0` を指定しました。

tspconfig.yaml

```
emit:
  - '@typespec/openapi3'
options:
  '@typespec/openapi3':
    emitter-output-dir: '{output-dir}'
    output-file: 'openapi.yaml'
    openapi-versions:
      - 3.0.0
output-dir: '{project-root}/schema'
```

複数の `openapi-versions` を指定することも可能です。詳しくは [Emitter usage | TypeSpec](https://typespec.io/docs/emitters/openapi3/reference/emitter/) を確認してください。

`tsp compile .` を実行すると `tspconfig.yaml` の内容で `schema/openapi.yaml` が自動生成されます。  
TypeSpec に文法誤りがあれば compile でエラーが発生します。

## 移行ステップ 2. OpenAPI から TypeSpec へのコンバート

[tsp-openapi3](https://typespec.io/docs/emitters/openapi3/cli/) を使って既存の openapi.yaml を 1 枚の TypeSpec ファイルに変換します。

既存の OpenAPI 定義が `openapi/openapi.yaml` に存在する場合、次のコマンドを実行することで `api-spec/main.tsp` にコンバートされます。

```
tsp-openapi3 openapi/openapi.yaml --output-dir ./api-spec
```

コンバートの結果、次の問題が発生したため順に対処していきました。

### 問題 1：一部の型が `unknown` に変換されてしまう

openapi.yaml で `type: object` の宣言が抜けていると TypeSpec への変換時に `unknown` になってしまいました。

対象箇所を TypeSpec の生成結果から洗い出し、yaml を修正しました。

```
components:
  schemas:
    Address:
+     type: object
      required:
        - street
        - city
      properties:
        street:
          type: string
        city:
          type: string
```

### 問題 2：namespace と model のバッティング

TypeSpec では同一の namespace 内で model と namespace の名前を重複することが禁止されています。

次のように namespace 記法で OpenAPI のスキーマを定義していた箇所で TypeSpec 変換後にエラーが発生しました。`Posts.Comment` のように、namespace を複数形にリネームすることで名前の衝突を回避しました。

```
components:
  schemas:
    Post: # TypeSpec の `model Post` に変換される
      type: object
      properties:
        id:
          type: integer
        title:
          type: string
-   Post.Comment:  # TypeSpec の `namespace Post { model Comment {} }` に変換される
+   Posts.Comment: # TypeSpec の `namespace Posts { model Comment {} }` に変換される
      type: object
      properties:
        id:
          type: integer
        text:
          type: string
        postId:
          type: integer
```

### 問題 3：インラインで`allOf`を使用したスキーマのプロパティが `unknown` となる

次のように `type: object` の `properties` 内で `allOf` を使用している箇所が TypeSpec への変換後に `unknown` になってしまいました。

```
openapi: 3.0.0
components:
  schemas:
    BaseObject:
      type: object
      properties:
        id:
          type: integer
        createdAt:
          type: string
          format: date-time
    ExtendedObject:
      type: object
      properties:
        # baseInfo プロパティが unknown に変換される
        baseInfo:
          allOf:
            - $ref: "#/components/schemas/BaseObject"
            - type: object
              properties:
                name:
                  type: string
        additionalProp:
          type: string
```

TypeSpec リポジトリをフォークして問題の箇所を修正しました。（後ほど PR 出したい）

92612b.patch

```
diff --git a/packages/openapi3/src/cli/actions/convert/generators/generate-types.ts b/packages/openapi3/src/cli/actions/convert/generators/generate-types.ts
index 09db960..34c20ca 100644
--- a/packages/openapi3/src/cli/actions/convert/generators/generate-types.ts
+++ b/packages/openapi3/src/cli/actions/convert/generators/generate-types.ts
@@ -1,8 +1,9 @@
-import { printIdentifier } from "@typespec/compiler";
 import { OpenAPI3Schema, Refable } from "../../../../types.js";
+
+import { generateDecorators } from "./generate-decorators.js";
 import { getDecoratorsForSchema } from "../utils/decorators.js";
 import { getScopeAndName } from "../utils/get-scope-and-name.js";
-import { generateDecorators } from "./generate-decorators.js";
+import { printIdentifier } from "@typespec/compiler";

 export class SchemaToExpressionGenerator {
   constructor(public rootNamespace: string) {}
@@ -79,6 +80,8 @@ export class SchemaToExpressionGenerator {
       type = getEnum(schema.enum);
     } else if (schema.anyOf) {
       type = this.getAnyOfType(schema, callingScope);
+    } else if (schema.allOf) {
+      type = this.getAllOfType(schema, callingScope);
     } else if (schema.type === "array") {
       type = this.generateArrayType(schema, callingScope);
     } else if (schema.type === "boolean") {
@@ -106,6 +109,43 @@ export class SchemaToExpressionGenerator {
     return type;
   }

+  private getAllOfType(schema: OpenAPI3Schema, callingScope: string[]): string {
+    const requiredProps: string[] = schema.required || [];
+    let properties: Record<string, Refable<OpenAPI3Schema>> = {};
+    const baseTypes: string[] = [];
+
+    for (const member of schema.allOf || []) {
+      if ("$ref" in member) {
+        // If it's a $ref, process it as inheritance/extension and add to the list
+        baseTypes.push(this.getRefName(member.$ref, callingScope));
+      } else if (member.properties) {
+        properties = { ...properties, ...member.properties };
+        if (member.required) {
+          requiredProps.push(...member.required);
+        }
+      }
+    }
+
+    const props: string[] = [];
+    for (const name of Object.keys(properties)) {
+      const isOptional = !requiredProps.includes(name) ? "?" : "";
+      props.push(
+        `${printIdentifier(name)}${isOptional}: ${this.generateTypeFromRefableSchema(properties[name], callingScope)}`
+      );
+    }
+
+    if (baseTypes.length > 0 && props.length > 0) {
+      // When there are both inherited types and properties
+      return `${baseTypes.join(" & ")} & {${props.join("; ")}}`;
+    } else if (baseTypes.length > 0) {
+      // When there are only inherited types
+      return baseTypes.join(" & ");
+    } else {
+      // When there are only properties
+      return `{${props.join("; ")}}`;
+    }
+  }
+
   private getAnyOfType(schema: OpenAPI3Schema, callingScope: string[]): string {
     const definitions: string[] = [];
```

修正後はローカルでビルドした `tsp-openapi3` コマンドを利用しました。

```
# typespecをローカルでビルド
cd typespec
pnpm install
pnpm build

# ローカルでビルドしたtsp-openapi3を実行
node ~/path-to/typespec/packages/openapi3/cmd/tsp-openapi3.js openapi/openapi.yaml --output-dir ./api-spec
```

## 移行ステップ 3. TypeSpec への移行

元々手作業で書いていた openapi.yaml を TypeSpec から自動生成した openapi.yaml に置き換えるワークフローに変更しました。  
移行前と同様のパスに openapi.yaml を出力することで、各種ツールチェーンの設定を変更せずに使い続けることができました。

自動生成した openapi.yaml を元に OpenAPI Generator で TypeScript コードを生成するとインラインで定義されている一部のモデルの型がリネームされたため、気合で TypeScript 側の import 文を修正しました。

## 移行ステップ 4. フロントエンドコードの型チェックとバックエンドの API テストの差分確認

最後に、フロントエンドの `tsc` とバックエンドの `rspec` が通ることを確認しました。

OpenAPI Generator が出力する TypeScript コードで `Array<number | null>` を期待する箇所が `Array<number>` になってしまっていたため調整しました。

### `Array<number | null>` 型の調整

冒頭で紹介した環境では OpenAPI Generator が生成する TypeScript コード上で `Array<number | null>` を表現するときは、次のようにスキーマを定義する必要がありました。（※ ライブラリのバージョンによって挙動差分あります）

```
openapi: 3.0.0
components:
  schemas:
    BaseObject:
      type: object
      properties:
        nullableNumberArrayData:
          type: array
          # `oneOf` と `nullable: true` を組み合わせると TypeScript で `Array<number | null>` が出力される
          items:
            oneOf:
              - type: number
            nullable: true
```

TypeSpec 上で TypeScript の `Array<number | null>` を期待するとき、素朴に書くと次のようになります。

```
model BaseObject {
  nullableNumberArrayData: Array<numeric | null>;
}
```

上記のコードで openapi.yaml を出力すると、`oneOf` は利用されないため、OpenAPI Generator では `Array<number>` が生成されてしまいます。

```
openapi: 3.0.0
components:
  schemas:
    BaseObject:
      type: object
      properties:
        nullableNumberArrayData:
          type: array
          # `Array<numeric | null>` だと `oneOf` は利用されないため、TypeScript で `Array<number>` が出力される
          items:
            type: number
            nullable: true
```

そこで、TypeSpec で次のユーティリティ型を定義しました。

```
import "@typespec/openapi";

using OpenAPI;

@oneOf
union NullableNumericArrayItem {
  numeric,
  numeric,
  null,
}

// 利用例
model BaseObject {
  nullableNumberArrayData: Array<NullableNumericArrayItem>;
}
```

上記をエミットすると、`openapi.yaml` では次のようになり、OpenAPI Generator は `Array<number | null>` を出力するようにできました。

```
openapi: 3.0.0
components:
  schemas:
    NullableNumericArrayItem:
      oneOf:
        - type: number
        - type: number
      nullable: true
```

しかし、上記の yaml だと Committee::Rails を使ったテストがスキーマの不一致で通らなくなってしまいました。

そこで、生成した openapi.yaml に対して重複した`- type: number`を削除するパッチスクリプトを用意して npm スクリプトに組み込みました。

patch.mjs

```
async function main() {
  const filePath = path.resolve(
    process.cwd(),
    "schema",
    "openapi.yaml"
  );

  try {
    // 1) ファイル読み込み
    const content = await readFile(filePath, "utf8");
    // 2) YAML をパース
    /** @type {any} */
    const doc = yaml.load(content);
    // 3) NullableNumericArrayItem スキーマにアクセス
    const schema = doc.components?.schemas?.NullableNumericArrayItem;
    if (schema?.oneOf && Array.isArray(schema.oneOf)) {
      // 4) oneOf の重複を排除
      schema.oneOf = Array.from(
        new Map(schema.oneOf.map((o) => [JSON.stringify(o), o])).values()
      );
      // 5) YAML にダンプ (アンカーなし)
      const fixed = yaml.dump(doc, { noRefs: true });
      // 6) 上書き
      await writeFile(filePath, fixed, "utf8");
      console.log("✅ NullableNumericArrayItem.oneOf の重複を削除しました");
    } else {
      console.error(
        "ℹ️ NullableNumericArrayItem.oneOf が見つからないか、重複なし"
      );
    }
  } catch (err) {
    console.error("❌ スクリプト実行中にエラー:", err);
    process.exit(1);
  }
}
```

これにより、すべての `tsc` と `rspec` がパスして無事に移行が完了しました。

## TypeSpec 移行後の対応

### モデルとルート定義のファイル分割

移行が完了してからモデルとルート定義のファイル分割して TypeSpec の可読性を向上しました。

`models` と `routes` ディレクトリを作成し、モデルやコントローラー単位でシンプルに分割しています。

### CI における tsp と yaml の差分チェック

移行後に `openapi.yaml` を直接編集することを禁止するため、CI で TypeSpec と OpenAPI の差分をチェックするフローを組み込みました。

package.json

```
{
  "scripts": {
    "build": "tsp compile . && node patch.mjs",
    "diff-check": "npm run build && git add schema/openapi.yaml && git diff --staged --quiet || (echo 'tspとyamlの変更が同期されていません' && exit 1)"
  },
}
```

## おわりに

調整が必要な箇所もありましたが、コンバーターを活用することで約 2 日間で TypeSpec への移行を完了することができました。

TypeSpec へ移行したことで次の恩恵を享受することができました：

-   移行後の TypeSpec ファイルの総量は約 2 万行になりました。手作業で管理するファイルの行数が減ったことは嬉しいポイントです。
-   モデルやルート定義を分割できるようになったことでスキーマ全体の可読性と開発体験が大幅に向上しました。
-   AI Agent のコンテキストを過度に圧迫することなく開発できるようになりました。
-   エミッターで OpenAPI のバージョンを簡単にアップデートできるようになりました.

OpenAPI の運用に悩まれている方は 1.0.0-RC がリリースされたこのタイミングで TypeSpec への移行を検討してみてはいかがでしょうか？

それでは、良きスキーマ駆動開発ライフをお過ごしください！