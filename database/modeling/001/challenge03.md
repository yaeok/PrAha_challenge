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
status_id INT REFERENCES order_statuses(id),
created_at TIMESTAMP NOT NULL DEFAULT NOW(),
updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE order_statuses (
id SERIAL PRIMARY KEY,
status VARCHAR(100) NOT NULL,
paid_at TIMESTAMP,
canceled_at TIMESTAMP
);

CREATE TABLE order_menus (
id SERIAL PRIMARY KEY,
order_id INT REFERENCES orders(id),
menu_id INT REFERENCES menus(id),
quantity INT NOT NULL,
option_id INT REFERENCES order_menu_options(id),
created_at TIMESTAMP NOT NULL DEFAULT NOW(),
updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE order_menu_options (
id SERIAL PRIMARY KEY,
has_wasabi BOOLEAN NOT NULL DEFAULT FALSE,
rice_size VARCHAR(100),
discount INT REFERENCES discounts(id),
created_at TIMESTAMP NOT NULL DEFAULT NOW(),
updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE discounts (
id SERIAL PRIMARY KEY,
rate NUMERIC(5, 2) NOT NULL,
startDate TIMESTAMP NOT NULL,
endDate TIMESTAMP NOT NULL,
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
INSERT INTO customers (name, phone) VALUES ('John Doe', '1234567890');
INSERT INTO customers (name, phone) VALUES ('Jane Smith', '0987654321');
INSERT INTO customers (name, phone) VALUES ('Alice Johnson', '1112223333');
INSERT INTO customers (name, phone) VALUES ('Bob Brown', '4445556666');
INSERT INTO customers (name, phone) VALUES ('Charlie White', '7778889999');

INSERT INTO genres (name) VALUES ('Main Dish');
INSERT INTO genres (name) VALUES ('Dessert');
INSERT INTO genres (name) VALUES ('Beverage');
INSERT INTO genres (name) VALUES ('Appetizer');
INSERT INTO genres (name) VALUES ('Side Dish');

INSERT INTO menus (name, price, genre_id, is_set_menu) VALUES ('Grilled Chicken', 1500, 1, FALSE);
INSERT INTO menus (name, price, genre_id, is_set_menu) VALUES ('Cheesecake', 500, 2, FALSE);
INSERT INTO menus (name, price, genre_id, is_set_menu) VALUES ('Cola', 200, 3, FALSE);
INSERT INTO menus (name, price, genre_id, is_set_menu) VALUES ('Spring Rolls', 800, 4, FALSE);
INSERT INTO menus (name, price, genre_id, is_set_menu) VALUES ('Meal Set A', 2000, 1, TRUE);

INSERT INTO order_statuses (status) VALUES ('Pending');
INSERT INTO order_statuses (status) VALUES ('Confirmed');
INSERT INTO order_statuses (status) VALUES ('In Progress');
INSERT INTO order_statuses (status) VALUES ('Completed');
INSERT INTO order_statuses (status) VALUES ('Canceled');

INSERT INTO orders (customer_id, content, total_price, status_id) VALUES (1, 'Grilled Chicken and Cola', 1700, 2);
INSERT INTO orders (customer_id, content, total_price, status_id) VALUES (2, 'Cheesecake and Spring Rolls', 1300, 1);
INSERT INTO orders (customer_id, content, total_price, status_id) VALUES (3, 'Meal Set A', 2000, 3);
INSERT INTO orders (customer_id, content, total_price, status_id) VALUES (4, 'Cola and Spring Rolls', 1000, 4);
INSERT INTO orders (customer_id, content, total_price, status_id) VALUES (5, 'Grilled Chicken', 1500, 5);

INSERT INTO order_menu_options (has_wasabi, rice_size, discount) VALUES (TRUE, 'Large', NULL);
INSERT INTO order_menu_options (has_wasabi, rice_size, discount) VALUES (FALSE, 'Medium', 1);
INSERT INTO order_menu_options (has_wasabi, rice_size, discount) VALUES (TRUE, 'Small', 2);
INSERT INTO order_menu_options (has_wasabi, rice_size, discount) VALUES (FALSE, 'Large', NULL);
INSERT INTO order_menu_options (has_wasabi, rice_size, discount) VALUES (TRUE, NULL, NULL);

INSERT INTO order_menus (order_id, menu_id, quantity, option_id) VALUES (1, 1, 1, 1);
INSERT INTO order_menus (order_id, menu_id, quantity, option_id) VALUES (2, 4, 2, 2);
INSERT INTO order_menus (order_id, menu_id, quantity, option_id) VALUES (3, 5, 1, 3);
INSERT INTO order_menus (order_id, menu_id, quantity, option_id) VALUES (4, 3, 3, NULL);
INSERT INTO order_menus (order_id, menu_id, quantity, option_id) VALUES (5, 1, 2, 1);

INSERT INTO discounts (rate, startDate, endDate) VALUES (0.10, '2025-03-01 00:00:00', '2025-03-31 23:59:59');
INSERT INTO discounts (rate, startDate, endDate) VALUES (0.15, '2025-07-01 00:00:00', '2025-07-31 23:59:59');
INSERT INTO discounts (rate, startDate, endDate) VALUES (0.20, '2025-10-01 00:00:00', '2025-10-15 23:59:59');
INSERT INTO discounts (rate, startDate, endDate) VALUES (0.25, '2025-12-20 00:00:00', '2025-12-26 23:59:59');
INSERT INTO discounts (rate, startDate, endDate) VALUES (0.05, '2026-01-01 00:00:00', '2026-01-15 23:59:59');

INSERT INTO set_menu_items (set_menu_id, menu_item_id) VALUES (5, 1);
INSERT INTO set_menu_items (set_menu_id, menu_item_id) VALUES (5, 3);
INSERT INTO set_menu_items (set_menu_id, menu_item_id) VALUES (5, 4);
INSERT INTO set_menu_items (set_menu_id, menu_item_id) VALUES (5, 2);
INSERT INTO set_menu_items (set_menu_id, menu_item_id) VALUES (5, 1);
```

テーブル削除用

```

DROP TABLE IF EXISTS set_menu_items CASCADE;
DROP TABLE IF EXISTS order_menus CASCADE;
DROP TABLE IF EXISTS order_menu_options CASCADE;
DROP TABLE IF EXISTS orders CASCADE;
DROP TABLE IF EXISTS menus CASCADE;
DROP TABLE IF EXISTS genres CASCADE;
DROP TABLE IF EXISTS discounts CASCADE;
DROP TABLE IF EXISTS order_statuses CASCADE;
DROP TABLE IF EXISTS customers CASCADE;

```

# ユースケース

## **注文合計金額を計算する**

クエリ

```

SELECT
    o.id AS order_id,
    o.customer_id,
    SUM(m.price \* om.quantity) AS total_price
FROM
    orders o
JOIN
    order_menus om ON o.id = om.order_id
JOIN
    menus m ON om.menu_id = m.id
GROUP BY
    o.id, o.customer_id;

```

実行結果

| order_id | customer_id | total_price |
| -------- | ----------- | ----------- |
| 5        | 5           | 3000        |
| 4        | 4           | 600         |
| 1        | 1           | 1500        |
