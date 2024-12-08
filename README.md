# Dify Custom Tool OpenAPI スキーマガイド

## 目次
- [概要](#概要)
- [基本構造](#基本構造)
- [詳細仕様](#詳細仕様)
- [実装例](#実装例)
- [セキュリティ考慮事項](#セキュリティ考慮事項)
- [トラブルシューティング](#トラブルシューティング)

## 概要

このガイドでは、Difyプラットフォーム用のカスタムツールを開発する際のOpenAPI 3.1.0スキーマの詳細な仕様を解説します。

## 基本構造

OpenAPIスキーマの基本構造は以下のセクションで構成されています：

### 1. バージョンと基本情報

yamlファイルの場合
```
openapi: "3.1.0"
info:
  title: "Custom Tool API"
  description: "Difyプラットフォーム用カスタムツール"
  version: "1.0.0"
```

基本情報の説明：

openapi: OpenAPI仕様のバージョン（必須）
info: APIの基本情報を含むセクション（必須）

title: API名（必須）
description: API概要
version: APIバージョン（必須）



2. サーバー設定

```
yamlCopyservers:
  - url: "https://api.example.com/v1"
    description: "本番環境"
  - url: "https://staging-api.example.com/v1"
    description: "ステージング環境"
```
サーバー設定の説明：

servers: APIサーバーの接続情報

url: エンドポイントのベースURL
description: 環境の説明



詳細仕様
1. パス定義
```
yamlCopypaths:
  /weather/{city}:
    get:
      operationId: "getWeatherForecast"
      summary: "天気予報取得"
      parameters:
        - name: "city"
          in: "path"
          required: true
```
パス定義の説明：

paths: 利用可能なエンドポイントを定義
operationId: 操作の一意識別子（必須）
parameters: リクエストパラメータの定義

2. コンポーネント
```
yamlCopycomponents:
  schemas:
    WeatherForecast:
      type: "object"
      properties:
        temperature:
          type: "number"
  securitySchemes:
    apiKeyAuth:
      type: "apiKey"
      in: "header"
```

コンポーネントの説明：

schemas: 再利用可能なデータモデル
securitySchemes: 認証方式の定義

実装例
基本的なエンドポイント定義
```
yamlCopy/users:
  get:
    operationId: "getUsers"
    summary: "ユーザー一覧取得"
    responses:
      '200':
        description: "成功"
        content:
          application/json:
            schema:
              type: "array"
              items:
                $ref: "#/components/schemas/User"
```
セキュリティ考慮事項
1. 認証設定
```
yamlCopysecurity:
  - apiKeyAuth: []
```
認証設定のポイント：

API全体のセキュリティ要件を定義
個別エンドポイントでオーバーライド可能

2. データ検証
```
yamlCopycomponents:
  schemas:
    User:
      required:
        - id
        - email
      properties:
        email:
          format: "email"
```
トラブルシューティング
よくあるエラー
1. スキーマ検証エラー
```
yamlCopy# 誤った例
openapi: 3.1.0  # クォートなし
```

# 正しい例
```
openapi: "3.1.0"  # クォートあり
```
2. 必須フィールドの欠落
```
yamlCopy# 誤った例
info:
  description: "説明"
```

# 正しい例
```
info:
  title: "API名"  # titleは必須
  description: "説明"
```
推奨プラクティス
1. バージョニング

セマンティックバージョニングの使用
下位互換性の維持

2. ドキュメント

詳細な説明の提供
使用例の記載

3. エラーハンドリング

標準的なHTTPステータスコードの使用
明確なエラーメッセージの定義

関連リソース

OpenAPI公式ドキュメント
Difyドキュメント


このガイドは継続的に更新されます。最新の情報は公式ドキュメントを参照してください。