---
tags:
  - postgresql
---
## what
- JSON Binary: JSONデータを分解されたバイナリ形式で格納するデータ型
- [[NoSQL]]のようにスキーマレスで柔軟なデータを格納できる
- 既存のjson型に比べて読み取り速度が非常に速い（解析済みであるため）
- [[GINインデックス]]が利用できる
## why
- スキーマに柔軟性を持たせられる
- GINインデックスによって、巨大なJSONデータの中から特定のキーや値を持つレコードを爆速で検索できる
- 「このキーが含まれているか？」「この配列に値が含まれているか？」といった操作を、専用の演算子（`@>`, `?`, `->`など）で直感的に記述できる
## how
```sql
CREATE TABLE users (
    id serial PRIMARY KEY,
    metadata jsonb
);

INSERT INTO users (metadata) VALUES 
('{"name": "Alice", "tags": ["admin", "editor"], "stats": {"login_count": 5}}');

-- 名前をテキストとして取得
SELECT metadata->>'name' FROM users; 

-- ネストした値を取得
SELECT metadata->'stats'->>'login_count' FROM users;

-- tagsに"admin"が含まれているユーザーを検索
SELECT * FROM users WHERE metadata->'tags' @> '["admin"]';
```
### Anti Pattern
- JSONの値を頻繁に更新する。
	- JSONの一部を変更するだけでもJSON全体を書き直すので遅い
- インデックス・ブロート
	- GINインデックスは強力だが更新が重い
- クエリ最適化（プランナー）はjsonbの内部までは見ない
- 