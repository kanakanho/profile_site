---
title: "翻訳アプリをリリースしました"
date: 2023-10-02
categories:
    - report
tags:
    - React
    - Tauri
    - JavaScript
    - github
keywords:
    - React
    - electron
    - Tauri
    - JavaScript
    - github
    - github actions
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /img/evening_resize.jpg
coverImage: /img/evening.jpg
metaAlignment: center
---

kanakanho です。最近は Swift と React を触っています。
今回 React + Tauri で翻訳アプリを作りました。

<!--more-->

![Tranquilpeak](/img/transrate/transrate-app.png)

<!-- {{< toc >}} -->

# 翻訳アプリ

簡素な UI のアプリケーションで左側に翻訳したい文章を入力すると、右側に翻訳された文章が出力されます。  
入力された文章と出力された文章はそれぞれ右上のクリップボードのアイコンからコピーできます。

真ん中の矢印アイコンを押すことで、【英語 → 日本語】と【日本語 → 英語】の入力言語の切り替えができます。

[レポジトリはこちら](https://github.com/kanakanho/transrate-tauri-app)  
アプリのダウンロードもこちらからできます。

# 技術スタック

フロントは React と Tauri を使っています。  
バックは Google Apps Script を使っています。

# 終わりに

web 側の技術をそのままにアプリが作れるのがとても便利だと感じました。最近 swift を触っているのでよりひしひしと感じました。  
初めに electron を使ったのですがアプリの起動速度が遅く感じたので、Tauri を使いました。Tauri ってめちゃめちゃ早いんですね。2 回目以降のビルドが早くなるのもいいですね。
GithubActions も初めて使いましたが、yml ファイルを書くだけでアプリがリリースできるのいいですね。

# Contact&Links

Email : [seikenshibata@gmail.com](seikenshibata@gmail.com)  
github : [kanakanho](https://github.com/kanakanho)  
twitter : [@Shiba_ao](https://twitter.com/Shiba_ao_)
