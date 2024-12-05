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


# 使用した機材
- RasPi Model B3
- BP35C2

# 使用したソフトウェア
- Python3.10
  - [nbtk/momonga: MomongaはBルートサービスを利用してスマートメーターと通信するPythonモジュールです](https://github.com/nbtk/momonga)
- VNC Viewer
  - ssh でリモートデスクトップができる

# 方法

## BP35C2 の挙動確認
- [ROHM BP35C2 + Bルート + Net-SNMP + Zabbixで電力監視 – Studio JamPack](https://jamfunk.jp/wp/works/svtools/pwrtb/)

## VNC Viewer のセットアップ

## Python3.13のインストール
2. Python 3.13 をソースからインストール
Debian Bullseye（Raspberry Pi OS）では、Python 3.13 が公式リポジトリで提供されていないため、ソースからコンパイルします。

2.1. 必要な依存パッケージをインストール
```bash
sudo apt update
sudo apt install -y wget build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev \
                    libreadline-dev libffi-dev curl libbz2-dev
```
2.2. Python 3.13 のソースコードをダウンロード
```bash
cd /usr/src
sudo wget https://www.python.org/ftp/python/3.13.0/Python-3.13.0.tgz
sudo tar xzf Python-3.13.0.tgz
cd Python-3.13.0
```
2.3. Python をビルドしてインストール
```bash
sudo ./configure --enable-optimizations
sudo make -j$(( $(nproc) / 2 ))  # 並列ビルドを実行.すべてコアを使うとVNCが切断されるので注意
sudo make altinstall
```
注意: make altinstall を使用すると、python3.10 コマンドとしてインストールされ、デフォルトの python3 を置き換えません。
ビルドに体感1時間ぐらいかかった気がする

2.4. インストール確認
```bash
python3.10 --version
正常にインストールされていれば、Python 3.13 のバージョンが表示されます。
```

4. python3 のデフォルトを Python 3.13 に設定
デフォルトの python3 を Python 3.13.0 に置き換えます。

4.1. 新しいバージョンを登録
update-alternatives を使用して Python 3.13.0 を登録します。

bash
コードをコピーする
sudo update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.13 1
4.2. デフォルトを選択
複数のバージョンが登録されている場合、Python 3.13.0 をデフォルトに設定します。

bash
コードをコピーする
sudo update-alternatives --config python3
リストが表示されますので、Python 3.13 のエントリ番号を入力してください。
5. インストール確認
デフォルトの Python バージョンが Python 3.13.0 になっていることを確認します。

bash
コードをコピーする
python3 --version
出力が以下のようになっていれば成功です。

コードをコピーする
Python 3.13.0

6. pip3 の更新
Python 3.13.0 に対応した pip をインストールまたは更新します。

bash
コードをコピーする
python3 -m ensurepip --upgrade
python3 -m pip install --upgrade pip


pytnonのインストールが思ったよりめんどくさいので python3.13.0の docker image を稼働させた方が楽だったかも...
## Pythonスクリプトの作成
[How to Use Momonga – ビットログ](https://blog.bitmeister.jp/?p=5342)



# 参考文献
- [How to Use Momonga – ビットログ](https://blog.bitmeister.jp/?p=5342)
- [スマートメーターから電力消費量を取得する #Python - Qiita](https://qiita.com/false-git@github/items/ebcaaae4d54f2393efa7)
- [ローム Wi-SUN対応無線モジュール｜チップワンストップ - 電子部品・半導体の通販サイト](https://www.chip1stop.com/sp/products/rohm_wi-sun-module)
- [電力メーター情報発信サービス（Bルートサービス）｜電力自由化への対応｜東京電力パワーグリッド株式会社](https://www.tepco.co.jp/pg/consignment/liberalization/smartmeter-broute.html)
- https://amzn.asia/d/5WtNyQL
- [自宅の消費電力をリアルタイムに グラフ化してみた | PPT](https://www.slideshare.net/slideshow/ss-231002401/231002401)
- [Bルートやってみた - Skyley Official Wiki](https://www.skyley.com/wiki/?B%E3%83%AB%E3%83%BC%E3%83%88%E3%82%84%E3%81%A3%E3%81%A6%E3%81%BF%E3%81%9F#b82f0466)