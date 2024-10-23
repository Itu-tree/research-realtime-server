---
title: "家の電力を測定したい"
emoji: "🙆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# 概要


# 用語
- [電力メーター情報発信サービス（Bルートサービス）｜電力自由化への対応｜東京電力パワーグリッド株式会社](https://www.tepco.co.jp/pg/consignment/liberalization/smartmeter-broute.html)
- [Wi-SUN | 無線とは？ | エレクトロニクス豆知識 | ローム株式会社 - ROHM Semiconductor](https://www.rohm.co.jp/electronics-basics/wireless/wireless_what4)
- [ローム Wi-SUN対応無線モジュール｜チップワンストップ - 電子部品・半導体の通販サイト](https://www.chip1stop.com/sp/products/rohm_wi-sun-module)
- 

# 技術選定

1. Nature Remo Lite を使う
2. RaspPi + Wi-SUN  + USB付きWi-SUNモジュール
   1. 取得したデータをどこかに投げるのが楽そう
3. M5StickC/Plus + HAT キット + Wi-SUNモジュール
   1. 設置予定場所とAPの距離が遠く通信が不安（自宅の環境依存ではある）

コスパや安定性使い勝手を考えると2とした


# 必要な機材
- RasPi
- 

# 方法



# 参考文献
- [How to Use Momonga – ビットログ](https://blog.bitmeister.jp/?p=5342)
- [スマートメーターから電力消費量を取得する #Python - Qiita](https://qiita.com/false-git@github/items/ebcaaae4d54f2393efa7)
- [ローム Wi-SUN対応無線モジュール｜チップワンストップ - 電子部品・半導体の通販サイト](https://www.chip1stop.com/sp/products/rohm_wi-sun-module)
- [電力メーター情報発信サービス（Bルートサービス）｜電力自由化への対応｜東京電力パワーグリッド株式会社](https://www.tepco.co.jp/pg/consignment/liberalization/smartmeter-broute.html)
- https://amzn.asia/d/5WtNyQL
- [自宅の消費電力をリアルタイムに グラフ化してみた | PPT](https://www.slideshare.net/slideshow/ss-231002401/231002401)
- [Bルートやってみた - Skyley Official Wiki](https://www.skyley.com/wiki/?B%E3%83%AB%E3%83%BC%E3%83%88%E3%82%84%E3%81%A3%E3%81%A6%E3%81%BF%E3%81%9F#b82f0466)