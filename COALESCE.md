---
tags:
  - SQL
---
nullを置き換える関数
```sql
SELECT c1, c2, c3 FROM test1s;

c1 | c2 | c3 
-----+--------+-------- 
AAA | BBB | CCC 
DDD | (null) | FFF 
GGG | HHH | (null) 
(3 rows)

SELECT c1, COALESCE(c2, 'QQQ') AS c2, COALESCE(c3, 'QQQ') AS c3 FROM test1s;

c1 | c2 | c3 
-----+--------+-------- 
AAA | BBB | CCC 
DDD | QQQ | FFF 
GGG | HHH | QQQ
(3 rows)

```