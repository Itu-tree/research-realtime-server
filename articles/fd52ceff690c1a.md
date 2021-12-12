---
title: "XR向けリアルタイムサーバーの調査"
emoji: "🎉"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["XR"]
published: false
---
# この記事は？
アドカレです。
UnityのXRで使いたいマルチオンラインアプリの「マルチ」を支える「リアルタイム」技術を整理してみました。
マルチプレイでどうやって位置同期を実現しているのかを気になったので調べてみました。
この記事がもととなっています。



「Unity」の「マルチプレイゲーム」によくでてくる「リアルタイム技術」に関する用語を整理しました。

マルチプレイゲームを作ってみたいと思っても、ネットワーク周りをどうしたらよいかわからないのでどのようなサービス、フレームワークがあるのかを調べて整理してみました。


# この記事で伝えたいこと

- リアルタイム技術を勉強するための取っ掛かりとする
- どのリアルタイム技術を試してみるかの判断材料とする
- 様々なレイヤーのリアルタイム技術選定の助けとなる
- リアルタイム技術を触ってみたい、勉強してみたい人向け勉強領域の取っ掛かり
- サービスを利用したい、実装を見てみたい。

とりあえず、マルチプレイゲームを作ってみたい->photon

ミドルウェアや通信ライブラリの勉強してみたい->オープンソース

プロトコルから勉強したい

Unityでマルチプレイアプリを作りたいなとなったときに、色々なサービス・用語・技術がありすぎて何を選んだらよいか、また、何を試しに触ってみればよいかが分かりませんでした。
そこで、これらの用語を整理し、ようと思った次第です。

色々なゲームサービスに関して調べモレとかがあるかもしれませんが、用語を整理するという目的です。

分かりませんでした。そこで、リアルタイムアプリを支える技術にどんなサービス

リアルタイムのマルチプレイヤー ゲームプレイを

# 伝えないこと
認証系、ボイスチャット

# 用語
ネットワーク、リアルタイム、マルチプレイ


# リアルタイム技術

## 設計やネットワークの用語
まず、クライアント同士が繋がるためにはどんな方法があるのでしょか

- ネットワークトポロジー
- pub/sub
- dedicated server
- ListenServer

#### p2p
ナット越え 


## 様々なレイヤー

## マルチプレイサービスから技術

### マネージドサービス
バックエンドのインフラを提供するサービス。アクセス数に応じたサーバーのスケーリングや、認証機能、適切なマッチメイキングや、プレイヤーデータ分析機能、マルチプラットフォーム機能などを提供する。
データ同期のためのリアルタイムサーバーは提供されているものや、自前で用意するものがある。次のBaaSと被る部分があるが、サーバーのスケーリングあるなど、私の解釈でインフラの役割が大きそうで、バックエンドの中身の入れ替えが可能そうなものをマネージドサービスにいれいている


- サービス
    - [Amazon GameLift（最上級クラスの専用ゲームサーバーホスティング）| AWS](https://aws.amazon.com/jp/gamelift/)
    - [PlayFab | Microsoft Azure](https://azure.microsoft.com/ja-jp/services/playfab/#overview)
    - [Unityゲーミングサービス | Unity](https://unity.com/ja/solutions/gaming-services)
        - [Multiplay の専用ゲームサーバーホスティング | Unity の Multiplayer Services](https://unity.com/ja/products/multiplay)
        - [Unity - NetCode](https://unity.com/ja/products/netcode)
            - [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)


### BaaS (Backend as a service)
ゲームのバックエンド　、データ同期のためのリアルタイムサーバー、認証機能、適切なマッチメイキングや、プレイヤーデータ分析機能、マルチプラットフォーム機能などを部分的に提供する。インフラを提供している。
インゲーム機能やアウトゲーム機能、バックエンドのインフラを提供するバックエンドサービス、マネージドも可能だが、マッチメイキングや、リアルタイムなど部分的にバックエンド機能だけ取り出してを使えるサービスはこちら。p2pでクライアントの接続処理やマッチメイキングのためのバックエンドサービスはこちら。
プラットフォームSDKの機能は、アプリに統合できる個々のコンポーネントです。これらの機能はそれぞれ独立して使用できますが、複数を組み合わせて使用すると、より深く魅力的なVRエクスペリエンスになります。

- サービス
    - [GAMESPARKS](https://www.gamesparks.com/)
    - [Steamworks](https://partner.steamgames.com/)
        - [マルチプレイヤー (Steamworks ドキュメント)](https://partner.steamgames.com/doc/features/multiplayer?l=japanese#gameservers)
    - [Oculus開発者センター](https://developer.oculus.com/)
        - [プラットフォームソリューション: ネイティブ | Oculus開発者](https://developer.oculus.com/documentation/native/ps-platform-intro/)
        - [マルチプレイヤー機能の概要: Unity | Oculus開発者](https://developer.oculus.com/documentation/unity/ps-multiplayer-overview)
    - [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
        - [Realtime | Photon Engine](https://www.photonengine.com/ja-JP/Realtime)はPUNとことなり、Unityから独立している。


- ミドルウェア
    - 

- 通信用ライブラリ


- 通信規格(インターフェース)
 - WebSocket
 - Quick
 - WebTransport
 - p2p

- プロトコル
    - TCP
    - UDP
    - RUDP
    - MQTT
    - HTTP / 1 2 3

- シリアラライズ
    - protobuf
    - json


# デモ、触りやすそうなやつ
- [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
- [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
- [WebRTC Gateway | ドキュメント | SkyWay(アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK)](https://webrtc.ecl.ntt.com/documents/webrtc-gateway.html#%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9)
# 個人で実装するには

タイトル
概要
この記事で伝えたいこと　←★重要★
解決したい課題　←★重要★
課題の原因
    課題を解決する技術、手法
    技術、手法の概要
    技術、手法の効果
課題がどう解決されるか
応用事例のカタログ化
留意点、デメリット