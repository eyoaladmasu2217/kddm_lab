
## Relationship cardinality

The ranges below are the observed minimum and maximum number of child rows per parent in the supplied CSV data. They describe this dataset; they are not necessarily business rules that would hold for all future data.

| Parent | Child | Observed child rows per parent | Observed relationship |
|---|---|---:|---|
| `customers` (`customer_id`) | `orders` (`customer_id`) | 1–1 | Exactly one order row per customer record; each order refers to one customer record. |
| `orders` (`order_id`) | `order_items` (`order_id`) | 0–21 | Zero or many items per order; each item refers to one order. |
| `products` (`product_id`) | `order_items` (`product_id`) | 1–527 | One or many items per product in this dataset; each item refers to one product. |
| `sellers` (`seller_id`) | `order_items` (`seller_id`) | 1–2,033 | One or many items per seller in this dataset; each item refers to one seller. |

All four child-to-parent relationships have zero orphan rows and zero null foreign-key values in the supplied data. There are 775 orders without any order items. Products and sellers all appear in at least one order item in this dataset, even though the ERD's optional circles allow for products or sellers with no order items.

## Primary-key observations

The single-column keys `customers.customer_id`, `orders.order_id`, `products.product_id`, and `sellers.seller_id` are unique and non-null in the supplied data. The `order_items` primary key is the composite `(order_id, order_item_id)`: together these columns uniquely identify each line item, while `order_id` alone is repeated. `customers.customer_unique_id` is not unique at the customer-row grain, so it is not a primary key for the `customers` table.

## Required note

`customer_id` identifies a row in the customers table, while `customer_unique_id` groups rows that belong to the same shopper; use `COUNT(DISTINCT customer_unique_id)` to count shoppers (96,096 in this data). `order_items` is the junction table that resolves the many-to-many relationship between orders and products, with each line also identifying its seller. The products-to-order_items relationship is stricter in the observed data than the optional circle in the ERD: every product has at least one order item. The circle on orders-to-order_items is justified because 775 orders have no items.
