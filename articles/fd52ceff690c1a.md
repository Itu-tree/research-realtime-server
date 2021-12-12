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
一体何から触ればいいのかが分からないので、リアルタイム、ネットワーク、マルチプレイゲームで調べ挙げて、個人的にさわってみたいものを挙げてみました。

個人のメモ的な側面が大きいです。

マルチプレイでどうやって位置同期を実現しているのかを気になったので調べてみました。
この記事がもととなっています。


ネットワーク周りの勉強ができるようなライブラリやサービスを使っていいきたい。

「Unity」の「マルチプレイゲーム」によくでてくる「リアルタイム技術」に関する用語を整理しました。

マルチプレイゲームを作ってみたいと思っても、ネットワーク周りをどうしたらよいかわからないのでどのようなサービス、フレームワークがあるのかを調べて整理してみました。

https://izm-11.hatenablog.com/entry/2019/05/09/120458
先のエントリでPUNの話をしたとき、モノビットエンジンはどうなっているのか気になったのでサイトを見たり、モノビットの人に教えてもらいました。 幾つかの点で競合の他サービスと比較して採用を検討する利点や、あるいは難点もあったので情報をまとめておきます。
調べて書いた程度なので、実案件運用をしての感想ではない事ご容赦ください。

前提として個人あるいは小規模のUnity向けMOゲーム開発を想定しています。

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
- サバクラモデル
    - pub/sub
- dedicated server
- ListenServer

#### p2p
ナット越え 

リレーサーバー


## 様々なレイヤー

## マルチプレイサービスから技術

