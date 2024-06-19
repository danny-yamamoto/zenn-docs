---
title: "Custom pipelines: AWS Amplify Gen 2"
emoji: "🛠️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["amplify", "codebuild", "nestjs"]
published: true
---
Custom pipeline について。

この文章を書くにあたっての要件は、以下の通り。
- Backend API を ECS に deploy する。
- Container Image の build を含めて pipeline をまとめたい。
    - これは必須ではない。build してある image を使う道もある。

## Custom pipelines
- Amplify CI 以外に、GitHub Acitons や Amazon CodeCatalyst での pipeline 実装が紹介されている。Official Docs では。 [^1]
- Amplify CI は Gen 1 から CI 内での Docker Commnad は実行できない。6月19日時点。[^2]
- サービスロールに必要な管理ポリシーを添付する。 `arn:aws:iam::aws:policy/service-role/AmplifyBackendDeployFullAccess`
- Frontend Application の pipeline と連携する場合は、Webhook で起動させる。
- 
```TypeScript: backend.ts
// ECS
// README at: https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs-readme.html
taskDefinition.addContainer("fargate-app", {
  image: ecs.ContainerImage.fromAsset("./"),
  portMappings: [
    {
      containerPort: 80, // コンテナ内部のポート
      protocol: ecs.Protocol.TCP,
    },
  ],
});
```

```yaml: buildspec.yml
version: 0.2

phases:
  build:
    commands:
      - echo Build started on `date`
      - echo set up the build-spec when using CodeBuild...
      - export CI=1
      - npm ci
      - npx ampx pipeline-deploy --branch $BRANCH_NAME --app-id $AMPLIFY_APP_ID --debug
```

## Tips
- `--debug` option を付与すると、より詳細な log が出る。

## Conclusion
「環境にこだわれ」

https://x.com/rakuencardman/status/1802987364433953250

これに同意する。

なぜなら、人を変えることは難しい。

自分を変えるのは自分との向き合い方次第。

組織は人の集まり。そうなると、環境にこだわるしかない。

組織を変える努力は、取り組んだほどの得られるものは少ないと思う。

今いる場所が自分が成長できる場所なのか常に考える。

そのために、毎月振り返りをスケジュール化した。


[^1]: https://docs.amplify.aws/react/deploy-and-host/fullstack-branching/custom-pipelines/
[^2]: https://github.com/aws-amplify/amplify-hosting/issues/2550
