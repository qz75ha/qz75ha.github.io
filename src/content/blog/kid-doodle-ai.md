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
- 予算は月 5 ドル程度
- 画像はユーザーごとに約 100 枚保存
- 保存期限はなし
- 登録制で運用（クローズドベータは招待制）
- 登録済みユーザーも回数制限を設ける
- OpenAI API の利用料金が爆発しないよう制限する
- 独自ドメインは後回しでよい

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

## 工夫した点

- コストと安定の拘り。静的ホスティングやDynamoDBの活用など、サーバーレスにこだわり運用コストと安定稼働の両立を実現。