---
Tags:
  - Ruby_on_Rails
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
```
class Foo < ApplicationRecord	with_options on: :update do	  validate :a_validation	  validate :b_validation	end	with_options on: :destroy do	  validate :a_validation	  validate :c_validation	end	private	def a_validation		p 'a_validation'	end	def b_validation		p 'b_validation'	end	def c_validation		p 'c_validation'	endend
```

こういうクラスがあるとして

```
foo = Foo.newfoo.valid(:update)#=> ???
```

何が出力されると思いますか？

わたしは文字列`”a_validation”`と`”b_validation”`が出力されると思ってました。

```
foo.valid(:update)#=> "b_validation"#=> true
```

a_validationが実行されません。

```
class Foo < ApplicationRecord  validate :a_validation, on: %i[update destroy]  validate :b_validation, on: :update  validate :c_validation, on: :destroy(略)foo = Foo.newfoo.valid(:update)#=> "a_validatoin"#=> "b_validatoin"#=> true
```

こうしないといけない