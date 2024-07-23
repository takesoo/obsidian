---
finished reading: false
---
# Render hooksをコンポーネントの拡張として理解する - Qiita
  #ReadItLater 
 #ReadableArticle

## articleURL
https://qiita.com/uhyo/items/cb6983f52ac37e59f37e

## siteName
Qiita

## date
2024-07-23

## articleContent
**[Render hooks](https://engineering.linecorp.com/ja/blog/line-securities-frontend-3)** とは、ReactにおいてカスタムフックからJSX式を返す設計パターンのことです。リンク先は私が当時在籍していた会社のテックブログに書いた記事で、当時の会社でこの設計パターンがハマる箇所に出会ったためアイデアを記事化したものです。ちなみに、Render hooksという命名は私ではなく当時の私の上司です。

私は当時から今までずっとこのパターンを推奨しているのですが、あまり流行る気配がありません。そこで、この記事では皆さんがこのパターンの考え方にもう少し納得できるように、render hooksパターンは普通のコンポーネントの拡張であるという見方をご紹介します。

## [](https://qiita.com/uhyo/items/cb6983f52ac37e59f37e#render-hooks%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%AE%E6%A6%82%E8%A6%81)Render hooksパターンの概要

Render hooksパターンは、UIの実装（JSX）と、そのUIに関連するロジック（たとえばステート）をまとめてカスタムフックから提供することを指します。簡単な例を示します。

```
function useCheckbox() {
  const [checked, setChecked] = useState(false);
  const checkbox = (
    <input
      type="checkbox"
      checked={checked}
      onChange={(e) => {
        setChecked(e.currentTarget.checked);
      }}
    />
  );

  return [checked, checkbox];
}
```

このフックでは、チェックボックスがチェック済みかどうかを表す状態`checked`と、その状態を操作するためのUIである`checkbox`をセットで提供しています。

この例のポイントは、`setChecked`がこのフックの中に隠されていることです。いわゆるカプセル化というもので、`checkbox`を介さずに`checked`を変更することが不可能な設計になっており、コードを書いた人の意図が分かりやすくなっています。

使う側はこういう感じです。

```
export default function App() {
  const [checked, checkbox] = useCheckbox();

  return (
    <div>
      <p>Checkbox is {checked ? "checked" : "not checked"}!</p>
      <p>{checkbox}</p>
    </div>
  );
}
```

また、UIがより複雑な場合は次のような設計も可能です。

```
// これはエクスポートする
export function useCheckbox() {
  const [checked, setChecked] = useState(false);
  const checkbox = <Checkbox checked={checked} setChecked={setChecked} />;

  return [checked, checkbox];
}

// 内部で使われているCheckboxコンポーネントはエクスポートしない
const Checkbox: React.FC<{
  checked: boolean;
  setChecked: (value: boolean) => void;
}> = ({ checked, setChecked }) => {
  return (
    <label>
      <input
        type="checkbox"
        checked={checked}
        onChange={(e) => {
          setChecked(e.currentTarget.checked);
        }}
      />
      チェックボックス
    </label>
  );
};
```

コンポーネントを直接エクスポートするのではなく、それをラップするカスタムフックだけをエクスポートして、それを介してコンポーネントを提供するという考え方です。

Render hooksパターンは、うまく使うことで、Reactアプリケーションの設計を改善することができます。

## [](https://qiita.com/uhyo/items/cb6983f52ac37e59f37e#%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0%E3%83%95%E3%83%83%E3%82%AF%E3%81%AF%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E3%81%AE%E6%8B%A1%E5%BC%B5%E6%A6%82%E5%BF%B5)カスタムフックはコンポーネントの拡張概念？

ここからこの記事の本題に入ります。筆者は、コンポーネントとカスタムフックはまったく別個の概念ではないと考えています。

まず、「単にJSXを返すカスタムフック」を考えてみましょう。

```
export function useCheckbox(props: {
  checked: boolean;
  setChecked: (checked: boolean) => void;
}) {
  return (
    <input
      type="checkbox"
      checked={props.checked}
      onChange={(e) => {
        props.setChecked(e.currentTarget.checked);
      }}
    />
  );
}
```

インターフェースに注目すると、「propsを受け取ってJSXを返す」というインターフェースになっています。これはコンポーネントのインターフェースと同じです。

つまり、このようなカスタムフックは、コンポーネントと同等のものと考えられます。

実際、この例の場合は同じ関数の名前だけ変えれば、コンポーネントにもなるしフックにもなります。

```
// コンポーネント
export function Checkbox(props: {
  checked: boolean;
  setChecked: (checked: boolean) => void;
}) {
  return (
    <input
      type="checkbox"
      checked={props.checked}
      onChange={(e) => {
        props.setChecked(e.currentTarget.checked);
      }}
    />
  );
}

// フック
export function useCheckbox(props: {
  checked: boolean;
  setChecked: (checked: boolean) => void;
}) {
  return (
    <input
      type="checkbox"
      checked={props.checked}
      onChange={(e) => {
        props.setChecked(e.currentTarget.checked);
      }}
    />
  );
}
```

フックの中で別のフックを使用している場合などは両者は完全に同等ではありませんが（フックの属するコンポーネントが変わるため）、それを踏まえても次のような単純な変換でコンポーネントとフックを相互変換できます（`hookToComponent`の方はもはや何もしていませんが）。

```
/**
 * フックをコンポーネントに変換
 */
export function hookToComponent<Props>(
  hook: (props: Props) => React.ReactElement | null
): React.FunctionComponent<Props> {
  return (props) => {
    return hook(props);
  };
}

/**
 * コンポーネントをフックに変換
 */
export function componentToHook<Props>(
  Component: React.FunctionComponent<Props>
): (props: Props) => React.ReactElement | null {
  return (props) => <Component {...props} />;
}

// 使用例
const useCheckbox = hookToComponent(Checkbox);
```

### [](https://qiita.com/uhyo/items/cb6983f52ac37e59f37e#%E3%83%95%E3%83%83%E3%82%AF%E3%81%AF%E6%8B%A1%E5%BC%B5%E3%81%A7%E3%81%8D%E3%82%8B)フックは拡張できる

実際は、こんなコンポーネントと同等のフックを作る機会は少ないでしょう。JSXを返すだけでなく、最初の `useCheckbox` のように他の情報を加えて返したほうが実用的です。これがフックの拡張です。

逆に言えば、単にJSXを返すフックというのは、最も単純で未拡張なものと言えます（JSXを返すという前提の中ではですが）。

そして、「単にJSXを返すフック」がコンポーネントと意味的に同等であることを踏まえると、カスタムフックというのはコンポーネントを拡張して、返り値を増やしたものであるという考えが得られます。

プログラミング言語によっては、返り値が複数ある関数というのが、返り値がひとつの関数の拡張として存在していることがあります。筆者の頭にパッと思い浮かぶのはGoやWebAssemblyあたりです。タプルがある言語ではタプルで代替できるし、JavaScriptでも配列をタプルのように利用してこれを実現していますね。

つまり、Reactにおけるコンポーネントは返り値がひとつで、JSXを返すだけに制限された関数です。一方で、カスタムフックにはそのような制限がなく、自由に拡張することができます。

Reactにおけるコンポーネントというのは、Reactのランタイムとアプリケーションコードのコミュニケーションの窓口です。なので、コンポーネントのインターフェースがこのように制限されているのは自然なことです。逆に、Reactと直接インタラクトしない内部実装としてのカスタムフックだから、自由に拡張できるのだと考えられます。

以上のことから、**render hooksは普通のコンポーネントを拡張した概念である**と言えます。コンポーネントがJSXを返すだけしかできなくて少し不便なので、より柔軟で適切な設計の道具としてコンポーネントを拡張したカスタムフックを使うというのがrender hooksの基礎となる考え方です。

### [](https://qiita.com/uhyo/items/cb6983f52ac37e59f37e#%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E3%81%AE%E3%82%B9%E3%83%86%E3%83%BC%E3%83%88%E3%81%AE%E4%B8%80%E9%83%A8%E3%82%92%E5%A4%96%E3%81%AB%E8%A6%8B%E3%81%9B%E3%82%8B)コンポーネントのステートの一部を外に見せる

ひとつの考え方として、コンポーネントをカスタムフックへと拡張することは、コンポーネントの内部情報を一部外に見せるようにしたとも考えられます。`useCheckbox`の例では、UIそのものというコンポーネントが提供できるものに加えて、ステートを`useCheckbox`の内部に保ったまま、`checked`という形でチェックボックスの内部情報を外に見せています。

ただし、実際のReactではステートはコンポーネントに属するものですから、コンポーネントをカスタムフックにした時点で`useState`はそのコンポーネントではなく`useCheckbox`を呼び出した親側に属するように変わります。

とはいえ、考え方としては、返り値の情報を増やすというのはコンポーネントの内部の状態を追加で見えるように拡張したと捉えることができるでしょう。それでいて、ステート等のロジックをカスタムフックの内側に隠したままにすることで、見せたいものだけを外に見せるというrender hooksパターンの恩恵が得られるのです。

このような考え方を知れば、render hooksをもう少し抵抗なく受け入れられるのではないでしょうか。

## [](https://qiita.com/uhyo/items/cb6983f52ac37e59f37e#jsx%E3%82%92%E8%BF%94%E3%81%99%E9%96%A2%E6%95%B0%E3%82%92%E8%BF%94%E3%81%99%E6%8B%A1%E5%BC%B5)JSXを返す関数を返す拡張

`useCheckbox`の例の場合、生のJSXを`checkbox`として返す以外にも、それを返す関数`renderCheckbox`を返すというやり方も考えられます。

```
function useCheckbox() {
  const [checked, setChecked] = useState(false);
  const renderCheckbox = () => (
    <input
      type="checkbox"
      checked={checked}
      onChange={(e) => {
        setChecked(e.currentTarget.checked);
      }}
    />
  );

  return [checked, renderCheckbox];
}
```

この例であれば、正直どちらでも良いです。

副作用を無視すれば、型`T`と型`() => T`は同等の表現力を持った値であり、相互に変換可能です。「JSX式の結果そのもの」と「JSX式の結果を返す関数」は同等ということです。

実際のrender hooksの活用例においては、後者の`renderCheckbox`型の設計がよく見られます。

その理由としては、そのほうがさらなる拡張性があるからだと思われます。`() => T` に対しては引数を受けるように拡張して`(args: U) => T` にすることが自然にできます。

```
// 関数が引数を受けるように拡張した例
function useCheckbox() {
  const [checked, setChecked] = useState(false);
  const renderCheckbox = (className: string) => (
    <input
      className={className}
      type="checkbox"
      checked={checked}
      onChange={(e) => {
        setChecked(e.currentTarget.checked);
      }}
    />
  );

  return [checked, renderCheckbox];
}
```

ありがちな例として `className` を受け取る例を出しましたが、実際には `className` を何でも受け取るのはアンチパターンなことも多いので注意しましょう。

このような引数を受け取るのは、`renderCheckbox`の引数ではなく`useCheckbox`の引数にするという選択肢もあります。しかし、コロケーションの観点から`renderCheckbox`の引数とするほうが有利に感じます。

「`children`を受け取りたい」という要望もこのやり方で達成できます。

```
function useCheckboxWithLabel() {
  const [checked, setChecked] = useState(false);
  const renderCheckbox = (label: React.ReactNode) => (
    <label>
      <input
        className={className}
        type="checkbox"
        checked={checked}
        onChange={(e) => {
          setChecked(e.currentTarget.checked);
        }}
      />
      {label}
    </label>
  );

  return [checked, renderCheckbox];
}
```

## [](https://qiita.com/uhyo/items/cb6983f52ac37e59f37e#%E3%81%BE%E3%81%A8%E3%82%81)まとめ

この記事では、カスタムフックからJSXとその他の値を一緒に返すという**render hooks**のパターンについて、コンポーネントの拡張という見方ができることを解説しました。

Reactの内部機序まで考えると多少違うとはいえ、関数のインターフェース（型と言い換えてもいいでしょう）に着目すれば、カスタムフックがコンポーネントの拡張であることは自然に理解できるのではないでしょうか。

この設計パターンは使いこなせばとても便利です。この記事を参考にrender hooksについて深く理解し、必要なときに使えるように自分の引き出しにしまっておきましょう。