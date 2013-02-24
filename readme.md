■ Fireworksを使ってオリジナルのWebアイコンを作成、実装しよう！

１．拡張機能のインストール
Fireworksの、SVG出力用のエクステンション（拡張機能）をインストールします。
Aaron Beall - Fireworks Guru - Export
http://fireworks.abeall.com/extensions/commands/Export/
より、Export.mxp をダウンロードします。
ちなみにこのエクステンション、CSSスプライトにも対応していて、htmlとcssを生成してくれたりします。

２．ICONを準備
パスツールをつかって、ICONにするパスを作っていく
※キャンバスが一文字の大きさになるので、キャンバス内になるべく大きくつくる

３．パスをSVGで書き出し
「コマンド」-「書き出し」-「SVG」で書き出していく

４．IcoMoonを使って、フォントファイルに変換！
IcoMoon App - Icon Font Generator http://icomoon.io/app/
「Import Icons」で、作成したSVGをアップロードし、
選択した状態で、
「FONT→」を押す。
次に、各iconに英数字を割り当てます。
iconを選択した状態で、割り当てたいキーを押します。
最後に「Download」をおした、
フォントファイルとサンプルのhtml,cssをダウンロードします。

５．実装

参考サイト
- 安全な@font-faceの書き方(抄訳) - Weblog - hail2u.net
	http://hail2u.net/blog/webdesign/bulletproof-at-font-face-syntax.html
- [CSS]IEを含めた主要ブラウザと各スマートフォンに対応した@font-faceの指定方法 | コリス
	http://coliss.com/articles/build-websites/operation/css/the-new-bulletproof-font-face-syntax-by-fontspring.html


CSS
	@font-face {
		font-family: 'testfont';
			src: url('testfont.eot?') format('eot'),
			url('testfont.woff') format('woff'),
			url('testfont.ttf')  format('truetype'),
			url('testfont.svg#testfont') format('svg');
	}
	span.icon {
		font-family: testfont;
	}

html
	<p>ここ <span class="icon">1</span> アイコン</p>

って、感じ。

ただ、この場合、アイコンを表示するためにテキストを入れるので、HTMLの構造的に問題があります。
記号に割り当てるのも一つの方法ですが、IcoMoonからダウンロードした時についてくるサンプルのように、
疑似クラス :before を使用することで、この問題を解決できます。

６．疑似クラス :before

HTMLの構造的に問題のないように、:before 疑似要素を使用した例です。

CSS
	@font-face {
		font-family: 'testfont';
		src: url(testfont.eot);
		src: local('testfont Regular'), local('testfont'),
			url(testfont.ttf) format("truetype");
	}
	.icon-01:before {
		font-family: 'testfont';
		speak: none;
		font-style: normal;
		font-weight: normal;
		line-height: 1;
		-webkit-font-smoothing: antialiased;
	}
	.icon-01:before {
		content: "\31";
	}

.icon-01:before　のところを、　[class*="icon-"]:before と書くことで、複数フォントを使用するとき
CSSを無駄に大きくしないことができますが、この場合、すべてのクラスを無駄にサーチします。（＝速度気にする人は注意）

html
	<p>ここ <span aria-hidden="true" class="icon-01"></span> :before疑似要素を使用</p>


