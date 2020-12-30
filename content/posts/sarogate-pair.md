---
title: "サロゲートペアってなんだ"
date: 2020-12-31T00:14:16+09:00
description: 文字コードって面倒だねって話かな
tags: ["Unicode"]
---

# 文字コードの簡単な勉強

[文字コード再入門 ─ Unicodeでのサロゲートペア、結合文字、正規化、書記素クラスタを理解しよう！](https://eh-career.com/engineerhub/entry/2020/04/28/103000)
[【図解】【3分解説】UnicodeとUTF-8の違い！【今さら聞けない】](https://qiita.com/omiita/items/50814037af2fd8b2b21e)
[バイト順マーク](https://ja.wikipedia.org/wiki/%E3%83%90%E3%82%A4%E3%83%88%E9%A0%86%E3%83%9E%E3%83%BC%E3%82%AF)

ざっくりいうと
- Unicodeはあらゆる文字を何でも頑張って16ビットで何でも表現（コードポイントと呼ぶ）しようとしたよ。
- でも、足りなかったよ。なので、2文字分（32ビット）で忖度することにして、こっちに突入するものをサロゲートペアと呼ぶよ。
- Unicodeを符号変換する方法にはUTF-16、UTF-32、UTF-8があるよ。
  - この辺を見分けるためにBOMという決め事で自己紹介できるよ。
  - ただし、UTF-8でBOM有無を判別しないソフトウェアもあったりするので気をつけてね。


ググると、日本では俗に「外字」と言われる範囲がそれに当たります。
**𠮷**野家とか。



# Javaでの落とし穴？

[Java の文字型 (char)](https://java.keicode.com/lang/data-types-char.php#3-1)


## 文字列長

- `String#length`だと、Unicode（UTF-16）換算の長さになる。
- `String#codePoints`だと、実際の文字の長さ（コードポイントの個数）になる

```java
public class CharacterValidatorTest {
  private final String yoshi = "\uD842\uDFB7";

  @Test
  public void Lengthだと2が返ってくる() {
    assertEquals(2, yoshi.length());
  }

  @Test
  public void CodePointだと1が返ってくる() {
    assertEquals(1, yoshi.codePointCount(0, yoshi.length()));
  }
}
```

## Bean Validator
`@Length`, `@Size`共に、Unicode単位。
`@CodePointLength`を使いましょう。

※ 日本人が[コントリビュート](https://github.com/hibernate/hibernate-validator/pull/860)してくれてる。
