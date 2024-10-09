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
```sql
SELECT * FROM recipes
INNER JOIN categoris ON recipes.category_id = category.id
```

| id  | title  | category_id | id  | name  |
| --- | ------ | ----------- | --- | ----- |
| 1   | スパゲッティ | 1           | 1   | イタリアン |
| 2   | 回鍋肉    | 2           | 2   | 中華    |
実行結果に「フォー」の列ががないことがポイント。
```sql
SELECT * FROM recipes
INNER JOIN recipe_ingredients ON recipes.id = recipe_ingredients.recipe_id
```

| id  | title   | category_id |     |
| --- | ------- | ----------- | --- |
| 1   | ペペロンチーノ | 1           |     |
| 2   | 回鍋肉     | 2           |     |
| 3   | フォー     | NULL        |     |
## 外部結合(OUTER JOIN)
```sql
SELECT * FROM recipes
OUTER JOIN tags ON recipes.id = tags.recipe_id
```