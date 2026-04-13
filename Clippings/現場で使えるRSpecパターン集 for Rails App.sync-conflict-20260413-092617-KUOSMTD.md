---
Created: Invalid date
URL: https://mogulla3.tech/articles/2019-08-29-01
---
RSpecは慣れるととても手に馴染むテスティングツールだが、割と癖があってRSpecでテストを書くのに苦労している人も多いのではないだろうか。

自分はまさにそうで、書きたいテストは決まっていてもそれをどう書けばよいか、というところで当初は時間がかかっていたように思う。

実際に実務で何年かRSpecを使ってきて、よく使うパターン（型）のようなものができてきたので、それらをここにまとめてみようと思う。同じように「どう書けばいいか」で躓いている人や書き方をド忘れしてしまった人の助けになれば幸いである。

**前提として、Railsアプリケーションを想定した内容になっている。**

- macOS Mojave
- rspec : 3.8.2
- ruby : 2.6.3
- rails : 5.2.3

便宜上、以下の方針でパターンを書く。

- `subject` でテストの主体を明示する
- `it` の引数は省略する
- `FactoryBot` や `webmock` といったライブラリは使わない

```
# ステータスコード200を期待する場合subject { get xxx_path }it do  is_expected.to eq 200end
```

または

```
subject { get xxx_path }it do  expect(response).to have_http_status(200)end
```

```
subject { get xxx_path }it do  subject  expect(response.body).to include('xxx')end
```

Fooモデルの件数が1増えていることを確認

```
subject { post xxx_path }it do  expect { subject }.to change(Foo, :count).by(1)end
```

fooのvalue属性が `"from"` から `"to"` になることを確認

```
# 実際にはパラメータを渡して、fooが更新対象となるようにするだろうsubject { put xxx_path }let(:foo) { Foo.find(1) } # 更新対象のインスタンスit do  expect { subject }.to change(foo, :value).from('from').to('to')end
```

Fooモデルの件数が1減っていることを確認

```
subject { delete xxx_path }it do  expect { subject }.to change(Foo, :count).by(-1)end
```

```
subject { get xxx_path }# yyy_pathにリダイレクトされることをテストit do  expect { subject }.to redirect_to(yyy_path)end
```

レスポンスヘッダの `Content-Disposition` から確認する

```
subject { get xxx_path }# 通常、ファイルダウンロード時はContent-Dispositionヘッダが次のような形式になることから# Content-Disposition: attachment; filename="YOUR_FILENAME.pdf"it do  subject  expect(response.headers['Content-Disposition']).to include('attachment')  expect(response.headers['Content-Disposition']).to include('YOUR_FILENAME.pdf')end
```

次のようなFooモデルクラスがあるとする。

```
class Foo < ApplicationRecord  validates :value, presence: trueend
```

```
subject { foo.valid? }let(:foo) { Foo.new(value: 'abc') }it do  is_expected.to be trueend
```

```
subject { foo.valid? }let(:foo) { Foo.new(value: nil) }it do  is_expected.to be falseend
```

```
subject { foo.valid? }let(:foo) { Foo.new(value: nil) }# 指定した属性でエラーが起きていることをテストit do  subject  expect(foo.errros).to include(:value)end# 指定した属性でエラーが起きており、かつメッセージも期待通りであることをテストit do  subject  expect(foo.errros.full_messages_for(:value)).to include("can't be blank")end
```

次のようなSampleJobクラスがあり、`sample_job` という名称でキューが登録されるものとする。

```
# app/jobs/sample_job.rbclass SampleJob < Application  queues_as :sample_job  def perform(name)    puts "Hello, #{name}!"  endend
```

```
subject { SampleJob.perform_later('Bob') }# subjectを実行することでエンキューされることをテストit do  expect { subject }.to have_enqueued_job(SampleJob).with('Bob').on_queue('sample_job')end
```

