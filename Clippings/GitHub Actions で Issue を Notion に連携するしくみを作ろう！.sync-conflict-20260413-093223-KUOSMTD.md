---
category: "[[Clippings]]"
author: "[[utahkaフォロー]]"
title: "GitHub Actions で Issue を Notion に連携するしくみを作ろう！"
source: https://zenn.dev/utahka/articles/a953af65069a5c
clipped: 2023-08-25
published: 2023-08-25
topics: 
tags: [clippings]
---

この投稿では、複数リポジトリに分散している GitHub Issue を、 Notion データベースに同期して一括管理するしくみを GitHub Actions でつくった方法を解説します。

一般的なチーム開発では、複数のリポジトリに Issue が別々に作られると思いますが、それらを元々タスク管理に使っていた Notion に連携するのが目的です。

今回は GitHub Actions を利用して Notion に Issue を連携しましたが、いくつかあるリポジトリに同じ設定をするのは少々面倒ですし保守のコストもかかります。

そこで、なるべく再利用できそうな処理は 1 つの GitHub Actions 用リポジトリに集約し、各リポジトリからはそのパイプラインを起動するようなしくみを目指しました。システム概要としては次のような形です。

また、Notion に同期するかどうかは選択できるようにしたかったので、開発リポジトリが Issue 同期 Workflow をキックするトリガーは、「Issue に **Notion** というタグを付けたとき」としました。

Issue 同期は、Notion API を叩き Notion データベースにタスクを追加することで実現します。

Notion にタスクを作成する処理はリポジトリ `sync-issues-to-notion-action` で管理。そのためには、大まかに下記の　3 つが必要です。

1.  `.github/workflows/main.yml` : スクリプトを実行する Workflow 定義ファイル
2.  `index.js` : Notion API を叩くスクリプト
3.  `dist/index.js` : index.js をビルドしたスクリプト

API を叩く部分はスクリプトに任せるので、Workflow は `dist/index.js` をチェックアウトして実行するだけのシンプルな内容です。

.github/workflows/main.yml

```
name: main

run-name: ${{ github.event.client_payload.title }} - ${{ github.event.client_payload.repo }}

on:
  repository_dispatch:
    types: [labeled-issue]

jobs:
  sync-github-issue-to-notion:
    runs-on: ubuntu-latest
    name: Create Notion Ticket
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v3.6.0
        with:
          node-version: 16
      - name: Sync GitHub Issue to Notion
        run: 'node ./dist/index.js'
        shell: bash
        env:
          NOTION_TOKEN: ${{ secrets.NOTION_TOKEN }}
          NOTION_HP2_TASK_DATABASE_ID: ${{ secrets.NOTION_TASK_DATABASE_ID }}
          TITLE: ${{ github.event.client_payload.title }}
          URL: ${{ github.event.client_payload.url }}
          REPO: ${{ github.event.client_payload.repo }}
```

開発リポジトリからは、この Workflow を GitHub API (repository\_dispatches) 経由でキックすることで、Notion に Issue 情報を同期します。

こうすることで、開発リポジトリの Workflow をシンプルにすることができるだけでなく、`sync-issues-to-notion-action` の Workflow 実行履歴から同期した Issue の履歴を一元管理できます。また、先の Workflow では履歴の一覧性を高めるために `run-name` に Issue タイトルとリポジトリ名を表示するように設定しています。

## Notion API 実行スクリプトの作成

スクリプトは、GitHub Action と相性が良さそう、かつ公式の Notion API クライアントが公開されている JavaScript で書きました。Notion DB の項目は、ご自身の環境に合わせて修正してください。

```
import { setFailed } from '@actions/core'
import { Client, LogLevel } from '@notionhq/client'

const notion = new Client({
  auth: process.env.NOTION_TOKEN,
  logLevel: LogLevel.DEBUG,
})

const NOTION_DATABASE_ID = process.env.NOTION_DATABASE_ID

try {
  await notion.pages.create({
    parent: { database_id: NOTION_DATABASE_ID },
    properties: {
      タスク名: {
        id: 'title',
        title: [{ text: { content: process.env.TITLE } }],
      },
      リポジトリ: {
        id: '＜ID＞',
        type: 'select',
        select: { name: process.env.REPO },
      },
      'GitHub Issue リンク': {
        id: '＜ID＞',
        type: 'url',
        url: process.env.URL,
      },
    },
  })
} catch (error) {
  setFailed(error.message)
}
```

