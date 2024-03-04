---
title: "About Fuse: GraphQL and BFF"
emoji: "🌏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["fuse", "typescript", "graphql"]
published: true
---
Fuse について。

https://fusedata.dev/

GraphQL API のクライアントを生成できる Framework らしい。

BFF として、動かすこともできそう。例えば、Cloudflare や Container で。

認知度はまだまだ。

[![Star History Chart](https://api.star-history.com/svg?repos=StellateHQ/fuse&type=Date)](https://star-history.com/#StellateHQ/fuse&Date)

## Getting Started
- 基本的に手順の通り。
```bash
npx create-fuse-app
npm install
npm install --save fuse graphql
npm install --save-dev @graphql-typed-document-node/core
npm i --save-dev @0no-co/graphqlsp
npx fuse dev
```

```bash
node ➜ /workspaces/fuse-dev (main) $ npx fuse dev
Server listening on http://localhost:4000/graphql
```

![image](/images/bc5f9cbbbc181c-a.png)

## Define the schema
- Fuse で定義した `schema.graphqls` を backend と schema を共有するケース。

### Fix `Types`
- 今回は簡単に field を追加する。

https://github.com/danny-yamamoto/fuse-dev/blob/a30454ce7756795b83d044523d7b88eba8178dab/types/User.ts#L7

https://github.com/danny-yamamoto/fuse-dev/blob/a30454ce7756795b83d044523d7b88eba8178dab/types/User.ts#L25

https://github.com/danny-yamamoto/fuse-dev/blob/a30454ce7756795b83d044523d7b88eba8178dab/types/User.ts#L36

### Run the Fuse API server
- 起動する
```bash
node ➜ /workspaces/fuse-dev (main) $ npx fuse dev
Server listening on http://localhost:4000/graphql
```

- schema が追加される

https://github.com/danny-yamamoto/fuse-dev/blob/a30454ce7756795b83d044523d7b88eba8178dab/schema.graphql#L29

> Fake function to fetch users. In real applications, this would talk to an underlying REST API/gRPC service/third-party API/…
> 
> ユーザーをフェッチするための偽の関数。実際のアプリケーションでは、これは基礎となるREST API/gRPCサービス/サードパーティAPI/...と話をするでしょう。

あとは、REST API を呼ぶなり、gRPC を呼ぶなり好きにしてくれと。

以上

## BTW
https://twitter.com/dec12171985/status/1727611120167637487?s=20

プロスポーツ選手は多くが自己破産すると聞きます。アメフトや NBA など。

そんな中、ロビン・ファン・ペルシが良い父になっていたことに驚きを隠せません。

なぜなら、現役のプレースタイルや言動を見ていた者として、プレーはすごい。けど、性格は？？という感じでした。

ロビン・ファン・ペルシ、ごめんなさい ⚽️

私もこんな父になりたい。
