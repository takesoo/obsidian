---
URL: https://dev.classmethod.jp/articles/20211008-vscode-extention-settings/
---
[![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/06/vscode-2020-eyecatch-1200x630-1.png)](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/06/vscode-2020-eyecatch-1200x630-1.png)

---

データアナリティクス事業本部コンサルティンググループのnkhrです。

このブログでは、CloudFormation開発で利用できるVS Code拡張機能の設定について紹介します。今回実施した環境のバージョンは、下記の通りです。

- Windows 10
- VS CodeのVersion 1.60.2
- PythonのVersion 3.9.7

※バージョンが違うとGUIの設定画面や、設定パラメータ名など異なる場合があるため、設定時は注意してください。

このブログでは、下記の拡張機能の設定について説明しています。

- [vscode-cfn-lint](https://marketplace.visualstudio.com/items?itemName=kddejong.vscode-cfn-lint)：テンプレートを解析しバリデーションを実施
- [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)：インデントをカラー表示（yaml形式で作成する場合は重宝しそう）
- [CloudFormation support for Visual Studio Code](https://github.com/aws-scripting-guy/cform-VSCode)：補完機能や公式ドキュメントのリンク表示

## vscode-cfn-lint

事前に以下のPythonライブラリをpipでインストールします。（Macの場合はbrewでインストールできるみたいです）

- cfn-lint (必須)
- pydot (任意)
    - コードのバリデーションのみ実施したい場合は不要、参照関係をグラフ化するために利用します。

### Pythonライブラリインストール

私は今のところローカルで複数バージョンのPythonを利用する想定がないので、Windowsインストーラで直接インストールしています。複数バージョンを切り替えたい場合は[pyenv-win](https://github.com/pyenv-win/pyenv-win)を使うと良いかなと思います。

その他のやり方として、直接インストールしたくない（DockerやEmbaded版を使ってすぐに消せるようにする）場合は、下記のブログが参考になると思います。

下記のコマンドでPythonのバージョンを確認し、pipでライブラリをインストールします。

```
> python --versionPython 3.9.7> pip install --user cfn-lint> pip install --user pydot
```

### 環境変数の設定

pipでインストールするスクリプトのパスを環境変数PATHに追加します。すでに追加済みであれば不要です。環境変数に入れたくない場合は、VS Codeの設定でcfn-lint.exeの絶対パスを指定する方法もあります。

スクリプトのパスは以下のコマンドで確認できます。

「Location」に「site-packages」のパスが表示されます。スクリプトのフォルダは「site-packages」の部分を「Scripts」に変更したパスになります。

```
> pip show cfn-lintName: cfn-lintVersion: 0.54.2Summary: Checks CloudFormation templates for practices and behaviour that could potentially be improvedHome-page: https://github.com/aws-cloudformation/cfn-python-lintAuthor: kddejongAuthor-email: kddejong@amazon.comLicense: MIT no attributionLocation: c:\users\<path-to-python-package>\python39\site-packagesRequires: aws-sam-translator, jsonschema, pyyaml, six, junit-xml, jsonpatch, networkxRequired-by:
```

変更は下記の太字の部分です。

- site-packagesのパス
    - c:\users[path-to-python-package]\python39\**site-packages**
- Scriptsのパス ※こちらを利用
    - c:\users[path-to-python-package]\python39\**Scripts**

パスが通ると、ターミナルでcfn-lintコマンドが利用できます。

```
> cfn-lint --versioncfn-lint 0.54.2
```

### 拡張機能のインストール

VS Codeで拡張機能をインストールします。

VS Codeの設定で、cfn-lintの絶対パスを指定する場合は「Cfn Lint: Path」に書きます。下記の画像では、環境変数でパスを通しているためcfn-lintのみで利用できます。

### cfn-lintの利用

VS Codeでパスが正しく設定されている場合、自動で問題を指摘してくれます。

### Pydotによる参照関係グラフの表示

pydotをインストールしている場合は、参照関係のグラフ表示ができます。

Ctrl+Shift+Pで[コマンドパレット]を起動し、「cloudformation」と打ち込むと表示される「Preview CloudFormation template as graph」を選択します。

こんな感じの図が出てきて、参照関係を図示してくれます。

## indent-rainbow

インデントをカラー表示してくる拡張機能です。インデントが見えやすくなるのでyamlでCloudFormationのコードを書く場合は、インデントのミスに気づきやすくなると思います。

VS Codeの機能拡張（Extention）画面からインストールすれば、自動で適用されます。

### 既存設定の変更方法

インデントの表示カラーや、インデントのエラー時の指摘を変更したい場合は、既存設定を変更します。

VS Codeの設定(settings.json)は、３種類（default < user < workspaces)あり、workspacesの設定が一番優先度が高いです。同じ設定項目が複数のsettings.jsonに書かれている場合は、優先度が高い設定で上書きされます。

[settingsに関する公式ドキュメント](https://code.visualstudio.com/docs/getstarted/settings)

1. default settingsを確認する場合
    - Ctrl+Shift+Pで「コマンドパレット」を起動し、検索バーに「settings.json」と入力
        - default設定のファイルはdefaultSettings.jsonという名称です。
            
    - プレフィックスが「indentRainbow」からはまるパラメータ値を確認
2. user settingsを変更する場合
    
    - 「Ctrl + , 」（または左下の歯車マーク）から設定画面を開く
    - 右上のマークをクリックしてユーザのsettings.jsonを起動（GUI画面で編集してもよい）
        
3. workspaces settingsを変更する場合（Workspacesごとに設定を変えたい場合）
    
    - ワークスペースのルートフォルダ配下に.vscode/settings.jsonを作成（または作成済みのファイルを起動）

### 設定例

- インデントをカラー表示する言語を制限

```
// 空[]の場合は、すべての言語でインデントカラーを有効にする// for example ["nim", "nims", "python"]"indentRainbow.includedLanguages": [],// 空[]の場合は、除外言語はなし"indentRainbow.excludedLanguages": ["plaintext"]
```

- インデントエラーを無視する言語を指定

```
// defaultではmarkdownが設定されている"indentRainbow.ignoreErrorLanguages" : [    "markdown",    "haskell" ]
```

その他の設定については、[リンク先](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)を参照してください。

## CloudFormation support for Visual Studio Code

キーワードの補完機能や、ドキュメントへのリンク表示ができる拡張機能です。

VS Codeの拡張機能画面からインストールします。

user settingsに以下の設定を追加します。

```
// Custom tags for the parser to use    "yaml.customTags": [        "!And",        "!If",        "!Not",        "!Equals",        "!Or",        "!FindInMap sequence",        "!Base64",        "!Cidr",        "!Ref",        "!Sub",        "!GetAtt",        "!GetAZs",        "!ImportValue",        "!Select",        "!Select sequence",        "!Split",        "!Join sequence"    ],    // Enable/disable default YAML formatter (requires restart)    "yaml.format.enable": true
```

## まとめ

今回は、VS Codeで、CloudFormationコードを作成するときに有効な拡張機能を追加しました。 SAM、CDKによるCloudFormation開発についても、そのうち書きたいと思います。