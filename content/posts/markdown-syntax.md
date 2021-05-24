---
title: "ページ作りました"
date: 2020-06-10T15:59:50+09:00
draft: false
description: このブログの最初のPostです。
tags: ["Hugo", "Zzo"]
---

Go の静的サイトジェネータ（Static Site Generator）である[Hugo](https://gohugo.io/)を使っています。

Hugo では色々な theme を選べるのですが、シンプルそうな[Zzo](https://zzo-docs.vercel.app/zzo/)を利用してみました。
theme を変えることは（そう簡単ではないですが、記事が Markdown である限り）難しくなさそうなので、とりあえず行けるところまでこれで行ってしまおうと思います。

## チートシート（随時更新）

### 新しいページを作る

```bash
hugo new ディレクトリ名/記事名.md
```

### ローカルで実行

```
hugo server -D
```

### ショートコード - Youtube

最初のカッコを半角に変えてください

```
｛{< youtube t5gbZCkpYmI >}}
```

{{< youtube t5gbZCkpYmI >}}

### ショートコード - Twitter

最初のカッコを半角に変えてください

```
｛{< tweet 877500564405444608 >}}
```

{{< tweet 877500564405444608 >}}

## 参考サイト

- [Hugo を触ってみたので、一通りの弄り方をまとめる](https://qiita.com/yakimeron/items/42d537374abde5517267)
- Zzo を利用している日本語サイト
  - [Inomaso Blog](https://www.inomaso.com/)
  - [Tweet Blog](https://encr.jp/blog/)
  - [Freaky Engineer Notes](https://fe-notes.work/)
