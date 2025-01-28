## 課題 URL

https://separated-rover-67e.notion.site/1-feff30c6daf84d80b0ef44d4853ea8a3

## 命名規則

- テーブル名は`複数形`、`スネークケース`で定義する
- 主キーは全て`id`で定義する
- カラム名は`単数形`、`スネークケース`で定義する
- リレーションを表現する場合、`テーブル名単数形_id`で定義する

## ER 図

```mermaid
erDiagram
    users ||--o{ workspace_users : ""
    workspaces ||--o{ workspace_users : ""
    workspaces ||--|{ channels : ""
    workspace_users ||--|{ channel_users : ""
    channels ||--|{ channel_users : ""
    channels ||--o{ threads : ""
    threads ||--|{ messages : ""
    users ||--|{ messages : ""

    users {
        int id PK
        varchar name
    }

    workspaces {
        int id PK
        varchar name
    }

    workspace_users {
        int id PK
        int workspace_id FK
        int user_id FK
    }

    channels {
        int id PK
        varchar name
        int workspace_id FK
    }

    channel_users {
        int id PK
        int user_id FK
        int channel_id FK
    }

    threads {
      int id PK
      int channel_id FK
      int message_id FK "メッセージの1番初めのメッセージ"
      timestamp created_at
      timestamp updated_at
    }

    messages {
        int id PK
        varchar content "メッセージ"
        int thread_id FK "スレッドId"
        int user_id FK "投稿者"
        enum status "送信前・送信済・更新・削除"
        timestamp created_at
        timestamp updated_at
    }
```
