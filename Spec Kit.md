## what
- [[仕様駆動開発|Spec Driven Development]]のためのツール
- 仕様定義とタスク計画を効率化する
- 開発プロセスを4つのステップに分けることで、仕様策定からコード実装までを一貫して支援する構造になっている
	- Step0: Constitution
		- プロジェクト全体で遵守すべき普遍のルールや原則の定義
		- 技術スタック、コーディング規約、スタイルガイド、セキュリティ要件、パフォーマンス要件、etc...
	- Step1: Specify
		- 仕様の詳細な記述
		- 開発の背景、目的、ユーザー要件など
	- Step2: Plan
		- 実装方針の決定
		- どのような技術スタックやアーキテクチャで実装するかを検討する
		- 設計上のトレードオフや代替案も記録することで、後続工程に透明性をもたらす
	- Step3: Tasks
		- タスクへと分解
	- Step4: Implement
		- 個々のタスクに対してAIエージェントが実装を実施する
- `specs/`ディレクトリ内にmdファイルで管理
## how
```shell
# 1. プロジェクト原則、開発ガイドラインの定義しconstitution.mdを作成(最初に一度実行)
/speckit.constitution

# 2. 機能仕様書(spec.md)を作成。技術スタックは含まず、何を、なぜ作るのかにフォーカス。
/speckit.specify ユーザー認証機能を追加したい

# 2.5 仕様の曖昧な点を質問形式で明確化
/speckit.clarify

# 3. 技術設計書(plan.md)を作成。技術スタックとアーキテクチャの選択
/speckit.plan

# 4. 実装タスク一覧(tasks.md)を作成
/speckit.tasks

# 5. tasks.mdに沿って実装を実行
/speckit.implement

# spec/plan/tasks間の整合性チェック
/speckit.analyze

# 要件品質検証用チェックリスト生成
/speckit.checklist

# タスクをGithub Issueに変換
/speckit.taskstoissues
```