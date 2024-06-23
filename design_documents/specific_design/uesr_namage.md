# ユーザー登録・管理機能詳細設計書

## 概要

このドキュメントは、Finance Worldのユーザー登録および管理機能の詳細設計について説明します。ユーザーはこの機能を通じてアカウントを作成し、プロフィールを管理することができます。

## 機能概要

1. **ユーザー登録・ログイン**
    - ユーザー名、パスワード、保護者のメールアドレスなどの登録。
    - 登録完了後、ログインしてアプリケーションを利用開始。
    - 保護者ユーザーに紐付く子ユーザーは、アプリケーション内で作成する。
    
2. **ユーザープロフィール**
    - プロフィールの編集（ユーザー名、アバターの設定など）。
    - ユーザー情報の確認と更新。
    - 子ユーザーのプロフィールは、子ユーザー・保護者ユーザーのどちらも編集可能。

## データベース設計

### 保護者テーブル

| カラム名         | データ型   | 説明             |
|--------------|--------|--------------------|
| id           | String | ユーザーID（主キー）     |
| name         | String | ユーザー名              |
| password     | String | パスワード（ハッシュ化）  |
| email        | String | 保護者のメールアドレス    |
| avatar_url   | String | アバターのURL          |
| created_at   | Date   | アカウント作成日時       |
| updated_at   | Date   | 最終更新日時           |
| is_deleted   | Boolean| 削除済みのユーザか      |

### 子テーブル
| カラム名         | データ型   | 説明                |
|--------------|--------|------------------------|
| id           | int    |　子ユーザID(主キー)        |
| parent_id    | String | ユーザーID（外部キー）     |
| wallet_id    | String | ウォレットID（外部キー）    |
| name         | String | ユーザー名                 |
| password     | String | パスワード（ハッシュ化しない）|

## API設計

### ユーザー登録(保護者)

- **URL**: `/api/parent/register`
- **メソッド**: POST
- **リクエストボディ**:
    ```json
    {
        "username": "username",
        "password": "password",
        "parent_email": "parent@example.com"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "User registered successfully",
        "parent_user_id": "123456789"
    }
    ```

### ユーザー登録(子)

- **URL**: `/api/child/register`
- **メソッド**: POST
- **リクエストボディ**:
    ```json
    {
        "parent_id": "123456789",
        "username": "username",
        "password": "password",
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "User registered successfully",
        "child_user_id": "123456789"
    }
    ```
### ユーザーログイン

- **URL**: `/api/login`
- **メソッド**: POST
- **リクエストボディ**:
    ```json
    {
        "username": "username",
        "password": "password"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Login successful",
        "token": "jwt_token"
    }
    ```

### ユーザー削除

- **URL**: `/api/profile`
- **メソッド**: DELETE
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "user deleted"
    }
    ```
### プロフィール取得

- **URL**: `/api/profile`
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
        "user_id": "123456789",
        "username": "username",
        "parent_email": "parent@example.com",
        "avatar_url": "https://example.com/avatar.png"
    }
    ```

### プロフィール更新

- **URL**: `/api/profile`
- **備考**: avatar_urlだけでなく、他の項目(名前、パスワードなど)も更新可能
- **メソッド**: PUT
- **ヘッダー**: 
    ```json
    {
        "Authorization": "Bearer jwt_token"
    }
    ```
- **リクエストボディ**:
    ```json
    {
        "username": "new_username",
        "avatar_url": "https://example.com/new_avatar.png"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Profile updated successfully"
    }
    ```

## 操作の流れ(シーケンス図)
```mermaid
sequenceDiagram
    participant User as ユーザー
    participant API as APIサーバー
    participant DB as データベース

    User ->> API: POST /api/register (username, password, age, parent_email)
    API ->> DB: ユーザー情報を保存
    DB -->> API: 保存成功
    API -->> User: ユーザー登録完了 (user_id)

    User ->> API: POST /api/login (username, password)
    API ->> DB: ユーザー認証
    DB -->> API: 認証成功 (jwt_token)
    API -->> User: ログイン成功 (token)

    User ->> API: GET /api/profile (token)
    API ->> DB: ユーザー情報取得
    DB -->> API: ユーザー情報 (username, age, parent_email, avatar_url)
    API -->> User: プロフィール情報

    User ->> API: PUT /api/profile (token, new_username, new_avatar_url)
    API ->> DB: プロフィール更新
    DB -->> API: 更新成功
    API -->> User: 更新完了