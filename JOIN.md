---
tags:
  - SQL
---
テーブルを結合する
[[#内部結合(INNER JOIN)]]と[[#外部結合(OUTER JOIN)]]がある
単にJOINの時は内部結合。

recipesテーブル

| id  | title   | category_id |
| --- | ------- | ----------- |
| 1   | ペペロンチーノ | 1           |
| 2   | 回鍋肉     | 2           |
| 3   | フォー     | NULL        |
categoriesテーブル

| id  | name  |
| --- | ----- |
| 1   | イタリアン |
| 2   | 中華    |
| 3   | 和食    |
recipe_ingredientsテーブル

| id  | recipe_id | ingredient_id | (ingredient_name) |
| --- | --------- | ------------- | ----------------- |
| 1   | 1         | 1             | スパゲッティ            |
| 2   | 1         | 2             | キャベツ              |
| 3   | 2         | 2             | キャベツ              |
## 内部結合(INNER JOIN)
INNER JOINは「内部」結合と呼ばれる理由は、結果セットが両方のテーブルに存在するデータの「内側」または「共通部分」のみを含むため。つまり、結合条件に一致するレコードのみが結果に含まれる。これは、ベン図で表すと二つの円の重なる部分（交差部分）に相当する。
```sql
SELECT * FROM recipes
INNER JOIN categoris ON recipes.category_id = category.id
```

| id  | title   | category_id | id  | name  |
| --- | ------- | ----------- | --- | ----- |
| 1   | ペペロンチーノ | 1           | 1   | イタリアン |
| 2   | 回鍋肉     | 2           | 2   | 中華    |
実行結果に「フォー」の列ががないことがポイント。
```sql
SELECT * FROM recipes
INNER JOIN recipe_ingredients ON recipes.id = recipe_ingredients.recipe_id
```

| id  | title   | category_id | id  | recipe_id | ingredient_id | (ingredient_name) |
| --- | ------- | ----------- | --- | --------- | ------------- | ----------------- |
| 1   | ペペロンチーノ | 1           | 1   | 1         | 1             | スパゲッティ            |
| 1   | ペペロンチーノ | 1           | 2   | 1         | 2             | キャベツ              |
| 2   | 回鍋肉     | 2           | 3   | 2         | 2             | キャベツ              |
「ペペロンチーノ」列が二つになることがポイント。
「フォー」の列もない
## 外部結合(OUTER JOIN)
OUTER JOINは「外部」結合と呼ばれる理由は、結合条件に一致しないレコードも結果セットに含まれるため。これは、ベン図で表すと二つの円の「外側」の部分も含むことを意味する。
```sql
SELECT * FROM recipes
LEFT OUTER JOIN categoris ON recipes.category_id = category.id
```

| id  | title  | id   | name  | category_id |
| --- | ------ | ---- | ----- | ----------- |
| 1   | スパゲッティ | 1    | イタリアン | 1           |
| 2   | 回鍋肉    | 2    | 中華    | 2           |
| 3   | フォー    | NULL | NULL  | NULL        |
「フォー」の列も含まれる
```sql
SELECT * FROM recipes
LEFT OUTER JOIN recipe_ingredients ON recipes.id = recipe_ingredients.recipe_id
```

| id  | title   | category_id | id   | recipe_id | ingredient_id | (ingredient_name) |
| --- | ------- | ----------- | ---- | --------- | ------------- | ----------------- |
| 1   | ペペロンチーノ | 1           | 1    | 1         | 1             | スパゲッティ            |
| 1   | ペペロンチーノ | 1           | 2    | 1         | 2             | キャベツ              |
| 2   | 回鍋肉     | 2           | 3    | 2         | 2             | キャベツ              |
| 3   | フォー     | NULL        | NULL | NULL      | NULL          | NULL              |
外部結合にはさらに3種類に分けられる。
- LEFT OUTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN
![[Pasted image 20250317091440.png]]