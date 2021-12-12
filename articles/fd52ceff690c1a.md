---
title: "XRで使いたいリアルタイム技術・サービスの整理"
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

先のエントリでPUNの話をしたとき、モノビットエンジンはどうなっているのか気になったのでサイトを見たり、モノビットの人に教えてもらいました。 幾つかの点で競合の他サービスと比較して採用を検討する利点や、あるいは難点もあったので情報をまとめておきます。
調べて書いた程度なので、実案件運用をしての感想ではない事ご容赦ください。

前提として個人あるいは小規模のUnity向けMOゲーム開発を想定しています。

Unityでマルチプレイアプリを作りたいなとなったときに、色々なサービス・用語・技術がありすぎて何を選んだらよいか、また、何を試しに触ってみればよいかが分かりませんでした。
そこで、これらの用語を整理し、ようと思った次第です。

色々なゲームサービスに関して調べモレとかがあるかもしれませんが、用語を整理するという目的です。


プロトコルから勉強したい

# この記事で伝えたいこと

- リアルタイム技術を勉強するための取っ掛かりとする
- どのリアルタイム技術を試してみるかの判断材料とする
- 様々なレイヤーのリアルタイム技術選定の助けとなる
- リアルタイム技術を触ってみたい、勉強してみたい人向け勉強領域の取っ掛かり
- サービスを利用したい、実装を見てみたい。
- どのサービス、ミドルウェア、ライブラリを触ってみようとしたときの難易度

とりあえず、マルチプレイゲームを作ってみたい->photon

ミドルウェアや通信ライブラリの勉強してみたい->オープンソース

# リアルタイム技術

## ネットワーク方式
まず、クライアント同士が繋がるためにはどんな方法がある

