---
title: 'Kid Doodle AI'
description: '子供のお絵描きAIアプリ'
pubDate: '2026-06-28'
tags: ['AWS', 'Serverless', 'Image-gen']
url: 'https://dvixfewgj9svk.cloudfront.net/'
github: 'Private repositories'
---

## 概要

子供向けの「お絵描き → AIリアル化」体験するシステム

## Requirements
- 子供の遊び目的なので、予算は月 5 ドル程度
- 画像はユーザーごとに約 100 枚保存
- 保存期限はなし
- 登録制で運用（クローズドベータは招待制）
- 登録済みユーザーも回数制限を設ける
- OpenAI API の利用料金が爆発しないよう制限する

## Architecture

- フロント: S3 Static Website Hosting + CloudFront
- API: API Gateway HTTP API + Lambda (Python)
- 認証: Cognito User Pool（招待制は Admin Create User only）
- 画像保存: S3 (ユーザー別プレフィックス)
- メタ情報 / 利用制限: DynamoDB
- 監視 / 料金防衛: CloudWatch, AWS Budgets

**Architecture Diagram**
```mermaid
flowchart LR
  user[User] -->|HTTPS| cf[CloudFront]
  cf --> s3web[S3 Static Site]
  s3web -->|API Call| apigw[API Gateway HTTP API]
  apigw -->|JWT| cognito[Cognito User Pool]
  apigw --> lambda[Lambda FastAPI]
  lambda --> openai[OpenAI API]
  lambda --> s3img[S3 Images]
  lambda --> ddb[DynamoDB]
  s3img -->|Signed URL| s3web
  lambda --> cw[CloudWatch Logs]
  cw --> budgets[AWS Budgets]
```

## 工夫

- コストと安定の拘り。静的ホスティングやDynamoDBの活用など、サーバーレスにこだわり運用コストと安定稼働の両立を実現。

- 画像生成はBedrockで複数モデルで比較検討。結論OpenAIに落ち着いた

## 学び

- はじめての仕様駆動開発。途中でCodexからClaude Codeに切り替えしたが、仕様書のおかげで全く苦労なく移行できた。

- 得意領域外の難しさ。自分の得意とするAWS領域はいくらでも口出しできたが、アプリケーションレイヤーは出てきたアウトプットに対する評価ができず言いなり状態。ユーザー目線でフィードバックを返すことしかできていなかった。