---
title: "UnityのXRで使いたいリアルタイム技術・サービスの整理"
emoji: "🎉"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["XR","Unity","リアルタイム","マルチプレイ"]
published: true
---
# 概要
[tou_tou（とうとう)](https://twitter.com/__tou__tou)と申します。
[Iwaken Lab.「好きな技術・コト」のカレンダー | Advent Calendar 2021 - Qiita](https://qiita.com/advent-calendar/2021/iwakenlab)の12日目の記事です。

UnityのXRで使いたいオンラインマルチプレイゲーム等の「マルチ」を支える「リアルタイム」技術を整理してみました。

動機としては、リアルタイム技術を勉強したいが、一体何から触ればいいのかが分からないので、リアルタイム、ネットワーク、マルチプレイゲームで調べ挙げて出てきた用語を整理して、個人的にさわってみたいものを探そうというものです。
個人のメモ的な側面が大きいです。

同じような事をされている先人の方々がおられるので、おすすめ記事として最初に紹介させて頂きます。
- [Unity リアルタイムネットワークメモ - フレームシンセシス](https://framesynthesis.jp/tech/unity/network/)
- [Unityのゲーム向けクライアント・サーバ・ネットワーク関連覚書 - Qiita](https://qiita.com/s_ryuuki/items/3663cda16cdfa5f14ad7)
- [今、Unityでネットワークマルチプレイ作るのに何を使えばいいのか – soy-software](https://soysoftware.sakura.ne.jp/archives/1686)
- [VR/ARで使いたいUnity対応マルチプレイゲーム用BaaSを比較する | by Takashi Miwa | Kadinche Engineering | Medium](https://medium.com/kadinche-engineering/unity%E5%AF%BE%E5%BF%9C%E3%83%9E%E3%83%AB%E3%83%81%E3%83%97%E3%83%AC%E3%82%A4%E7%94%A8baas-1c5d1861c335)



# この記事の目的

- リアルタイム技術を勉強するための取っ掛かりとする
- 様々なレイヤーのリアルタイム技術選定の助けとなる
- リアルタイム技術を触ってみたい、勉強してみたい人向けのインデックス
- どのサービス、ミドルウェア、ライブラリを触ってみようとしたときの難易度の感覚を掴む

# リアルタイム技術・サービスの整理
はじめに、ネットワーク方式について紹介し、BaaS (Backend as a service)、 Unityで利用可能なな通信ミドルウェア、リアルタイム通信用ライブラリ、プロトコル、データシリアライズ毎にサービス・技術を並べています。

## ネットワーク方式
クライアント同士が繋がるためにはどんな方式があるか

- Dedicated Server 方式
    - [Dedicated Server（専用サーバー） - フレームシンセシス](https://framesynthesis.jp/tech/unity/network/#dedicated-server%E5%B0%82%E7%94%A8%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC)
    - Pub/SubモデルやROOMモデル
        - この記事がとても分かりやすかったです。[ソーシャルゲームを支える「リアルタイムサーバー」の作り方 - ログミーTech](https://logmi.jp/tech/articles/322569)
        - clusterのリアルタイムサーバー構成はpubsubベース (2021/12) [thara](https://thara.dev/posts/cluster-advent-calendar-day24/)
    - スター型のネットワークが多め
 - Listen Server 方式
    - [Unity リアルタイムネットワークメモ - フレームシンセシス](https://framesynthesis.jp/tech/unity/network/#listen-server)
    - 実質的にはP2P的なクライアント同士での通信になるが、NAT越えのためのリレーサーバーを経由することが多い
    - メッシュ型のネットワーク多め

## BaaS (Backend as a service)
バックエンドのインフラや様々なアウトゲーム的な機能を提供するサービス。アクセス数に応じたサーバーのスケーリングや、認証機能、適切なマッチメイキングや、プレイヤーデータ分析機能、マルチプラットフォーム機能などを提供する。
BaaS (Backend as a service)と呼ばれるサービスを含む。PhotonやMonobitよりも豊富なバックエンドサービスを提供しているサービスを並べました。


- サービスの例
    - [Amazon GameLift（最上級クラスの専用ゲームサーバーホスティング）| AWS](https://aws.amazon.com/jp/gamelift/)
        - サーバーカスタマイズが可能
    - [GAMESPARKS](https://www.gamesparks.com/)
    - [PlayFab | Microsoft Azure](https://azure.microsoft.com/ja-jp/services/playfab/#overview)
    - [Unityゲーミングサービス | Unity](https://unity.com/ja/solutions/gaming-services)
        - [Multiplay の専用ゲームサーバーホスティング | Unity の Multiplayer Services](https://unity.com/ja/products/multiplay)
        - [Unity - NetCode](https://unity.com/ja/products/netcode)
            - [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
    - [Steamworks](https://partner.steamgames.com/)
        - [マルチプレイヤー (Steamworks ドキュメント)](https://partner.steamgames.com/doc/features/multiplayer?l=japanese#gameservers)
    - [マルチプレイヤー機能の概要: Unity | Oculus開発者](https://developer.oculus.com/documentation/unity/ps-multiplayer-overview)
    - [Epic Online Services (EOS) の概要 | Epic Online Services ドキュメンテーション](https://dev.epicgames.com/docs/services/ja/GameServices/Overview/index.html)
      - [Epic Online Services が Unity と Unreal Engine 向けのプラグインをリリース - Epic Online Services](https://dev.epicgames.com/ja/news/epic-online-services-releases-plugins-for-unity-and-unreal-engine)


## Unityで利用可能なな通信ミドルウェア
ネットワークエンジン、通信エンジン、ネットワークライブラリと言われていそうなミドルウェア。Unityクライアントの実装のみで通信の完結が可能なミドルウェアを並べています。
Server方式が適切かどうかは保障できませんが、分類するとしたらこんな感じかなという印象です。

- Listen Server方式
    - リレーサーバーを提供 
      - サーバー側をそんなに考えなくてもいい。
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
        - [Normcore. Seamless multiplayer for Unity.](https://normcore.io/)
          - [Documentation | Normcore.](https://normcore.io/documentation/)
          - TrasnportにWebRTCが使われている
          - ListenServer方式といっていいのか疑問
          - [Normcore Private](https://normcore.io/normcore-private)はDedicatedServerを提供
        - [Diarkis](https://diarkis.io/ja)

    - 自前リレーサーバーが必要になる場合
        - [vis2k/Mirror: #1 Open Source Unity Networking Library](https://github.com/vis2k/Mirror)
            - オープンソースなのでコードを読んで勉強できる
            - LAN内ならリレーサーバは必要ない
            - Transport層をカスタマイズできる
                - WebRTCのDataChannelの利用：[WebRTCのDataChannelを使ってUnityでリアルタイム通信するための仕組みを作る](https://zenn.dev/5ena/articles/184f208f7a1d03e1d876)
                - [Noble Connect | Unity Asset Store](https://assetstore.unity.com/packages/tools/network/noble-connect-140535)の利用：[2019年における個人開発あるいは小規模開発のUnityリアルタイムネットワークの技術選定 - izm_11's blog](https://izm-11.hatenablog.com/entry/2019/05/03/204813)
- Dedicated Server方式
    - 自前で書くことが多い？
    - HeadLessUnityサーバー
      - [vis2k/Mirror: #1 Open Source Unity Networking Library](https://github.com/vis2k/Mirror)
        - NAT越えが必要になる
    - [Normcore Private](https://normcore.io/normcore-private)
    
## リアルタイム通信用ライブラリ
トランスポート層のTCPやUDPを扱いやすくするライブラリやフレームワークを並べた。

- Listen Server 方式で利用するタイプ
    - [RevenantX/LiteNetLib: Lite reliable UDP library for Mono and .NET](https://github.com/RevenantX/LiteNetLib)
        - [LiteNetLib #features](https://revenantx.github.io/LiteNetLib/index.html#features)
        - Mirror向けのLiteNetLib:[MichalPetryka/LiteNetLib4Mirror: LiteNetLib based transport for Mirror](https://github.com/MichalPetryka/LiteNetLib4Mirror)
        - .NET用のRUDPを扱うライブラリ
        - NAT越えはどうやって実現するのか？
    - [Noble Connect | ネットワーク | Unity Asset Store](https://assetstore.unity.com/packages/tools/network/noble-connect-140535)
        - Mirror向けでNAT越え用のリレーサーバーを提供
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
            - [aws-summit-2020-fixed](https://pages.awscloud.com/rs/112-TZM-766/images/CUS-64_AWS_Summit_Online_2020_cluster-inc.pdf)
            - clusterがどのAWSのMQTTサーバーを使ってるかは分からなかった。これか？[Amazon MQ（ActiveMQ 向けマネージド型メッセージブローカーサービス）| AWS](https://aws.amazon.com/jp/amazon-mq/?amazon-mq.sort-by=item.additionalFields.postDateTime&amazon-mq.sort-order=desc)
- その他
  - [WebRTC](https://webrtc.org/)
    - [WebRTCの基礎 - panda's tech note](https://ja.tech.jar.jp/webrtc/basics.html)


## プロトコル
リアルタイム系の通信用ライブラリに関連するプロトコルを並べた。

- [WebSocket - Wikipedia](https://ja.wikipedia.org/wiki/WebSocket)
    - TCP上で双方向通信を可能にしたHTTPと互換性のあるプロトコル。443番ポートと80番ポート上で動作する
- [MQTT(MQ Telemetry Transport) - Wikipedia](https://ja.wikipedia.org/wiki/MQ_Telemetry_Transport)
    - TCP/IPによるPub/Sub型データ配信モデルのプロトコルでIoT機器向けで使われることがある。
    - [MQTT プロトコルの概要](https://docs.kii.com/ja/guides/thingifsdk/non_trait/mqtt/protocol_overview/)
- [HTTP/2 - Wikipedia](https://ja.wikipedia.org/wiki/HTTP/2)
- [TCP(Transmission Control Protocol) - Wikipedia](https://ja.wikipedia.org/wiki/Transmission_Control_Protocol)
- [RUDP(Reliable User Datagram Protocol) - Wikipedia](https://ja.wikipedia.org/wiki/Reliable_User_Datagram_Protocol)
    - [詳解 Reliable UDP - Speaker Deck](https://speakerdeck.com/fadis/xiang-jie-reliable-udp)
- [UDP(User Datagram Protocol) - Wikipedia](https://ja.wikipedia.org/wiki/User_Datagram_Protocol)
    

- 将来のプロコル
    - HTTP/3
    - Quick
    - WebTransport
    - 上3つに関して、とても分かりやすい記事です。[WebSocketの次の技術！？WebTransportについての解説とチュートリアル - Qiita](https://qiita.com/yuki_uchida/items/d9de148bb2ee418563cf)

## シリアライズ
送りたいデータを人が読める形(json等)ではなく、バイナリでおくることで、通信効率を高めることができる。

- [Protocol Buffers  |  Google Developers](https://developers.google.com/protocol-buffers)
- [neuecc/MessagePack-CSharp: Extremely Fast MessagePack Serializer for C#(.NET, .NET Core, Unity, Xamarin). / msgpack.org[C#]](https://github.com/neuecc/MessagePack-CSharp)
    - C#向けの[MessagePack](https://msgpack.org/)



# 検証・小規模向けにマルチプレイゲームを作ってみたい
## 難易度低そう
クライアントの実装が必要。リレーサーバーが必要な場合は、それぞれのサービスのコンソールで準備する。
- [PUN2 | Photon Engine](https://www.photonengine.com/ja-JP/PUN)
- [Monobit Unity Networking 2.0 (MUN) - モノビットエンジン公式サイト](http://www.monobitengine.com/mun/)
- [Strix Unity SDK 1.4 ドキュメント](https://www.strixengine.com/doc/unity/guide/ja/index.html)
    - [Strixアクションゲームのサンプル — Strix Unity SDK 1.4 ドキュメント](https://www.strixengine.com/doc/unity/guide/ja/unitysdk/sample.html)
    - [わずかな時間でオンラインアクションゲームをつくるライブコーディング〜Strix Cloud × Unity〜 - YouTube](https://www.youtube.com/watch?v=VosQHRh9diI)
## 難易度高そう
### ListenServer型
- [Mirror](https://github.com/vis2k/Mirror) + [LiteNetLib4Mirror](https://github.com/MichalPetryka/LiteNetLib4Mirror)
    - LAN内で使う場合

### DedicatedServer型
サーバーとクライアントの両方の実装が必要になる。
- HeadLessUnityサーバー方式
    - [Mirror](https://github.com/vis2k/Mirror) + [LiteNetLib4Mirror](https://github.com/MichalPetryka/LiteNetLib4Mirror)
        - NAT越えの問題あり
    - [Mirror](https://github.com/vis2k/Mirror) + [Noble Connect | ネットワーク | Unity Asset Store](https://assetstore.unity.com/packages/tools/network/noble-connect-140535)
        - リレーとNAT越えが可能
- 土管サーバー方式
    - WebSocket + protobuf
    - MajicOnion + MessagePack
    - gRPC + protobuf
    - [リアルタイムサーバー 〜Erlang/OTPで作るPubSubサーバー〜](https://www.slideshare.net/yugoshimizu/erlangotppubsub-63215899)

## 難易度分からないけど気になる
- [WebRTC Gateway | ドキュメント | SkyWay(アプリやWebサービスに、ビデオ・音声通話をかんたんに導入・実装できるSDK)](https://webrtc.ecl.ntt.com/documents/webrtc-gateway.html#%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9)
- [Firebase Realtime Database  |  Firebase Documentation](https://firebase.google.com/docs/database?authuser=0)
    - XRの位置同期につかえるのか気になる
- [Netcode for GameObjectsを使ってみよう - Unityステーション - YouTube](https://www.youtube.com/watch?v=GRUtGLL8iMQ)
- [chkr1011/MQTTnet](https://github.com/chkr1011/MQTTnet)


# 参考にしたリンクのリンク集
- [リアルタイム技術調査のスクラップ](https://zenn.dev/toutou/scraps/3749a4e062a549)

# 今後の予定
- PubSubモデルについてTopicモデルではなく高頻度通信向けがあることを付け足す
    - https://twitter.com/izm/status/1470063502299922433?s=20
- この記事に図を追加する
- 触ってみたい技術・サービスを使ってアレのデモをつくる
