---
tags:
  - ruby/gems
  - Ruby_on_Rails
---
[jhawthorn/discard: 🃏🗑 Soft deletes for ActiveRecord done right](https://github.com/jhawthorn/discard)

```ruby
class AddDiscardToPosts < ActiveRecord::Migration[5.0]
  def change
    add_column :posts, :discarded_at, :datetime
    add_index :posts, :discarded_at
  end
end

Post.all             # => [#<Post id: 1, ...>]
Post.kept            # => [#<Post id: 1, ...>]
Post.discarded       # => []

post = Post.first   # => #<Post id: 1, ...>
post.discard        # => true
post.discard!       # => Discard::RecordNotDiscarded: Failed to discard the record
post.discarded?     # => true
post.undiscarded?   # => false
post.kept?          # => false
post.discarded_at   # => 2017-04-18 18:49:49 -0700

Post.all             # => [#<Post id: 1, ...>]
Post.kept            # => []
Post.discarded       # => [#<Post id: 1, ...>]
```
