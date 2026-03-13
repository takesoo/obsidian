---
aliases:
  - Generalized Inverted Index
tags:
  - postgresql
---
## what
- [[PostgreSQL]]の[[jsonb型]]カラムに使用できる[[インデックス]]の一種
- 巨大なJSONデータの中から特定のキーや値を持つレコードを爆速で検索できる
- 包含演算子`@>`にはインデックスが効くが、抽出演算子`->>`には効かないので注意。抽出演算子に対しては[[式インデックス]]が有効
## how
```sql
CREATE TABLE users (
    id serial PRIMARY KEY,
    metadata jsonb
);

INSERT INTO users (metadata) VALUES 
('{"name": "Alice", "tags": ["admin", "editor"], "stats": {"login_count": 5}}');

-- metadata全体に対してインデックスを作成
CREATE INDEX idx_users_metadata ON users USING GIN (metadata);

-- これらはインデックスが効いて爆速になります
SELECT * FROM users WHERE metadata @> '{"name": "Alice"}';
SELECT * FROM users WHERE metadata @> '{"tags": ["admin"]}';
SELECT * FROM users WHERE metadata @> '{"stats": {"login_count": 5}}';

-- これは原則としてインデックスが効かず、全件スキャンになります
SELECT * FROM users WHERE metadata->>'name' = 'Alice';

-- 特定のキー専用のインデックス
CREATE INDEX idx_users_name ON users ((metadata->>'name'));

-- これで ->> を使った検索も爆速になります
SELECT * FROM users WHERE metadata->>'name' = 'Alice';
```