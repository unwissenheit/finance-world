# Finance World
<p align="center">
  <img src="https://github.com/unwissenheit/finance-world/assets/78391723/b580e6ff-e067-4ce9-b01e-22e5796a3b4e" />
</p>

## 子供向け金融教育Webアプリケーション

このプロジェクトは、子供たちが金融の基本を学ぶためのWebアプリケーションです。アプリ内で使用する仮想通貨を用いて、貯金、支出、投資などの概念を楽しく学ぶことができます。

## 主な機能

### ユーザー登録・管理
- **ユーザー登録・ログイン**: ユーザー名、パスワード、年齢、保護者のメールアドレスなどの登録。
- **ユーザープロフィール**: プロフィールの編集やアバターの設定。

### 仮想通貨管理
- **仮想通貨ウォレット**: 現在の残高を表示。
- **取引履歴**: 入出金、送金、支出などの履歴を確認。

### 収入・報酬システム
- **タスク管理**: 家事や勉強などのタスクを設定し、完了時に仮想通貨を報酬として受け取る。
- **報酬カレンダー**: 報酬の履歴をカレンダー形式で表示。

### 支出・投資システム
- **仮想商品店**: 仮想通貨でアイテムや特典を購入。
- **貯金口座**: 一定額を貯金し、利子を付ける機能。
- **投資ゲーム**: 簡単な投資シミュレーションゲームで投資の基本を学ぶ。

### 学習コンテンツ
- **金融教育コース**: 貯金、投資、予算管理などの基礎を学ぶコース。
- **クイズとテスト**: 学んだ内容を確認するためのクイズやテスト。

### 保護者向け機能
- **親の管理ダッシュボード**: 子供の活動を監視し、報酬やタスクを設定。
- **通知機能**: 子供の進捗や特定のアクティビティに対する通知。

### ゲーミフィケーション
- **アチーブメントシステム**: 特定の目標を達成することでバッジや特典を獲得。
- **レベルアップシステム**: アクティビティや学習の進捗に応じてレベルアップ。

### コミュニティ機能
- **フレンド機能**: 友達を追加して仮想通貨を送金したり、チャットしたりできる。
- **ランキングボード**: タスクの完了数や学習の進捗をランキング形式で表示。

### セキュリティ・プライバシー
- **データ保護**: 個人情報や取引履歴の暗号化。
- **アクセス制限**: 保護者の許可がないと特定の機能にアクセスできないように設定。

### サポート機能
- **FAQ・ヘルプデスク**: よくある質問や問題に対するサポート。
- **チュートリアル**: 初回利用時にアプリの使い方をガイドするチュートリアル。

## インストール方法

1. リポジトリをクローンします。
    ```bash
    git clone https://github.com/unwissenheit/finance-world.git
    ```
2. プロジェクトディレクトリに移動します。
    ```bash
    cd finance-world
    ```
3. 開発サーバーを起動します。
    ```bash
    docker compose up --build -d
    ```

## 使用技術

- **フロントエンド**: React, Next.js
- **バックエンド**: Go, Gin
- **データベース**: PostgreSQL
- **認証**: JWT (JSON Web Token)
- **デプロイ**: 未定

## 作者

- [Unwissenheit](https://github.com/unwissenheit)
