# 支出・投資システム機能詳細設計書

## 概要

このドキュメントは、子供向け金融教育Webアプリケーションの支出および投資システム機能の詳細設計について説明します。ユーザーはこの機能を通じて仮想通貨でアイテムを購入したり、貯金口座に貯金をしたり、投資ゲームを楽しむことができます。

## 機能概要

1. **株取引機能**
    - 疑似通貨で株を購入して運用できる。

## データベース設計
### 保有投資テーブル

| カラム名       | データ型   | 説明                    |
|------------|--------|----------------------------|
| invest_id      | String | 貯蓄ID（主キー）          |
| brand_id       | String | 投資銘柄ID(外部キー)      |
| wallet_id      | Int    | ウォレットID(外部キー)     |
| purchase_price | Int    | 購入時の価格             |
| cash_amount    | Int    | 保有株数                |

### 投資銘柄テーブル

| カラム名       | データ型   | 説明                    |
|------------|--------|----------------------------|
| stock_brand_id | String | 貯蓄ID（主キー）          |
| name           | String | 銘柄名                   |
| type           | String | 種類(個別株、積み立て)     |

## API設計

### 商品一覧取得

- **URL**: `/api/invest/items`
- **メソッド**: GET
- **レスポンス**:
    ```json
    {
        "items": [
            {
                "stock_brand_id": "4475",
                "name": "stock_brand_name",
                "type": "mutual"
            },
            {
                "stock_brand_id": "5562",
                "name": "stock_brand_name",
                "type": "individual"
            },
        ]
    }
    ```

### 商品購入

- **URL**: `/api/invest/purchase`
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
        "stock_brand_id": "4755",
        "quantity": 100
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Purchase successful"
    }
    ```


## 操作の流れ

### 仮想商品店での購入

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant API as APIサーバー
    participant DB as データベース

    User ->> API: GET /api/invest/items
    API ->> DB: 商品一覧取得
    DB -->> API: 商品一覧
    API -->> User: 商品一覧表示 (items)

    User ->> API: POST /api/invest/purchase (item_id)
    API ->> DB: 購入処理
    DB -->> API: 購入成功
    API -->> User: 購入完了 (message)