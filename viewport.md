## what
- ユーザーが自裁にコンテンツを見ている画面上の領域のこと
	- PCの場合はブラウザのウィンドウサイズ
	- スマートフォンの場合はディスプレイサイズに合わせた仮想領域
## why
- デバイスごとに画面サイズが異なるため、ビューポート設定によってwebサイトのレイアウトを自動的に変化させるため
## how
```html
<!--
	width: ビューポートの（最小の）大きさ
		device-width: CSSピクセル数による端末の画面の物理的なサイズ
	initial-scale: ページを読み込んだ時の表示倍率
-->
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```