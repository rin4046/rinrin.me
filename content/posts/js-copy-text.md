---
title: "JavaScriptでテキストをコピー"
date: 2016-12-30T00:00:00+09:00
draft: false
---

最近やっと知ることができたので、メモしとく。

```
document.addEventListener('DOMContentLoaded', function() {
  document.getElementById('copybutton').addEventListener('click', function() {
    var range = document.createRange();
    var copytext = document.getElementById('copytext');
    range.selectNode(copytext);
    window.getSelection().addRange(range);
    document.execCommand('copy');
  });
});
```

IDを自分のものに置き換えればいい。

~~[Demo]()~~

`document.execCommand('copy')`は、変数の中身をコピーすることはできなくて、画面に表示されているものしかコピーできないらしい。
