---
tags:
  - Rails
  - デザインパターン
aliases:
  - パラメータオブジェクトパターン
---
- パラメータを属性として持つ素朴なRubyオブジェクト
- フロントから渡されたパラメータの整合性確認などをコントローラから分離する

```ruby
module Parameters
    class ProfileParameter
      attr_reader :name,
                  :email,
                  :avatar,
                  :birthday,
                  :gender,
                  :phone_number,
                  :living_address,
                  :house_name,
                  :room_number

      def initialize(opts = {})
        update(opts)
      end

      def update(opts)
        @name = attrs[:name] if attrs.key?(:name)
        @email = attrs[:email] if attrs.key?(:email)
        @avatar = attrs[:avatar] if attrs.key?(:avatar)
        @birthday = attrs[:birthday] if attrs.key?(:birthday)
        @gender = attrs[:gender] if attrs.key?(:gender)
        @phone_number = attrs[:phone_number] if attrs.key?(:phone_number)
        @living_address = attrs[:living_address] if attrs.key?(:living_address)
        @house_name = attrs[:house_name] if attrs.key?(:house_name)
        @room_number = attrs[:room_number] if attrs.key?(:room_number)
        validate!(opts)
        self
      end

      def attributes
        {
            name: name,
            email: email,
            avatar: avatar,
            birthday: birthday,
            gender: gender,
            phone_number: phone_number,
            living_address: living_address,
            house_name: house_name,
            room_number: room_number
        }.compact
      end

      private

	# 必須パラメータの存在確認やフォーマットの確認など
	# データの整合性はモデル層ですべき
      def validate!
        raise_bad_request if name.blank?
        raise_bad_request if email.blank?
        raise_bad_request if birthday.blank?
        raise_bad_request if phone_number.blank?
      end

      def raise_bad_request
        raise ActionController::BadRequest, '必須項目が入力されていません'
      end
    end
  end
end

def update
  profile_params = Parameters::ProfileParameter.new(params)
  current_user.update(profile_params.attributes)
  ...
end

```

---
[Rails tips: Parameter Objectパターンでリファクタリング（翻訳）｜TechRacho by BPS株式会社](https://techracho.bpsinc.jp/hachi8833/2018_03_20/53807)
[【Rails】Parameter Objectパターン #Ruby - Qiita](https://qiita.com/kat0/items/9a5d9a23dce0df4531e3)
[Railsプロジェクトにおけるパラメータ処理](https://zenn.dev/virginia_blog/articles/1b08dc567b4059)