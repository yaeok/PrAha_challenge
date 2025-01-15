# DDL

```
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    phone VARCHAR(12) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE genres (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE menus (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price INT NOT NULL,
    genre_id INT REFERENCES genres(id),
    is_set_menu BOOLEAN NOT NULL DEFAULT FALSE,
    is_deleted BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(id),
    content VARCHAR(500),
    total_price INT NOT NULL,
    status_id INT REFERENCES order_status(id),
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE order_status (
    id SERIAL PRIMARY KEY,
    status VARCHAR(100) NOT NULL,
    paid_at TIMESTAMP,
    canceled_at TIMESTAMP'
);

CREATE TABLE order_menus (
    id SERIAL PRIMARY KEY,
    order_id INT REFERENCES orders(id),
    menu_id INT REFERENCES menus(id),
    quantity INT NOT NULL,
    option_id INT REFERENCES order_menu_option(id),
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE order_menu_option (
    id SERIAL PRIMARY KEY,
    has_wasabi BOOLEAN NOT NULL DEFAULT FALSE,
    rice_size VARCHAR(100),
    discount INT REFERENCES discount(id),
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE discount (
    id SERIAL PRIMARY KEY,
    rate NUMERIC(5, 2) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE set_menu_items (
    id SERIAL PRIMARY KEY,
    set_menu_id INT REFERENCES menus(id),
    menu_item_id INT REFERENCES menus(id),
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

```

# DML

```

```

# ユースケース

## **注文合計金額を計算する**

クエリ

```

```
