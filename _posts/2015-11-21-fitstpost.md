---
layout: default
comments: true
tags: jekyll github
title: github.io + Jekyll + bootstrap を導入するまで
---

### Jekyll + Jekyll-Bootstrapで投稿するまで。

以下のサイトを参照しながら。

気をつけるべき点は、2015/11/23時点ではJekyll-BootstrapがJekyll3.0に対応していないこと。

インストール時のコマンドで

`gem install jekyll -v '2.4.0'`

とバージョンを指定して解決。

##### 参考サイト
* [jekyll bootstrapをgithubページにインストールしてみた](http://mattn.github.io/2012/03/22/installed-jekyll-bootstrap/)

##### 本家
* [Jekyll](http://jekyllrb-ja.github.io)
* [Jekyll Bootstrap](http://jekyllbootstrap.com)

### シンタックスハイライト

ブロックコードには気に入ったテーマカラーを使いたい。以下のサイトはリンクが切れていたが、
この記事の時点ではgoogleのキャッシュで閲覧できた。必要部分を引用しておく。

***

{% raw %}
GitHubにはpygmentsは既に入っているみたいなので、テーマのhtmlファイルの中でcssを読み込む記述さえ入れておけば良い。

cssを読み込む

テーマ’twitter’ならば、_includes/themes/twitterのdefault.htmlの中に
<link href="{{ ASSET_PATH }}/css/syntax.css?body=1" rel="stylesheet" type="text/css" media="all">を追加しておく。そして当然css実物も必要ですので、
assets/themes/twitter/cssの中にsyntax.cssを追加しておく。

記事中に貼り付ける際には、{% highlight ruby linenos%}と{% endhighlight %}で囲んでコードを記述すると、以下のように色分けされて表示されます。
{% endraw %}

 [Jekyll-Bootstrapでpigmentsを使ってシンタックスハイライト表示するには](http://masudakoji.github.io/2014/10/05/turn-on-syntax-highlighting-in-Jekyll-Bootstrap-via-pigments/) (**リンク切れ**)

***

これを参考に、monokaiを導入。が、ここでtwitterテーマと背景色の設定がかぶってうまくいかない。

そこで、bootstrap-3テーマの一部を編集した。

/assets/themes/bootstrap-3/css/style.css に以下を追記。

```css
code{background-color: #aee}
pre{background-color: #49483e}
```

すると、以下のような感じにできる。

* インライン: `val a = 3`

* ブロック: scala

```scala
sealed trait Tree[+A]

case class Leaf[A](value: A) extends Tree[A]
case class Branch[A](left: Tree[A], right: Tree[A]) extends Tree[A]

val tree = Branch(Leaf(1), Leaf(2))
```

なんか変な感じもするが、まぁ納得の出来になった。

### コメント欄設置

* [このブログにDIDQUSのコメント欄を設置した / JekyllでDISCUSを設置する方法](http://www.xmisao.com/2014/07/22/setuped-disqus-to-this-blog.html)

設置の有無を選択できるタイプのコードを `/_includes/disqus.html` など適当なファイルを_includeに追加する。
さらに `_layouts/default.html` に以下を追記。
