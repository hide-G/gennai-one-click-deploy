# 源内AI ワンクリックデプロイ

[デジタル庁の源内AI](https://github.com/digital-go-jp/genai-web)をAWS環境にワンクリックでデプロイするためのCloudFormationテンプレート集です。

## デプロイ可能なコンポーネント

### 1. 源内Web（AIインターフェース）

GenU（Generative AI Use Cases）ベースのWebアプリケーション。チャット、文章生成、翻訳、画像生成などの汎用AI機能を提供します。

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?templateURL=https://s3.ap-northeast-1.amazonaws.com/YOUR_BUCKET/gennai-web-deployment.yaml&stackName=GennaiWeb)

> ⚠️ テンプレートをS3にアップロード後、上記URLの `YOUR_BUCKET` を実際のバケット名に置き換えてください。

### 2. Query Expansion RAG API

Bedrock Knowledge Base + OpenSearch Serverlessを使ったクエリ拡張RAG APIテンプレート。源内Webの行政実務用AIアプリとして登録可能です。

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?templateURL=https://s3.ap-northeast-1.amazonaws.com/YOUR_BUCKET/gennai-rag-deployment.yaml&stackName=GennaiRAG)

> ⚠️ テンプレートをS3にアップロード後、上記URLの `YOUR_BUCKET` を実際のバケット名に置き換えてください。

## 前提条件

- AWSアカウント
- Amazon Bedrockのモデルアクセスが有効化されていること
  - Claude Sonnet 4.5 / Claude Haiku 4.5
  - Amazon Nova Lite
  - Amazon Nova Canvas（画像生成用）

## デプロイの流れ

1. 「Launch Stack」ボタンをクリック
2. AWSコンソールでパラメータを入力（メールアドレス、IP制限等）
3. 「スタック作成」をクリック
4. CodeBuildが自動的にCDKデプロイを実行（20〜45分）
5. 完了後、メールでアクセスURLが通知される

## コスト目安

| コンポーネント | 月額目安 |
|---|---|
| 源内Web（CloudFront + Cognito + Lambda + DynamoDB） | $10〜30 |
| RAG API（OpenSearch Serverless 2 OCU） | $350〜 |
| Bedrock（従量課金） | 利用量による |
| CodeBuild（デプロイ時のみ） | $0.01〜0.02 |

> ⚠️ OpenSearch Serverlessは最低2 OCU（約$350/月）のコストが発生します。検証後は速やかに削除してください。

## 削除方法

1. AWSコンソールでCloudFormationスタックを削除
2. CDKで作成されたスタックは手動削除が必要な場合があります

詳細は各テンプレートの説明を参照してください。

## 参考リンク

- [源内Web（genai-web）](https://github.com/digital-go-jp/genai-web)
- [源内AIアプリ（genai-ai-api）](https://github.com/digital-go-jp/genai-ai-api)
- [デジタル庁 プロジェクト「源内」構想紹介](https://digital-gov.note.jp/)

## ライセンス

このリポジトリのテンプレートはMITライセンスです。
源内AI本体のライセンスは各リポジトリを参照してください。
