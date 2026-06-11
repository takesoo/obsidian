---
aliases:
  - nvm
  - nvmrc
---
## what
- [[Node.js]]のバージョン管理ツール
- [[npm]]のバージョン管理もできる
## why
- プロジェクトごとに異なるバージョンのNode.jsが必要になるため
## how
```shell
nvm install stable --latest-npm
nvm install --lts --latest-npm
nvm use stable
```

Node.jsのバージョンを固定したい場合は`.nvmrc`を使用する。`nvm use`で`.nvmrc`で指定されたバージョンが使用される。
## .nvmrc
- プロジェクトごとに異なるnodeバージョンを指定するためのファイル
```rc
v24.16.0
```
```bash
nvm use # 自動でv24.16.0が使用される
```
```zshrc
# ディレクトリに移動したら自動でnvm useを実行するhook
autoload -U add-zsh-hook
load-nvmrc() {
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$(nvm version)" ]; then
      nvm use
    fi
  elif [ -n "$(PWD=$OLDPWD nvm_find_nvmrc)" ] && [ "$(nvm version)" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```
