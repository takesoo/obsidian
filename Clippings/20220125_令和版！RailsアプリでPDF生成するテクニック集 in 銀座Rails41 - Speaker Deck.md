---
URL: https://speakerdeck.com/morimorihoge/20220125-ling-he-ban-railsapuridepdfsheng-cheng-surutekunitukuji-in-yin-zuo-rails-number-41?slide=26
---
[jacobian 263 20k](https://speakerdeck.com/jacobian/how-to-ace-a-technical-interview)

## Transcript

1. \#41 ※スライドは発表後に \#ginzarails で公開します
    
    ### [令和版！ RailsアプリでPDF生成するテクニック集 森 雅智 / @morimorihoge 2022/01/25 1 銀座Rails](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_0.jpg)
    
2. Ruby/Rails歴は11年くらい。Web開発は17年くらい ◦ いわゆる受託業務アプリケーション開発屋です • 銀座Rails\#37より銀座Railsの運営を引き継いでいます About BPS & TechRacho • Web受託開発の他、電子書籍製品・勤怠・入退室サービス • TechRachoという自社技術Blogを運営しています ◦ 平日毎日更新してます ◦ https://techracho.bpsinc.jp/ • お仕事相談、転職相談、TechRachoへのご意見など気軽にどうぞ ◦ https://www.bpsinc.jp/ 2
    
    ### [About Me • 森 雅智： @morimorihoge • BPS株式会社でRailsの受託開発チームをやってたり、週1大学非常勤で Web開発を教えてたりします •](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_1.jpg)
    
3. コンテンツ配信システムで、再配布抑止用のウォーターマークを入れたい 業務システム開発経験がある程度あれば、一度は経験があるのでは 3
    
    ### [PDF作成機能、依頼されたことある人ー？ • 領収書、請求書、納品書をシステムから出したい！ • 倉庫等での業務で必要なピッキングリストや在庫一覧を紙に印刷したい！ • 業者指定伝票などの専用用紙への手書きを印刷で自動化したい！ • チケット予約システム等で、印刷用のQRコード付きPDF等を出したい •](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_2.jpg)
    
4. ### [「PDF作成機能」の奥深さ 最終的に出力するものがPDFでも、何に使うか・どんな出力をしたいかによってその難 易度・実装方法はかなり異なる 言葉尻だけを捉えて「以前このやり方でうまくいったから今回もいけるだろう」とやると失 敗しがち 4 refer: https://dic.nicovideo.jp/a/顧客が本当に必要だったもの](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_3.jpg)
    
5. • 文書改変や流出を抑止して証跡として利用できる ◦ 暗号化やセキュリティ機能で実現可能 ◦ 印刷を許可してしまうと プリンタドライバ経由でPDF生成するツール などがあるので、厳密に全ての ケースで理想的に使えるかは要注意 • 複数ページに跨がる文書データをファイルとして長期保存できる ◦ 正しい。クラウドストレージなどを使えば実質無限の文書保存が可能 ◦ 紙を保存するコストがかからず便利なので、積極的に利用したい • 世間一般で受け入れられやすいファイル形式である ◦ 正しい。とりあえずPDFにしておけば困らないケースは多い ◦ プログラムがPDFの中身を解析するユースケースでは相互運用に問題が発生する可能性があるの で要注意（後述） 7
    
    ### [なぜPDFにしたいのか？～PDFにまつわる誤解～ • あらゆる印刷環境で印刷して同じ印刷結果が得られる ◦ 間違い。PDFの規格によっては仕上がりが想定通りにならない。埋め込まれてないフォント指定や 開く環境依存のコンテンツが含まれる可能性がある ◦ PDF/XやPDF/Aにするのであれば概ねOK。ただし、印刷物の解像度や余白については印刷環境 依存（プリンタ性能や設定）が残る](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_6.jpg)
    
6. ◦ 改行を挟むと別の文字列として埋め込まれてたり ◦ 見た目に現れない内部メタデータや指定方法が異なったり ◦ PDFファイルからdiffを取ったり中身を（正確に）読みとるのは簡単ではない • PDFなら安心という謎の安心感からか、問題発生まで誰も気づかないことがある ◦ 印刷データ入稿や専用紙印刷など、 精度の高い印刷が求められるケースで踏みがち 8 こうした技術的な罠にはまらないように 事前にユースケースをヒアリング しておきたい
    
    ### [PDFの闇 • フォーマットの種類が大量にある ◦ PDF、PDF/A、PDF/X、PDF/E他。さらにPDF/X-1a、PDF/X-4等詳細バージョンも・・・ • PDF作成方法（ライブラリ等）によって見た目は同じでも異なる内部データになる ◦ 文章が一連の文字列になってたり分割された文字列になってたり](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_7.jpg)
    
7. 請求書番号請求先・請求元 ページがバラバラになっても分かるようになっている 明細のヘッダ行を改ページに合わせて再表示
    
    ### [請求書：明細が増えた時、レイアウトし直すver 13 1ページ目 2ページ目 多分現実にはこれくらいの仕様は必要なはず・・・ • 何ページ綴りの何枚目か？ •](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_12.jpg)
    
8. ぱっと見似たような一覧だが用途別に色々な種類のものを要望されがちなので、業 務でどのように使われるのかを具体的に意識したい ◦ ピッキングするときは棚番号（置き場所）でソートしたい ◦ 棚卸しするときは区分別の数（棚にある、倉庫にはあるがピッキングされた、返品数他）が出て欲し い ◦ 入荷担当者としては日次・週次でどのくらい在庫が動いているかを知りたい ◦ その他諸々 15
    
    ### [在庫一覧やピッキングリスト • 物流系や在庫管理システムで、印刷したリストをピッキングや棚卸担当者に渡し、 担当者はそれを見ながら業務をするような想定 • レイアウトの作りこみが業務効率に直結するので、顧客ごとに独自のレイアウトを 要求されることが多い ◦ 文字サイズ～項目の順序や許容文字数、1ページあたりの行数など •](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_14.jpg)
    
9. プリンタ相性等もあるため、トライ＆エラーと微調整の手間がかなりかかる • その他独自の指定もあったりなかったり ◦ OCR前提の伝票の場合、フォントの指定などがあることも 17 ＋ ＝ 指定用紙（物理的な紙） 印刷データ 仕上がり
    
    ### [専用用紙への印刷 • 既に罫線等のレイアウトが引かれた配達業者の指定伝票用紙や専用の申込書 等、記載済み指定用紙の上から印刷するケース ◦ カーボン複写シートでも、ドットインパクトプリンタを使えば印刷できる • 印刷位置をmm単位で合わせる必要がある ◦](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_16.jpg)
    
10. ウォーターマークの埋め込み位置が元コンテンツに被ったりしないか等も要確認 19 SER: AABBCCDDEEFF9988 Owner: morimorihoge SER: AABBCCDDEEFF9988 Owner: morimorihoge SER: AABBCCDDEEFF9988 Owner: morimorihoge
    
    ### [既存コンテンツへの透かし追記 • コンテンツ販売時に利用者のダウンロードするPDFにウォーターマーク（電子透か し）を入れるようなケース ◦ 技術的にコピー配布禁止の完全な手段ではないが、再配布を抑制する意図などで使われる • 提供される元PDFデータに追加書き込みしたデータを作る必要がある ◦](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_18.jpg)
    
11. 動的レイアウトの場合、システム側の実装都合でレイアウトを決める自由度はある か？ ◦ 改ページ対応も含めて完全に自由に作ってしまって良い ◦ ある程度ビジネスサイドから細かい指定がある ◦ 完全に決まっていて動かせない • レイアウトのメンテナンス（修正・追加）頻度はどれくらいか？ ◦ 一度作ったらほとんど変更することはない（あっても画像差し替えレベル） ◦ ビジネス要件に応じて頻繁に更新していきたい • データ連携などPDFデータ仕様に関する指定はあるか？ ◦ PDFさえ出力できればそれで良い（印刷も気にしない） ◦ PDF/Xで印刷の一意性が担保されればそれで良い（内部データは気にしない） ◦ ファイルのセキュリティ要件他細かい指定がある 21 各項目上が簡単、下に行くほど難しい（めんどくさい）ので参考までに
    
    ### [技術的な確認ポイント • レイアウトはほぼ固定か、動的レイアウトか？ ◦ 常に同一ページ数、同一レイアウトに収まる ◦ 埋め込むデータによってページ数・レイアウトが可変になる •](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_20.jpg)
    
12. ◦ 歴史があって人気のGemのため、事例が豊富で調べものをしやすい • 欠点 ◦ 複雑なレイアウト・場合分けになってくるとコード量が増え、メンテ困難になりやすい ◦ Rubyのコードが読める人にしかレイアウトがメンテできない 25 PDF生成機能がコアバリューになるプロダクト なら拡張性の高さから選択もありだが、単に PDF帳票を出せれば良いといった用途で使うには primitiveすぎる印象
    
    ### [プログラムでPDFをゼロからレイアウトする系 • 皆さんご存じ prawnpdf/prawn • 利点 ◦ プログラムでレイアウト・コンテンツ埋め込みをしていけるので、 プログラミングできる仕様であれば 固定～可変どんなPDFでも作れる](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_24.jpg)
    
13. ◦ puppeteerで出力させるLambda関数などを自前で用意するとかでも良さそう • 利点 ◦ レイアウトデータをHTML/CSSで記述できる！ ◦ `@media print`な印刷用CSSを記述することで、改ページ制御などもある程度は可能 • 欠点 ◦ 縦位置を細かく揃えたり細かい配置制御をやろうとすると ネ申CSSを書かざるを得なくなり、可読性 が低下していく ◦ 長めのコンテンツを流し込んだ時に思わぬ改ページが発生したりしがち 26 できあがりの詳細な制御をしようとすると難しい点もあるが、うまく作ればHTML用とPDF用の テンプレートHTMLを共存できたり、システム側のメンテナンス性は高い
    
    ### [HTML/CSSからPDFに変換する系 • headlessブラウザにレンダリングさせたWebページをブラウザの機能でPDF出力さ せる • Railsだと mileszs/wicked_pdf がメジャーだが、後述する理由により現代では Studiosity/grover を使う方が良いかも](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_25.jpg)
    
14. 停滞気味。本家WebKitとも相当離れてしまった。wkhtmltopdfも追随できていない • puppeteerのpage.pdf APIがとってもwkhtmltopdfのAPIに似ているぞ🤔 こうしたこともあり、公式ではuntrustedなHTMLは食わせるななど注意喚起されている 27 https://wkhtmltopdf.org/status.html
    
    ### [注意：wicked_pdf（wkhtmltopdf） の利用について wicked_pdfがPDF生成に利用するwkhtmltopdfは、色々あって更新が止まっている。詳細 は公式のStatusページを読んで欲しいが、ざっとまとめると • wkhtmltopdfはWebKitの古いin-process APIに依存していて、ユーザーが多い割に 開発の手が足りず更新できていない • wkhtmltopdfのレンダリングにはQtWebKitを使っているが、QtWebKit自体の更新が](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_26.jpg)
    
15. Grover(Ruby)
    
    ### [puppeteerのPDF出力API ブラウザ操作の自動化ライブラリのPuppeteerがPDF出力をサポートしているので、 headless ChromiumやFirefoxが実行できればそちらからPDFを生成できる Studiosity/grover はPuppeteerを利用して出力している 28 新規で実装するならこちらを使うのが良さそうに見える Puppetteer(JS)](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_27.jpg)
    
16. Thinreports Editor Thinreports レイアウト ファイル .tlf Rubyコードから レイアウトファイルを読み込み PDF \#generate HTMLtoPDFが要求を満たさない場合、 僕のオススメはこれ です
    
    ### [レイアウトエディタとプログラムの組み合わせ系 GUIのレイアウトエディタでレイアウトテンプレートファイルを作成し、レイアウトファイルを プログラムで読み込んでデータ埋め込みを行う Ruby界隈では thinreports/thinreports が古くからあり、帳票であればこれでかなりの ケースが対応できる 29 レイアウトエディタ（GUIアプリ）](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_28.jpg)
    
17. ▪ 位置合わせがシビアなPDF作成にも力を発揮する ◦ 編集内容によっては Rubyが書けなくてもレイアウト調整できる ▪ 既存項目の位置調整やフォントサイズ調整などはGUIで完結できる ▪ エンジニア側で仮の位置決め＋変数名決めをしてから、ビジネスサイドに微調整をお願いす るといった役割分担が可能 ◦ 多ページに跨がる伝票を表現する帳票特化機能がある • 欠点 ◦ GUIの挙動がやや個性的 ◦ レイアウトファイルの編集には多少の慣れが必要 32 HTMLtoPDFが要件を満たない。かつ帳票の更新頻度が高かったり、取り扱う帳票 の種類が多い場合にオススメしたい
    
    ### [ThinreportsでPDF生成する特徴 • 利点 ◦ レイアウトエディタがGUIなので、 直感的に微調整ができる ▪ 「このブロックとこのブロックは縦位置を合わせてるから動かすときは同時に」みたいなのは GUIになっていないと見落としがち](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_31.jpg)
    
18. https://github.com/boazsegev/combine_pdf
    
    ### [PDF結合系 PDFファイルAとPDFファイルBを「重ね合わせて」結合する boazsegev/combine_pdf を使うと実現可能。 33 ＋ ＝ A.pdf B.pdf combined.pdf](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_32.jpg)
    
19. ◦ 他多数 • テンプレート形式なもの ◦ Anvil ◦ PDF Generator API ◦ 他にも探せばありそう あとは、Excelxで書式を作ってExcelxファイルのデータをRubyで更新し、LibreOffice APIでPDF出力させる などの方法もある。 LaTeXのコードをテンプレート生成してコンパイルする とかもありだし、やりようはいくらでもある。どのやり 方を選択するかの問題。 34 各々の前提条件に応じて一番楽、かつ運用に耐える選択 をしていきたい
    
    ### [その他の選択肢 クラウドサービスにやってもらうというのもあり。ただし、sensitiveなデータを生成させる場合にはサービス の身元チェックもしておきたい（無料サービスは恐い気がする） • HTML to PDFなもの ◦ HTML2PDF](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_33.jpg)
    
20. ### [ちゃんとPDF/Xにしたい 埋め込みフォント化やCMYK化など色々とやる必要があるが、恐らく最もてっとり早いの は vibranthq/press-ready を使うことだと思われる。ありがたい • 内部的にはGhostscriptを使って変換をかけているようだ • フォント周りでカスタマイズしている場合、解決のための試行錯誤は必要そう 36](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_35.jpg)
    
21. 動かすだけならWindowsやMacからフォント抜いてきて使うようなサンプルコードが転 がってたりしますが、仕事でやると危険です フォントのライセンスに要注意 38
    
    ### [フォントのライセンスはフォントごとにかなり異なるので、自由に使えるフリーフォント以外 を利用する場合にはライセンスに十分注意する • Webフォントとして使うのは自由にできても、印刷・埋め込みは別ライセンスだったり • クレジット表記や商標利用に関する制限があったり • インストールに関する要件（端末台数）がある場合、Docker Imageに入れてコンテナ deployしても大丈夫そうか](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_37.jpg)
    
22. プロポーショナルフォントを使うと禁則処理との泥沼の戦いを要求される（言い過ぎか） • 禁則処理の暴走 ◦ HTMLtoPDF方式を使っている場合は特に 禁則処理にも要注意。ブロックが横にはみ出したり文字 が突き抜けたりするケースがある。 要文字数パターンテスト ◦ 大抵色々試行錯誤したあげく ` word-break: break-word; `することになりがち ◦ CSS禁則処理についてはググると色々なドキュメントが見つかるのでそちらを参照して下さい 39 なるべく早い段階から実データ・かつその中でも文字数の多いサンプル を使って開発をすると良 い
    
    ### [枠に文字を収めるの大変問題 • 等幅 or プロポーショナルフォント ◦ 「N文字を超えたら罫線をはみ出る（or コンテナが大きくなりすぎる）ので改行」といった処理をする には等幅フォントを使う必要がある ◦](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_38.jpg)
    
23. ◦ 初回アクセス時に一度生成したら保存しておき、二回目以後は前回生成データをそのまま返すよう な実装が必要 • その他、インボイス制度なども始まるので、制度に合致した書類形式になっている かどうかは会計に詳しいメンバーにも相談して確認すると良い 40
    
    ### [伝票系PDF出力の注意 • 「領収書」などの会計上証跡が重要になるデータはリクエストの都度オンデマンド生 成してはいけない ◦ マスタ参照しているデータに変更があったとしても 一度出力した伝票の内容が書き換わってはいけ ない ◦ 仕様を間違えると1注文で沢山の宛名違い領収書を生成できてしまい、脱税に利用される可能性](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_39.jpg)
    
24. 暗号化など、パイプライン的に処理するような内容になると 特に即時応答は難しいので非同期ジョブ化必須。 ページ数の多い機能になる場合は特に、ベンチマークはしておいた方が良い（または時 間がかかっても問題無いUX設計にしておく） 41
    
    ### [PDF生成が重い問題 生成方式にもよるが、画像ファイルを含むPDFは特にメモリ・CPUをがっつり食う傾向に ある。 Railsサーバーのプロセスに処理させるとmemory bloatが起きやすいので、できれば ActiveJob等でPDF生成部分だけ非同期のジョブにバラすのが良い。 PDF生成 -> 透かし結合 ->](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_40.jpg)
    
25. ### [mm単位での位置合わせがうまく合わない・・・ 根気です。割とマジで • 紙に印刷して定規で測りながら微調整したり、プリンタ側の設定との干渉を疑ったり など、根気よく調整して見ていくしかない • 専用用紙印刷系の場合はPDFビューアでは問題無いのに印刷するとズレる、みた いなケースもあるので、必ず実機チェックもすること 42](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_41.jpg)
    
26. 軽に登録下さい :) ◦ https://ginza-rails.connpass.com/ 43
    
    ### [まとめ • これまでの自分の開発経験の中で扱ってきたPDF生成のバリエーションを紹介しま した。恐らくほとんどのユースケースはここで紹介した方法の組み合わせでカバー できると思います • 本資料を皆さまがPDF生成機能を要望された場合の要件整理・設計方針決めの参 考にして頂ければ幸いです • 【宣伝】銀座Railsの発表者は毎月募集しています！LT枠もできましたのでぜひお気](https://files.speakerdeck.com/presentations/6f5b1153c0ca4a24bd76dc36159009dc/slide_42.jpg)