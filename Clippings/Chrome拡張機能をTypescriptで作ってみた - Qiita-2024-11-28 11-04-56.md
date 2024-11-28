---
finished reading: false
favorite: false
tags:
  - clippings
---
# Chrome拡張機能をTypescriptで作ってみた - Qiita
  #ReadItLater 
 #ReadableArticle

## articleURL
https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f

## siteName
Qiita

## date
2024-11-28

## articleContent
この記事は最終更新日から1年以上が経過しています。

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)はじめに

皆さんは`マジカルミライ`をご存知でしょうか。

初音ミクのライブなわけですが、このチケットを応募する際に面倒な点がありました。

**個人情報を応募するたびに入力しなければいけない点です！！！**

マジカルミライ2023は大阪、東京合わせて12公演あるので、  
もし、すべてに応募する場合、12回入力する必要が出てきます。

ということで、今回はこの個人情報を入力する部分を`Chrome拡張機能`を利用して、  
自動入力してもらおうと思います。

ただChrome拡張機能作っても面白くないので、Typescriptで作成し、Github Actionsで自動Build&Releaseまで目指します。

今回作成した成果物

念のため

自動入力を目的に作り始めますが、実際のソースの解説は今回しません。  
フォームの内容をlocalstorage に記録し、次回以降localstorage　から呼び出せるようにする感じです。  
どこかで別の記事にはするかもしれませんが…  
また、botと同じ扱いとなる可能性があります。  
今回作成した拡張機能の使用は自己責任でお願いします。

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#typescript-%E3%81%A7chrome-%E6%8B%A1%E5%BC%B5%E6%A9%9F%E8%83%BD%E3%81%AE%E9%9B%9B%E5%BD%A2%E4%BD%9C%E6%88%90)Typescript でChrome 拡張機能の雛形作成

まずプロジェクトフォルダを作成して、npmの初期設定をしていきます。  
コマンドラインでpackage.jsonを作成することができます。

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#packagejson-%E3%81%AE%E4%BD%9C%E6%88%90)package.json の作成

package.jsonの中身はよしなに必要事項を書いてください。  
この後buildコマンドをここに記載していきます。

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#typescript-%E3%81%AE%E5%B0%8E%E5%85%A5)Typescript の導入

今回はTypescriptで作成するので、Typescriptを導入していきます。  
Chromeの型定義も一緒に導入します。

```
npm install -D typescript @types/chrome
```

tsconfig.jsonを作成します。  
下記コマンドで初期化できますが、ファイルを作成しても問題ありません。

ビルドツールを使用する予定なので、tscによるトランスパイルは行わない設定にしていきます。  
ただ、型チェックは引き続き行いたいので、"outDir"は入れず、"noEmit"をオプションに追加します。

tsconfig.json

```
{
  "compilerOptions": {
    "target": "ESNext",
    "lib": ["ESNext", "DOM", "DOM.Iterable"],
    "useDefineForClassFields": true,
    "module": "ESNext",
    "rootDir": "./src",
    "moduleResolution": "node",
    "baseUrl": "src",
    "resolveJsonModule": true,
    "allowJs": true,
    "checkJs": true,
    "removeComments": true,
    "esModuleInterop": true,
    "strict": true,
    "noImplicitAny": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "skipLibCheck": true,
    "noEmit": true,
    "isolatedModules": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist", "./tsconfig.json"]
}

```

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#vite-%E3%81%AE%E5%B0%8E%E5%85%A5)Vite の導入

Typescriptなので、ビルドを行い、Javascriptにしていく必要があります。  
webpackも検討しましたが、思ったより簡単にできなかったので見送りました。  
実行時間もViteのほうが早いようなので、今回は様々あるビルドツールの中でViteを採用していきます。

導入が終わったら設定ファイルを書いていきます。  
rootディレクトリやoutput先、inputするファイルなどを設定します。

vite.config.js

```
import { resolve } from 'node:path'
import { defineConfig } from 'vite'

export default defineConfig((opt) => {
  return {
    root: 'src',
    build: {
      outDir: '../dist',
      rollupOptions: {
        input: {
          index: resolve(__dirname, 'src/index.ts'),
        },
        output: {
          entryFileNames: '[name].js',
        },
      },
    },
  }
})

```

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#manifestjson%E3%81%AE%E4%BD%9C%E6%88%90)manifest.jsonの作成

これで基本的な設定が整いました。  
あとはChrome拡張に必要なmanifest.json とindex.ts を作成するだけです。  
今回はva.pia.jp に適用できるよう設定していきます。  
また、出力されるファイルはindex.js となるため、content\_scripts にindex.jsを読み込むよう設定していきます。  
manifest.json はsrc/public に作成していきます。

src/public/manifest.json

