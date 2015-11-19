# Web制作ガイドライン
以下の文書は、ECC EXPO 2016に関わるWeb制作において、従うべき規約について記したものです。

## 動作保証環境
全ての制作物は以下の環境で動作することを保証する。

- OS
  - Windows Vista+
  - Mac OS X 10.8+
  - Android 4.1+
  - iOS 8+
- Browser
  - Windows
    - Internet Explorer 11+
    - Mozilla Firefox
    - Google Chrome
    - Safari
  - Mac
    - Mozilla Firefox
    - Google Chrome
    - Safari
  - Android
    - Google Chrome
  - iPhone, iPad
    - Google Chrome
    - Safari

バージョンが記載されてない項目に関しては、制作時の最新バージョンを対象とする。

## ディレクトリ構成
フロントエンドの全てのプロジェクトはnpmを用いて管理する。  
srcディレクトリに元データを格納し、distディレクトリに変換後のデータを書き出す。

- [ src ]
  - [ images ]
    - sample01.png
    - sample02.png
    - …
  - [ scripts ]
    - index.js
    - module.js
    - …
  - [ styles ]
    - index.scss
    - reset.css
    - module.scss
    - …
  - index.html
  - favicon.ico
- [ dist ]
  - [ assets ]
    - [ img ]
      - sample01.png
      - sample02.png
      - …
    - style.css
    - bundle.js
  - index.html
  - favicon.ico
- [ node_modules ]
- gulpfile.js
- package.json
- …

## 共通ルール
- インデントは半角スペース2つで行う。
  ```html
  <ul>
    <li>foo</li>
    <li>bar</li>
  </ul>
  ```

- 文末にスペースを残さない。
  ```html
  NG:
  <p>lorem ipsum dolor...</p>_

  OK:
  <p>lorem ipsum dolor...</p>
  ```

- エンコードはUTF-8で行う。
  ```html
  <meta charset="utf-8">
  ```

  ```css
  @charset "utf-8";
  ```

- タグ及び属性は小文字で記述する。
  ```html
  NG:
  <a HREF="/">Home</a>

  OK:
  <a href="/">Home</a>
  ```

- パスの記述はルートパスを利用する。
  ```html
  NG:
  <img src="../assets/img/sample01.png" width="128" height="128" alt="Sample01">

  OK:
  <img src="/assets/img/sample01.png" width="128" height="128" alt="Sample01">
  ```

## HTMLコーディングガイドライン
- 最初にHTML5として宣言する。
  ```html
  NG:
  <html>
    ...
  </html>

  OK:
  <!DOCTYPE html>
  <html>
    ...
  </html>
  ```

- lang属性に適切な値を指定する。
  ```html
  NG:
  <html>

  OK:
  <html lang="ja">
  ```

- headの一番最初にエンコードを指定する。
  ```html
  NG:
  <head>
    <title>Document</title>
    <meta charset="utf-8">
  </head>

  OK:
  <head>
    <meta charset="utf-8">
    <title>Document</title>
  </head>
  ```

- faviconはルートディレクトリに配置し、linkを記述しない。
  ```html
  NG:
  <link href="/favicon.ico" rel="icon" type="image/vnd.microsoft.icon">
  ```

- 省略可能な閉じタグを省略しない。
  ```html
  NG:
  <ul>
    <li>foo  
    <li>bar
  </ul>

  OK:
  <ul>
    <li>foo</li>
    <li>bar</li>
  </ul>
  ```

- 空要素のスラッシュを省略する。
  ```html
  NG:
  Hello !<br />

  OK:
  Hello !<br>
  ```

- CSSは外部ファイルにのみ記述する
  ```html
  NG:
  <head>
    ...
    <style>
      .btn {
        display: inline-block;
        background-color: green;
      }
    </style>
  </head>

  <body>
    <a class="btn" style="color: #fff;" href="/path/to/link">title of link page</a>
  </body>

  OK:
  <head>
    ...
    <link rel="stylesheet" href="/style.css">
  </head>

  <body>
    <a class="btn" href="/path/to/link">title of link</a>
  </body>
  ```

- script要素にtypeを記述しない
  ```html
  NG:
  <script type="text/javascript" src="/script.js"></script>

  OK:
  <script src="/script.js"></script>
  ```

