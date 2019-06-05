---
title: CSS Questions
layout: layouts/page.njk
permalink: /questions/css-questions/index.html
---

* What is CSS selector specificity and how does it work?
* What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
* Describe Floats and how they work.
* Describe z-index and how stacking context is formed.
* Describe BFC (Block Formatting Context) and how it works.
* What are the various clearing techniques and which is appropriate for what context?
* How would you approach fixing browser-specific styling issues?
* How do you serve your pages for feature-constrained browsers?
  * What techniques/processes do you use?
* What are the different ways to visually hide content (and make it available only for screen readers)?
* Have you ever used a grid system, and if so, what do you prefer?
* Have you used or implemented media queries or mobile specific layouts/CSS?
* Are you familiar with styling SVG?
* Can you give an example of an `@media` property other than `screen`?
* What are some of the "gotchas" for writing efficient CSS?
* What are the advantages/disadvantages of using CSS preprocessors?
  * Describe what you like and dislike about the CSS preprocessors you have used.
* How would you implement a web design comp that uses non-standard fonts?
* Explain how a browser determines what elements match a CSS selector.
* Describe pseudo-elements and discuss what they are used for.
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
* What is the CSS `display` property and can you give a few examples of its use?
* What's the difference between inline and inline-block?
* What's the difference between the "nth-of-type()" and "nth-child()" selectors?
* What's the difference between a relative, fixed, absolute and statically positioned element?
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
* Have you played around with the new CSS Flexbox or Grid specs?
* Can you explain the difference between coding a web site to be responsive versus using a mobile-first strategy?
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?

---

## What is CSS selector specificity and how does it work?
CSS selector specificity CSSセレクタ詳細度とは、CSS命令が競合したとき、優先度を判定するための仕組みである。要素型セレクタ（h1など）、クラスセレクタ、idセレクタの順で優先順位が高くなる。それらが組み合わされているときは、それぞれの単位ごとに計算し、最もスコアの高いCSS命令を選ぶ。
`!important`命令は詳細度を無視してそのCSSを直接適用する。しかしデバッグを困難にするため`!important`は外部ライブラリを上書きするときを除いて使うべきではないとされる。

https://developer.mozilla.org/ja/docs/Web/CSS/Specificity

## What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?

1. resetting CSSとは、各ブラウザがもっているデフォルトCSSを消去するためのCSSファイルである。resetting CSSの利用すると、ブラウザ間のデフォルトCSSの差異を消去して、どのブラウザで見ても同じスタイリングを提供できる。一方、normalizing CSSは、できる限り元のデフォルトCSSを活かして最小限のリセットを行うことを目的としている。

2. その2つのうち、normalizing CSSを用いることが望ましい。第一に、十分共通のものとして利用できるスタイリングを自分で宣言するのは手間がかかる。第二に、normalize CSSではresetting CSSがよく引き起こすバグを修正している。特にフォームについてはresetting CSSが不適切な動作を引き起こすことが多いと言われている。第三にデバッグツールのCSSインスペクタにおける表示が読みにくくならない。resetting cssを用いてCSSを分析すると巨大な継承チェーンに邪魔をされて状況が確認できないことがよくある。第四に、normalizing cssはドキュメントが充実している。系統的な調査の結果やその実装の詳細について、GitHubの該当リポジトリからチェックすることが可能である。

http://nicolasgallagher.com/about-normalize-css/

## Describe Floats and how they work.
Floatは支配下の要素を横並びにする効果をもつ。より詳しく言うと、通常の要素は上から下に並べられるが、`float:right`なら右並びになる。floatした領域について、テキストやインライン要素が"回り込める"ようになる。しばしば”回り込み”は意図しない動作をもたらすので、`clear`を適宜用いてそれを解除する。

## Describe z-index and how stacking context is formed.
z-indexはz軸上の位置(レンダリング上のレイヤ)を定める値である。そのような仮想的なZ軸上に並べられたレイヤの概念図を重ね合わせコンテキスト stacking contextと呼ぶ。値が小さいほど、その要素はレイヤの奥に表示される。z-indexは親子関係によって基本的には決定される（親よりも子に高い値がつけられる）。兄弟要素に対しては独立している。望むなら、任意の要素におけるz-indexの値を任意に指定することも可能である。
https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context

