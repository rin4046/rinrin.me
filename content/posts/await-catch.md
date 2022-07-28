---
title: "try/catchではなくawait/catchを使おう"
date: 2021-11-16T06:47:33+09:00
draft: false
---

async関数はファイルの読み込みや書き込みといった、エラーの可能性を伴うような関数に使われることが多い。この際のエラーハンドリングは通常try/catchパターンで行われる(私もそうしてきた)。

しかし、この方法にはデメリットもある。

- 先に結果を保持するための変数をletで宣言するのが気持ち悪い
- 面倒臭くなってtry/catchのブロックの中身が肥大化しがち

などである。

そもそも、tryの中身をデカくして、なんでもcatchしてしまうようなコードを書くのは好ましくない。エラーを握りつぶしてしまう可能性があるからである。

個人的には、try/catchでエラーハンドリングをするよりも、エラーが発生する可能性のある箇所ごとにawait/catchでエラーハンドリングをして、早期リターンなどで対処する、といったコードの方が適切に思う。

### await/catchパターンの利点

- 関数の結果をconstで保持できる
- エラーが発生したら適切に処理した後、早期リターンで安全に終了できる
- <s>Goのエラーハンドリングみたいで個人的に好き
</s>
### コード例

```javascript
// 1/2の確率でエラーを返す関数
const func = async () => {
  if (Math.random() > 0.5) {
    throw new Error("error");
  }
  return "success";
};

// await/catchパターン
const result = await func().catch((err) => err.message);
console.log(result);

// thenでも書けるよ(TypeScriptのPromiseLike型とかで役に立つかも)
const resultWithThen = await func().then(
  (data) => data,
  (err) => err.message,
);

// 早期リターンで関数を正常に終了
(async () => {
  const earlyReturn = await func().catch(() => undefined);
  if (!earlyReturn) {
    return;
  }
  console.log(earlyReturn);
})();
```
