---
title: "色を探すサイトを作りました"
date: 2024-01-03
categories:
    - report
tags:
    - React
    - Vite
    - TypeScript
    - styled-components
    - github
keywords:
    - React
    - Vite
    - TypeScript
    - styled-components
    - github
    - github actions
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /img/munsellcolor.jpeg
coverImage: /img/munsellcolor.jpeg
metaAlignment: center
---

色を探せる web サービスを作りました。

<!--more-->

![Tranquilpeak](/img/munsellcolor.jpeg)

<!-- {{< toc >}} -->

## 色見本アプリ
https://kanakanho.github.io/munsell-colors/  
マンセル表色系に基づいた色立体を web 上で閲覧、使いたい色をクリックするとカラーコードをコピーするサービスを開発しました。

# 開発に至った経緯

マンセル表色系に基づく色立体が欲しいと考えていたのですが非常に高価で、また私が普段するデザインは web の UI といったパソコンでの表示を前提としたものなので web で色立体を表示して色を見つけられるサービスを作ろうということで開発に着手しました。

# 技術選定

色立体が欲しいと考えていたので、最初は 3 次元での描画を想定していたのですが、WebGL を使うとライトの当て方によって色の見え方が変わってしまう問題があると感じたため、あくまで平面的に色を探せるサイトにしようと考えました。当初、生の HTML と JavaScript で開発していたのですが、色を表示するにあたりコンポーネントベースでの表示が適していると考え、React を使いました。

# 工夫したところ

web サイトを開くとデフォルトで 左側に 色のカード、右側にスクロールして色を調べられるビューが表示されます。当初は色の表示を左右で同期させ、1 つの色を選ぶことに特化させようと考えていましたが、色を選ぶ際には見比べて選ぶという需要の方が高いだろうと考えて、左右でそれぞれ色を選んで見比べられる現在の形に落ち着きました。  
card ビューではより直感的にカードが選択できるように、それぞれのカードを代表する色を円に並べ、その色をクリックすることで自分の欲しい色に辿り着きやすくなる機能を実装しました。色を円形に配置するのは相対的に生成した円の半径から計算して絶対的な位置を補正して実現しました。また色の表示は画像を用いるのではなく、svg を使って直接書きました。

# 苦労した点

左右の状態をそれぞれ持つことになったため、状態の管理がやや複雑になりました。今自分が見ているカードと選択した色を左右でそれぞれ保持しておかなければいけないため、そのあたりの実装は大変でした。  
アプリの特性上、カードの表示が崩れると一気に利便性が下がってしまうのでそのあたりが絶対に崩れないように注意を払いながら開発しました。カードのサイズや位置が絶対に崩れないように調整を行ないました。レスポンシブにも対応させ、スマートフォンでも使えるサービスとしました。

# 技術スタック

フロントは React + Vite + TS と styled-components を使っています。    
色のデータは GithubAPI を使って、今回のレポジトリとは別で用意した自分のレポジトリから取得しています。

# 終わりに

年末の思いつきで時間があったので作成しました。これから XR 関係の技術をメインに開発していくことになるので、web 関連の開発はこれから減っていくと思いますが、これまで習得した技術は忘れないようにしたいです。

# Contact&Links

Email : [seikenshibata@gmail.com](seikenshibata@gmail.com)  
github : [kanakanho](https://github.com/kanakanho)  
twitter : [@Shiba_ao](https://twitter.com/Shiba_ao_)
