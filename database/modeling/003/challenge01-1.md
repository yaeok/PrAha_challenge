## 課題 URL

https://separated-rover-67e.notion.site/3-6ac2d25623324f6f835c22d1907d53f8

## 命名規則

## ER 図

```mermaid
erDiagram
    users ||--|{ directories : ""
    users ||--|{ documents : ""
    directories }|--|{ structures : ""
    directories ||--|{ documents : ""


    users {
        int id PK
        varchar name
        timestamp created_at
        timestamp updated_at
    }

    directories {
        int id PK
        varchar title
        timestamp created_at
        timestamp updated_at
    }

    structures {
        int id PK
        int parent_directory_id FK
        int child_directory_id FK
        timestamp created_at
        timestamp updated_at
    }

    documents {
        int id PK
        varchar content
        int user_id FK
        int directory_id FK
        timestamp created_at
        timestamp updated_at
    }
```
