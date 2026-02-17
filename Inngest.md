## what
- サーバーレス・キューイングプラットフォーム
	- [[AWS SQS]]や[[Step Functions]]のような機能を提供するOSS
	- [[AWS Lambda|Lambda]]のような関数ランタイムは提供しておらず、実行環境はLambdaや[[Cloud Run]]やNext.js [[Route Handler]]などで実装し、Inngestからそれらを呼び出す仕組み
- Durable Execution: 耐障害実行。
	- ワークフローの途中で失敗しても、そこからやり直せる仕組み。
	- 関数の状態をInngestが保持し、失敗したら自動リトライ、再実行する時は失敗した手前から再開できるようにしてくれている
	- Inngestではこれを **関数＋Steps** という形で提供している
		- 各ステップは個別のHTTPリクエストとして実行される
```ts
const fn = inngest.createFunction(
  { id: "import-contacts" },
  { event: "contacts/csv.uploaded" },
  // The function handler:
  async ({ event, step }) => {
    const rows = await step.run("parse-csv", async () => {
      return await parseCsv(event.data.fileURI);
    });

    const normalizedRows = await step.run("normalize-raw-csv", async () => {
      const normalizedColumnMapping = getNormalizedColumnNames();
      return normalizeRows(rows, normalizedColumnMapping);
    });

    const results = await step.run("input-contacts", async () => {
      return await importContacts(normalizedRows);
    });

    return { results };
  }
);
```
- Inngest Function: 信頼できるバックグラウンド処理（ジョブ〜ワークフロー）を実行するための単位
	- [[Step Functions]] + [[AWS Lambda|Lambda]]群をもっとコード寄りに、１ファイルで書けるようにしたもの
	- `inngest.createFunction()`の戻り値
	- 構成要素
		- Triggers
		- FlowControl
		- Steps
	- Inngestがイベントをトリガーにして実行する
```ts
inngest.createFunction({
    id: "sync-systems",
    // Easily add Throttling with Flow Control
    throttle: { limit: 3, period: "1min"},
  },
  // A Function is triggered by events
  { event: "auto/sync.request" },
  async ({ step }) => {
    // step is retried if it throws an error
    const data = await step.run("get-data", async () => {
      return getDataFromExternalSource();
    });

    // Steps can reuse data from previous ones
    await step.run("save-data", async () => {
      return db.syncs.insertOne(data);
    });
  }
);
```
- Durable Endpoint: 
	- APIエンドポイントとしてワークフローを公開する形式
	- クライアントがHTTPリクエストを送って実行する
```ts
// Next.js

// src/libs/inngest-client.ts
import { Inngest } from "inngest";
import { endpointAdapter } from "inngest/next";

const inngest = new Inngest({
  id: "my-app",
  endpointAdapter,
});

// src/app/api/inngest/route.ts
import { step } from "inngest";
import { inngest } from "@/inngest/client";
import { NextRequest } from "next/server";

export const POST = inngest.endpoint(async (req: NextRequest) => {
  const { userId, data } = await req.json();

  // Step 1: Validate and enrich the data
  const enriched = await step.run("enrich-data", async () => {
    const user = await db.users.find(userId);
    return { ...data, account: user.accountId };
  });

  // Step 2: Process the enriched data
  const result = await step.run("process", async () => {
    return await processData(enriched);
  });

  // Step 3: Send notification
  await step.run("notify", async () => {
    await sendNotification(userId, result);
  });

  return Response.json({ success: true, result });
});
```
- steps
	- `step.run()`: 実行＋リトライ＋[[メモ化]]
	- `step.sleep()`, `step.sleepUntil()`: スリープ
	- `step.waitForEvent()`: 別イベントを待つ
	- `step.waitForSignal()`: 特定のrunを再開する
	- `step.invoke()`: 別のInngest Functionをサブワークフローとして呼び出す
	- `step.sendEvent()`fire-and-forgetなトリガー