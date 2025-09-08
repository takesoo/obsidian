---
finished reading: false
favorite: false
tags:
  - clippings
---
# rulesync: Claude CodeやCursor、Clineのrulesを統一管理するツールを公開しました | DevelopersIO
  #ReadItLater 
 #ReadableArticle

## articleURL
https://dev.classmethod.jp/articles/rulesync/?s=09

## siteName
rulesync: Claude CodeやCursor、Clineのrulesを統一管理するツールを公開しました | DevelopersIO

## date
2025-06-30

## articleContent
[@dyoshikawa](https://twitter.com/dyoshikawa1993)です。

さまざまなAIコーディングツールのルール（rules）ファイルを単一のマークダウンファイル群で一括管理できるrulesyncというツールを作りましたので紹介します。

`.rulesync/*.md` ファイルを作成することで、それを元に各ツールの仕様に沿ったファイルを生成します。

現状、以下のツールに対応しています。

| ツール名 | 生成されるファイル |
| --- | --- |
| Claude Code | `CLAUDE.md` `.claude/memories/*.md` |
| GitHub Copilot | `.github/instructions/*.instructions.md` |
| Cursor | `.cursor/rules/*.mdc` |
| Cline | `.clinerules/*.md` |
| Roo Code | `.roo/rules/*.md` |

Claude Codeに関しては、 `CLAUDE.md` 以外のファイルの場所は任意（ `@{path}` で参照させる ）なので筆者の判断で `.claude/memories/*.md` に生成することにしました。

## 動機

さまざまなAIコーディングツールが出てきていますが、そのそれぞれがルールファイルの仕様を独自に定義しています。

これらのファイルを個別に管理するのはなかなか面倒です。`.github/instructions/*.instructions.md`、`.cursor/rules/*.mdc`、`CLAUDE.md`など、ツールごとに異なる場所に異なる形式でルールを記述しなければなりません。

また、どれか一つのツールに固定することも難しいです。AIツールの進化は速く、数ヶ月単位で性能が高いとされるツールが入れ替わります。さらに、開発チーム内でもメンバーによって好んで使用するツールが異なることもあります。これにより、ルールの内容を変更するたびに各ツール間のルールファイルを手動で「同期」する手間が発生します。

そこで「単一のルールファイルから各ツールのルールファイルを自動生成できたら便利ではないか」と考え、rulesyncを開発しました。

コンセプトは以下です。

-   ルールファイルを一括管理・生成することにより、**AIコーディングツールを自由に選択**できる。
-   ルールファイルを一括管理・生成することにより、**異なるAIコーディングツールをハイブリッドに組み合わせて開発**できる。
-   すべてのAIコーディングツールに同じ内容のルールを適用できるため、**どのツールを使っても生成されるコードの品質や規約の一貫性を保つ**ことができる。
-   **rulesyncへのロックインは一切ない。**
    -   rulesyncはいつでも捨てられます。生成されたルールファイル（`.github/instructions/`、`.cursor/rules/`、`.clinerules/`、`CLAUDE.md`など）はそのまま残るので、そのまま手動管理に移行すれば良いです。

## rulesyncの使い方

### Getting Started

npmパッケージとして公開しているので、Node.jsがインストールされていれば `npx rulesync` ですぐに使い始めることができます。

```
# ルールファイルのサンプルを生成
npx rulesync init

# もしくは既存のrulesを取り込んで `.rulesync/*.md` を作成
# （Cursor Project Rulesの例）
npx rulesync import --cursor
```

ルールファイルの一括生成、rulesyncファイルの追加は以下のコマンドです。

```
# 対応ツール全てを対象にルールファイルを生成
npx rulesync generate

# 特定ツールのみ対象にルールファイルを生成（CursorとClaude Codeの例）
npx rulesync --cursor --claudecode

# rulesyncルールファイルを追加
npx rulesync add new-rule.md
```

### .rulesync/\*.md の書き方

[Cursor Project Rules](https://docs.cursor.com/context/rules)ライクなFrontmatter入りのMarkdownファイルを記述するようにしています。

以下はrulesyncリポジトリの `.rulesync/overview.md` の例です。

.rulesync/overview.md

```
---
root: true # 1つだけRootファイルに指定できる。Rootに指定したファイルは原則常に適用される（Cursorの `ruletype: always` やClaude Codeの `CLAUDE.md` として出力する）。
targets: ["*"] # このファイルの出力対象ツールを `copilot`, `cursor`, `claudecode` などで配列指定。 `*` で全ツール対象になる。
description: "rulesync project overview and architecture guide" # AIがこのルールを適用するかどうかの判断に使用される説明。
globs: ["*"] # ルールを適用するファイルパスをGlob指定。 `*` で全ファイル対象になる。例: `src/**/*.ts`
---

# rulesync Project Overview

rulesync is a unified AI configuration management CLI tool that supports multiple AI development tools (GitHub Copilot, Cursor, Cline, Claude Code, Roo Code).

## Core Architecture

...
```

### 生成ファイルをgitignoreに追加

生成されたファイルをgit管理したくない場合は、 `gitignore` コマンドを叩くことで `.gitignore` ファイルに各種生成先のパスを追加できます。

```
# .gitignoreに生成ファイルを追記
npx rulesync gitignore
```

### 既存のルールファイルを取り込んでrulesyncのファイルを生成する

`import` コマンドを叩くことで、 `generate` コマンドとは逆に既存のルールファイルから `.rulesync/*.md` に変換して取り込むことができます。

```
# 既存のCursor Project Rulesを取り込んで `.rulesync/*.md` を作成
npx rulesync import --cursor
```

## 実装にはClaude Codeを使用

rulesyncの実装にはClaude Codeを活用しました。筆者自身はコードを1行も書いていません。

開発プロセスはシンプルで、「仕様を伝える」→「手動で動作確認」→「フィードバック」のサイクルを繰り返すだけでした。テストコードもすべてClaude Codeに書いてもらい、カバレッジ90%以上を達成しています。

rulesyncのような小規模なCLIツールは、AIにとって実装しやすいようです。実行環境がローカルで完結し、外部システムとの複雑な連携もないため、仕様を伝えるだけで期待通りの挙動をしています。

一方で、別の機会にRailsアプリケーションを作成してCloud Runにデプロイする作業をClaude Codeに依頼してみたところ、これは人間の開発者（筆者）がかなり介入しないとうまくいきませんでした[\[1\]](https://dev.classmethod.jp/articles/rulesync/?s=09#fn-2c00-1)。似たような体験をされた方の記事もありました。

ネットワークをまたいで複数のシステムを連携させるような作業になると、AIにとっては複雑度や問題の切り分けの難易度が一気に跳ね上がるようです。エラーが発生した際に、それがローカルの問題なのか、ネットワークの問題なのか、クラウド側の設定の問題なのかを判断するのが難しいのかもしれません。

## まとめ

複数のAIコーディングツールを横断的に使うためにrulesyncというツールを公開しました。

AIツールのトレンドの移り変わりの早さを考えると、特定のツールにロックインされることなく、柔軟に乗り換えられる仕組みは今後も有用なのではないかと考えています。rulesのファイル管理に煩わされている方の一助になれば嬉しいです。

もし興味を持っていただけたら、ぜひ試してみていただけると幸いです。PRやIssueも歓迎しています。

脚注

1.  デプロイ自体はできるが、ヘルスチェックを通過させることができませんでした。 [↩︎](https://dev.classmethod.jp/articles/rulesync/?s=09#fnref-2c00-1)