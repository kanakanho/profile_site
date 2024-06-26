---
title: "XR で研究室紹介を行うサービスを作りました"
date: 2024-03-25
categories:
    - kajilab
tags:
    - kajilab
keywords:
    - meta qeust
    - XR
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /img/XRStudyWatch/server.png
coverImage: /img/evening_blue.jpg
metaAlignment: center
---

梶研究室で研究室メンバーと 3 ヶ月ほどチーム開発していたプロダクトの紹介です。

<!--more-->

<!-- {{< toc >}} -->

# 概要

2023 年 12 月から 2024 年 3 月まで 4 ヶ月ほど開発していたプロダクトの紹介です。

このプロダクトでは Android のモバイルと MetaQuest3 の XR , Go,Python,TS などで構成されたバックエンドで構成されています。詳細についてはシステム構成の章で説明します。

## スライドでの説明はこちら

{{<xr-study-watch>}}

# プロジェクトの概要

## 背景

研究室配属前の学生は研究室内での活動について知る機会がとても少ないという現状がある。研究室は基本的に閉じられていて、外部から人が入ってくるのに適していない。
また配属前の学生はどの研究室がどこにあるのかを認識しておらず、見学までに知る機会がない。そして研究室配属後に活動や雰囲気を知るため配属されて初めて判断できるため、学生が自分にとってよい研究室を探すのは難しい。

## 目的

-   研究室の場所と活動内容を知ってもらう
-   研究室の人と交流を促し、学生の間で縦のつながりを増やす

研究室に配属されるまえから研究を知って、自分の興味のある分野を探求していくのは学生にとって利点になる。そのために配属される研究室を検討するために調べるだけでなく、早い段階から研究室を認知できるとよい。

## アプローチ

研究室の活動は閉じていて、外の人からはどんな活動をしているのか分からないという問題がある。研究室の前を訪れた人にその研究室の活動を知ってもらえたら、交流の輪を広げられるだけでなく、配属前の学生が研究室を知るきっかけになると考えた。そのために研究内容や学生の様子をまとめたポスターを研究室の前に掲示するのを提案する。

研究室の前にポスター展示をする利点として以下のようなものがある。ポスターをその場所で見ると研究室の場所と内容をセットを覚えられる。また研究室前に行くとその研究室に参加している学生さんや教授と交流する機会を増やせる。

しかし実際の建物にポスターを設置するのはハードルが高い。大判のプリンターの使用には紙やインクにお金がかかる。また大きなポスターを壁に設置するためにテープや画鋲といった留め具を用いると壁紙に損傷を与える可能性がある。しかしポスター用の掲示板を新しく用意するのにもお金が必要になるだけでなく、場所を取るため通行の邪魔となる。

そこで AR（拡張現実） の利用を提案する。AR を利用すると先に挙げた物理的な問題点が解消されるだけでなく、ポスターのサイズや掲示場所を自由に決められる。クラウド上で表示するポスターを管理すると、ユーザーは手持ちの端末から簡単に表示内容の編集や追加が行えるようになる。

## 機能

主な機能は以下の 3 つに分けられる。

-   ポスターのアップロードおよび表示を行うクライアント
-   ポスターと位置情報を関連させるサーバー
-   位置推定を行うサーバー

### クライアント

クライアントは Android と MetaQuest 3 の 2 つがある。
Android は、ポスターのアップロードと位置推定のためのデータの収集、ポスターの表示を行う。
MetaQuest は Android が取得したデータに基づく位置情報を参照したポスターの表示を行う。

Android だけではなく、 MetaQuest を取り入れた理由は現実により近い状態で研究室の紹介を可能にするためである。Anroid 端末を通してポスターは見えるが、スマートフォンでもタブレットでも画面サイズが小さく、ポスターを見るために端末を大きく動かす必要があり、ポスターを現実に近い状態で見るのは難しい。しかしヘッドマウントディスプレイである MetaQuest を用いれば現実でポスターを眺めると同じようにできる。

### ポスターと位置情報を関連させるサーバー

Android がアップロードした 推定のためのデータ とポスターを関連付け、Android を持つユーザーがポスターのある場所にたどり着いた場合にデータを渡す。

### 位置推定を行うサーバー

位置推定には GPS と FP（フィンガープリント） を利用する。
GPS と FP の 2 種類を用いる理由は、GPS が屋内での位置推定に向いていないというものがある。研究室はほとんどの場合屋内にあり、GPS のみを用いて推定を行なっても実際にどの研究室の前にいるかといった建物内での座標を取得するのは難しい。
そのため FP を用いて、建物内の電波情報を元にどの研究室の近くにいるかの推定を行う。

## システム構成

バックエンドのシステム構成は以下の図のようになった。
![システム構成図](/img/XRStudyWatch/server.png)

それぞれ複数の機能が絡みあって機能を提供しているため、それぞれをマイクロサービス化した。

ミドルウェアとして MySQL と MinIO を利用している。 MinIO を利用している理由はオンプレミスで画像情報を保存したかったためである。AWS の S3 と同じように利用できるため選んだ。

また位置推定については今後別のアプリ開発でも利用できるように汎用性を高く持たせるように設計を行った。位置推定の機能は API として提供するようにした。

---

# 実装した部分

MetaQuest で研究室のポスターを可視化する部分と、FP を用いた屋内位置推定の精度向上のための検証を行った。

## MetaQuest

### デモムービー

{{<youtube 7g9YKQMvopg>}}

### 苦労した点

