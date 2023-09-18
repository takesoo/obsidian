[https://qiita.com/zaru/items/d545c0baebeb8d40370b](https://qiita.com/zaru/items/d545c0baebeb8d40370b)

  

[ベーシック Advent Calendar 2017](https://qiita.com/advent-calendar/2017/basicinc)Day 15

Rails というか ActiveRecord で `has_many through` なモデルをさわっていてハマった事象があったので書きます。ただ、ハマりを回避する方法は分かっても、なぜそのような挙動になるかはまだ分かっていません。誰か教えてもらえると助かります。

- Rails 5.x
- ActiveRecord

```
class User < ApplicationRecord  has_many :articles  has_many :game_users  has_many :games, through: :game_users  belongs_to :pinned_article, class_name: 'Article'endclass Article < ApplicationRecord  belongs_to :userendclass Game < ApplicationRecord  has_many :game_users  has_many :users, through: :game_usersendclass GameUser < ApplicationRecord  belongs_to :game  belongs_to :userend
```

User と Game が `has_many through` の関係になっています。そして、 User と Article が `has_many` になっています。また、 `User.pinned_article` で Article を `belongs_to` で参照しています。

## 親子関係をまとめて save

User を基点として親子関係をまとめて save しようと思い、以下のようなコードを書きました。

```
# Game は予め作っておくGame.createu = User.newa = u.articles.buildu.pinned_article = au.games << Game.lastu.save!
```

これを実行すると以下のような SQL が発行されます。

```
BEGININSERT INTO `users` (`created_at`, `updated_at`) VALUES ('2017-12-15 02:24:44', '2017-12-15 02:24:44')INSERT INTO `articles` (`user_id`, `created_at`, `updated_at`) VALUES (35, '2017-12-15 02:24:44', '2017-12-15 02:24:44')INSERT INTO `game_users` (`game_id`, `user_id`, `created_at`, `updated_at`) VALUES (9, 35, '2017-12-15 02:24:44', '2017-12-15 02:24:44')UPDATE `users` SET `pinned_article_id` = 32 WHERE `users`.`id` = 35INSERT INTO `game_users` (`game_id`, `user_id`, `created_at`, `updated_at`) VALUES (9, 35, '2017-12-15 02:24:44', '2017-12-15 02:24:44')COMMIT
```

一見上手く行っているように見えますが、最後の SQL でなぜか `game_users` に同じレコードが重複して INSERT されています。`User.pinned_article` の ID を反映するために、 INSERT 後に改めて UPDATE が走るのですが、なぜか関係のない GameUser の中間テーブルデータが改めて INSERT されています。

実際には、中間テーブルを作った場合重複データを防ぐために DB や Rails でユニーク制約を付けると思うので、SQL が失敗して ROLLBACK してしまいます。

```
class GameUser < ApplicationRecord  belongs_to :game  belongs_to :user  validates :user, uniqueness: { scope: [:game_id] }end
```

### 期待した動きをするコード

以下のコードにすると期待したとおりの動きをします。違いは `User.games` に直接 append するのではなく、中間テーブルの GameUser を `build` して作成する所です。

```
u = User.newa = u.articles.buildu.pinned_article = a# build をして作るgu = u.game_users.buildgu.game = Game.lastu.save!
```

これで無事できました。基本は `build` 系を使っていくのが良いんですかね。

ちなみに ActiveRecord の `<<` は collection_proxy で定義されています。

```
def <<(*records)  proxy_association.concat(records) && selfendalias_method :push, :<<alias_method :append, :<<
```

この `append` を実行すると、親の ID が確定していたら即時 INSERT が実行されます。 `save` とか必要ありません。

ActiveRecord の動きを step 実行しながら追っていきました。が、結論、なぜそうなるのかは時間切れで解明できず…。無念。また時間を作って調査をしたいと思います。

`save!` で保存をする時に [save_collection_association](https://github.com/rails/rails/blob/a33e934697370892bfb0989cd9363465e9d3d783/activerecord/lib/active_record/autosave_association.rb#L387) が呼ばれるのですが、ここでなぜか最後に重複データが渡ってきてしまうという…。

Why not register and get more from Qiita?

1. We will deliver articles that match youBy following users and tags, you can catch up information on technical fields that you are interested in as a whole
2. you can read useful information later efficientlyBy "stocking" the articles you like, you can search right away

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fzaru%2Fitems%2Fd545c0baebeb8d40370b&realm=qiita)

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fzaru%2Fitems%2Fd545c0baebeb8d40370b&realm=qiita)