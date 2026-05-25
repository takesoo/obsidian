## what
- アクセシビリティと正しい相互作用（キーボード操作、フォーカス管理、ARIA）を担保した低レベルのUIプリミティブ（Dialog, Popover, Dropdownなど）を提供するコンポーネントライブラリ
- ヘッドレス（見た目のスタイルを持たない）
- コンポーネントそれぞれが個別のnpmパッケージ（プリミティブと呼ばれる）として提供されている。使用するプリミティブだけをインストールして、スタイルを当てて使用する。
## how
1. パッケージインストール
	```
   npm install @radix-ui/react-dialog
	```
2. コードへ組み込み
	```ts
	import * as Dialog from '@radix-ui/react-dialog';
	import './styles.css'; // 自分のCSS

	const ModalExample = () => (
	  <Dialog.Root>
	    {/* モーダルを開くボタン */}
	    <Dialog.Trigger className="Button violet">
	      プロファイル編集
	    </Dialog.Trigger>
	
	    <Dialog.Portal>
	      {/* 背景の黒いマスク */}
	      <Dialog.Overlay className="DialogOverlay" />
	      
	      {/* モーダルのコンテンツ本体 */}
	      <Dialog.Content className="DialogContent">
	        <Dialog.Title className="DialogTitle">編集</Dialog.Title>
	        <Dialog.Description className="DialogDescription">
	          プロフィールを変更してください。変更後は保存ボタンを押してください。
	        </Dialog.Description>
	        
	        {/* ここにフォームなどを自由に配置 */}
	        <input type="text" placeholder="名前" />
	
	        {/* モーダルを閉じるボタン */}
	        <Dialog.Close asChild>
	          <button className="Button green">保存</button>
	        </Dialog.Close>
	      </Dialog.Content>
	    </Dialog.Portal>
	  </Dialog.Root>
	);
	
	export default ModalExample;
	```