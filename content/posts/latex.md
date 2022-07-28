---
title: "LaTeX、結局どうやって環境構築するのが一番いいのか？？"
date: 2022-01-30T21:33:18+09:00
draft: false
---

Docker Image を使うのもいいと思うんだけどな。。。

<!-- more -->

### Windows の場合

- TeX Live をインストールする  
(W32TeXはもう古いので使わない)
- Docker でごにょごにょする

### macOS の場合

- TeX Live をインストールする
- MacTeX をインストールする
- Docker でごにょごにょする

### Linux の場合

- TeX Live をインストールする
- Docker でごにょごにょする

こうして書いてみても、環境差異に関係ないDockerを使った方法が一番手軽でいいのではと思う。

[https://github.com/rin4046/texlive-ubuntu](https://github.com/rin4046/texlive-ubuntu)

[https://hub.docker.com/r/rin4046/texlive-ubuntu](https://hub.docker.com/r/rin4046/texlive-ubuntu)

手前味噌だが、自作のDocker Imageを載せておく。

いずれ使用法をまとめる。