`\#perform_enqueued_job` によりジョブが同期的に実行されるため、その後に期待する状態をテストすればOK。  
※ 参考 : [https://api.rubyonrails.org/v5.2.3/classes/ActiveJob/TestHelper.html#method-i-perform_enqueued_jobs](https://api.rubyonrails.org/v5.2.3/classes/ActiveJob/TestHelper.html#method-i-perform_enqueued_jobs)

```
subject { SampleJob.perform_later('Bob') }it do  perform_enqueued_jobs { subject }  # ジョブ実行後に期待する振る舞いを以下に書くend
```

次のようなSampleMailerクラスがあり、`\#send_mail` でメールが送信されるものとする。

```
# app/mailers/sample_mailer.rbclass SampleMailer < ApplicationMailer  default from: 'from@example.com'  def send_mail    mail(to: 'to@example.com', subject: 'title', body: 'body')  endend
```

通常、テスト環境では実際にメールを送信せず、送信されたはずのメールは `ActionMailer::Base.deliveries` から参照できる。  
※ 参考 : [https://railsguides.jp/testing.html](https://railsguides.jp/testing.html)#メイラーをテストする

```
subject { SampleMailer.send_mail }it do  expect { subject }.to change(ActionMailer::Base.deliveries, :count).by(1)end
```

```
subject { SampleMailer.send_mail }it do  mail = subject  expect(mail.from).to eq 'from@example.com'end
```

```
subject { SampleMailer.send_mail }it do  mail = subject  expect(mail.to).to eq 'to@example.com'end
```

```
subject { SampleMailer.send_mail }it do  mail = subject  expect(mail.subject).to eq 'title'end
```

```
subject { SampleMailer.send_mail }it do  mail = subject  expect(mail.body).to eq 'body'end
```

外部APIを呼び出すオブジェクトを使っている場合などに使う。

```
let(:my_obj) { instance_double('MyObj') }before do  allow(my_obj).to receive(:my_method).and_return('Hello, world!')  allow(MyObj).to receive(:new).and_return(my_obj)endit do  my_obj = MyObj.new  expect(my_obj.my_method).to eq 'Hello, world!'end
```

```
subject { ... }let(:my_obj) { instance_double('MyObj') }before do  allow(my_obj).to receive(:my_method).and_return('Hello, world!')  allow(MyObj).to receive(:new).and_return(my_obj)endit do  subject  expect(my_obj).to have_received(:my_method).onceend
```

その他、Specの種類によらず共通でよく使うパターンを少しだけ。

```
subject { 1 / 0 }it do  expect { subject }.to raise_error(ZeroDivisionError)end
```

```
subject { 1 / 0 }it do  expect { subject }.to raise_error(ZeroDivisionError).with('divided by 0')end
```

```
subject { OpenStruct.new(name: 'Bob') }it do  expect(subject).to have_attributes(name: 'Bob')end
```

```
subject { [1, 10, 'Hello, world!'] }it do   expect(subject).to match [     1, 10, 'Hello, world!'   ]end
```

[Composing Mathcer](https://relishapp.com/rspec/rspec-expectations/v/3-8/docs/composing-matchers) を使えばより柔軟なテストができる。

```
subject { [1, 10, 'Hello, world!'] }it do   expect(subject).to match [     eq(1),     a_kind_of(Integer),     match(/^Hello/),   ]end
```

```
subject { { key: 'value' } }it do  expect(subject).to include(:key)end
```

```
subject { { key: 'value' } }it do  expect(subject).to include(key: 'value')end
```

```
subject do  [    { key: 'value1' },    { key: 'value2' },    { key: 'value3' },  ]endit do  expect(subject).to all(include(:key))end
```

```
subject { '123' }it do  expect(subject).to match(/^[0-9]+$/)end
```

```
subject { nil }it do  expect(subject).to be_nilend
```

`be_nil` に限らず、`nil?` のような `?` で終わるメソッドは `be_xxx` として使うことができる。  
※ 参考 : [https://relishapp.com/rspec/rspec-expectations/v/3-8/docs/built-in-matchers/predicate-matchers](https://relishapp.com/rspec/rspec-expectations/v/3-8/docs/built-in-matchers/predicate-matchers)

```
subject { 'Hello, world!' }it do  expect(subject).to be_a(String) # => success  expect(subject).to be_a(Object) # => successend
```

```
subject { 'Hello, world!' }it do  expect(subject).to be_an_instance_of(String)  # => success  expect(subject).to be_an_instance_of(Object) # => failend
```