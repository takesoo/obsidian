[https://blog.jxck.io/entries/2018-10-26/same-site-cookie.html](https://blog.jxck.io/entries/2018-10-26/same-site-cookie.html)

  

Cookie はブラウザによって保存され、紐づいたドメインへのリクエストに自動で付与される。

この挙動によって Web におけるセッション管理が実現されている一方、これを悪用した攻撃方法として、 CSRF や Timing Attack などが数多く知られており、個別に対策がなされてきた。

現在、提案実装されている SameSite Cookie は、そもそもの Cookie の挙動を変更し、こうした問題を根本的に解決すると期待されている。

Cookie の挙動とそれを用いた攻撃、そして Same Site Cookie について解説する。

Cookie は、 Set-Cookie によって提供したドメインと紐づけてブラウザに保存され、同じドメインへのリクエストに自動的に付与される。

最も使われる場面は、ユーザの識別子となるランダムな値を SessionID として付与し、その有無によってセッション(=ログイン状態)を維持するという用途だろう。

もし SessionID が盗まれれば成りすましが可能となるため、セキュリティのコンテキストでは Cookie 自体が _Credential_ として扱われることもある。

そして今回注目するのは、 Cookie が「_ドメインをまたぐリクエストにも自動で付与される_」という挙動だ。

もし同じドメイン内でしか付与されなければ、例えば Site A から Site B に遷移した時に、必ず B のログイン画面が出てしまい、一旦リロード(B から B への遷移)するとログイン済みになるという挙動になってしまう。

まず、この Cookie の仕様を利用した攻撃方法について解説し、その後 SameSite Cookie について解説する。

## [Cookie の挙動を利用した攻撃](https://blog.jxck.io/entries/2018-10-26/same-site-cookie.html#cookie-%E3%81%AE%E6%8C%99%E5%8B%95%E3%82%92%E5%88%A9%E7%94%A8%E3%81%97%E3%81%9F%E6%94%BB%E6%92%83)

「自動的に付与される」という挙動を利用した攻撃方法はいくつか存在する。

例として、 `https://sns.example.com` という SNS の Session Cookie がブラウザに保存されていたとする。

ブラウザは `https://sns.example.com` へのリクエストには、その Cookie を自動で付与する。

### [CSRF](https://blog.jxck.io/entries/2018-10-26/same-site-cookie.html#csrf)

CSRF は、この SNS の場合、別の罠サイトを用意し SNS への正規の投稿と同じリクエストを作り出すことで、ユーザに対して意図しない投稿を行わせるといった攻撃である。

例えば、正規の投稿はログイン状態で `https://sns.example.com/posts/new` にある以下の form から行うものであったとする。

```
<form action=/posts method=post>  <textarea name=msg>hello</textarea>  <button type=submit>post</button></form>
```

この form を submit した時に送信される HTTP Request の概観は以下のようになるだろう

```
POST /posts HTTP/1.1Host: sns.example.comCookie: deadbeefOrigin: https://sns.example.comReferer: https://sns.example.com/posts/newmsg=hello
```

サーバは、 Cookie の正当性を確認し、正ければ投稿を受け付ける。

ここで、攻撃者は `https://darkside.jxck.io/csrf` に以下のような HTML を仕込んだ罠ページを用意する。

```
<form action=https://sns.example.com/posts/new method=post>  <textarea name=msg>こんにちはこんにちは</textarea>  <button type=submit>post</button></form>
```

Form の action に URL 全体を入れている点に注目したい。

そして、 `https://sns.example.com` にログイン済みのユーザをこのページに誘導し、なんらかの方法で submit させる。もしくは Form を隠して JS で強制的に submit してもよい。

ここで生成される HTTP Request は以下のようになるだろう。

```
POST /posts HTTP/1.1Host: sns.example.comCookie: deadbeefOrigin: https://darkside.jxck.io/csrfReferer: https://darkside.jxck.io/msg=こんにちはこんにちは
```

このとき Cookie は sns.example.com で取得したものが自動で付与される。

つまり、サービスが Cookie だけを検証しているならば、このリクエストは正しいものとして受け入れられ、ユーザが意図しない投稿が成功してしまうのである。

これが、商品の購入や送金、パスワードの変更だった場合は、さらに被害が大きくなるだろう。

対策としては、まず Origin ヘッダや Referer ヘッダの確認が思いつく。

ところが、 Referer を送らないように設定するユーザもいることが知られており、 Referer が無かったらリクエストを弾くという実装は現実的ではない。

罠サイトは Referrer Policy を用いることで、 Referer が送らないように設定できるため、 Referer をベースとした対策は万全とは言い難い。

Origin ヘッダは、そもそもが生成元の Origin を通知するためのものなので、この検証用途で使うには妥当と言える。

しかし、 valid な値が推測可能なため、もしブラウザからのリクエストに _任意のヘッダを追加できる脆弱性_ が別で存在した場合には、簡単に偽装できてしまう。

そして、残念ながら近年も [そうした脆弱性](https://insert-script.blogspot.jp/2018/05/adobe-reader-pdf-client-side-request.html) が見つかっているため、追加で別の対策が求められているのが現状だ。

[参考: Referrer-Policy によるリファラ制御 | blog.jxck.io](https://blog.jxck.io/entries/2018-10-08/referrer-policy.html)

結果として、暗号論的に安全な乱数を One Time Token として生成し、それを Form に hidden で隠して、_意図した Form からのリクエスト_ かを検証する方法が主流となっている。

```
<form action=https://sns.example.com/posts/new method=post>  <textarea name=msg>こんにちはこんにちは</textarea>  <input type=hidden name=csrf_token value=asdfqwerzxcv>  <button type=submit>post</button></form>
```

```
POST /posts HTTP/1.1Host: sns.example.comCookie: deadbeefOrigin: https://darkside.jxck.io/csrfReferer: https://darkside.jxck.io/msg=こんにちはこんにちは&csrf_token=asdfqwerzxcv
```

ここまでの内容は POST で解説したが、現実には「足跡」や「既読」機能のように GET が副作用を持つ設計の場合も、同じように攻撃が可能だ。

その場合は、単にリンクを設置して踏ませるだけで良いので、攻撃はシンプルだが対策は難しい。

元をたどれば、罠サイトからの偽装されたリクエストに Cookie が付与されなければ、この攻撃は防ぐことができる。

Timing Attack は、古くから存在する手法であり、その方法も対象も様々なものがある。

Web のコンテキストで共通するのは、罠サイトから対象のサービスに向けて XHR などでリクエストを投げさせるというものだろう。

この時、実際にレスポンスを取得することができなくてもよく、リクエストにかかる時間を計測し、リクエストを変化させた時のレスポンスタイムの変化から機密情報を推測するというシナリオだ。

例えば先の SNS でユーザのページへリクエストする場合、 block していなければプロフィールページが表示されるが、 block していれば定型文が返るためレスポンスが速いとする。

```
function timing_attack() {  img = new Image()  t1 = performance.now()  img.onerror = () => {    t2 = performance.now()    // block していれば速く    // block していなければ遅い    console.log(t2-t1)  }  img.src = "https://sns.example.com/jxck"}
```

これを利用して、対象者が誰を block しているか推測するといった攻撃が考えられる。

直近では Twitter に報告された Silhouette (シルエット攻撃)が、まさしくこれにあたる。

こうした攻撃の推測精度を下げるため、 performance.now の精度を落とすという施策がブラウザベンダによってなされることもある。

しかしこの場合も、罠サイトからのリクエストに Cookie 送信されなければ、毎回ログインページのレスポンスが返ることになり攻撃が成立しない。

CRIME/BEAST/BREACH は、 TLS の暗号化や圧縮に関わるロジックを逆手に取り、ペイロードを推測していく Side Channel Attack の亜種である。

こうした攻撃は TLS のプロトコルレベルでの解決(暗号方式の変更や圧縮の無効など)によって防ぐことが基本となるうえ、攻撃が成立した場合 Cookie 以外の情報も抜くことは可能である。

しかし、代表的な攻撃シナリオはやはり、ターゲットを罠サイトなどに誘導し、微妙にペイロードを操作しながら連続でリクエストを発生し、自動で付与される Cookie を推測するというものが多い。

例えば CRIME の場合は、 Cookie の値が一致すれば圧縮が効きパケットが小さくなることを観測するだけでよい、イメージとしては以下のような感じだ。

```
for(...) {  word = `ここを変えて行く`  await fetch("https://sns.example.com/", {    body: `Cookie: ${word}`  })}
```

今後、類似する未知の攻撃があったとしても、そもそも Cookie が付与されていなければ推測できない場合は、攻撃を未然に防げる可能性もある。

XSSI は JSONP のエンドポイントから取得できる情報を、罠サイトで横取りする脆弱性である。

これも Cookie が自動で送られなければと言いたいところだ(そうした解説もある)が、 JSONP を使っていることの方が問題なので、解説は割愛する。

そもそも「_他のドメインからのリクエストでも、 Cookie が自動で付与される_」という挙動を制御できれば、多くの脆弱性に有効な対策になることがわかっただろう。

そこで、「この Cookie は他のサイトからのリクエストには _付与してはならない_」ということを明示的にブラウザに知らせるのが SameSite Cookie である。

SameSite Cookie は、 Set-Cookie ヘッダに付与する新しい属性であり、現状 2 つの値を取る。

```
Set-Cookie: key=value; SameSite=StrictSet-Cookie: key=value; SameSite=Lax
```

- [https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-02#section-5.3.7](https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-02#section-5.3.7)

SameSite 以外からの全てのリクエストで一切 Cookie を送らなくなる。

これを適用できれば、かなりの問題が解決する強い制限である。

しかし、単に Session Cookie にこの属性を付与すると、例えば別のサイトからリンクで遷移した場合にも Cookie が送られなくなる。

すると、別のサイトから遷移する場合は、毎回ログインが必要となるため、ユーザの利便性も考えると難しい。

CrossSite のリクエストでは、 Top Level Navigation 以外は Cookie を送らない。

これにより、別サイトからの遷移でもログイン状態が維持できるため、既存の Session 管理にも導入しやすい。

POST では Cookie が送られないため、後述する CSRF のような攻撃への耐性が高まる。

また、 XHR/fetch() や `<img>` を用いて発生させる GET は Top Level Navigation ではないため Cookie がつかない。

従って、 JS からリクエストを生成し、時間を測るタイプの攻撃への耐性もある。

しかし、 Popup Window や `<link rel=prerender>` などは Top Level Navigation となるため、いくつかの攻撃には依然として耐性が無い点に注意したい。

Lax で送られるかどうかをまとめると以下のようになる。

Lax Cookie が送られるかどうかの API まとめ

[https://www.sjoerdlangkemper.nl/2016/04/14/preventing-csrf-with-samesite-cookie-attribute/](https://www.sjoerdlangkemper.nl/2016/04/14/preventing-csrf-with-samesite-cookie-attribute/)

実際に Same Site Cookie の導入する方法は、大きく 2 つある。

既存のセッション管理が単体の Session Cookie で運用されている場合、そこに Strict を付けるのは難しい。

他のサイトから遷移した場合に毎回ログインを要求してしまうため、利便性の観点から問題が出る。

そこで、互換性を崩さずに導入するためには、すでに運用されている Session Cookie に lax を付与するのが安全かつ導入しやすいだろう。

これにより、典型的な POST での CSRF や一部の Timing Attack は防ぐことができる。

しかし、 Top Level Navigation を用いた攻撃があった場合は効果がない。

より根本的に安全性を高めるのであれば、副作用のある操作は全て Strict にするのが望ましい。

そこで、 RFC では Cookie を Read Cookie と Write Cookie の 2 つにわける構成が言及されている。

- [https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-5.2](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-5.2)

この 2 つに Cookie を分離し、 Write Cookie に Strict を適用すれば、より堅牢な設計となる。

Read Cookie は、副作用を起こす能力がなくなっているのであれば、 Lax でも SameSite 無しでも良い。

```
Set-Cookie: read=zxcv; Path=/; Secure; HttpOnly;Set-Cookie: write=asdf; Path=/; Secure; HttpOnly; SameSite=Strict
```

もし新規の開発や改修が可能であるならば、こうした構成も可能かもしれないが、既存のサービスのほとんどはここまで手を入れるのは難しだろう。

また、 HTTP Method に関わらず、副作用を起こす操作を全て Strict にしているため、例えば前述の足跡や既読のように、別ドメインからの Top Level Navigation で遷移しただけで副作用を起こす設計では、この構成は難しい。

例えば、既読を write cookie にしか許さない場合、ユーザページのリンクを別のサイトで踏んで遷移してきたユーザの既読がつかない。

Redirect を挟むのは Write Cookie を分けた意味がないため、一旦別のページを挟んでリンクを踏ませるなどし、 Same Site Request に誘導してやる必要がある。

そもそも、副作用は POST/PUT/DELETE などでしか起こさないという設計になっているのであれば、 Lax で十分であるため、ここまでの設計は不要な場合が多そうだと筆者は考えている。

## [Cookie の改善](https://blog.jxck.io/entries/2018-10-26/same-site-cookie.html#cookie-%E3%81%AE%E6%94%B9%E5%96%84)

この Cookie の仕様に問題があるという認識は共通しており、 SameSite のような属性の付与では無く、根本的に設計し直そうという話もある。

[mikewest/http-state-tokens: Incrementally better HTTP state management](https://github.com/mikewest/http-state-tokens)

また、ちょうど Read/Write の Cookie 分離を仕様レベルで行う提案が出たりもしている。

[[CSP3] Suggestion for COOKIE directive](https://lists.w3.org/Archives/Public/public-webappsec/2018Oct/0029.html)

他にも類似する問題(Cookie に限らず)は、 Cross Origin Info Leaks という文脈で議論されることが多いが、長くなるのでここではこれ以上言及しない。

本サイトでは Google Analytics 以外に Cookie は使っていないため、適用は無い。