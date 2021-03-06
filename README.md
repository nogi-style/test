# [トップページコーディングをしてわかった！コードレビュー]

![html_cording_image](https://github.com/nogi-style/test/blob/master/images/html_cording_image.png)

---
## 目次
---

* [初めに](#初めに)
* [HTMLのコーディング](#HTMLのコーディング)
  + [head](#head)
    - [meta description](#meta-description)
    - [link icon](#link-icon)
  + [body](#body)
    - [drawer.js](#drawerjs)
    - [レスポンシブ](#レスポンシブ)
    - [最新ニュース](#最新ニュース)
* [CSSのコーディング](#CSSのコーディング)
    - [変数宣言](#変数宣言)
    - [ダークモード](#ダークモード)
    - [font-sizeとrem指定](#font-sizeとrem指定)
* [終わりに](#終わりに)

---
## 初めに
---

今回初めてコードレビュー用発表を行うにあたり、簡易的なソースコードへのコメント、GitHubのREADMEによる資料作成をしました。GitHubのREADMEはマークダウン形式で記述できるので、よく使うマークダウン記法も併せて学びました。

昨今、Webページはモバイルファーストインデックス(スマホサイトが検索順位の評価対象であること。略MFI)で記述するのが当たり前になるみたいなので、モバイルファーストのレスポンシブで記述するよう心がけました。

---
## HTMLのコーディング
---
### head

#### meta description
直接SEO(検索エンジン最適化のこと)に影響はないみたいですが、ユーザーが検索した際、ページタイトルとともに検索結果に表示される要素なので設定しました。(適切な内容を設定することで、ユーザーのクリック率を上げる効果が期待できるため)

なお、現在は、スマートフォンでは70文字前後、PCでは90～120文字前後、省略されず表示するみたいです。

今回は省略されますが、150文字程、設定しました。
```HTML :今回設定したdescription設定
<meta name="description" content=">日本臨床心理学会は、「臨床心理学やそれに基づく現場の活動について、様々な立場や角度からその行為を考えている学会」です。 本学会は、臨床心理学を専門とする人だけではなく、「患者」「クライアント」「障害者」と呼ばれる人やその家族や関係者、 また臨床心理学に関心を持つ人など、様々な立場の人々が参加しています。">
```

#### link icon
ブラウザのタブなどに表示されるfaviconですが、favicon generatorというサイトで複数サイズのfavicon画像を用意するということも考えたのですが、シンプルなロゴ画像だったのと、対応させるブラウザが今回、ChromeとEdgeなこともあり、ロゴ画像を参考にイラストレーターでsvg(スケーラブル・ベクター・グラフィックス)を作成し、設定しました。
```HTML :svg favicon指定
<link rel="icon" href="./images/favicon.svg" type="image/svg+xml">
```
svg形式のメリットとして、ベクター形式なので、サイズが変わっても画質が劣化しないことと、ダークモードへの対応が簡単なことです。

今回試したダークモードへの対応は、svgファイルに
```CSS :ダークモード対応
@media (prefers-color-scheme: dark) {
  path {
    fill: #01a1cb; <!-- 取り敢えずライトモードの反転色を指定 -->
  }
}
```
を記述し、ダークモード時に、反転色でfaviconを表示させました。

また、作成したsvgファイルサイズ削減のため、[SVGOMG](https://jakearchibald.github.io/svgomg/)というサイトで最適化しました。

---
### body

#### drawer.js
今回、スマートフォン・タブレットなどのモバイルデバイス(ブラウザ横幅960px未満)からの閲覧時に、ドロワーメニューを表示するためにjQueryプラグインのdrawer.jsを用いました。各所、bodyタグ、headerタグ内の要素に特定のクラスを付与などしてハンバーガーメニューを実装しました。(詳しい使い方はまだわかりません...)

#### レスポンシブ
SP表示でのみ適応されるsp_only、PCのみで適応されるpc_onlyというクラスを与え、表示、非表示をCSSのmediaクエリを設定し、レスポンシブ対応しました。

ブレイクポイントは、[【2020年度決定版】レスポンシブデザインのブレークポイントはこれで決まり❗️](https://www.webdlab.com/labs/responsive-web-design-5/)を参考に960pxを設定しました。

#### 最新ニュース
解像度の低いスマホでは、表示横幅が狭くなってしまうので、一列表示に設定しました。(もう少し細かくブレイクポイントで設定したほうがよかったかもと、作成後思いました。)

---
## CSSのコーディング
---

#### 変数宣言
色を管理しやすいように変数にしました。

#### ダークモード
ユーザーが[ダークモード設定](https://www.atmarkit.co.jp/ait/articles/1912/19/news028.html)されている際に、Webページの色を画像以外反転させることで、夜間でも眼に優しい？ようにしました。
```CSS :ダークモード時の記述
@media (prefers-color-scheme: dark){
  :root {
    /* 簡易的設定。ダークモード時反転色へ */
    filter: invert(100%);

    /* 細かくダークモード時の色を変更する場合、以下の色を一つずつ変更する */
    /* --main-text: #444;
    --sub-text: #333;
    --h3-text: #a5a5a5;
    --main-anchor-text: #fff;
    --logo-span-text: #666;
    --footer-text: #fff;

    --main-bg: #f6f6f6;
    --main-btn-bg: #F2A36E;
    --footer-bg: #767171;
    --hover-bg: skyblue;

    --main-border: #767171;
    --sub-border: #000; */
  }

  /* 画像も色反転してしまっているので戻す場合の設定 */
  img {
    filter: invert(100%);
  }
}
```

[filter](https://developer.mozilla.org/ja/docs/Web/CSS/filter)プロパティの[invert](https://developer.mozilla.org/ja/docs/Web/CSS/filter-function/invert)関数に値100%または1を渡すことで反転させることができました。

また、数値を変えることで完全な反転ではなく細かく設定も可能でした。

#### font-sizeとrem指定

ほとんどの数値指定をremで指定することで、htmlタグのfont-sizeを変更するだけで、文字や余白など簡単に変更できるよう、後の修正がしやすくなるようにしました。

モバイルでは文字が小さくなりすぎないように、htmlタグのfont-sizeは62.5%(10px)の固定値を指定。

PC表示では、10pxから20pxまでfont-sizeを可変で設定し、解像度の高い4Kディスプレイなどで表示した際は、文字や余白を大きくし、見やすくなるように設定しました。

```CSS :レスポンシブ設定
@media screen and (min-width: 960px) {

  html {
    /* 10pxから20pxまで可変設定 PC表示のとき(960px以上)、ブラウザ幅によって可変の設定 値は960pxのときhtmlタグのfont-sizeが10px、2560pxのときfont-sizeが20pxになるように設定 */
    /* 算出ツール https://websemantics.uk/tools/responsive-font-calculator/ */
    font-size: calc(0.625rem + ((1vw - 9.6px) * 0.625));
  }

}
```

---
## 終わりに
ブランクのあったコーディングですが、記述していて分からないところは、その都度調べながら行うことで、少しは書けたのではないかと思います。

しかし、まだまだ分からないことが多いので、他の方のコードレビューを見て、「あーこうしたほうがよかったな」と思うポイントは、調べながら徐々に習得していき、覚えておきたいなと感じました。

---

> サイトテスト

>> [モバイルフレンドリーテスト](https://search.google.com/test/mobile-friendly?utm_source=support.google.com%2Fwebmasters%2F&utm_medium=referral&utm_campaign=6155685&id=o7n9kd8_Ao35jJ6e82V4mA)

> 参考文献

>> [今すぐ使えるかんたんPLUS+ HTML5&CSS3完全大事典](https://www.amazon.co.jp/dp/4774198110)

> 参考サイト

>> [【Google】2020年9月にモバイルファーストインデックスを全サイトに適用](https://www.itra.co.jp/webmedia/what-is-mfi.html)

>> [viewportのshrink-to-fitは誰が為に在る](https://lambda-tonight.hatenadiary.jp/entry/2017/12/26/025152)

>> [Safariの新機能 - ビューポートの変更](https://developer.apple.com/library/archive/releasenotes/General/WhatsNewInSafari/Articles/Safari_9_0.html#//apple_ref/doc/uid/TP40014305-CH9-SW5)

>> [Can I use svg favicon ?](https://caniuse.com/?search=svg%20favicon)

>> [Chromeを「ダークモード」に切り替えて省電力や眼の負担軽減（Windows／Mac編）](https://www.atmarkit.co.jp/ait/articles/1912/19/news028.html)

>> [Webサイトをダークモードに対応させよう](https://www.webcreatorbox.com/tech/dark-mode)

>> [ドロワーメニューを実装するためのプラグイン【drawer.js】](https://takano-weblog.com/2020/01/11/drawer-js/)

>> [【2020年度決定版】レスポンシブデザインのブレークポイントはこれで決まり❗️](https://www.webdlab.com/labs/responsive-web-design-5/)

>> [invert()](https://developer.mozilla.org/ja/docs/Web/CSS/filter-function/invert)

>> [CSS：ホバー時のアンダーラインアニメーションの実装サンプルとmixinを用いた実装方法](https://www.nxworld.net/tips/css-hover-underline-animation-examples-and-sass-mixin.html)

>> [Fluid-responsive font-size calculator](https://websemantics.uk/tools/responsive-font-calculator/)

>> [Markdown記法 チートシート](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)
