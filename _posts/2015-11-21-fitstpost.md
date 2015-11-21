---
layout: post
title: テスト投稿
---

Jekyllを使った初めての投稿です。

##### シンタックスハイライト

```scala
sealed trait Tree[+A]
case class Leaf[A](value: A) extends Tree[A]
case class Branch[A](left: Tree[A], right: Tree[A]) extends Tree[A]

val tree = Branch(Leaf(1), Leaf(2))
```
