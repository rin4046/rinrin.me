---
title: "TypeScript(の型)でフィボナッチ数列をやる"
date: 2022-02-25T03:01:24+09:00
draft: false
---

暇つぶしに[こんな記事](https://zenn.dev/kerukukku1/articles/b66844ba02bc8c)を読んでいたら、

TypeScriptの型システムを駆使すれば、フィボナッチ数列くらい容易く演算できるだろうと思い立ち、書いてみた。

## 加算の実装

まずは元記事を参考にして、再帰を用いて任意の長さの配列を生成してみる。

```typescript
type GenArr<A extends number, Result extends number[] = []> =
  Result["length"] extends A ? Result : GenArr<A, [...Result, 0]>;

GenArr<3>; // [0, 0, 0]
```

Resultの"length"プロパティがAと一致するまでResultの末尾に0を追加し続ける。

次にこれを用いて加算を実装する。上で生成した任意の長さの配列を二つ連結して、それのlengthを取得して返せばいいみたい。

ただ、これだけだとTypeScriptのコンパイラに「戻り値がnumber型の制約を満たさない」と怒られてしまう。なので戻り値がnumber型の制約を満たしていたら戻り値をそのまま返し、それ以外は0を返す、といったConditional Typesを書いておくとコンパイラに怒られなくて済む。

```typescript
type Sum<A extends number, B extends number> =
  [...GenArr<A>, ...GenArr<B>]["length"] extends number
    ? [...GenArr<A>, ...GenArr<B>]["length"]
    : 0;

Sum<1, 2>; // 3
```

## フィボナッチ数列の実装

ここまで来ればもう簡単で、数列のn番目の数をf(n)とした時、f(n-2)をB、f(n-1)をCというジェネリクスで保持しながら再帰するコードを書けばフィボナッチ数列ができる。

```typescript
type Fibonacci<
  A extends number,
  B extends number = 0,
  C extends number = 1,
  Result extends number[] = [],
> = Result["length"] extends A ? Result
  : Fibonacci<A, C, Sum<B, C>, [...Result, B]>;
```

フィボナッチ数列が正しく生成できているかどうかチェックするには、

```typescript
const fibonacci: Fibonacci<16> = null;
```

こんな感じのコードを書いてコンパイラの吐くエラーを見ればいい。

## 備考

元記事にもあるように、TypeScriptのコンパイラには再帰できる回数に制限があるらしい(1000回?)ので、この方法ではせいぜいf(16)くらいまでしかフィボナッチ数列を生成できない。まあ、所詮ネタ記事なのでこれ以上を求める必要もないし、もし求めたいのであれば元記事の著者様が丁寧に解説されているのでそれを参考にSumを実装したらもっと長い数列を生成できそう。

TypeScript初心者なんで間違いあったら許して〜
