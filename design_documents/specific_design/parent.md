# 保護者向け機能詳細設計書

## 概要

このドキュメントは、Finance Worldの保護者向け機能の詳細設計について説明します。
保護者はこの機能を通じて子供の活動を監視し、報酬やタスクを設定することができます。

## 機能概要

1. **親の管理ダッシュボード**
    - 子供の活動を監視。
    - 報酬やタスクを設定。
    
## データベース設計

### タスクテーブル

| カラム名       | データ型   | 説明                 |
|------------|--------|--------------------|
| task_id    | String | タスクID（主キー）       |
| user_id    | String | ユーザーID             |
| parent_id  | String | 保護者ID             |
| description| String | タスクの説明            |
| reward     | Number | 報酬                 |
| status     | String | タスクの状態（未完了、完了） |
| due_date   | Date   | 締め切り日              |

## API設計

### 子供のアカウント作成

- **URL**: `/api/parent/create_child_user`
- **メソッド**: POST
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer parent_jwt_token"
    }
    ```
- **リクエストボディ**:
    ```json
    {
        "name": "child_user_name",
        "password": "child_user_password"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "User registered successfully",
        "child_user_id": "123456789"
    }
    ```
### タスク設定

- **URL**: `/api/parent/set_task`
- **メソッド**: POST
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer parent_jwt_token"
    }
    ```
- **リクエストボディ**:
    ```json
    {
        "child_user_id": "123456789",
        "description": "Clean your room",
        "reward": 50,
        "due_date": "2024-07-01"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Task set successfully"
    }
    ```

### 子供の活動取得

- **URL**: `/api/parent/children_activities`
- **メソッド**: GET
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer parent_jwt_token"
    }
    ```
- **リクエストボディ**:
    ```json
    {
        "child_user_id": "123456789",
    }
    ```
- **レスポンス**:
    ```json
    {
        "activities": [
            {
                "activity_id": "1",
                "child_user_id": "123456789",
                "description": "Completed math homework",
                "date": "2024-06-21T12:00:00Z"
            },
            {
                "activity_id": "2",
                "user_id": "child2",
                "description": "Finished reading a book",
                "date": "2024-06-22T15:00:00Z"
            }
        ]
    }
    ```


## 操作の流れ

### 親の管理ダッシュボードでの子供の活動監視

```mermaid
sequenceDiagram
    participant Parent as 保護者
    participant API as APIサーバー
    participant DB as データベース

    Parent ->> API: POST /api/parent/create_user (token)
    API ->> DB: 子ユーザーの作成
    DB -->> API: 子ユーザーの作成完了
    API -->> Parent: 子ユーザーの作成完了 (message)

    Parent ->> API: POST /api/parent/set_task (token)
    API ->> DB: タスクの追加 
    DB -->> API: タスクの追加完了
    API -->> Parent: タスクの追加完了 (message)
    
    Parent ->> API: GET /api/parent/children_activities (token)
    API ->> DB: 子供の活動を取得
    DB -->> API: 子供の活動一覧
    API -->> Parent: 活動一覧表示 (activities)