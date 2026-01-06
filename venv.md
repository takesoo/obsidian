---
tags:
  - Python
---
## what
- Pythonプロジェクトごとに独立した仮想環境を作成するためのPython標準モジュール
## why
- プロジェクト間での依存関係の分離
## how
```shell
# 仮想環境の作成
$python3 -m venv .venv

# 仮想環境の有効化
$source .venv/bin/activate
(.venv)$pip install requests
(.venv)$deactivate
```