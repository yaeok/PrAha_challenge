# 設計方針

シャリサイズは、アプリ側で Enum 管理することで、ステータスの値については DB と切り離して考える。

# ER 図

```mermaid
erDiagram
    customers ||--|{ orders : ""
    orders ||--|{ order_menus : ""
    order_menus ||--o{ menus : ""
    menus ||--|{ set_menu_items : ""
    genres ||--|{ menus : ""
    orders ||--|| order_status : ""

    customers {
        int id PK
        varchar name
        int phone
        timestamp created_at
        timestamp updated_at
    }

    menus {
        int id PK
        varchar name "寿司名"
        int price "値段"
        int genre_id FK "ジャンル"
        boolean is_set_menu "セットメニュー判定"
        boolean is_deleted "削除フラグ"
        timestamp created_at
        timestamp updated_at
    }

    orders {
        int id PK
        int customer_id FK "顧客Id"
        varchar content "その他"
        int total_price "合計金額"
        int status_id FK
        timestamp created_at
        timestamp updated_at
    }

    order_status {
        int id PK
        varchar status "状態"
        timestamp paid_at "支払日"
        timestamp canceled_at "キャンセル日"
    }

    order_menus {
        int id PK
        int order_id FK "オーダーId"
        int menu_id FK "メニューId"
        int quantity "数量"
        varchar rice_size "シャリサイズ"
        timestamp created_at
        timestamp updated_at
    }

    set_menu_items {
        int id PK
        int set_menu_id FK
        int menu_item_id FK
        timestamp created_at
        timestamp updated_at
    }

    genres {
        int id PK
        varchar name "ジャンル名"
        timestamp created_at
        timestamp updated_at
    }
```
