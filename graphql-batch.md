---
tags:
  - ruby/gems
  - GraphQL
---
- [[graphql-ruby]]の遅延実行を利用してN+1問題の発生を解消する
```ruby
module Types
 class ArticleType < Types::BaseObject
   field :id, ID, null: false
   field :title, String, null: false
   field :comments, [Types::CommentType], null: false

   def comments
     AssociationLoader.for(Article, :comments).load(object)
   end
 end
end
```
- `for`
	- 引数は`AssociationLoader#initialize`に渡されて、Loaderインスタンスが作成される
	- `Executor`の`@loaders[key]`にLoaderインスタンスを格納
	- `AssociationLoader`のインスタンスを返す
- `load`
	- `AssociationLoader`の`@queue`に引数のobjectが格納される
	- `@cache`からPromiseインスタンスを返す
		-  `@cache`にない場合は、Promiseインスタンスを生成する。この時、Promiseの`source`にself（loaderインスタンス）を格納する
- 一通りのfieldのresolveが実行された後、Promiseを返していたfieldの解決が遅延実行される
	- `GraphQL::Schema#sync_lazy`が実行される
		- 引数：`.lazy_resolve`によって登録されたクラスのインスタンス
			- `load`で作成されたPromiseインスタンス
		- `lazy_method`は`sync`
		- `value.public_send(lazy_method)`で`wait`が実行され、Promiseのsourceに指定されているloaderインスタンスの`perform`が実行される
			- 引数：records=`@queue`、つまりobjectのインスタンスの配列
			- recordsに対してpreloadが実行される。SQL実行。
			- 各recordsに対してfullfillメソッドを実行。Promiseインスタンスがfullfilled状態になる。
			- 戻り値：なし
		- 戻り値：GraphQL-readyオブジェクト
![[Pasted image 20241119095041.png]]
[graphql-batchが何をしているか](https://zenn.dev/2bo/articles/graphql-batch-mechanism)

---
```ruby
# form_answer_approval_step_type.rb
def approvers
	Loaders::Association.for(FormAnswerApprovalStep, :form_answer_approval_step_members).load(object).then do
	Rails.logger.debug "FormAnswerApprovalStepType#approvers: object=#{object.inspect}: start"
	ret = object.sorted_approval_step_members
	Rails.logger.debug "FormAnswerApprovalStepType#approvers: object=#{object.inspect}: end"
	ret
	end
end

# form_answer_approval_step.rb
def sorted_approval_step_members
	not_approved_members, approved_members = form_answer_approval_step_members.includes(
		member: { approver_form_answers: %i[signature_attachment form_answer] }
	).partition { |asm|
		asm.approved_date.nil?
	}
	approved_members.sort_by(&:approved_date) + not_approved_members
end
```
![[スクリーンショット 2024-11-19 18.29.56.png]]
- loaderでpreloadしたFormAnswerApprovalStepインスタンスそれぞれで、sorted_approval_step_membersが実行される。つまりN+1問題の発生