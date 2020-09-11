# SaSS
## って何？
- CSSの機能を拡張した言語
- CSSを効率的かつ簡潔に書けるようになる
- CSSの中で変数を使ったり計算ができるようになる

## Sassを扱えるファイル
- 通常の.cssファイルには書けない
- SassファイルにCSSを記述することはできる
- .sassと.scssの2種類がある

### .sass拡張子
- こちらが先に作られた拡張子らしい。バージョン1。
- 波括弧やセミコロンなしで記述できる。rubyライクな記法？
- markdownのソースコード記述に対応していない
- cssデザイナーに不評だったようで、ほとんど用いられていない様子

```sass
.body p
  color: #333
  font-size: 10px
  font-weight: normal
  strong
    color: red
    font-weight: bold
```

### .scss拡張子
- .sassより後に作られた。バージョン2？
- こちらは一転してcssライクな記法。
- markdownのソースコード記述に対応してるので、こちらの方がメジャーかも

```scss
.body p {
    color: #333;
    font-size: 10px;
    font-weight: normal;
    strong {
        color: red;
        font-weight: bold;
    }
}
```

## 記述を簡略化できる
- CSSとの大きな違いは、**親子関係にあるセレクタ**を**入れ子にして書ける**こと

### 例）こういうhtmlだとする
```html
<div class="block">
    <h1 class="title">hoge</h1>
    <p class="text">fuga<span>span</span></p>
</div> 
```

### 例)CSSの場合はこう指定せざるを得ない
```css
.block {
    background-color: #000;
}

.block .title {
    color: #FFF;
    font-size: 50px;
    text-align: center;
}

.block .text {
    font-size: 10px;
    color: #FFF;
}

.block .text span {
    color: red;
    font-size: 20px;
}
```

### 例)Scssならこれだけで済む
```scss
.block {
    background-color: #000;
    .title {
        color: #FFF;
        font-size: 50px;
        text-align: center;
    }
    .text {
        font-size: 10px;
        color: #FFF;
        span {
            color: red;
            font-size: 20px;
        }
    }
}
```

## Sassで変数や条件分岐
- $(変数名): (変数の値) で変数を定義できる
- $(変数名) で変数を呼び出せる
- #{} で波括弧内を計算させる
- #{$(変数名) - 1} のように変数を用いた計算も可能
- 文字列結合もできる
- if-else, for, for-each, whileにも対応
- 配列も使える
- 関数も定義できる
- 特定のスタイルを別のスタイルに継承させることもできる

```scss
/* 変数定義、使用、変数計算、文字列結合 */
$baseFontSize: 14px;
$section-color: rgb(30,30,30);
$imgDir: "../img/";

section {
    font-size: #{$baseFontSize - 2px}; /* font-size: 12px として扱われる */
    background-color: $section-color;
    div {
        background: url("#{imgDir}bg.png") /* background: url("../img/bg.png") */
        /* url($imgDir + "bg.png") でも同じことができる */
    }
}

/* if文 */
$debugMode: true;
@if $debugMode == true{
    color: red;
} @else {
    color: green
}

/* for文 */
@for $i from 10 through 14 {
    .fs#{$i} {font-size: 10px;} /* fs12,fs13,fs14クラスのfont-sizeが10pxになる */
}

/* while文 */
$j: 15;
@while $j < 18 {
    .fs#{$j} {font-size: 10px;}
    $j: $j + 1;
}

/* 配列, for-each文 */
$animals: cat, dog, tiger;

@each $animal in $animals {
    .#{$animal}-icon { background: url("#{$animal}.png"); }
}

/* 関数定義 */
@function getColumnWidth($width, $count) {
    $padding: 10px;
    $columnWidth: floor(($width - ($padding * ($cound - 1))) / $count);
    @return $columnWidth;
}

.grid {
    float: left;
    width: getColumnWidth(940px, 10); /* = 85px */
}

/* 継承 */
.msg {
    font-size: 12px;
    font-weight: bold;
    padding: 2px 4px;
    color: white;
}

.errorMsg {
    @extend .msg;
    background: red;
}

.warningMsg {
    @extend .msg;
    background: orange;
}

```

## 複数のsass(scss)ファイルを1つのファイルにまとめる
- vbhtmlでフレーム部分と本文のファイルを分けるが如く、ファイルを分割することができる

```scss
@import "reset";  /* _reset.scssを読み込む */
@import "header"; /* _header.scssを読み込む */
```

## まとまったスタイルを定義する
- @mixinというアノテーションを用いることで、同じようなスタイルの組み合わせを使い回せる
- @includeというアノテーションでそれを呼び出せる

### mixinの例
```scss
@mixin clearfix() {
    /* &は疑似要素であるafterが適用されているセレクタを示す */
    &:after {
        content: '';
        display: block;
        clear: both;
    }
}

.menu {
    @include clearfix();

    .menu__list {
        float: left;
    }
}
```

### 上のmixinは実際にはこう解釈される
```scss
.menu {
    &:after {
        content: '';
        display: block;
        clear: both;
    }

    .menu__list {
        float: left;
    }
}
```