---
title: "自分で作ったサンプルアプリをFlutter v1->v2にアプデした"
date: 2021-03-14T09:00:59+09:00
draft: false
description: 意外と簡単に上がったけど・・・
tags: []
---

作業ログです

```
yuki@taniyamayuukinoMacBook-Air amplify_flutter_sample % flutter --version
Flutter 2.0.2 • channel stable • github.com:flutter/flutter.git
Framework • revision 8962f6dc68 (2 days ago) • 2021-03-11 13:22:20 -0800
Engine • revision 5d8bf811b3
Tools • Dart 2.12.1
yuki@taniyamayuukinoMacBook-Air amplify_flutter_sample %
```

### 1 回目

```
    /Users/yuki/code/my-github/amplify_flutter_sample/ios/Pods/AppSyncRealTimeClient/AppSyncRealTimeClient/Connection/AppSyncConnection/AppSyncSubscript
    ionConnection+ErrorHandler.swift:9:8: error: compiling for iOS 9.0, but module 'Starscream' has a minimum deployment target of iOS 11.0:
    /Users/yuki/code/my-github/amplify_flutter_sample/build/ios/Debug-iphonesimulator/Starscream/Starscream.framework/Modules/Starscream.swiftmodule/x86
    _64-apple-ios-simulator.swiftmodule
    import Starscream
           ^
```

- とりあえず iOS11 以上にしてみる → ダメ
- Amplify 側の Pods 周りでうまく動いてない問題っぽい →Workaround 対応
  - https://github.com/aws-amplify/amplify-flutter/issues/359#issuecomment-790052655

### 2 回目

Amplify の VUP を実施すると下記が怒られた。基本的には Doc も更新されてるのでその通りに。
https://docs.amplify.aws/lib/auth/signin/q/platform/flutter

- amplify-core が amplify-flutter に鞍替え
- AuthError->AuthException に鞍替え
