# ユーザー登録・管理機能詳細設計書

## 概要

このドキュメントは、子供向け金融教育Webアプリケーションのユーザー登録および管理機能の詳細設計について説明します。ユーザーはこの機能を通じてアカウントを作成し、プロフィールを管理することができます。

## 機能概要

1. **ユーザー登録・ログイン**
    - ユーザー名、パスワード、年齢、保護者のメールアドレスなどの登録。
    - 登録完了後、ログインしてアプリケーションを利用開始。
    
2. **ユーザープロフィール**
    - プロフィールの編集（ユーザー名、アバターの設定など）。
    - ユーザー情報の確認と更新。

## データベース設計

### ユーザーテーブル

| カラム名         | データ型   | 説明                 |
|--------------|--------|--------------------|
| user_id      | String | ユーザーID（主キー）       |
| username     | String | ユーザー名              |
| password     | String | パスワード（ハッシュ化）    |
| age          | Number | 年齢                 |
| parent_email | String | 保護者のメールアドレス      |
| avatar_url   | String | アバターのURL          |
| created_at   | Date   | アカウント作成日時        |
| updated_at   | Date   | 最終更新日時           |

## API設計

### ユーザー登録

- **URL**: `/api/register`
- **メソッド**: POST
- **リクエストボディ**:
    ```json
    {
        "username": "example_user",
        "password": "example_password",
        "age": 10,
        "parent_email": "parent@example.com"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "User registered successfully",
        "user_id": "123456789"
    }
    ```

### ユーザーログイン

- **URL**: `/api/login`
- **メソッド**: POST
- **リクエストボディ**:
    ```json
    {
        "username": "example_user",
        "password": "example_password"
    }
    ```
- **レスポンス**:
    ```json
    {
        "message": "Login successful",
        "token": "jwt_token"
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
        "username": "example_user",
        "age": 10,
        "parent_email": "parent@example.com",
        "avatar_url": "https://example.com/avatar.png"
    }
    ```

### プロフィール更新

- **URL**: `/api/profile`
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

## 操作の流れ

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