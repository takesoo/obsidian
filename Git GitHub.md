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