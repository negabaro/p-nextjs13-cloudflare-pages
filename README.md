# Next.js Edge Runtime + Cloudflare Pages


## 目的 

nextjs13をcloudflare pagesにデプロイする

方法は二つあって
githubリポジトリー連携機能を利用するのとwrangler cliを利用して実行する方法がある

このリポジトリでは後者の方法でデプロイしてる

## 手順

### build
デプロイ時、next-on-pagesこのパッケージを利用するので
localで以下のコマンドを実行する

yarn && npx @cloudflare/next-on-pages@1

成功した場合、以下のようになる
<img width="804" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/cd87d0c1-fe76-40b5-9b4a-1ab30eb8c52a">

### deploy



`cloudflareのアカウントid`と`cloudflareのproject name`
そして `secrets.CLOUDFLARE_API_TOKEN`をgithubリポジトリシクレート設定に入れる

デプロイ時以下のコマンド実行する(wrangler cliを利用)

```
CLOUDFLARE_ACCOUNT_ID=`cloudflareのアカウントid` CLOUDFLARE_API_TOKEN=${{ secrets.CLOUDFLARE_API_TOKEN }} npx wrangler pages publish .vercel/output/static  --project-name=`cloudflareのproject name`
```

<img width="800" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/a3b1b0d6-6c98-47b8-8281-49cbd766a4be">

#### cloudflareのアカウントidは？

workers & pages -> overviewの右にありますよ

<img width="985" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/1f31acfb-2a58-4fec-9930-9867ae46e69a">

#### cloudflareのproject nameは？

これのこと
<img width="998" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/fe26827a-9933-4b00-bc0b-6776ebe6dec7">

デプロイするプロジェクト名ね

#### CLOUDFLARE_API_TOKENとは？

マイプロフィール -> APIトークンから発行できる

<img width="1423" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/ddaff451-3e52-4d14-92e0-295c7453f08c">

### 互換性フラグ設定

workers & pages -> projectクリック -> 設定 -> Functionsにあるよ
nodejs_compact入れないとエラーが出たので

<img width="971" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/089d676b-5e5c-47b2-92e5-e3ed570c73c7">

### ビルドの構成

これはおそらく関係ないかもだが、念の為

<img width="833" alt="image" src="https://github.com/cloudflare/wrangler-action/assets/4640346/54204907-b337-4ae1-aac6-a687da13a49a">

### github actions経由でcli実行する方法

.github/workflows/publish.ymlを参考してください。

`cloudflare/wrangler-action@2.0.0`とか`cloudflare/pages-action@1`だとうまく動かなかったので直接cliを叩いてる