### マネージドサービス
バックエンドのインフラを提供するサービス。アクセス数に応じたサーバーのスケーリングや、認証機能、適切なマッチメイキングや、プレイヤーデータ分析機能、マルチプラットフォーム機能などを提供する。サーバーをホスティング
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
    - [マルチプレイヤー機能の概要: Unity | Oculus開発者](https://developer.oculus.com/documentation/unity/ps-multiplayer-overview)
    - [モノビットエンジンクラウド](https://web.cloud.monobitengine.com/top)


### Unityで利用可な通信ミドルウェア
ネットワークエンジン、通信エンジン、ネットワークライブラリと言われていそうな
通信エンジンといわれるものや、NAT越えが必要なさそうなミドルウェア
Unityクライアントの実装のみで通信の完結が可能なミドルウェアということにしています。

Unity専用！クライアントプログラムだけでマルチプレイを簡単に実装できる通信ミドルウェアです。独立したソフトウェアとプロトコルを使ってやり取りするソフトうぇえあ
- 例
    - [Unity - NetCode](https://unity.com/ja/products/netcode)
        - [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
        - 旧MLAPI
    - [Monobit Unity Networking 2.0 (MUN) - モノビットエンジン公式サイト](http://www.monobitengine.com/mun/)
    - [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
    - [Fusion | Photon Engine](https://www.photonengine.com/ja-JP/Fusion)
        - PUNに置き換わるものとして開発された
        - [Fusionイントロダクション | Photon Engine](https://doc.photonengine.com/ja-jp/fusion/current/getting-started/fusion-intro)
    - [vis2k/Mirror: #1 Open Source Unity Networking Library](https://github.com/vis2k/Mirror)
        - OSSなのでコードが読める
    - [Firebase Realtime Database  |  Firebase Documentation](https://firebase.google.com/docs/database?authuser=0)
        - RealTimeDatabaseとあるのでXR向けで位置同期できるのか気になる
    - [SkyWay | アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK](https://webrtc.ecl.ntt.com/)
        - [WebRTC Gateway | ドキュメント | SkyWay(アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK)](https://webrtc.ecl.ntt.com/documents/webrtc-gateway.html#%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9)
        - 使用例に「VRゲームの通信部分をWebRTCで実装する」とある
        - ホントにUnityで利用できるのか気になる

### 通信用ライブラリ
トランスポート層のTCPやUDPを扱いやすくするライブラリ、LLAPI(LowLevelAPI)に
リアルタイム通信エンジン、NAT越えの実装や、リレーサーバーの実装が別に必要になりそうなもの、


- [sta/websocket-sharp: A C# implementation of the WebSocket protocol client and server](https://github.com/sta/websocket-sharp)
    - UnityでWebSocket Protocolを扱えるようにしたUnityAsset
- [gRPC](https://grpc.io/)
    - 言語に依存しないRPC(RemoteProcedureCall)フレームワーク
- [Cysharp/MagicOnion: Unified Realtime/API framework for .NET platform and Unity.](https://github.com/Cysharp/MagicOnion)
- [RevenantX/LiteNetLib: Lite reliable UDP library for Mono and .NET](https://github.com/RevenantX/LiteNetLib)
    - [LiteNetLib #features](https://revenantx.github.io/LiteNetLib/index.html#features)
    - .NET用のRUDPを扱うライブラリ
- [chkr1011/MQTTnet](https://github.com/chkr1011/MQTTnet)
    - [メタバースプラットフォーム cluster（クラスター）](https://cluster.mu/)は2018/12の時点で[m2mqtt](https://github.com/eclipse/paho.mqtt.m2mqtt)を使っている
        - [MQTTnet を Unity で使う - Qiita](https://qiita.com/johnson65t/items/230360b4cec41e8aafa4)
    - 
アプリに組み込んで利用するプログラム

### プロトコル
リアルタイム系の通信用ライブラリで扱われるプロトコルを並べた

TCPやUDPについては[TCP/IPとは？通信プロトコルの階層モデルを図解で解説 | ITコラム｜アイティーエム株式会社](https://www.itmanage.co.jp/column/tcp-ip-protocol/)

- [WebSocket - Wikipedia](https://ja.wikipedia.org/wiki/WebSocket)
    - TCP上で双方向通信を可能にしたHTTPと互換性のあるプロトコル。443番ポートと80番ポート上で動作する
- [MQ Telemetry Transport - Wikipedia](https://ja.wikipedia.org/wiki/MQ_Telemetry_Transport)
    - TCP/IPによるPub/Sub型データ配信モデルのプロトコルでIoT機器向けで使われることがある。
    - [MQTT プロトコルの概要](https://docs.kii.com/ja/guides/thingifsdk/non_trait/mqtt/protocol_overview/)
- TCP
- [Reliable User Datagram Protocol - Wikipedia](https://ja.wikipedia.org/wiki/Reliable_User_Datagram_Protocol)
    - [詳解 Reliable UDP - Speaker Deck](https://speakerdeck.com/fadis/xiang-jie-reliable-udp)
- UDP
    

- 将来のプロコル
    - HTTP 2 3
    - Quick
    - WebTransport

### シリアラライズ
送りたいデータを人が読める形ではなく、バイナリでおくることで、通信効率を高まることができる。
    - protobuf
    - messagepack
    - json + Base64
    - [neuecc/MessagePack-CSharp: Extremely Fast MessagePack Serializer for C#(.NET, .NET Core, Unity, Xamarin). / msgpack.org[C#]](https://github.com/neuecc/MessagePack-CSharp)
        - C#向けの[MessagePack](https://msgpack.org/)
        - バイナリシリアリゼーションフォーマット



# デモ、触りやすそうなやつ
- [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
- [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
- [WebRTC Gateway | ドキュメント | SkyWay(アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK)](https://webrtc.ecl.ntt.com/documents/webrtc-gateway.html#%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9)
- [Unity プロジェクトに Firebase を追加する  |  Firebase Documentation](https://firebase.google.com/docs/unity/setup?authuser=0)
- [Firebase Realtime Database  |  Firebase Documentation](https://firebase.google.com/docs/database?authuser=0)
    - XRの位置同期につかえるのか気になる
- [SkyWay | アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK](https://webrtc.ecl.ntt.com/)
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