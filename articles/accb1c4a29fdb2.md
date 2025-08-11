---
title: "AgentCore Browser を使用したエージェントの開発"
emoji: "🤖"
type: "tech" # In this page, I'll break down how to create tech in different ways"
topics: ["agentcore", "ai", "llm", "aws"]
published: false
---

## はじめに

この blog では、AgentCore Browser を使用したエージェントの開発について書きます。AI エージェントの開発環境は日々進化しており、適切なツールとフレームワークの選択が開発の効率と品質に影響すると考えています。

今回は、AgentCore Browser を使用して、自然言語での指示を与えることで、エージェントが Web アプリを操作する方法を紹介します。

## AgentCore の概要

https://aws.amazon.com/jp/blogs/news/introducing-amazon-bedrock-agentcore-securely-deploy-and-operate-ai-agents-at-any-scale/

AgentCore Runtime を筆頭に、AgentCore Browser や AgentCore Memory などエージェント開発に必要な機能を包括的に提供しています。

## AgentCore Browser

![agentcore browser](https://docs.aws.amazon.com/images/bedrock-agentcore/latest/devguide/images/browser-tool.png)

エージェントが Web アプリ操作を行うための Managed Browser を提供します。もし、AgentCore Browser と同等の処理を実装する場合、以下のような処理が必要になると思います。
- Browser 操作用のインスタンスを起動
- インスタンスへの接続
- クロニウムベースのブラウザを起動

AgentCore Browser はこれらの処理をすべてマネージドで提供します。

以下のコードは、AgentCore Browser を使用して、特定の Web サイトにログインするエージェントの例です。

```python
        try:
            client = BrowserClient(region)
            logger.debug("BrowserClient作成完了")

            client.start()
            logger.debug("BrowserClient起動完了")

            ws_url, headers = client.generate_ws_headers()
            logger.debug(f"WebSocket URL取得完了: {ws_url[:50]}...")

        except Exception as e:
            logger.error(f"BrowserClient初期化エラー: {e}")
            return f"failed: ブラウザクライアント初期化失敗 - {str(e)}"

        try:
            with sync_playwright() as playwright:
                logger.debug("Playwright WebSocket接続試行中...")
                browser = playwright.chromium.connect_over_cdp(
                    endpoint_url=ws_url, headers=headers
                )
                logger.debug("Playwright WebSocket接続成功")
                default_context = browser.contexts[0]
                page = default_context.pages[0]

                context = browser.new_context(locale="ja-JP")
                page = context.new_page()
                page.set_extra_http_headers(
                    {"Accept-Language": "ja-JP,ja;q=0.9,en;q=0.8"})

                # サイトにアクセス
                page.goto(target_url)
                page.wait_for_load_state("networkidle")
                page.wait_for_timeout(2000)

                # ログイン処理
                page.fill(
                    'input[name*="LoginID"], input[type="text"]', login_id)
                page.fill(
                    'input[name*="PassWd"], input[type="password"]', password)
                page.click(
                    'img[onclick="FMSubmit()"], input[type="submit"], button[type="submit"]')

                page.wait_for_load_state("networkidle")
                page.wait_for_timeout(2000)
```

## Summary

この 1 ヶ月、AgentCore と Agent Engine という 2 つのエージェント開発基盤の比較検証を行ってきました。どちらの基盤が優れているかは「何を求めるか」によって変わると思います。AgentCore は、エージェントに必要な機能を提供することに特化していると思います。

## Closing

現職に入社してから関わってきたプロダクト開発が一つの区切りを迎えました。このプロダクト開発から色々な意味で学びを得ました。1つ言えることは人それぞれにとって時間は有限であるということです。限られた時間の中で、どのように「自分の残りの時間」を使うかは自分次第だと思います。そういう意味で、今までのチームを抜け、新しいプロダクト開発チームに参加するという決断は重要でした。なぜなら、僕にとってのソフトウェア エンジニアとして生きる意味は、自分が関わったプロダクトが「多くの人に使われ、喜ばれる事」です。これからも「自分の残りの時間」を何に使うかを考えながら、特定の誰かのためではなく、より多くの人に喜ばれるプロダクト開発に取り組んでいきたいと思います。