```
{
  "manifest_version": 3,
  "name": "Auto Form Input",
  "description": "Base Level Extension",
  "version": "1.0",
  "content_scripts": [
    {
      "matches": ["http://va.pia.jp/*", "https://va.pia.jp/*"],
      "js": ["index.js"],
      "css": ["global.css"]
    }
  ]
}

```

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#packagejson-%E3%81%AE%E4%BF%AE%E6%AD%A3)package.json の修正

最後にビルドを行うために、package.json のscript に以下を追加します。  
tsc を行った後、vite のbuildを行います。  
vite のbuild を行う際に、outdirが空でない場合、エラーが発生しますが、emptyOutDirオプションをつけてあげることで、エラーを発生させないようにできます。  
今回は空でなくてもビルドできるよう、オプションをつけていきます。

package.json

```
"build": "tsc && vite build --emptyOutDir"
```

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#typescript-%E3%81%A7chrome-%E6%8B%A1%E5%BC%B5%E6%A9%9F%E8%83%BD%E3%81%AE%E9%9B%9B%E5%BD%A2%E5%AE%8C%E6%88%90)Typescript でChrome 拡張機能の雛形完成

これで、Typescript でChrome 拡張機能を作成するひな形が完成しました。  
あとはsrc フォルダ内にindex.ts ファイルを作成し、書き始めるだけです！

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#github-%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9%E3%83%8E%E3%83%BC%E3%83%88%E3%82%92%E8%87%AA%E5%8B%95%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)Github リリースノートを自動作成する

皆さんご存じの通り、Github はGithub Actions が存在します。  
このGithub Actions を利用して、Chrome 拡張機能をbuild して、Github のリリースノートにbuild した成果物を掲載したいと思います。

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#releaseyml-%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)release.yml を作成する

.github/workflows フォルダにymlファイルを作成します。  
今回はリリースノートには特に記載せず、build を行った日時のzipファイルが生成されるように設定しました。  
また、発火するタイミングはmain ブランチにpush されたタイミングとしました。  
yml ファイルの全体像は以下の通りです。

release.yml

```
name: build and release

on:
  push:
    branches: [main]

jobs:
  release:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.step_version.outputs.version }}
      upload_url: ${{ steps.step_upload_url.outputs.upload_url }}
    steps:
      - name: Generate release tag
        id: release_tag
        env:
          TZ: 'Asia/Tokyo'
        run: echo "nowDATE=$(date +'%Y-%m-%d_%H:%M:%S')" >> $GITHUB_ENV

      - uses: actions/checkout@v3
      - name: ci
        run: npm ci
      - name: Build
        run: npm run build
      - name: Zip output
        run: zip -r ${{ env.nowDATE }}.zip dist

      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload-release-asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./${{ env.nowDATE }}.zip
          asset_name: ${{ env.nowDATE }}.zip
          asset_content_type: application/zip

```

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#%E5%90%84yml-%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E8%AA%AC%E6%98%8E)各yml ファイルの説明

少しだけですが、何を行っているか説明しようと思います。

```
      - name: Generate release tag
        id: release_tag
        env:
          TZ: 'Asia/Tokyo'
        run: echo "nowDATE=$(date +'%Y-%m-%d_%H:%M:%S')" >> $GITHUB_ENV
```

リリースファイルのための日時を作成しています。  
シンプルにdateをnowDate としてGITHUB\_ENV に残しています。

```
      - uses: actions/checkout@v3
      - name: ci
        run: npm ci
      - name: Build
        run: npm run build
      - name: Zip output
        run: zip -r ${{ env.nowDATE }}.zip dist
```

リリースノートを作成します。  
各項目を記載しますが、今回はtag をブランチ名にし、そのほかは特に設定しないことにしました。

```
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release
          draft: false
          prerelease: false
```

build された成果物をリリースノートにアップロードします。  
パスと名前は日付にしているため、追記した環境変数から取得するようにしています。

```
      - name: Upload Release Asset
        id: upload-release-asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./${{ env.nowDATE }}.zip
          asset_name: ${{ env.nowDATE }}.zip
          asset_content_type: application/zip
```

これで自動的にmain ブランチにpush されるとリリースされるようになりました！

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#%E6%84%9F%E6%83%B3)感想

今回はChrome の拡張機能をTypescript で作成しましたが、実際ここまでする必要はなんだろうなぁという気持ちはあります。  
ただ、React を今まで使ってこなかった&Next.js などはすでにTypescript のビルドが含まれていたりと、考慮することがなかったので良い経験になりました。  
また、自動でリリースノートを作成するワークフローも作成してみたかったので、勉強になりました。  
何かの参考になれば幸いです。

## [](https://qiita.com/y_inoue15/items/8c3d21b97ec251af853f#%E5%8F%82%E8%80%83)参考

新規登録して、もっと便利にQiitaを使ってみよう

1.  あなたにマッチした記事をお届けします
2.  便利な情報をあとで効率的に読み返せます
3.  ダークテーマを利用できます

[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)