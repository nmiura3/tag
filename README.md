# Toolbox Accelerating Glycomics (TAG) version 5.4 manual
## TAG リリース v5.4
2023.3.22更新

## 5.4 が現在の最新版です.
- TAGで読み込めるエクセルファイルは"xlsx"です. xlsのマスリストの場合は一回開いてxlsxで保存直してください.
- 5.4は現在windows版のみが提供されています.
- 糖鎖リストの糖鎖数は上限4000まで, サンプル数（実験数, Masslistのタブ数）は50に限定されています.ご連絡頂ければ多少の変更はできますが, gfortranの都合で静的に確保できるメモリが限られているのでプログラムの仕様を変更すべく検討中です. 血清の大規模解析に資するために糖鎖リストの上限を800に限定しつつ, 質量分析の1プレート分384サンプルを一気に解析できるロードモジュールも揃えましたが, こちらはまだ未テストなので公開していません. ご興味ある方はご連絡ください.
- ファイル名や途中のフォルダ名に「スペース」はなるべく使わないで下さい. ハウスウェアを使う際の常識だと思っていたのですが, そうでもないようなので明記しておきます.

## 1.  変更点
バージョン5.4では基本的に5.3と同様に動作させますが, 糖鎖解析の安定性と信頼性にかかる機能改善を行いました. 5.3.2bのreadmeは[こちら](https://github.com/nmiura3/tag/blob/master/README_v5.3.2b.md)

- サマリーファイルにおいてlinked glycanの表示を帰属した糖鎖の組成から見た際に糖残基が増加するものと減少するものに分けて表示するようにしました. また帰属した糖鎖から見て+/-2つまでの残基を示すようにしました. これはどの同位体が確からしいかを議論する際に発現の流れをより見やすくするためです. 将来的にすべてのlinked glycanを表示させるオプションを用意する計画です. これによって2022年のIJMSに書いたように生合成経路を追跡する事も視野に入れています.
- 帰属の際に理論的m/zとΔm/zの散布図からクラスターを構築することで糖鎖帰属を行っていますが, どのクラスターを選ぶかによって結果が変わってきます. これはキャリブレーションの精度にも強く依存します. どのクラスターを選ぶかについていくつかのオプションを用意しました. plate fileを用意する際に1行目のDカラムに入れる整数で制御します. オプションは以下の通りです.

|option|name|description|
|:---|:---|:---|
|1|no clustering| クラスタリングしない. toleranceで指定した範囲に収まる質量の糖鎖を抽出する|
|2|range|理論的m/zについて最も広範囲に分布するクラスターを取得する|
|3|internal standard|内部標準を含むクラスターを抽出. このオプションを選択する場合は, 内部標準について入力する2行目以外に, 内部標準の組成をリスト内に含む必要がある.|
|9|maxmum cluster|最も多くの要素を含むクラスターを取得する|
<br>

- 出力ファイルでtotal glycanの量の表示がおかしかったissueを修正しました. 大丈夫なはずです. ただし, 異性体をダブルカウントしていますのであまり使い物にならないと考えています. この辺は今後のバージョンアップで対応していきます.

## 2. 概要
- windows版 tag_v5.4_win.zip
  - MS windows10 pro 20H2にて動作確認.
  - 開発は mingw上のgcc/fortranで行い静的実行ファイルを生成
- Mac版 tag_v5.3.2_M1mac.zip
  - [for_mac.md](https://github.com/nmiura3/tag/blob/master/for_mac.md)を参照してください.  いくつかアプリケーションをインストールする必要があります.


TAGについての詳細は以下の論文にあります

- [Toolbox Accelerating Glycomics (TAG): Glycan Annotation from MALDI-TOF MS Spectra and Mapping Expression Variation to Biosynthetic Pathways,  Biomolecules 2020, 10(10), 1383; https://doi.org/10.3390/biom10101383  ](https://www.mdpi.com/2218-273X/10/10/1383)
- [Toolbox Accelerating Glycomics (TAG): Improving Large-Scale Serum Glycomics and Refinement to Identify SALSA-Modified and Rare Glycans,  Int. J. Mol. Sci. 2022, 23(21), 13097; https://doi.org/10.3390/ijms232113097](https://www.mdpi.com/1422-0067/23/21/13097)

TAGを用いて解析を行った場合は上記論文を参照して頂ければ幸いです.

またPathwaySALSA拡張については[DrawGlycan](http://www.virtualglycome.org/DrawGlycan/)を用いています.

[DrawGlycan-SNFG: a robust tool to render glycans and glycopeptides with fragmentation information, Glycobiology, Volume 27, Issue 3, March 2017, Pages 200–205](https://doi.org/10.1093/glycob/cww115)

リリースのところからダウンロードしてください. 基本的にフォルダを解凍してできるTAGのフォルダをどこかにおいて, win版であればフォルダ内のtag.batを, mac版であればtag.commandをダブルクリックする事で起動できます.

TAGは以下のソフトを活用して動作しています. 
- [exceltoCSV](https://www.zamzar.com/convert/xls-to-csv/): 複数タブのエクセルフォーマットで出力されたmasslistを複数のCSVに分割するためのソフト
- [tcl/tk 8.6](https://www.tcl.tk/software/tcltk/8.6.html) GUIとコマンドドライバ
- [gawk 4.2.1](https://www.gnu.org/software/gawk/) awkスクリプト実行用
- [gnuplot 5.2](http://www.gnuplot.info/index.html) グラフ描画
#- [cluster 3.0](http://bonsai.hgc.jp/~mdehoon/software/cluster/software.htm#ctv) クラスター解析

TAGは, glycoblotting法などによって得られた質量分析スペクトルデータから糖鎖の発現量を得る, 系列間の平均や検定など主に発現量に関する解析を行うためのTAG_expと, 発現結果を糖鎖の生合成経路に配置, 可視化する事によって糖鎖発現と投薬に対する応答や疾患などの生命現象に対する糖鎖が果たす役割を見出す支援としての一連の可視化・マイニングツールであるTAG_viewからなっています.

<img src="image/tag_overviewJ.png" alt="center" width="480"/>

現在のバージョンでは, N-glycanとN型遊離糖鎖の解析を行う事ができます. そのための実行ファイルとマップ, 糖鎖リストを含んでいます. GSLについては現在開発中であり, おまけ程度の出来栄えです.

TAGの実行に必要なプログラム群は, tag_v5.4_winまたはtag_v5.3_M1macというフォルダに収められています.これは配布されるファイルを展開する事によって得られます.windows版ではtcl/tk処理系, gnuplotなどのツールも含んでいます.このフォルダをどこかにコピーして使います. mac版ではこれら必要なツールはhomebrewを用いてインストールします. homebrewについては好みもあるかと思いますが, できるだけ実行環境をそろえるという意味合いがありますのでご理解ください. ご自分で環境を構築頂いても結構です.

TAGは, tag.bat(mac版ではtag.command)というバッチファイルをダブルクリックすることで起動します.このバッチファイルへのショートカットをデスクトップなどに作っても大丈夫のようです.（ファイル・フォルダ等の配置について, 空白を含むフォルダ名や日本語のフォルダ名は試していません.空白を含まず日本語のフォルダ名を用いていないパスに置くことを推奨します.）

<img src="image/tag_folder_tag_bat_5.3.png" alt="drawing" width="480"/>

すると, tag起動画面, 小さなメニューが出現します.

<img src="image/title_5.3_2.png" alt="drawing" width="200"/>

上半分はそれぞれの処理を行うためのボタンで, 下半分にはtagが置かれているフォルダやリストファイルなどの情報が出力されます. それぞれボタンを押すことによってそれぞれプログラムが実行され処理が行われます.立ち上げたばかりの時は, tagが置かれているフォルダ以外のデータフォルダやリストファイルは未選択ですので空欄になっています.処理が実行されるとこれらは表示されます.TAG_Pathwayについて, N-glycanとNFGについてはグラフを貼り付けたタイプと, 発現量の増減を示したタイプの2種類が出力されます.GSL(SALSA)の場合は現在棒グラフを貼り付けたタイプのみ動作します. 5.3にバージョンアップするにあたり糖鎖組成のIDを大幅に変更したため現在このTAG_pathwayは動作しません. 対応しだいこの場でお知らせいたします.

機能としては下記の表にあるものがあります.
| bottun| description of function |
| :-------------------- | :----- |
|make input file<br>from MassList and plate file|質量分析データMassList.xlsxファイルと次節で解説するプレートファイルを用いて検量線(calibration line)および実際の解析(analysis)用のデータを自動で作成します. MassListと同じディレクトリにMassListフォルダを作成しその中にプレートファイルの記述に応じてcalibration_lineとanalysisというフォルダを作成しそれぞれのTAGexpression用のインプットファイル格納します.|
|run TAG_expression<br> (calibration line)|検量線用のTAGexpressionを実行します.|
|run TAG_expression<br> (actual analysis)|実際の解析用のTAGexpressionを実行します. <font color="yellow"> 従来のようにインプットを作成した場合はこのボタンで実行してください.</font>|
|TAG_Pathway<br>(N-glycan 15dig)|N-glycan用のTAG_Pathwayを実行します.15桁の糖鎖IDに対応しました. MTT修飾用です. シアル酸はアセチルのみに対応しています.|
|TAG_pathway<br>(N-glycan 15dig SALSA)| SALSA対応したTAG Pathwayを実行します. 現在マップは2本差のみです. 中性糖鎖については上記従来のTAG_Pathwayとマップを共有しています|
|Convert a excel MS file to multi csv| MassListをマルチcsvに変更するボタンです. これまでは同様の機能を持つフリーソフトを同梱していましたが自前で機能を実装しました.|
|exit|TAGを終了します.


## 3. プレート情報ファイルの準備

本バージョンでは, 血清の大量の解析に対応すべくMALDIの384スポットのそれぞれにどのような測定が対応するかをエクセルファイルに記述することでインプットを自動で作成する機能をもっています.

masslistを用意したら, すべてのタブの2行目に内部標準の量やtoleranceなど各種条件を入力します. 各セルに入力する量は以下の通りです.  従来版で入力していたD2, E2に対応する情報を入力するデータをプレート情報から得ます.

| cell | data and description |
| :---- | :----- |
| A1 | 内部標準の量(ug) |
| B1 | tolerance (default 2) |
| C1 | タンパク量(ug)この数値を基にタンパク質100ugあたりの糖鎖量に変換される.血清などの場合でタンパクが無い場合は100を入れておけば数値の変更が無い |
| D1 | m/zとΔm/zによる分布をクラスター分析しどのクラスターを取得するかについてのフラグ. 内容は上述|
<br>
  
<img src="image/plate_information.png" alt="center" width="480"/>

上図はプレート情報を入力するエクセルファイルの例になります. 検量線を引く際に用いるスポットについては何番目の実験に対応するかの整数と乗せた量をアンダースコア'_'で区切って入力します. 解析に用いる場合は実験の番号とラベルをシャープ'#'で区切って入力します.
タンパク量などをスポットごとに変えたい場合はcsvを作成したのちに, そちらを編集してください.

## 4.エクセルからCSVへコンバート

TAG_expでは, 質量分析の結果についてcsv(コンマ区切り)ファイルでの入力を前提にしています.この入力を作成するために, MassListとプレート情報ファイルplate1.xlsxを用意して下図一番上のボタン（赤枠）をクリックして変換を行います. クリ九すると最初にplateファイルを選択するセレクター, 次にMassListを選択するセレクターが出ますのでそれぞれファイルを選択して開くをクリックして進めてください下さい. 

<img src="image/select_masslist.png" alt="center" width="200"/>
<img src="image/select_plate.png" alt="center" width="200"/>



MassListファイルがあるフォルダにMassListファイル名から'.xlsx'を除いた名前のフォルダを作成し, さらに検量線用のcsvファイルはその中のcalibration_lineというフォルダに, 解析用のcsvファイルはanalysisという名前のフォルダに出力されます. これらをTAG expressionの解析に用いることになります.

MassFileのタブの名前に対して'_0_'と'_1'で囲まれた文字列をプレート情報として取得していますのでBruker以外の質量分析機器の場合はご相談ください. スクリプト変更で対応させて頂きます.

## 5.TAG expressionの実行
TAGのタイトル画面

<img src="image/title_5.3.png" alt="center" width="480"/>

において, 二番目のボタン **runTAG_Expression (calibration line)** がN-glycanとFNGの検量線を引くプログラムを走らせるもので, 三番ののボタン **runTAG_Expression (analysis)** がN-glycanとFNGの検量線を引くプログラムを走らせるものです.バージョン5.2と同様に, これらのボタンを押すと糖鎖リストを入力するための選択画面が出ますのでリストを選択ます. LIstファイルはListを生成するためのawkスクリプトと一緒にlistディレクトリに格納されています. N-glycanであれば **N_list_auto.csv**をFNGであれば **F_list_auto** をお使いください. 生成した糖鎖リストにに独自に糖鎖を追加する事も可能です. またスクリプトを変更して異なるリストを生成する事も可能です. リストは糖鎖構造（または名前）とm/zおよび糖鎖タイプをカンマで区切るCSV形式で保存してださい.

SALSAやなどにも対応したので以下に糖鎖リストの整理をしておきます.
|ファイル名|N/F/G| 内容|
|:-----|:-----|:----|
|N_list_2008_BOA.csv|N-glycan|MTT/BOAラベルNa addact|
|N_list_SALSA_BOA_full_221129_noGlcA.csv|N|SALSA BOAラベル, GlcA含まず|
|N_list_SALSA_BOA_full_221129_noNeuGc_noGlcA_sorted_by_mass.csv|N|SALSA BOAラベル GlcAおよびNeuGc含まず, 質量順に並べ替え |
|N_list_SALSA_BOA_full_221129.csv|N|SALSA BOAすべて含む|
|N_list_SALSA_aowr_full_221122.csv|N|SALSA aoWRラベル すべて含む|
|N_list_SALSA_aowr_full_221122_noGlcA.csv|N|SALSA aoWRラベル GlcA含まず｜
|N_salsa_glcA_MA_BOA2d.cav|N|SALSA/BOAラベルNa adduct/GlcAはMAラベル|
|N_salsa_glcA_iPA_BOA2d.csv|N|SALSA/BOAラベルNa adduct/GlcAはいPAラベル|
|N_salsa_glcA_aoWRd2.csv|N|MTT/aoWRラベル/GlcAフリー|
|N_list_auto.csv|N|MTT/aoWRラベル|
|F_list_auto.csv|F|MTT/aoWRラベル|
|g219_salsa_sphingomap3.csv|G|SALSA/aoWR|
|g219_mtt_sphingomap.csv|G|MRR/aoWR|
<br>
その他ファイル名でおおよそのタイプの見当が付くようにしてあります.


また実行についてはバージョン5.2と変わりませんので[readme_5.2.md](https://github.com/nmiura3/tag/blob/master/READM_5.2.md)を参照してください.

出力ファイルは以下の通りです.

| ファイル名 | 内容 ｜
| :------ | :------|
|summary.csv|サマリーファイル, m/z, 組成, linked glycan []ないの番号は糖鎖番号, 観測質量, 発現量(pmol), 発現量の系列ごとの平均, 標準偏差, CV, glyconnectへの検索URLがまとめられたファイル|
|summary_lg2.csv|summaryファイルと同じであるが, linked glycanに対して, 残基が増える方向と減る方向に分けて表示し, 異なる残基数も2まで表示した.|
| out_list.csv | 各リスト内の糖鎖に対して帰属された糖鎖の実測m/z,理論からのずれ,area強度, タンパク量100ugあたりの発現量(pmol).また各系列の平均値, 標準偏差, CV値, t値, p値, また各タイプごとのこれらの集計を収めてあります. |
| exp_list.csv | 上記と同様だが少し省略されたデータが収録されている.糖鎖の発現量, 平均値, 標準偏差, CV値, t値, p値, 各タイプごとの集計が収められています.<font color="red">下の方にタイプ別の集計も掲載していますが, TAGでは可能性のある構造をすべて帰属しているのでダブルカウントしているタイプがあるため, ほんの目安にしかできません.</font> |
| exp_list_zerocut.csv | exp_list.csvからさらにすべての実験について発現量がゼロの糖鎖を削除してコンパクトな表示してあります. exp_list.csvと同様に<font color="red">下の方にタイプ別の集計も掲載していますが, TAGでは可能性のある構造をすべて帰属しているのでダブルカウントしているタイプがあるため, ほんの目安にしかできません.</font>|
| for_pathway_X.csv | TAG_viewでの処理に必要な情報をまとめています.現状ではファイル名は固定で, TAG_expを動作後に実行することを前提としています.Xには, N-glycanの場合はN, FOSの場合はF, O-glycanの場合はO, GSLの場合はGが入ります. |
|each_glycan_quant.html|帰属した糖鎖の発現量を系列ごとの棒グラフにまとめたもの. html形式なので一般のウェブブラウザで閲覧可能 |
|<font color="yellow">each_glycan_quant_point.html</font>|検量線を引くために散布図にまとめたもの. 直線フィッティングを行ってa,b,R^2も出力している. html形式なので一般のウェブブラウザで閲覧可能 |
|<font color="yellow">each_glycan_quant_point_rcut.html</font>|検量線を引くために散布図にまとめたものでR^2>0.9の糖鎖のみを示した. 直線フィッティングを行ってa,b,R^2も出力している. html形式なので一般のウェブブラウザで閲覧可能 |
|calb_ms_value_plot.html|糖鎖を帰属する際に用いるリストにある理論的m/zと実測値の理論からのずれΔm/zの散布図. 図の詳細は論文を参照ください.|
| *.dat1, *.dat2 | 糖鎖を帰属する際に行うクラスタリングの精度をチェックするためのグラフを作成する元データ.tolerance値でピックした糖鎖についての*.dat1は理論m/zと実測のずれで*.dat2はピックした糖鎖の理論m/zと実測値からのずれが収められています. |
| *.png | 上記データとクラスタリングデータから最小二乗法によって引いた直線をプロットしたグラフ.白抜き四角がtoleranceでピックしたデータ, 塗りつぶされた丸がクラスタリングによってピックした糖鎖のデータ. |
| *.eps | 上記のepsバージョン.白黒. |
| *.plt | グラフデータを描画するためのgnuplotスクリプト |
| mkgraph.plt | *.pltをまとめて描画するためのスクリプト |
<br>


## 5.TAG_Pathwayの実行

バージョン5.3で導入した15桁の糖鎖IDに対応したPathwayです. 現在N-glycanのMTTとSALSAにのみ対応しています. SALSAとMTTで使い分けてください. MTTはNeu5Acにのみ対応しています. 使い方は以下の通りこれまでと変わりません. pathway用のcsvファイルを選択し, 出力先を指定するだけです.

TAG Expressionを実行した際にできる **for_pathway_X.csv** と用意されたマップファイルから発現量（変動）を生合成経路へマップする. TAGのメニュ―画面から下4つのボタンがTAG_pathwayの実行に対応している.

| ボタン | 機能 |
| :---- | :---- |
| TAG Pathway (N-glycan) | N-glycanのpathwayを描画する. 発現量の棒グラフをマッピングしたpathwayと発現変動を色(増：赤, 減:青)で示したpathwayの両方を出力する 出力はhtml形式で行われ, pathwayはウェブブラウザで閲覧する事ができる. |
| TAG Pathway (FNG) | N型遊離糖鎖のpathwayを描画する. 出力についてはN-glyucanと同様. |
| TAG Pathway (N-glycan+FNG) |N-glycanとN型遊離糖鎖の混合pathwayを描画する. その他は同じ. 入力の際にN-glycanとFNGの結果ファイルの入力が求められる. |
| TAG Pathway (barchart GSL SALSA) | SALSA法によるGSL解析結果を生合成経路にマップする. 出力は発現量の棒グラフのみ. |

前節で事項したN-glycanの解析結果からTAG Pathwayを実行する. **TAG Pathway (N-glycan)** をクリックする. ファイル選択画面が表示されるので, **for_pathway_N.csv** を選択する. 結果をどこのディレクトに出力するかを次の画面で選択する.
終了するとメッセージが表示される.

<img src="image/tag_pathway_finish.png" alt="center" width="200"/>

TAG Pathwayの出力ファイルは以下の通り
| ファイル名 | 内容 |
| :---- | :----|
|barchart_pathway.html | 生合成経路に発現量の棒グラフをマップ. これを見るとどの糖鎖がどのような変化をしているかが一覧できる. |
|系列A - 系列B.html  (ex. wt- - NPC-.html) | 系列間の発現量増減をマッピングした生合成経路図. これによってt検定的に有意に増加した糖鎖を生合成経路上で一覧可能.|

SALSA用のPathwayを実行した場合には上記ファイル名に中性糖鎖であれば<font color="yellow">Pathway2_N_neutral_</font>2本鎖であれば<font color="yellow">Pathway2_N_1a_1hy_</font>という接頭辞が付きます.


## 6. その他
- exp_list_zero_cut.csvでtotalの表示がおかしい

- おかしな点があれば, ご連絡いただければできる限り対応します. 作者は別に本務があるため時間がかかる場合がありますが, ご連絡いただければ, 見通しも含めできるだけ早くリプライをさせて頂きます.
連絡先: miura.nobuaki3@gmail.com


- 本研究の一部はJSPS科研費 21K12124の助成を受けて進められています.

