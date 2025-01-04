## 命名規則

- テーブル名、カラム名は全てスネークケースで定義する
- boolean 型の項目は、`is_`または`has_`を使って定義する
- テーブル名は複数形で定義する
  - 調べている過程で、複数、単数で派閥ありそう。個人開発で複数形が多いので、複数形で定義してます

## 設計意図

### 支払いの扱いについて

`paid_at`に日付が入ることで、支払い済みとし、`Null`の場合は未払いとなる。
万が一キャンセルとなった場合、`canceld_at`に日付が入り、キャンセルデータは見ない。

### セットメニューの扱いについて

セットメニューと単品メニューの扱いを分けて定義した
セットメニュー内の寿司ネタは`set_products`にて単品メニューと結合することで、内容が見ることができる

## ER 図

```mermaid
erDiagram
    customers ||--|{ orders : "1人のユーザーは1以上の注文を持つ"
    orders ||--|{ order_products: "1回の注文で1以上の商品を注文できる"
    order_products ||--o{ products: "1つの商品は0以上の単品メニューを注文できる"
    order_products ||--o{ sets: "1つの商品は0以上のセットメニューを注文できる"
    sets ||--|{ set_products: "1つのセットメニューは1以上の単品メニューを持つ"
    genres ||--|{ products : "1つのジャンルに1以上の単品メニューを持つ"
    genres ||--|{ sets : "1つのジャンルに1以上のセットメニューを持つ"

    customers {
        int id PK
        varchar name
        int phone
        timestamp created_at
        timestamp updated_at
    }

    products {
        int id PK
        varchar name "寿司名"
        int price "値段"
        int genre_id FK "ジャンル"
        boolean is_deleted "削除フラグ"
        timestamp created_at
        timestamp updated_at
    }

    orders {
        int id PK
        int customer_id FK "顧客Id"
        varchar content "その他"
        int total_price "合計金額"
        timestamp paid_at "支払日"
        timestamp canceled_at "キャンセル日"
        timestamp created_at
        timestamp updated_at
    }

    order_products {
        int id PK
        int order_id FK "オーダーId"
        int product_id FK "寿司Id"
        int set_id FK "セットメニューId"
        boolan has_wasabi "わさび有無"
        int quantity "数量"
        timestamp created_at
        timestamp updated_at
    }

    sets {
        int id PK
        varchar name "セットメニュー名"
        int price "値段"
        int genre_id FK "ジャンル"
        boolean is_deleted "削除フラグ"
        timestamp created_at
        timestamp updated_at
    }

    set_products {
        int id PK
        int set_id FK
        int product_id FK
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