一方、JS のモジュールを利用していることで、`node_modules` をどうするか...という問題がつきまとってくるのですが、ここは [`vercel/ncc`](https://github.com/vercel/ncc) でビルドした成果物を `./dist` というディレクトリに置いておくことで妥協しています。

そのため、index.js を修正したら忘れずに ncc でビルドしなければいけないのが手間ではあります。

さいごに、開発リポジトリに設置する　Workflow ファイルについて解説します。

内容は、認証トークンを払い出して GitHub API を叩き、 `sync-issues-to-notion-action` の Workflow をキックしているだけです。

.github/workflow/sync\_notion.yaml

```
name: sync notion
on:
  issues:
    types: labeled

jobs:
  call-worflow:
    if: github.event_name == 'issues' && github.event.action == 'labeled' && github.event.label.name == 'Notion'
    runs-on: ubuntu-latest
    steps:
      - name: Generate GitHub Apps token
        id: generate
        uses: tibdex/github-app-token@v1
        with:
          app_id: ${{ secrets.CALL_GITHUB_ACTIONS_WORKFLOW_APP_ID }}
          private_key: ${{ secrets.CALL_GITHUB_ACTIONS_WORKFLOW_KEY }}

      - name: Call Workflow
        run: |
          TOKEN="${{ steps.generate.outputs.token }}"
          TITLE="${{ github.event.issue.title }}"
          URL="${{ github.server_url }}/${{ github.repository }}/issues/${{ github.event.issue.number }}"
          REPO="${{ github.repository }}"

          curl \
            -X POST \
            -H "Authorization: Bearer ${TOKEN}" \
            -H "Accept: application/vnd.github.v3+json" \
            https://api.github.com/repos/＜Organization名＞/sync-issue-to-notion-action/dispatches \
            -d "{\"event_type\": \"labeled-issue\", \"client_payload\": {\"repo\": \"${REPO}\", \"title\": \"${TITLE}\", \"url\": \"${URL}\"}}"
```

### トークンどうする問題

さて、個人的に沼ったのが認証トークンまわりです。 GitHub Actions では勝手に払い出される GITHUB\_TOKEN というトークンがあります。ただ、これの権限が狭いので、 repository\_dispatches を実行するには他のトークンを払い出す必要があります。

GITHUB\_TOKEN 以外に使えそうなトークンは、下記 2 つでした。

1.  PAT (Personal Access Tokens) : 個人用アクセストークン
2.  GitHub Apps トークン : GitHub Apps というアプリ単位の認証トークン

このあたりの話題でよく出てくるのが PAT なのですが、これは個人アカウントに紐づいていることや期限を設定する必要があるため、組織的な運用をしている場合は使いづらい印象を受けました。企業の場合、PAT が紐づいているアカウントの持ち主がいなくなる可能性がありますし、期限切れする度に更新対応が必要になってしまうからです。

一方、GitHub Apps を作成して、毎回トークンを払い出すようにすれば、期限切れや個人アカウントへの紐づけも必要ありません。そこで、最終的には GitHub Apps を利用する形に落ち着いています。

(余談) 省力化のため、トークンの払い出しに `tibdex/github-app-token@v1` を使っているのですが、セキュリティ的にイケてないという意見もあると思いますので、このあたりは課題かなと思います。

複数リポジトリに分散している GitHub Issue を、 Notion データベースに同期して一括管理するしくみを GitHub Actions でつくった方法を解説しました。

Issue 同期処理は 1 つのリポジトリに集約し、開発リポジトリからはその Workflow を起動するようなしくみを目指しました。GitHub API を利用して Worflow を Issue 同期処理の管理リポジトリ上で実行することで、Issue の同期履歴を一覧できるようにしました。