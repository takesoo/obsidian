## what
- [[Document Object Model|DOM]]をJavaScriptオブジェクトとしてシミュレートした軽量なコピー
## why
- DOMをJavaScriptで直接更新するのはオーバーヘッドが大きいので、仮想DOMを更新し、差分をDOMに反映させるアプローチ
## how
1. データ更新で新しい仮想DOMを作成する
2. 古い仮想DOMと新しい仮想DOMを比較して差分を特定する
3. 差分をDOMに反映
