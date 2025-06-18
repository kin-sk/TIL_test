# API設計書

データモデル設計にご納得いただきありがとうございます。それでは次に、フロントエンド（React）とバックエンド（Spring Boot）間の通信を定義する「API設計」に進みましょう。

API設計は、アプリケーションの各機能がどのようにデータのやり取りを行うかを明確にする上で非常に重要です。ここでは、RESTfulな原則に基づき、認証関連とTodoタスク関連の主要なAPIエンドポイントを定義します。

## APIのベースURL
すべてのAPIエンドポイントは、以下のベースURLを想定します。
```
http://localhost:8080/api (開発環境の場合)
```

## 1. 認証 (Authentication) API
ユーザーの登録、ログイン、ログアウト、パスワードリセットに関連するAPIです。

| HTTPメソッド | エンドポイント | 説明 | リクエストボディ (JSON) | レスポンスボディ (JSON) | ステータスコード |
|------------|--------------|------|----------------------|----------------------|----------------|
| POST | `/api/auth/register` | ユーザー登録 | `{ "username": "string", "email": "string", "password": "string" }` | `{ "message": "User registered successfully!" }` またはエラーメッセージ | 201 Created, 400 Bad Request |
| POST | `/api/auth/login` | ユーザーログイン | `{ "email": "string", "password": "string" }` | `{ "token": "string", "userId": "string", "username": "string" }` またはエラーメッセージ | 200 OK, 401 Unauthorized |
| POST | `/api/auth/logout` | ユーザーログアウト | (なし) | `{ "message": "Logged out successfully!" }` | 200 OK |
| POST | `/api/auth/forgot-password` | パスワードリセット要求 | `{ "email": "string" }` | `{ "message": "Password reset link sent to your email." }` | 200 OK, 404 Not Found |
| POST | `/api/auth/reset-password` | パスワードリセット実行 | `{ "token": "string", "newPassword": "string" }` | `{ "message": "Password has been reset successfully." }` | 200 OK, 400 Bad Request |

### 認証トークンについて
ログイン成功後、バックエンドはJWT (JSON Web Token) などの認証トークンを返します。フロントエンドはこのトークンを安全に保存し（例: HttpOnly Cookie または Web Storage）、以降の保護されたAPIリクエストのAuthorizationヘッダーにBearer Tokenとして含めます。

例: `Authorization: Bearer <your_jwt_token>`

## 2. Todoタスク (Task) API
TodoタスクのCRUD操作を行うAPIです。これらのAPIは、認証されたユーザーのみがアクセスできる保護されたエンドポイントとします。

| HTTPメソッド | エンドポイント | 説明 | リクエストボディ (JSON) | レスポンスボディ (JSON) | ステータスコード |
|------------|--------------|------|----------------------|----------------------|----------------|
| GET | `/api/todos` | 全Todoタスクの取得 | (なし) | `[ { "id": "string", "title": "string", "description": "string", "dueDate": "string", "isCompleted": boolean, "createdAt": "string", "updatedAt": "string" } ]` または空の配列 | 200 OK, 401 Unauthorized |
| GET | `/api/todos/{id}` | 特定のTodoタスクの詳細取得 | (なし) | `{ "id": "string", "title": "string", "description": "string", "dueDate": "string", "isCompleted": boolean, "createdAt": "string", "updatedAt": "string" }` またはエラーメッセージ | 200 OK, 401 Unauthorized, 404 Not Found |
| POST | `/api/todos` | 新しいTodoタスクの作成 | `{ "title": "string", "description": "string", "dueDate": "string" }` | `{ "id": "string", "title": "string", "description": "string", "dueDate": "string", "isCompleted": boolean, "createdAt": "string", "updatedAt": "string" }` またはエラーメッセージ | 201 Created, 400 Bad Request, 401 Unauthorized |
| PUT | `/api/todos/{id}` | 既存Todoタスクの更新 | `{ "title": "string", "description": "string", "dueDate": "string", "isCompleted": boolean }` (更新したいフィールドのみ) | `{ "id": "string", "title": "string", "description": "string", "dueDate": "string", "isCompleted": boolean, "createdAt": "string", "updatedAt": "string" }` またはエラーメッセージ | 200 OK, 400 Bad Request, 401 Unauthorized, 404 Not Found |
| PATCH | `/api/todos/{id}/complete` | Todoタスクの完了状態更新 | `{ "isCompleted": boolean }` | `{ "id": "string", "title": "string", "description": "string", "dueDate": "string", "isCompleted": boolean, "createdAt": "string", "updatedAt": "string" }` またはエラーメッセージ | 200 OK, 400 Bad Request, 401 Unauthorized, 404 Not Found |
| DELETE | `/api/todos/{id}` | 特定のTodoタスクの削除 | (なし) | `{ "message": "Todo deleted successfully!" }` | 204 No Content, 401 Unauthorized, 404 Not Found |

### 補足
- PATCHメソッドは、PUTとは異なり、リソースの一部を更新する場合に利用します。ここではisCompletedのみを更新するケースを想定しPATCHを提案しています。PUTで全てのフィールドを送っても問題ありません。
- 認証されたユーザーは、自身のTodoタスクのみを操作できるように、バックエンドで認可のチェックを行う必要があります。

## 次のステップ
このAPI設計案について、ご質問や修正点があればお知らせください。特に問題がなければ、次は**UI/UX設計（ワイヤーフレームなど）**に進むことができます。
