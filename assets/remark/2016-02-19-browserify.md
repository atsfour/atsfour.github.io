class: center, middle

# Browserify使ってみた

Browserity + TypeScript + Angular.JS

---

### 自己紹介

- @atsfour

- エンジニア歴4ヶ月

- JSでハイブリッドアプリを作りたい。

--

- 発表中も適宜マサカリや罵詈雑言を食らわせて下さい。

---

### Agenda

- Browserity?
- ほどほどにBrowserity
- ガッツリBrowserity

---

### 参考文献



---
### Browserity?

- CommonJSの`export`や`require`をクライアントのJSでも使えるようにするもの
- CommonJSに対応しているライブラリであれば、`<script>`に書かずに自作コードとまとめて1ファイルにできる
- ライブラリ間での名前の衝突を解決出来る
- 依存関係が明確になる

---

### 着地点

- TypeScriptで1クラス1ファイルでコードを書き
- Browserifyで一つにまとめて
- SourceMapを作ってデバッグ時に書くファイルでできるように
- 以上をgulpコマンドで自動化して
- vs-codeの補完機能を使える状態

を目指します。

[作ったリポジトリ](https://github.com/atsfour/BrowserifySample)

---

### ほどほどにBrowserity

- 自作コードだけまとめる
- Angular.JSなどの外部ライブラリはindex.htmlに書いちゃう(bowerとかで管理する)
  - つまり、グローバル変数

---

### TypeScript

雰囲気をつかむための抜粋です。

- main/app.ts

```{javascript}
/// <reference path="../../typings/tsd.d.ts" />

import angular = require('angular');
import {appName, externalModules} from './constants';

angular.module(appName, externalModules);

import './views/MainCtrl' //使うファイルを全部インポート

```

- main/constants.js

```{javascript}
export const appName = 'sample';
export const prefix = 'sample';
export const externalModules = [
  'ngRoute',
  'ui.bootstrap'
];

```

---

- main/directives/Main.js

```{javascript}
/// <reference path="../../typings/tsd.d.ts" />
import angular = require('angular');
import {appName, prefix} from '../../constants';

class MainCtrl {
  public comment: string;
  constructor() {
    this.comment = 'world';
  }
}

class MainDifinition {
  public static ddo(): ng.IDirective {
    return {
      restrict: 'E',
      controller: MainCtrl,
      controllerAs: 'main',
      scope: {},
      templateUrl: 'templates/views/main.html'
    };
  }
}

let app = angular.module(appName);
app.directive(prefix + 'Main', MainDifinition.ddo);
```

---

### gulpfile.js

使うライブラリ

```{javascript}
var gulp = require('gulp');
var babelify = require('babelify');
var buffer = require('vinyl-buffer');
var browserify = require('browserify');
var sourcemaps = require('gulp-sourcemaps');
var source = require('vinyl-source-stream');
var ts = require('gulp-typescript');
var tsify = require('tsify');
var ngAnnotate = require('gulp-ng-annotate');
```
---
### gulpfile.js

処理

```{javascript}
gulp.task('browserify', function () {
  return browserify({
      entries: './ts/app.ts', // エントリーは最初のファイルだけ。
      debug: true
    })
    .plugin(tsify) // TypeScriptのtargetはes6にしておく
    .transform(babelify, { // babelでes5に変換
      extensions: [".js", ".ts"], presets: ['es2015']
      })
    .bundle() //ここまでbrowserify
    .pipe(source('bundle.js'))
    .pipe(buffer())
    .pipe(ngAnnotate())
    .pipe(sourcemaps.init({ loadMaps: true }))
    //minify処理があればここに書く
    .pipe(sourcemaps.write('./'))
    .pipe(gulp.dest('www/js'))
});
```

---

### ガッツリBrowserity

- 外部ライブラリもrequire
- なるべくnpmだけで管理する。
  - ライブラリ側がCommonJSに対応してないときは通常ではダメ。Bower + Browserity + Debowerify。
- browserifyの処理時に外部ライブラリも全部読みにいくので、gulpの処理に時間が掛かる

---

*変更点のみ*

- main/app.ts

```{javascript}

import * as angular from 'angular'; // node_modulesから呼んできてくれる
import {appName, externalModules} from './constants';

// 以下同様

```

---

- gulpfile.js

```{javascript}
browserify({
    entries: ['./ts/app.ts,
      './node_modules/angular-route/index.js',
      './node_modules/angular-ui-bootstrap/index.js']
      // importしないモジュールをエントリーに加える。angular本体はimportしているので不要。
    debug: true
  })
```

---

### まだわからないこと

- watchify。ファイルを監視して差分Browserityしてくれるらしい(時間がかからなくなる)。

---
