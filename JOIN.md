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

| id  | title  | id  | name  | category_id |
| --- | ------ | --- | ----- | ----------- |
| 1   | スパゲッティ | 1   | イタリアン | 1           |
| 2   | 回鍋肉    | 2   | 中華    | 2           |
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
