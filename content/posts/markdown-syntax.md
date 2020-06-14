---
title: "ページ作りました"
date: 2020-06-10T15:59:50+09:00
draft: false
description: このブログの最初のPostです。
tags: ["Hugo","Zzo"]
---

Goの静的サイトジェネータ（Static Site Generator）である[Hugo](https://gohugo.io/)を使っています。

Hugoでは色々なthemeを選べるのですが、シンプルそうな[Zzo](https://zzodocs.netlify.app/)を利用してみました。
themeを変えることは（そう簡単ではないですが、記事がMarkdownである限り）難しくなさそうなので、とりあえず行けるところまでこれで行ってしまおうと思います。

## チートシート（随時更新）
### 新しいページを作る
```bash
hugo new ディレクトリ名/記事名.md
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
- [Hugoを触ってみたので、一通りの弄り方をまとめる](https://qiita.com/yakimeron/items/42d537374abde5517267)
- Zzoを利用している日本語サイト
  - [Inomaso Blog](https://www.inomaso.com/)
  - [Tweet Blog](https://encr.jp/blog/)
  - [Freaky Engineer Notes](https://fe-notes.work/)