# Toolbox Accelerating Glycomics (TAG) version 5.3.2 manual
## M1 Mac版について

2022.1.19
2022.1.10

内容にほぼ変化ないのでv5.3と同じドキュメントを流用しています.


Mac版では, 糖鎖リストの長さを2,000に限定し, 実験数を200としました.
リリースよりtag_v5.3_M1mac.zipをダウンロードして下さい. 以下のアプリケーションをインストールしておけばwindows版と同様に動作するはずです. 不具合などありましたらご連絡下さい.

Windows版は, ロードモジュールを静的に結合して作成していました. よってwindows版はフォルダをコピーするだけで動作しました. Macではこのようなビルドができず（少なくとも開発者の技術力において）必要なライブラリをHOmebrew経由でインストールして動作させます. 以下にその方法について述べます.

## 1.バージョン
インストールすべきソフトウェアのバージョンを記します. 2022年1月の段階でインストールしたバージョンです. 基本的に
```
brew install XXXX(アプリ名)
```
でインストールできます. バージョンが合わない場合はバージョンを指定してインストールしてください.

|ソフトウェア|バージョン|
|:-----|:-----|
|gcc|11.2.0_3|
|tcl-tk| 8.6.12|
|gnuplot|5.4.2|
|gawk|5.1.1|

homebrewのインストールは
https://brew.sh/index_ja


## 2. CSVの準備
v5.3になるにあたり, MassListのXlsxからCSVファイルを生成する機能を追加しました. この機能を用いて, CSVを作成することができます. この機能については[readme.md](https://github.com/nmiura3/tag/blob/master/README.md)を参照してください.

従来通りにCSVファイルを作成して実行する場合は[for_mac_v5.2.md](https://github.com/nmiura3/tag/blob/master/for_mac_v5.2.md)を参照してください.


## 3. Mac版での注意点
- 系列のソートが今一つうまく行っていないようです. 平均値その他統計量は正しいと思っていますが, ソートがうまく行っていないのでグラフ糖のキャプションが変な場合があります. その際にはお手数ですが, CSVファイルがあるフォルダにできるinputというファイルを編集してあらかじめソートしてください. 各系列の最初のショートネームがグラフのラベルに用いられます.
- 糖鎖のIDを変更した事に合わせてN-glycanのPathwayを実装しなおしました.さらにSALSAに対応しました. 他のクラスの糖鎖についても追って進めていきます.