# 源内AI ワンクリックデプロイ

> **⚠️ 本リポジトリはデジタル庁の公式プロジェクトではありません。** 個人が利便性向上のために作成した非公式のデプロイ支援ツールです。動作保証はありません。利用は自己責任でお願いします。問題が発生した場合は、源内AI本体のリポジトリではなく[本リポジトリのIssue](https://github.com/hide-G/gennai-one-click-deploy/issues)にご報告ください。

[デジタル庁の源内AI](https://github.com/digital-go-jp/genai-web)をAWS環境にワンクリックでデプロイするためのCloudFormationテンプレート集です。

## デプロイ可能なコンポーネント

### 1. 源内Web（AIインターフェース）

GenU（Generative AI Use Cases）ベースのWebアプリケーション。チャット、文章生成、翻訳、画像生成などの汎用AI機能を提供します。

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?templateURL=https://s3.ap-northeast-1.amazonaws.com/gennai-one-click-deploy/gennai-web-deployment.yaml&stackName=GennaiWeb)

### 2. Query Expansion RAG API

Bedrock Knowledge Base + OpenSearch Serverlessを使ったクエリ拡張RAG APIテンプレート。源内Webの行政実務用AIアプリとして登録可能です。

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?templateURL=https://s3.ap-northeast-1.amazonaws.com/gennai-one-click-deploy/gennai-rag-deployment.yaml&stackName=GennaiRAG)

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

## CloudFormation Quick Create Link について

このリポジトリの「Launch Stack」ボタンは、AWS CloudFormation Quick Create Link（1-Click Deploy）の仕組みを利用しています。S3に配置したCloudFormationテンプレートのURLをパラメータとして埋め込むことで、ユーザーがボタン1つでスタック作成画面に遷移できます。

詳細については、AWS公式ドキュメントをご参照ください：
- [クイック作成リンクを使用したスタックの作成](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/cfn-console-create-stacks-quick-create-links.html)

## 上流リポジトリの更新について

本テンプレートはデプロイ実行時に、デジタル庁の公式リポジトリから最新コードを `git clone` で取得します。

```yaml
# 源内Web
- git clone https://github.com/digital-go-jp/genai-web.git

# RAG API
- git clone https://github.com/digital-go-jp/genai-ai-api.git
```

そのため、**デプロイボタンを押した時点での最新版が常に自動的に反映されます。** 本リポジトリ側を更新しなくても、源内AI本体の改善・新機能が取り込まれます。

ただし、上流で破壊的変更（ディレクトリ構成の変更、設定ファイルのフォーマット変更など）があった場合、テンプレート内のパッチ処理が失敗しデプロイがエラーになる可能性があります。その場合は本リポジトリのテンプレートを修正する必要があります。

## 注意事項

- **本リポジトリはデジタル庁の公式プロジェクトではありません。** 個人が利便性向上のために作成した非公式のデプロイ支援ツールです。
- 本テンプレートの利用は自己責任でお願いします。動作保証はありません。
- 源内AI本体（genai-web / genai-ai-api）の更新により、デプロイが失敗する場合があります。
- 問題が発生した場合は、源内AI本体のリポジトリではなく、[本リポジトリのIssue](https://github.com/hide-G/gennai-one-click-deploy/issues)にご報告ください。
- AWSリソースの利用料金はご自身のAWSアカウントに請求されます。特にOpenSearch Serverlessは高額なため、検証後は速やかに削除してください。

## ライセンス

このリポジトリのテンプレートはMITライセンスです。
源内AI本体のライセンスは各リポジトリを参照してください。
