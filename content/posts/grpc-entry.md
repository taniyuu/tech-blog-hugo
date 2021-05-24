---
title: "gRPC入門編"
date: 2021-04-14T17:14:05+09:00
draft: true
description: とりあえずネットで調べてみたものを中心に抜粋したりする
tags: [go, gRPC]
---

## 入門で調べる

- [サービス間通信のための新技術「gRPC」入門 | さくらのナレッジ](https://knowledge.sakura.ad.jp/24059/)

### 歴史

- Stubby っていう Google 内で使われてた RPC インフラストラクチャを利用しつつ、SPDY, HTTP/2, QUIC とか標準的に使用されてる奴に対応しようとしたで、ってのが歴史的な経緯らしい([公式ブログより](https://grpc.io/blog/principles/))

- > プロトコルに`HTTP/2`、データのシリアライズに`Protocol Buffers`を利用する

  - Protocol Buffer には 2 と 3 があるらしいが、3 だけを対象に読んでいく
  - ちなみに開発言語は Go 使うつもりなので、そのつもりで読んでいく

### Protocol Buffer の文法

[Google 先生の公式](https://developers.google.com/protocol-buffers/docs/proto3)

- `message`
  - やりとりするデータの定義(なんのことかよくわからんので実装したら確認)
  - `型 名前=順番`で定義する
  - 型はよくあるプリミティブな型をはじめ色々ある →[Scalar Value Types](https://developers.google.com/protocol-buffers/docs/proto3#scalar)と呼ぶ
  - `repeated`で繰り返し属性？を定義できる
  - `oneof`で配下のどれか１つの意味
- `enum`（必要に応じてさらに調べる）
  - デフォルトの順番を 0 で定義する
  - message 内に message を入れたり enum を入れたりできる(使いどころが出たら…)
- `import`と`import public`
- `package`
  - 名前空間を分けるために使う（命名規則は？）
- `service`
  - 公式は、gRPC 使うよね？って流れで書かれていたので、他のプロトコル？を思い描きづらいけど、RPC だったら RPC の呼び出し、IF を定義するもの
- `option go_package`

### protoc

proto ファイルから、それぞれの言語のコードを生成する、くらいの解釈に留めておく。

## チュートリアル

[gRPC の Quick Start](https://grpc.io/docs/languages/go/quickstart/)を参考にして Go の場合を実施

```
$ protoc --go_out=. --go_opt=paths=source_relative \
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    helloworld/helloworld.proto
```

訳がわからん。[このページ](https://note.com/dd_techblog/n/nb8b925d21118)が解説してくれているので、わかるまで頼っておく。とりあえず、2020 年に何かが変わったらしいので、それ以前の記事に気をつける。
[この辺](https://grpc-ecosystem.github.io/grpc-gateway/docs/tutorials/generating_stubs/using_protoc/)や[この辺](https://blog.ebiiim.com/posts/grpc-with-go-mod/)も解説してくれてる。

生成されたファイルが何を意味するかくらいは押さえておきたい。

- `helloworld.pb.go`
- `helloworld_grpc.pb.go`

## 用語

### stub

スタブ、というと一般的に模したものっていうイメージがあるけど、合ってる？