- JavaScriptは外部ファイルにのみ記述し、JavaScriptの振る舞いを要素の属性に含めない
  ```html
  NG:
  <button onClick="handleClick()">click me !</button>

  <script>
    function handleClick() {
      alert('button was clicked');
    }
  </script>

  OK:
  <button id="btn">click me !</button>

  <script src="/scripts.js"></script>
  ```

## CSSコーディングガイドライン
- CSSプリプロセッサーとしてSassをSCSS記法で使用し、新規に作成するファイルは全てSCSS記法で記述する。

- エントリーポイントとなるファイルの最初に文字コードを宣言する。
  ```css
  @charset "utf-8";
  ```

- 必要に応じてスタイルのリセット・正規化を行う。
  ```css
  @import "sanitize";
  ```

- 適切な単位でファイルを分割する。
  ```css
  _header.scss

  .header {
    @extend .clearfix;
    display: block;

    &-inner {}
  }
  ```

  ```css
  _layouts.scss

  .container {
    @extend .clearfix;
    width: 1200px;
  }

  .col-6 {
    float: left;
    width: 50%;
  }
  ```

- ファイルは可能な限り単一ファイルとして出力する。
  ```css
  @charset "utf-8";

  @import "variables";
  @import "mixins";
  @import "normalize";
  @import "grid";
  @import "buttons";
  ```

  ```html
  <link rel="stylesheet" href="/style.css">
  ```

- タイプセレクタを記述しない。
  ```css
  NG:
  ul.list {}
  div.error{}

  OK:
  .list {}
  .error {}
  ```

- セレクタを不要にネストさせない。
  ```css
  NG:
  .header .nav {}
  .header .nav li {}
  .header .nav li a {}

  OK:
  .nav {}
  .nav li {}
  .nav a {}
  ```

- ショートハンドプロパティをやみくもに使わない。
  ```css
  NG:
  .example {
    margin: 0 auto;
  }

  OK:
  .example {
    margin-right: auto;
    margin-left: auto;
  }
  ```

- 値が0の場合は単位を省略する。
  ```css
  NG:
  .example {
    border-left: 0px;
  }

  OK:
  .example {
    border-left: 0;
  }
  ```

- 値が0以下の小数の場合は接頭を省略する。
  ```css
  NG:
  .example {
    opacity: 0.8;
  }

  OK:
  .example {
    opacity: .8;
  }
  ```

- url()のクォートを省略しない。
  ```css
  NG:
  .example {
    background-image: url(/img/sample.png);
  }

  OK:
  .example {
    background-image: url("/img/sample.png");
  }
  ```

- カラーコードは小文字で記述する。
  ```css
  NG:
  .example {
    color: #FEFEFE;
  }

  OK:
  .example {
    color: #fefefe;
  }
  ```

- 省略できるカラーコードは3文字で表記する。
  ```css
  NG:
  .example {
    color: #333333;
  }

  OK:
  .example {
    color: #333;
  }
  ```

- 全てのプロパティの終端にセミコロンをつける。
  ```css
  NG:
  .example {
    display: inline-block;
    color: #333;
    background-color: #eee
  }

  OK:
  .example {
    display: inline-block;
    color: #333;
    background-color: #eee;
  }
  ```

- 複数のセレクタを指定する際は改行して表記する。
  ```css
  NG:
  .example1, .example2, .example3 {
    border: 1px solid #eee;
  }

  OK:
  .example1,
  .example2,
  .example3 {
    border: 1px solid #eee;
  }
  ```

- 各宣言は一行ごとに記述する。
  ```css
  NG:
  .example { display: inline-block; color: #333; background-color: #eee; }

  OK:
  .example {
    display: inline-block;
    color: #333;
    background-color: #eee;
  }
  ```

- 宣言がひとつの場合は全てを一行に記述する。
  ```css
  NG:
  .example {
    color: red;
  }

  OK:
  .example { color: red; }
  ```

- !importantは積極的に使用しない。  
  適切な場面であれば可。禁止することはしない。

- マジックナンバーを極力使わない。
  ```css
  NG:
  .example {
    position: relative;
    top: 41px;
  }

  OK:
  .example {
    position: relative;
    top: 100%;
  }
  ```

- Media Queryの記述場所。  
  可能な限りそれが関係する要素の近くに記述する。  
  ファイルの最後にまとめたり、別ファイルのまとめたりしない。
