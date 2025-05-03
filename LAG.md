---
tags:
  - SQL
  - sql/window関数
---
- 現在の行から見て「前の行」のデータを取得するための関数
```sql
lag(column_name) over (order by column2)
# column_name: 取得したい列の名前
# order_by: データの並び順を指定
```

```sql
select
id,
sale,
lag(sale) over (order by date) as prev_sale,
round(sale * 1.0 / lag(sale) over (order by date) * 100, 1) as ratio
from sales;

| id | sale | prev_sale | ratio |
| -- | ---- | --------- | ----- |
| 1  | 100  | null      | null  |
| 2  | 200  | 100       | 200.0 |
| 3  | 300  | 200       | 150.0 |
```
