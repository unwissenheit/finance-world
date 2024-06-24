# 仮想通貨管理機能詳細設計書

## 概要

このドキュメントは、Finance Worldの仮想通貨管理機能の詳細設計について説明します。ユーザーはこの機能を通じて仮想通貨の残高を確認し、取引履歴を管理することができます。

## 機能概要

1. **疑似通貨ウォレット**
    - 現在の残高を表示。
    
2. **取引履歴**
    - 入出金、送金、支出などの履歴を確認。

## データベース設計

### ウォレットテーブル

| カラム名       | データ型   | 説明               |
|------------|--------|------------------------|
| wallet_id  | String | ウォレットID（主キー）     |
| user_id    | String | ユーザーID(子ユーザーに限る)|
| cash_amount| Int    | 現在の残高                |


### 貯蓄取引履歴テーブル

| カラム名       | データ型   | 説明                 |
|------------|--------|--------------------|
| id             | String | 貯蓄取引ID（主キー）   |
| saving_id      | String | 貯蓄ID(外部キー)      |
| type           | String | 取引タイプ（入金、出金） |
| amount         | Number | 金額                 |
| date           | Date   | 取引日時             |

### 投資取引履歴テーブル

| カラム名       | データ型   | 説明                 |
|------------|--------|--------------------|
| id             | String | 投資取引ID（主キー）    |
| invest_id      | String | 投資ID(外部キー)      |
| type           | String | 取引タイプ（購入、売却） |
| quantity       | Number | 株数                 |
| date           | Date   | 取引日時             |

## API設計

### 残高取得

- **URL**: `/api/wallet/balance`
- **メソッド**: GET
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **レスポンス**:
    ```json
    {
        "balance": 1000
    }
    ```

### 株式取引履歴取得

- **URL**: `/api/invest/transactions`
- **メソッド**: GET
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **レスポンス**:
    ```json
    {
        "transactions": [
            {
                "transaction_id": "1",
                "invest_id": "1",
                "type": "buy",
                "quantity": 500,
                "date": "2024-06-21T12:00:00Z",
            },
            {
                "transaction_id": "2",
                "invest_id": "1",
                "type": "sell",
                "amount": 200,
                "date": "2024-06-22T15:00:00Z",
                "description": "Bought a toy"
            }
        ]
    }
    ```
### 貯蓄取引履歴取得

- **URL**: `/api/saving/transactions`
- **メソッド**: GET
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **レスポンス**:
    ```json
    {
        "transactions": [
            {
                "transaction_id": "1",
                "saving_id": "1",
                "type": "deposit",
                "amount": 500,
                "date": "2024-06-21T12:00:00Z",
            },
            {
                "transaction_id": "2",
                "saving_id": "1",
                "type": "withdraw",
                "amount": 200,
                "date": "2024-06-22T15:00:00Z",
            }
        ]
    }
    ```

## 操作の流れ

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant API as APIサーバー
    participant DB as データベース

    User ->> API: GET /api/wallet/balance (token)
    API ->> DB: ウォレット残高を取得
    DB -->> API: 現在の残高
    API -->> User: 残高表示 (balance)

    User ->> API: GET /api/wallet/transactions (token)
    API ->> DB: 取引履歴を取得
    DB -->> API: 取引履歴 (transactions)
    API -->> User: 取引履歴表示 (transactions)