Unity で MetaQuest の開発を行うのに必要な Meta が提供している SDK に破壊的なアップデートが発生し、新しい SDK を利用するために移行を行いました。  
これまでは web の開発を多く行っていて、ライブラリのバージョン変更による破壊的なアップデートに影響されなかったので、初めての経験でした。新しいバージョンでどのような変更があったのかを理解し、変更点を適応するのに苦労しました。

### 工夫した点

ユーザーにより直感的な体験を与えるためにコントローラーを使用しないアプリケーションとして開発を行った。
現在多くの MetaQuest アプリはコントローラーを利用したものであり、SDK もコントローラーの利用を前提として提供されている。そのため手のみでの快適な操作を提供するために模索した。
テキストの入力を 1 つとっても、ユーザーの手がどのように動いたらテキストの入力を可能とするのかの検証が必要となる。ユーザーの入力を快適にするために UI を操作する体験がよくなるように調整を重ねた。

研究室を探すというアプリの目的を達成するためには、建物内を視界が十分に確保された状態で自由に歩ける必要がある。
視界を確保するためにパススルー機能を有効化しヘッドマウントディスプレイをした状態でも周りがよく見渡せるようにした。
しかし Meta は自由に歩くための機能を一般ユーザーに提供しておらず、開発者にのみ利用可能な試験的な機能としている。それゆえ現状公式がリリースしている空間認識技術が利用できず、ポスターをより現実に近い状態で表示させるには、自前で空間認識をする必要があった。DepthAPI は試験的な機能ではあるが、本アプリでも利用可能であったため導入した。深度情報に基づく表示が可能になり、より現実で見ているのに近い体験をユーザーに与えられるものとなった。

Android が取得したデータを元に推定を行った結果はリクエストを送った相手である Android にはレスポンスとして返せるが、MetaQuest にはそれができない。
これを解決する方法としては Android か サーバー と MetaQuest で双方向通信を行うか、MetaQuest がポーリングを行う方法がある。
今回は MetaQuest がポーリングする方法を選んだ。
双方向通信を選ばなかった理由にはクライアントのデータフローが複雑になる問題がある。Android もしくは サーバー が推定した結果をユーザーに返す場合通常の HTTP 通信の途中で双方向通信が発火し、全体としての流れが分かりにくくなる。また研究室のポスターを見るという行為にリアルタイム性はさほど重要ではないためポーリングを採用した。

## FP を用いた屋内位置推定の精度向上のための検証

FP を用いた位置推定をより正確なものとするために RSSI を使った分析結果の重み付けを行った。

RSSI と距離の関係は以下のように定義されている。

```
RSSI = (TxPower - 10) * n * log(10,距離)
```

TxPower は 送信側から 1 ｍ離れたところで測定可能な値で、その地点でおおよそ RSSI 値がどれくらいであるかを示す。
大抵は 0 となる。
https://zenn.dev/yukichi_tech/articles/1539483ed67180

n はその場所の電波状況を表す。
電波状況がよい場合は 0.2 を用いて、電波状況が悪い場合には 0.4 を用いる。

先にあげた式を距離について整理すると以下のようになる。

```
距離 = 10 ^ ((RSSI) / n * 10)
```

n についてはその場に飛んでいる電波を見境なく取得しているためどの程度正しいのか評価するのは難しいため、0.2 と 0.4 の間である 0.3 を採用した。
これにより強い電波は信用でき、弱い電波はあまり信用できないとバイアスをかけられる。

![RSSIの確率密度関数](/img/XRStudyWatch/RSSI_function.png)

RSSI の有効な範囲はおそよ-100 ~ -20 であるためその範囲の面積を確率密度関数とし、精度向上のために推定結果にバイアスをかけた。

バイアスをかける前と後でどのように変化したかを以下のグラフに示す。

![RSSIの適応前](/img/XRStudyWatch/RSSI_before.png)
![RSSIの適応後](/img/XRStudyWatch/RSSI_after.png)

それぞれ比較に利用したスキャン回数と共に比較しているが、いずれの場合でも RSSI を推定に組み込むとデータの散らばりが抑えられ、違う場所での評価については正しい結果を得られたと考えられる。

また 回数による比較を行うと 5 回 では近くに研究室がある場合間違った結果を返すケースがあるが、10 回以上ではおおむね同じ特徴が現れていると分かる。

今回の手法で FP を使った評価をするには 10 回以上スキャンしたデータがあればよいと分かる。

# 終わりに

今回の開発では Unity を使った MetaQuest 開発に加えて、数学的な知識が要求される実装が必要となりました。
SDK の仕様を把握したり、まだ誰も記事にしていない実装を多く行いました。そのおかげで Qiita の記事がいくつか増えました。最新の情報を元に実装をするため ChatGPT が使えない場面が多く存在して、これまでの開発で一番リファレンスを読んだと思います。

MetaQuest でハンドトラッキングのみでテキスト入力する
https://qiita.com/kanakanho/items/b0c68bb1b81c9e95624d

MetaQuest3 で DepthAPI を利用した オクルージョン の設定
https://qiita.com/kanakanho/items/94285233f3848fc41f83

数学的な知識が足りていないと感じました。確率や統計の知識が不足していて、数学的にどんなアプローチができるのかがなかなか見当がつかず、仮説を立てて検証を行う繰り返しでした。
今後はもっと数学を大切にしていきたいと感じました。

# Contact&Links

Email : [seikenshibata@gmail.com](seikenshibata@gmail.com)  
github : [kanakanho](https://github.com/kanakanho)  
twitter : [@Shiba_ao](https://twitter.com/Shiba_ao_)
