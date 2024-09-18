---
finished reading: false
---
# いつuseCallbackを使うべきなのか - asoview-engineering - Medium
  #ReadItLater 
 #ReadableArticle

## articleURL
https://medium.com/asoview-engineering/when-to-use-react-usecallback-30d4dcc5099e

## siteName
asoview-engineering

## date
2024-09-18

## articleContent
[

![noguchia](Clippings/assets/いつuseCallbackを使うべきなのか%20-%20asoview-engineering%20-%20Medium-2024-09-18%2009-58-33/noguchia.jpg)



](https://medium.com/@akinogu1?source=post_page-----30d4dcc5099e--------------------------------)[

![asoview-engineering](Clippings/assets/いつuseCallbackを使うべきなのか%20-%20asoview-engineering%20-%20Medium-2024-09-18%2009-58-33/asoview-engineering.png)



](https://medium.com/asoview-engineering?source=post_page-----30d4dcc5099e--------------------------------)

Photo by [Joshua Earle](https://unsplash.com/@joshuaearle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[アソビュー！ Advent Calendar](https://qiita.com/advent-calendar/2020/asoview) 6日目です。

フロントエンドエンジニアの野口です。  
今回はReactのHooks APIの一つであるuseCallbackの使い所について書いていきます。

## useCallbackとは

メモ化したコールバック関数を返すHooks APIです。  
useCallbackを使うことで再レンダー時の不要な関数の再生成を防ぐことが出来ます。

## **使い方**

const memoizedCallback = useCallback(() => doSomething(a, b), \[a, b\])

公式のサンプルをもとに説明します。  
useCallbackは、以下の引数を取ります。

-   第1引数: メモ化するコールバック関数
-   第2引数: コールバック関数が依存する値（deps）

() => doSomething(a, b)

上記のサンプルだとこのコールバック関数はメモ化され、depsに指定した値が変わらなければ関数は再生成されません。

※depsの値はメモ化された関数を使うのか、関数を再生成するのかを判定する重要な値になります。設定のし忘れやミスは、不要な再生成やバグのもとになるのでESlintの[exhaustive-deps](https://github.com/facebook/react/issues/14920) を有効にすることが公式でも推奨されています。

## useCallbackの使い所

まず、useCallbackを使うメリットは主に

1.  関数の再生成コストを抑制
2.  React.memoやreact-reduxのconnectされたコンポーネントの再レンダーを抑制

ただし、1.の場合はuseCallbackを使うことでパフォーマンスが悪くなるケースも多いため、Profiler APIで計測しつつ使用するか、もしくは”**1.の目的のためだけには使用しない”**のが良いと思います。（関数の再生成のコストよりdepsの比較のコストなどが高くなる可能性があるため）

そのためuseCallbackの使い所は2.のケースになります。  
React.memoに軽く触れながら説明していきます。

## React.memo

React.memoはHOCであり、memoでラップしているコンポーネントのpropsに変更がなかった場合、最後に描画した結果を再利用します。

以下にサンプルを作成しています。consoleのlogを見ながらボタンを押してみてください。

-   React.memo適用前

React.memo適用前

-   React.memo適用後

React.memo適用後

適用前だと、状態に変更がないTitleコンポーネントも描画されてしまっていますが、適用後だと不要な描画を抑制できているのがわかります。

しかし、まだCounterButtonのコンポーネントは再描画の必要がないのに描画されてしまっています。  
これは、propsとして渡しているonClickが影響しています。functional component内に定義したアロー関数はレンダーのたびに毎回再生成されてしまうため、propsに変更があったと判定され再レンダーされています。

ここで使えるのがuseCallbackです。  
useCallbackで関数の再生成を防ぐことで、CounterButtonの不要な再レンダーを抑制できます。

-   React.memo × useCallback適用後

useCallback適用後

## 注意点

useCallbackは、React.memoなど参照の同一性をみるように最適化されたコンポーネントのみ効果があります。

\- React.memo  
\- react-reduxのconnect  
\- PureComponentを継承するclassコンポーネント  
\- shouldComponentUpdateを実装したclassコンポーネント  
\- （recomposeのpureでラップしたコンポーネント） など

そのためそれ以外のコンポーネントや直接HTML要素にコールバック関数を渡す以下のような場合では効果がありません。

const onClick = useCallback(() => doSomething(a), \[a\])  
return <button onClick={onClick} />

## どこまで最適化すべきか

useCallbackに関して言えば、React.memoなどを使ったコンポーネントにアロー関数を渡す場合は、基本的にuseCallbackを使うべきだと思います。 コンポーネントの再レンダーコストを抑えられるからです。

ただ、depsの計算コストはかかってくるので、React.memoを使うかどうかと合わせProfiler APIなどで検証しつつ最適化を進めていくのがベターになりそうです。  
個人的には、[こちらの記事](https://kentcdodds.com/blog/usememo-and-usecallback)にもある通り再レンダーによる最適化に時間をかけすぎるのは賢明ではないので、判断迷う場合には**useCallbackを使わない**というのも選択肢の一つとしてありだと思っています。

## おわりに

useCallbackはうまく使えばパフォーマンスを改善することができる有用なhooksですが、意図せずパフォーマンスを悪化させてしまうケースもあります。  
アソビューでもそうでしたが、どこまで最適化するかチームメンバーと話し合い対応していくのが良いと思います。

## 参考

[https://qiita.com/uehaj/items/99f7cd014e2c0fa1fc4e](https://qiita.com/uehaj/items/99f7cd014e2c0fa1fc4e)  
[https://qiita.com/soarflat/items/b9d3d17b8ab1f5dbfed2](https://qiita.com/soarflat/items/b9d3d17b8ab1f5dbfed2)  
[https://ja.reactjs.org/docs/hooks-reference.html](https://ja.reactjs.org/docs/hooks-reference.html)  
[https://kentcdodds.com/blog/usememo-and-usecallback](https://kentcdodds.com/blog/usememo-and-usecallback)  
[https://dmitripavlutin.com/use-react-memo-wisely/](https://dmitripavlutin.com/use-react-memo-wisely/)