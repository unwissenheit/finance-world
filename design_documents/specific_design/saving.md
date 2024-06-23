# 保護者向け機能詳細設計書

## 概要

このドキュメントは、Finance Worldの貯蓄機能の詳細設計について説明します。銀行を選択して貯蓄することで利子を受け取れること、入金と出金を管理する機能が提供されます。

## 機能概要

1. **貯金口座**
    - 一定額を貯金し、利子を付ける機能。

## データベース設計

### 保有貯蓄テーブル

| カラム名       | データ型   | 説明                    |
|------------|--------|----------------------------|
| saving_id      | String | 貯蓄ID（主キー）          |
| bank_id        | String | 銀行ID(外部キー)         |
| wallet_id      | Int    | ウォレットID(外部キー)     |
| amount         | Int    | 購入時の価格             |
| start_date     | Date   | 貯蓄開始日(利息をつけるため)|

### 銀行テーブル

| カラム名       | データ型   | 説明                    |
|------------|--------|----------------------------|
| bank_id        | Int    | 銀行ID（主キー）          |
| name           | String | 銀行名                   |
| interest       | Double | 利息                    |

## API設計
### 銀行入金

- **URL**: `/api/saving/deposit`
- **メソッド**: POST
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **リクエストボディ**:
    ```json
    {
        "bank_id": "1",
        "amount": 100,
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Complete deposit"
    }
    ```

### 銀行出金

- **URL**: `/api/saving/withdraw`
- **メソッド**: POST
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **リクエストボディ**:
    ```json
    {
        "bank_id": "1",
        "amount": 100,
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Complete withdraw"
    }
    ```


## 操作の流れ

### 親の管理ダッシュボードでの子供の活動監視

```mermaid
sequenceDiagram
    participant Child as 子ユーザー
    participant API as APIサーバー
    participant DB as データベース

    Child ->> API: GET /api/saving/deposit
    API ->> DB: 銀行に入金
    DB -->> API: 成功
    API -->> Child: 入金成功(message)

    Child ->> API: GET /api/saving/withdraw
    API ->> DB: 銀行から出金
    DB -->> API: 成功
    API -->> Child: 出金成功(message)