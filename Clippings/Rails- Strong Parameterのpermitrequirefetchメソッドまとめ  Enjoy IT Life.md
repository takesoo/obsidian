---
AI summary: Ruby on RailsのStrong
  Parameterについて、permit/require/fetchメソッドの使い方や組み合わせ例を紹介したブログ記事です。具体的なコード例を交えながら、許可する要素を絞り込む方法や、多段のハッシュの要素を許可する方法などを解説しています。
Created: Invalid date
URL: https://nishinatoshiharu.com/overview-strong-parameter/
カテゴリー: 技術
---
[![](https://i1.wp.com/nishinatoshiharu.com/wp-content/uploads/2023/01/overview-strong-parameter.001.jpeg?fit=800%2C418&ssl=1)](https://i1.wp.com/nishinatoshiharu.com/wp-content/uploads/2023/01/overview-strong-parameter.001.jpeg?fit=800%2C418&ssl=1)

---

## permitについて

`permit`は許可する要素を指定するメソッドです。

`permit`によって生成された`params`(`ActionController::Parameters`)の`permitted`属性は`true`になります。

`permit(要素名)`で要素の許可ができます。

```
> params = ActionController::Parameters.new({ name: 'Francesco', age: 22 })> params.permitted?=> false> permitted_params = params.permit(:name)=> #<ActionController::Parameters {"name"=>"Francesco"} permitted: true>> permitted_params.permitted?=> true> permitted_params.has_key?(:name)=> true> permitted_params[:name]=> "Francesco"> permitted_params.has_key?(:age)=> false> permitted_params[:age]=> nil
```

配列の場合は`permit(要素名: [])`と記述します。

```
> params = ActionController::Parameters.new({ id: [1, 2, 3] })> permitted_params = params.permit(id: [])=> #<ActionController::Parameters {"id"=>[1, 2, 3]} permitted: true>> permitted_params[:id]=> [1, 2, 3]
```

多段のハッシュの要素の場合は`permit(ハッシュオブジェクト: [要素名])`と記述します。

```
> params = ActionController::Parameters.new({ user: { name: 'Francesco', age: 22 } })### userオブジェクトの中にあるname要素が許可される> permitted_params = params.permit(user: [:name])=> #<ActionController::Parameters {"user"=>#<ActionController::Parameters {"name"=>"Francesco"} permitted: true>} permitted: true>> permitted_params[:user][:name]=> "Francesco"> permitted_params[:user][:age]=> nil
```

ハッシュ内のすべての要素を許可する場合は`permit(ハッシュオブジェクト: {})`でもOKです。

```
> params = ActionController::Parameters.new({ user: { name: 'Francesco', age: 22 } })> permitted_params = params.permit(user: {})> permitted_params[:user][:name]=> "Francesco"> permitted_params[:user][:age]=> 22
```

## requireについて

必須パラメータを指定するメソッドです。

`require`で指定されたパラメータが`params`(`ActionController::Parameters`)に存在しない場合、`ActionController::ParameterMissing`例外が発生します。

```
> params = ActionController::Parameters.new({ name: 'Francesco' })> params.require(:name)=> "Francesco"> params.require(:age)# ActionController::ParameterMissing: param is missing or the value is empty: age
```

```
> params = ActionController::Parameters.new({ user: { name: 'Francesco' } })> params.require(:user)=> #<ActionController::Parameters {"user"=>{"name"=>"Francesco"}} permitted: false>> params.require(:admin)# ActionController::ParameterMissing: param is missing or the value is empty: admin> params.require(:user).require(:name)=> "Francesco"> params.require(:user).require(:age)# ActionController::ParameterMissing: param is missing or the value is empty: age
```

## fetchについて

パラメータのデフォルト値を設定できるメソッドです。パラメータが存在していない場合は第2引数の値を返します。

```
> params = ActionController::Parameters.new({ name: 'Francesco' })> params.fetch(:name, 'Bob')=> "Francesco"> params.fetch(:age, 20)=> 20
```

## require、fetch、permitを組み合わせたStrong Parameterの具体例

`require`(もしくは`fetch`)と`permit`を組み合わせることで、『`require`(もしくは`fetch`)で許可するハッシュの範囲を絞り込み、`permit`で絞り込まれたハッシュの中から許可する具体的な要素を指定する』ということが実現できます。

```
params = ActionController::Parameters.new({  user: {    name: 'Francesco',    age: 22,    address: {      country: 'Japan',      prefecture: 'Tokyo',    },    emails: ['example1@example.com', 'example2@example.com']  }})
```

以下では上記のオブジェクトを利用して`require`(もしくは`fetch`)と`permit`の組み合わせ例について紹介します。

### userのnameとemailsを許可する

```
> permitted_params = params.require(:user).permit(:name, emails: [])> permitted_params[:name]=> "Francesco"> permitted_params[:emails]=> ["example1@example.com", "example2@example.com"]
```

### userのnameを許可する。userがパラメータに存在しない場合も考慮

```
> permitted_params = params.fetch(:user, {}).permit(:name)> permitted_params[:name]=> "Francesco"
```

### userのnameとaddressのprefectureを許可する

```
> permitted_params = params.require(:user).permit(:name, address: [:prefecture])> permitted_params[:name]=> "Francesco"> permitted_params[:address][:prefecture]=> "Tokyo"
```

### userのaddressのprefectureを許可する

```
> permitted_params = params.require(:user).require(:address).permit(:prefecture)> permitted_params[:prefecture]=> "Tokyo"
```

### userのすべてのパラメータを許可する

```
> permitted_params = params.require(:user).permit(:name, :age, address: [:country, :prefecture], emails: [])> permitted_params[:name]=> "Francesco"> permitted_params[:emails]=> ["example1@example.com", "example2@example.com"]> permitted_params[:address][:country]=> "Japan"
```

ハッシュの要素すべてを許可する場合は`permit!`を利用して以下のようにも記述できます。

```
> permitted_params = params.require(:user).permit!> permitted_params[:name]=> "Francesco"> permitted_params[:emails]=> ["example1@example.com", "example2@example.com"]> permitted_params[:address][:country]=> "Japan"
```

あるいは以下のようにも記述できます。

```
> permitted_params = params.permit(user: {})> permitted_params[:user][:name]=> "Francesco"> permitted_params[:user][:emails]=> ["example1@example.com", "example2@example.com"]> permitted_params[:user][:address][:country]=> "Japan"
```

## さらに複雑なStrong Parameterの例

```
params = ActionController::Parameters.new({  user: {    name: 'Francesco',    emails: ['example1@example.com', 'example2@example.com'],    friends: [      {        name: 'Bob',        hobbies: ['Baseball', 'Soccer'],        family: {          name: 'Tom'        }      }    ],    address: {      country: 'Japan'    }  }})
```

上記のパラメータの要素をすべて許可する場合は以下のようになります。

```
permitted_params = params.require(:user).permit(  :name,  emails: [],  friends: [    :name,    hobbies: [],    family: [      :name    ]  ],  address: [    :country  ])> permitted_params[:name]=> "Francesco"> permitted_params[:emails]=> ["example1@example.com", "example2@example.com"]> permitted_params[:friends][0][:name]=> "Bob"> permitted_params[:friends][0][:hobbies]=> ["Baseball", "Soccer"]> permitted_params[:friends][0][:family][:name]=> "Tom"> permitted_params[:address][:country]=> "Japan"
```

## 参考資料