- Dedicated Server 方式
    - [Dedicated Server（専用サーバー） - フレームシンセシス](https://framesynthesis.jp/tech/unity/network/#dedicated-server%E5%B0%82%E7%94%A8%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC)
    - Pub/SubモデルやROOMモデル
        - この記事がとても分かりやすかったです。[ソーシャルゲームを支える「リアルタイムサーバー」の作り方 - ログミーTech](https://logmi.jp/tech/articles/322569)
 - Listen Server 方式
    - [Unity リアルタイムネットワークメモ - フレームシンセシス](https://framesynthesis.jp/tech/unity/network/#listen-server)
    - 実質的にはP2P的なクライアント同士での通信になるが、NAT越えのためのリレーサーバーを経由することが多い

## 様々なレイヤー

## マルチプレイサービスから技術

### BaaS (Backend as a service)
バックエンドのインフラや様々なアウトゲーム的な機能を提供するサービス。アクセス数に応じたサーバーのスケーリングや、認証機能、適切なマッチメイキングや、プレイヤーデータ分析機能、マルチプラットフォーム機能などを提供する。
BaaS (Backend as a service)と呼ばれるサービスを含む。PhotonやMonobitよりも豊富なバックエンドサービスを提供しているサービスを並べました。


- サービスの例
    - [Amazon GameLift（最上級クラスの専用ゲームサーバーホスティング）| AWS](https://aws.amazon.com/jp/gamelift/)
    - [GAMESPARKS](https://www.gamesparks.com/)
    - [PlayFab | Microsoft Azure](https://azure.microsoft.com/ja-jp/services/playfab/#overview)
    - [Unityゲーミングサービス | Unity](https://unity.com/ja/solutions/gaming-services)
        - [Multiplay の専用ゲームサーバーホスティング | Unity の Multiplayer Services](https://unity.com/ja/products/multiplay)
        - [Unity - NetCode](https://unity.com/ja/products/netcode)
            - [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
    - [Steamworks](https://partner.steamgames.com/)
        - [マルチプレイヤー (Steamworks ドキュメント)](https://partner.steamgames.com/doc/features/multiplayer?l=japanese#gameservers)
    - [マルチプレイヤー機能の概要: Unity | Oculus開発者](https://developer.oculus.com/documentation/unity/ps-multiplayer-overview)


### Unityで利用可能なな通信ミドルウェア
ネットワークエンジン、通信エンジン、ネットワークライブラリと言われていそうなミドルウェア。Unityクライアントの実装のみで通信の完結が可能なミドルウェアを並べています。

- Listen Server 方式
    - リレーサーバーを提供
        - [Unity - NetCode](https://unity.com/ja/products/netcode)
            - [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
            - 旧MLAPI
            - Netcode自体にリレーサーバーの機能はないが、Unity Relay β を使うと良いらしい。[Unity Relay β でリレーサーバを建てて入室する【Unity Gaming Services】 - デニッキ！](https://xrdnk.hateblo.jp/entry/2021/10/30/175456)
        - [Monobit Unity Networking 2.0 (MUN) - モノビットエンジン公式サイト](http://www.monobitengine.com/mun/)
        - [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
        - [Fusion | Photon Engine](https://www.photonengine.com/ja-JP/Fusion)
            - PUNに置き換わるものとして開発された
            - [Fusionイントロダクション | Photon Engine](https://doc.photonengine.com/ja-jp/fusion/current/getting-started/fusion-intro)
        - [Strix Unity SDK 1.4 ドキュメント](https://www.strixengine.com/doc/unity/guide/ja/index.html)
            - [Strixアクションゲームのサンプル — Strix Unity SDK 1.4 ドキュメント](https://www.strixengine.com/doc/unity/guide/ja/unitysdk/sample.html)
            - [わずかな時間でオンラインアクションゲームをつくるライブコーディング〜Strix Cloud × Unity〜 - YouTube](https://www.youtube.com/watch?v=VosQHRh9diI)
        - [SkyWay | アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK](https://webrtc.ecl.ntt.com/)
            - [WebRTC Gateway | ドキュメント | SkyWay](https://webrtc.ecl.ntt.com/documents/webrtc-gateway.html#%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9)
            - [WebRTCに必要な4つのサーバー(シグナリングサーバー、STUNサーバー、TURNサーバー、SFUサーバー)をAPI経由で利用できる | SkyWay](https://webrtc.ecl.ntt.com/skyway/overview.html#%EF%BC%94%E3%81%A4%E3%81%AE%E3%82%B5%E3%83%BC%E3%83%90)
            - 使用例に「VRゲームの通信部分をWebRTCで実装する」とある
            - ホントにUnityで利用できるのか気になる
    
    - 自前リレーサーバーが必要になる場合がある
        - [vis2k/Mirror: #1 Open Source Unity Networking Library](https://github.com/vis2k/Mirror)
            - OSSなのでコードが読める
            - ローカルネットワークならリレーサーバは必要ない
- Dedicated Server的な役割をする
    - [Firebase Realtime Database | Firebase Documentation](https://firebase.google.com/docs/database?authuser=0)
        - RealTimeDatabaseとあるのでXR向けで位置同期できるのか気になる
        - Pub/Subモデル
### 通信用ライブラリ
トランスポート層のTCPやUDPを扱いやすくするライブラリ、LLAPI(LowLevelAPI)に
リアルタイム通信エンジン、NAT越えの実装や、リレーサーバーの実装が別に必要になりそうなもの、
- Listen Server 方式で利用するタイプ
    - [RevenantX/LiteNetLib: Lite reliable UDP library for Mono and .NET](https://github.com/RevenantX/LiteNetLib)
        - [LiteNetLib #features](https://revenantx.github.io/LiteNetLib/index.html#features)
        - .NET用のRUDPを扱うライブラリ
        - NAT越えはどうやって実現するのか？
- Dedicated Server方式で実装する場合に利用
    - [sta/websocket-sharp: A C# implementation of the WebSocket protocol client and server](https://github.com/sta/websocket-sharp)
        - UnityでWebSocket Protocolを扱えるようにしたUnityAsset
    - [gRPC](https://grpc.io/)
        - 言語に依存しないRPC(RemoteProcedureCall)フレームワーク
    - [Cysharp/MagicOnion: Unified Realtime/API framework for .NET platform and Unity.](https://github.com/Cysharp/MagicOnion)
        - gRPCをベースとしたHTTP/2のRPCストリーミングフレームワーク

    - [chkr1011/MQTTnet](https://github.com/chkr1011/MQTTnet)
        - [メタバースプラットフォーム cluster（クラスター）](https://cluster.mu/)は2018/12の時点で[m2mqtt](https://github.com/eclipse/paho.mqtt.m2mqtt)を使っている
            - [MQTTnet を Unity で使う - Qiita](https://qiita.com/johnson65t/items/230360b4cec41e8aafa4)

### プロトコル
リアルタイム系の通信用ライブラリに関連するプロトコルを並べた

- [WebSocket - Wikipedia](https://ja.wikipedia.org/wiki/WebSocket)
    - TCP上で双方向通信を可能にしたHTTPと互換性のあるプロトコル。443番ポートと80番ポート上で動作する
- [MQ Telemetry Transport - Wikipedia](https://ja.wikipedia.org/wiki/MQ_Telemetry_Transport)
    - TCP/IPによるPub/Sub型データ配信モデルのプロトコルでIoT機器向けで使われることがある。
    - [MQTT プロトコルの概要](https://docs.kii.com/ja/guides/thingifsdk/non_trait/mqtt/protocol_overview/)
- [HTTP/2 - Wikipedia](https://ja.wikipedia.org/wiki/HTTP/2)
- [Transmission Control Protocol - Wikipedia](https://ja.wikipedia.org/wiki/Transmission_Control_Protocol)
- [Reliable User Datagram Protocol - Wikipedia](https://ja.wikipedia.org/wiki/Reliable_User_Datagram_Protocol)
    - [詳解 Reliable UDP - Speaker Deck](https://speakerdeck.com/fadis/xiang-jie-reliable-udp)
- [User Datagram Protocol - Wikipedia](https://ja.wikipedia.org/wiki/User_Datagram_Protocol)
    

- 将来のプロコル
    - HTTP/3
    - Quick
    - WebTransport

### シリアラライズ
送りたいデータを人が読める形(json等)ではなく、バイナリでおくることで、通信効率を高まることができる。

- [Protocol Buffers  |  Google Developers](https://developers.google.com/protocol-buffers)
- [neuecc/MessagePack-CSharp: Extremely Fast MessagePack Serializer for C#(.NET, .NET Core, Unity, Xamarin). / msgpack.org[C#]](https://github.com/neuecc/MessagePack-CSharp)
    - C#向けの[MessagePack](https://msgpack.org/)
    - バイナリシリアリゼーションフォーマット


# マルチプレイゲームを作ってみたい
## 難易度低め
- [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
- [Monobit Unity Networking 2.0 (MUN) - モノビットエンジン公式サイト](http://www.monobitengine.com/mun/)
- [Strix Unity SDK 1.4 ドキュメント](https://www.strixengine.com/doc/unity/guide/ja/index.html)
    - [Strixアクションゲームのサンプル — Strix Unity SDK 1.4 ドキュメント](https://www.strixengine.com/doc/unity/guide/ja/unitysdk/sample.html)
    - [わずかな時間でオンラインアクションゲームをつくるライブコーディング〜Strix Cloud × Unity〜 - YouTube](https://www.youtube.com/watch?v=VosQHRh9diI)
## 難易度高め
### ListenServer型
- Mirror + LitNetLib

### DedicatedServer型
サーバーは土管の役割をすることが多い
- WebSocket + protobuf
- gRPC + protobuf
- MajicOnion + Messagepack

## 難易度分からないけど気になる
- [WebRTC Gateway | ドキュメント | SkyWay(アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK)](https://webrtc.ecl.ntt.com/documents/webrtc-gateway.html#%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9)
- [Firebase Realtime Database  |  Firebase Documentation](https://firebase.google.com/docs/database?authuser=0)
    - XRの位置同期につかえるのか気になる
- [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
- [chkr1011/MQTTnet](https://github.com/chkr1011/MQTTnet)



# 参考にしたリンク集のリンク
- [リアルタイム技術調査のスクラップ](https://zenn.dev/toutou/scraps/3749a4e062a549)
- 