## Describe BFC (Block Formatting Context) and how it works.
BFC ブロック整形コンテキストはCSSの視覚的なwebページ生成の一部である。その中でブロックボックス（ex. float, 表のセル要素, display, フレックスアイテム）が整理され、それぞれの配置が決定される。ブロックボックスとして定められた要素は下のリンクの一覧がある。`float`ブロックを用いる場合はBFCの概念が重要である。回り込みの作用やマージン相殺の処理はこのBFCによって行われる。

## What are the various clearing techniques and which is appropriate for what context?
クリアリングには、
```css
.cloar {
  clear:both;
  height:10px
}
```
などとする。clearfixのための専用クラスを作るのも良い。他にも、`overflow auto`や`overflow auto`によっても回り込みをclearすることができる。しかしこの方法は画面サイズによっては表示が破綻する可能性があるので気をつけなければならない。

吉田真麻「HTML5/CSS3 モダンコーディング」翔泳社 p26,27

## How would you approach fixing browser-specific styling issues?
CSS normalizeを用いる。また　https://caniuse.com/ などを用いてその機能の対応状況を調べ、プロダクトが対応するべき範囲を取り決め、それから実装を調整する。あるいは、複数のCSSを作り、ブラウザごとに読み込むCSSを変更するように設定する。

https://neal.codes/blog/front-end-interview-css-questions
https://www.quora.com/How-do-you-approach-fixing-browser-specific-styling-issues

## How do you serve your pages for feature-constrained browsers?
graceful-degradationなデザインを目指す。グレースフル デグラデーションとは、機能の足りないブラウザであっても、基本的なコンテンツや機能を引き続き提供できるように調整したデザインのことをいう。一方で、プログレッシブ・エンハンスのアプローチもある。つまり、ベースラインとしては誰もが同じように見えるように作るが、ブラウザが強力であるほどリッチな対応が可能にしていくという仕方である。両者は補完的な関係にある。

https://developer.mozilla.org/ja/docs/Glossary/Graceful_degradation
https://developer.mozilla.org/ja/docs/Glossary/Progressive_enhancement

### What techniques/processes do you use?
例えば次のような機能はgraceful-degradationの対象となる：
 - border-radius
 - multiple-background
 これらの機能はHTML5で使われていて、あればよいがなくてもよい。
どうしても必要なら、Polyfillによって新機能を古いブラウザで擬似的にエミュレートすることもできる。

https://www.adobe.com/jp/devnet/dreamweaver/articles/html5pack_css3_part6.html
https://developer.mozilla.org/ja/docs/Glossary/Polyfill

## What are the different ways to visually hide content (and make it available only for screen readers)?

```css
.hoge{
  display:none;
}

.huga{
  visibility:hidden;
}

.ham{
  visibility: collapse;
}
```
などの方法がある。

https://developer.mozilla.org/ja/docs/Web/CSS/visibility

## Have you ever used a grid system, and if so, what do you prefer?
グリッド システム grid systemとは、二次元グリッドで領域を決定するCSSの機能である。グリッドレイアウトでは、要素ごとの割合や画面比率、アイテムのの並べ方などをより自由度高く設定できる。

私はmaterial-uiから用いるラッピングされたグリッドデザインシステムが好きである。それは画面の長さを１２等分した値で領域を割り取り、他のmaterial-componentと統合的にははたらく。

https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout  
https://material-ui.com/layout/grid/

## Have you used or implemented media queries or mobile specific layouts/CSS?

メディアクエリは、一般的な機器の種類や画面の特性に応じてスタイルを変更するのに利用する方法である。`@media`命令などで指定する。
```scss
@media (max-width: 12450px) { ... }
```  
はif isMaxWidth(12450px) do { ... } に相当する。コード例からもわかる通り、@media には **メディア特性** を渡すことができる。メディア特性の多くは範囲であるが(min-,max-)、その環境が特性値をもつかどうかをチェックするメディア特性もある(color)。

https://developer.mozilla.org/ja/docs/Web/CSS/Media_Queries/Using_media_queries