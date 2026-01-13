---
tags:
  - Git
  - GitHub
aliases:
  - Git
  - GitHub
---
## ブランチ
### ローカルブランチ
- ローカルリポジトリ内に存在する作業用ブランチ
- 通常は`.git/refs/heads/<branch>`に保存され、HEADがどのコミットかを示すポインタとなる
### リモートブランチ
- GitHubなどにホストされたリモートリポジトリ上のブランチ
- origin
- ローカルに直接は存在せず、リモートリポジトリを参照することで認識される
- `git ls-remote <remote>`, `git remote show <remote>`
### リモート追跡ブランチ
- ローカルリポジトリ内に作成される、「リモートブランチを過去に取得した状態」を保持する参照
- 通常は`.git/refs/remotes/origin/<branch>`のように保存される
![[スクリーンショット 2025-04-28 11.41.58.png]]

### worktree
- 一つのローカルリポジトリで作業ツリーを複数同時に持てる
```shell
# 新規worktree作成
git worktree add -b feature/new-ui ../my-project-new-ui
# 1. feature/new-uiブランチ作成
# 2. ../my-project-new-uiディレクトリ作成
# 3. ↑ディレクトリでfeature/new-uiブランチにチェックアウト

git worktree list
/User/me/my-project         abc1234 [main]
/User/me/my-project-new-ui  def5678 [feature/new-ui]

cd ../my-project-new-ui
echo 'new ui' >> new-ui.txt
git add new-ui.txt
git commit -m 'edit new-ui.txt'
git push

# worktreeの削除（ブランチは残る）
git worktree remove ../my-project-new-ui

# worktree情報の整理
git worktree prune

```