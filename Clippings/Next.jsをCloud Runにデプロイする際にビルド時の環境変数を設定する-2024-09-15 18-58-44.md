---
finished reading: false
---
# Next.jsをCloud Runにデプロイする際にビルド時の環境変数を設定する
  #ReadItLater 
 #ReadableArticle

## articleURL
https://zenn.dev/igz0/articles/76c3214392986f?s=09

## siteName
Zenn

## date
2024-09-15

## articleContent
## 1\. はじめに

Next.js(App Router)をCloud Runにデプロイしようとしたのですが、Next.jsのビルド時に環境変数を設定するのに苦労したので備忘録です。  
Vercelを使えば即効で終わるのですが、それでもCloud Runを使いたい人向けの記事です。

## 2\. 前提

-   Next.jsの環境変数は[Cloud Runに設定済みとします](https://cloud.google.com/run/docs/configuring/environment-variables?hl=ja)
-   Cloud RunにNext.jsをビルドしようとする際はCode Buildが走り`next build`を行います
-   Cloud Runに環境変数を設定してもCode Buildは環境変数を知らないので正常にビルドできません
-   Next.jsが正常にビルドできるようにするには`.env`ファイルなどで環境変数を教えてあげる必要があります
-   `Dockerfile`や`cloudbuild.yaml`(つまりコード内)に機密情報をハードコーディングすることもできますが無論悪手なので違う手を使いました

## 3\. 現象

Google CloudのCloud Runにデプロイする際にビルドエラーが出るのでCode Buildを覗いてみると下記のようなエラーが出ます。  
最初はNextAuthのエラーだと思っていましたが、ローカル環境では正常に`next build`できること等から、環境変数が設定されていないことで起こるエラーだと突き詰めました。

```
Step #0 - "Build": ▲ Next.js 14.2.10
Step #0 - "Build":
Step #0 - "Build": Creating an optimized production build ...
Step #0 - "Build": ✓ Compiled successfully
Step #0 - "Build": Linting and checking validity of types ...
Step #0 - "Build": Collecting page data ...
Step #0 - "Build": [91mTypeError: Cannot read properties of undefined (reading 'replace')
Step #0 - "Build": at /usr/src/app/.next/server/app/api/auth/[...nextauth]/route.js:1:4632
Step #0 - "Build": [0m[91m
Step #0 - "Build": [0m[91m> Build error occurred
Step #0 - "Build": [0m[91mError: Failed to collect page data for /api/auth/[...nextauth]
Step #0 - "Build": at /usr/src/app/node_modules/next/dist/build/utils.js:1268:15 {
Step #0 - "Build": type: 'Error'
Step #0 - "Build": }
Step #0 - "Build": The command '/bin/sh -c npm run build' returned a non-zero code: 1
Finished Step #0 - "Build"
ERROR
ERROR: build step 0 "gcr.io/cloud-builders/docker" failed: step exited with non-zero status: 1
```

## 4\. 解決方法

### (1) Code Buildの代入変数に値を設定する

Google CloudのCloud Build > トリガー > 該当のトリガーを「編集」でメニューの下の方に代入変数の設定画面があります。ここに環境変数を設定します。

命名ルールに迷ったら「NEXT\_PUBLIC\_API\_URL」なら「\_NEXT\_PUBLIC\_API\_URL」にするなど\_を先頭につけるのが分かりやすいでしょう。

設定が終わったら変更を保存します。

![Cloud Buildの代入変数の設定画面](https://storage.googleapis.com/zenn-user-upload/bd079492abf2-20240915.jpg)

### (2) cloudbuild.yamlを変更する

ビルド時に.envファイルにCode Buildの代入変数を入れるステップを追加します。  
この例では\_NEXT\_PUBLIC\_API\_URLなどの値をechoで設定していますが、 1で保存した値に読み替えてください。

cloudbuild.yaml

```
steps:
  # .envにCloud Buildの代入変数を埋め込むステップを追加する
  - name: "gcr.io/cloud-builders/docker"
    entrypoint: "bash"
    args:
      - "-c"
      - |
        echo "NEXT_PUBLIC_API_URL=${_NEXT_PUBLIC_API_URL}" >> .env
        echo "NEXT_PUBLIC_BASE_URL=${_NEXT_PUBLIC_BASE_URL}" >> .env
        echo "NEXTAUTH_SECRET=${_NEXTAUTH_SECRET}" >> .env
        echo "NEXTAUTH_URL=${_NEXTAUTH_URL}" >> .env
        echo "JWT_SECRET=${_JWT_SECRET}" >> .env
        echo "TWITTER_CLIENT_ID=${_TWITTER_CLIENT_ID}" >> .env
        echo "TWITTER_CLIENT_SECRET=${_TWITTER_CLIENT_SECRET}" >> .env
        echo "GOOGLE_CLIENT_ID=${_GOOGLE_CLIENT_ID}" >> .env
        echo "GOOGLE_CLIENT_SECRET=${_GOOGLE_CLIENT_SECRET}" >> .env
        echo "FIREBASE_PROJECT_ID=${_FIREBASE_PROJECT_ID}" >> .env
        echo "FIREBASE_PRIVATE_KEY_ID=${_FIREBASE_PRIVATE_KEY_ID}" >> .env
        echo "FIREBASE_CLIENT_EMAIL=${_FIREBASE_CLIENT_EMAIL}" >> .env
        echo "FIREBASE_PRIVATE_KEY=${_FIREBASE_PRIVATE_KEY}" >> .env
        echo "STRIPE_SECRET_KEY=${_STRIPE_SECRET_KEY}" >> .env

        docker build -f Dockerfile -t gcr.io/$PROJECT_ID/【あなたのCloud Runサービス名】:$COMMIT_SHA .

  # ビルドしたコンテナイメージをArtifact Registryにプッシュ
  - name: "gcr.io/cloud-builders/docker"
    args: ["push", "gcr.io/$PROJECT_ID/【あなたのCloud Runサービス名】:$COMMIT_SHA"]

  # Artifact RegistryのコンテナイメージをCloud Runにデプロイ
  - name: "gcr.io/cloud-builders/gcloud"
    args:
      - "run"
      - "deploy"
      - "【あなたのCloud Runサービス名】"
      - "--image"
      - "gcr.io/$PROJECT_ID/【あなたのCloud Runサービス名】:$COMMIT_SHA"
      - "--region"
      - "${_DEPLOY_REGION}"
images:
  - "gcr.io/$PROJECT_ID/【あなたのCloud Runサービス名】:$COMMIT_SHA"
timeout: 900s

options:
  logging: CLOUD_LOGGING_ONLY
```

この際の、Dockerfileは以下のようになっています。

Dockerfile

```
# Use a base image with Node.js LTS
FROM node:lts-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of your application code to the working directory
COPY . .

# Build the Next.js application
RUN npm run build

# Expose the port your app runs on
EXPOSE 3000

# Define the command to run your app
CMD ["npm", "run", "start"]
```

### (3) レポジトリーの変更をプッシュする

レポジトリーの変更をプッシュして、Code Buildのトリガーを発火させます。  
Code Buildのビルドログを読むと先ほどと違って「Environments: .env」とあるように.envファイルを読み込んでいることが分かります。  
ビルドが成功したら、問題は解決です。お疲れ様でした。

```
Step #0: ▲ Next.js 14.2.10
Step #0: - Environments: .env
Step #0:
Step #0: Creating an optimized production build ...
Step #0: ✓ Compiled successfully
Step #0: Linting and checking validity of types ...
Step #0: Collecting page data ...
Step #0: Generating static pages (0/12) ...
Step #0: Generating static pages (3/12)
Step #0: Generating static pages (6/12)
Step #0: Generating static pages (9/12)
Step #0: ✓ Generating static pages (12/12)
Step #0: Finalizing page optimization ...
Step #0: Collecting build traces ...
```

## 5\. 参考資料

-   [Configuring: Environment Variables | Next.js](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables#default-environment-variables)
-   [変数値の置換  |  Cloud Build Documentation  |  Google Cloud](https://cloud.google.com/build/docs/configuring-builds/substitute-variable-values?hl=ja)
-   [デプロイ：Next.js フロントエンドを Cloud Run へデプロイする](https://zenn.dev/waddy/books/graphql-nestjs-nextjs-bootcamp/viewer/deploy_nextjs#next_public_xxx-%E3%81%A8%E3%81%97%E3%81%A6%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0)
-   [Cloud Run上でNext.jsのアプリを動かす方法 | Pilefort](https://pilefort.dev/notes/how-to-deploy-to-cloud-run)