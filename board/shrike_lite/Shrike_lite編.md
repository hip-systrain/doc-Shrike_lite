<!--
  手順書テンプレート(全ボード共通)
  - このファイルをコピーして、ボードごとのフォルダに保存してから書き始める。
  - 章の番号と名前は変えない。該当しない章は消さずに「本ボードでは作成しない(理由:〜)」と書く。
  - ボード独自の内容は、第9章以降に追加章として書く。
  - このコメントはプレビューには表示されない。消さずに残してよい。
-->

# Shrike-lite 開発手順書

| 項目 | 内容 |
|---|---|
| 対象ボード | Shrike-lite |
| 開発言語 | Python (MicroPython) |
| IDE・ツール | Thonny 5.0.1 |
| SDK・ライブラリ | Shrike MicroPython v1.26.0-preview |
| 開発PCのOS | Windows 11 26H2 |
| 最終検証日 | (yyyy/mm/dd) |

## 改版履歴

| 版 | 日付 | 担当者 | 内容 |
|---|---|---|---|
| 0.1 | (yyyy/mm/dd) | | 新規作成 |

---

## 0. 概要
本章では、本手順書の目的や対象となる開発環境、開発に必要な機材および前提知識について説明する。  
### 0.1 この手順書でできること
<!-- 読者がこの手順書を最後まで進めると何ができるようになるかを書く -->
本手順書は、初めて Shrike-lite を扱う人でも、記載された手順に従って開発環境を構築し、プログラムの作成・実行および基本的な動作確認を行えることができる手順書である。

### 0.2 必要な機材

| 品名 | 数量 | 備考 |
|---|---|---|
| Vicharak Shrike-lite | 1人1台 | 開発するボード |
| USBケーブル(Type-C、データ通信対応) | 1人1本 | 給電・書込み・REPL |
| ブレッドボード・ジャンパワイヤ | 1人1式 | ボタンの配線 |
| タクトスイッチ | 1人1個 | 題材2・3・8で使用 |

### 0.3 前提知識・事前準備
<!-- 既存の Git/GitHub 運用・Redmine 運用を参考に、前提とする知識と準備を書く -->

#### マイコン
CPUやメモリ、入出力を1つにまとめた小型コンピュータ。  
#### Shrike-lite(シュライクライト)
RP2040マイコンとFPGAを搭載した小型の開発ボード。  
マイコンによるソフトウェア制御とFPGAによるハードウェア制御の両方を利用できることが特徴。  
#### FPGA(エフピージーエー)
内部の回路を自由に設計・変更できる集積回路。  
#### MicroPython（マイクロパイソン）
マイコンなどの組み込み機器向けに開発されたPythonの実装。本手順書では、Shrike-lite上でプログラムを実行するために使用する。  
#### REPL（レプル）
Read-Eval-Print Loop の略称。入力したプログラムを逐次実行し、その結果を確認できる対話型の実行環境。  
#### Verilog（ベリログ）
FPGAなどのデジタル回路を設計するために使用するハードウェア記述言語。  

## 1. 開発環境構築
本章では、 Shrike-lite を使用した開発を行うために必要なソフトウェアのインストール、初期設定、ボードの接続および MicroPython の書き込み方法について説明する    
### 1.1 Thonny のインストール
本節では、 MicroPython のプログラムを作成・編集し、Shrike-lite 上で実行するために使用する Thonny のダウンロードとインストール方法を記載する。  
1. 下記リンクより、Thonny にアクセスする。  
[Thonny 公式サイト](https://thonny.org/)   
1. **Download version 5.0.1 for Windows・Mac・Linux** から **Windows** にカーソルを合わせる。  
![ダウンロード画面](../images/S1_DownLoad.png)  
1. **thonny-5.0.1-x64.exe** を選択し、インストーラーをダウンロードする。  
![ダウンロード選択画面](../images/S1_DownLoadSelect.png)  
1. エクスプローラーのダウンロードからダウンロードしたインストーラーをダブルクリックする。  
![インストーラー画面](../images/S1_Install.png)  
1. 「Select Setup Install Mode」画面が開かれ、 **Install for me only (recommended)** を押下する。  
![インストール設定画面](../images/S1_InstallSet.png)  
1. 押下すると「Set up - Thonny」画面が開かれ、 **Next** を押下する。  
![セットアップネクスト押下画面](../images/S1_Setup-Thonny.png)  
1. 「License Agreement」画面が開かれ、**I accept the agreement** を押下する。  
![ライセンス許諾画面](../images/S1_LicenseAgreement.png)
1. **Next** を押下する。  
![ライセンスネクスト押下画面](../images/S1_LicenseAgreementNext.png)  
1. 「Select Destination Location」画面が開かれ、**Next** を押下する。  
![フォルダ場所ネクスト押下画面](../images/S1_SelectDestinationLocation.png)  
1. 「Select Start Menu Folder」画面が開かれ、**Next** を押下する。  
![スタートアップネクスト押下画面](../images/S1_SelectStartupMenuFolder.png)  
1. 「Select Additional Tasks」画面が開かれ、**Create a desktop icon** を押下する。  
![ショートカット作成画面](../images/S1_SelectAdditionalTasks.png)  
1. **Next** を押下する。  
![ショートカットネクスト押下画面](../images/S1_SelectAdditionalTasksNext.png)  
1. 「Ready to Install」画面が開かれ、**Install** を押下する。  
![インストール押下画面](../images/S1_ReadytoInstall.png)  
    > 確認：Great succes! が表示されるとインストール成功    
    ![インストール完了画面](../images/S1_Greatsucces!.png)  

### 1.2 Thonny の初期設定
本節では、インストールした Thonny を初めて起動するときに必要となる初期設定についての手順を記載する。  
1. Language を**日本語**にする。  
![初期言語設定画面](../images/S1_ThonnyLanguage.png)  
1. InitialSetting を **Standard** にする。  
![初期設定画面](../images/S1_ThonnyInitialSetting.png)  
1. Let'sGo を押下する。  
![Let'tGo押下画面](../images/S1_ThonnyLetsGo.png)  
    > 確認：上部のタブが日本語で表示されていると成功  
    ![設定確認画面](../images/S1_LanguageSetting.png)  

### 1.3 ボードの接続
本節では、 Shrike-lite と PC が USB 経由で正常に接続されていることを確認するため、  
デバイスマネージャーを使用して PC が Shrike-lite を認識しているか確認する方法を記載する。
1. Shrike-lite を PC に USB 接続する。  
1. PC が Shrike-lite を認識しているか、デバイスマネージャーを開いて確認する。  
![デバイスマネージャー検索画面](../images/S1_DeviceManager.png)  
1. USB 接続前には表示されていなかったデバイスマネージャーの「ポート（COMとLPT）」を開き、Shrike-lite に対応する COM ポートが表示されていることを確認する。  
接続前  
![接続前画面](../images/S1_COMLPTBefore.png)  
接続後  
![接続後画面](../images/S1_COMLPT.png)  
    > 確認：Shrike-lite に対応する COM ポートが表示されていれば接続成功。

### 1.4 Shrike 専用 MicroPython の書き込み
本節では、 Shrike-lite のマイコンで MicroPython のプログラムを実行するには、マイコン側に MicroPython の実行環境が必要となるため、   Shrike-lite 専用の MicroPython の書き込みを行う方法を記載する。  
なお、本章では、2026年11月18日時点の最新版である **Shrike MicroPython Firmware v1.0.0** を使用する。  
1. Shrike_lite の **BOOTボタン** を押しながらPCにUSB接続を行う。  
1. PC に **RPI-RP2** のドライブが表示される。  
![RPI-RP2ドライブ画面](../images/S1_ExplorerRPIRP2.png)  
1. 次のリンクから **Shrike 公式 GitHub** の Releases ページ にアクセスする。  
[Shrike 公式 GitHub](https://github.com/vicharak-in/shrike/releases)  
1. Releases の Assets から **Shrike-lite-micropython.uf2** をダウンロードする。  
![ShrkeMicroPythonダウンロード画面](../images/S1_ShrikeMicroPythonDownLoad.png)  
1. ダウンロードした Shrike-lite-micropython.uf2 を **RPI-RP2** にコピーする。  
![RPI-RP2コピー画面](../images/S1_MicroPythonCoppy.png)  
注意：コピーが完了すると、RPI-RP2 のドライブが自動的に切断されるため、エクスプローラーが閉じられるのは正常の動きです。
1. Thonny を起動する。
1. 上部のタブから**表示**を押下する。  
![表示押下画面](../images/S1_ThonnyDisplay.png)  
1. 開かれた項目から**ファイル**を押下する。  
![ファイル押下画面](../images/S1_ThonnyFile.png)  
    > 確認：左記にファイル項目が表示されれば成功  
    ![ファイル設定確認画面](../images/S1_ThonnyFileSetting.png)  
1. 上部のタブから**ツール**を押下する。  
![ツール押下画面](../images/S1_ThonnyTool.png)  
1. 開かれた項目から**オプション**を押下する。  
![オプション押下画面](../images/S1_ThonnyOption.png)  
1. Thonnyオプションの**インタプリタ**を押下する。  
![インタプリタ押下画面](../images/S1_ThonnyInterpreter.png)  
1. 「どのインタプリタを使用してコードを実行しますか？」を **MicroPython(Raspberry Pi Pico)** を選択する。  
![インタプリタ選択画面](../images/S1_OptionSelectCode.png)  
1. 「ポートまたは WebREPL」を[1.3 ボードの接続](#13-ボードの接続)で確認した**Shrike-lite に割り当てられた COM ポート**を選択する。  
![ポート選択画面](../images/S1_PortSelect.png)  
1. **OK** を押下する。  
![オプションOK押下画面](../images/S1_OptionOK.png)  
1. 上部のタブから**ファイル**を押下する。  
![ファイル押下画面](../images/S1_FileSelect.png)  
1. **新規ファイル**を押下する。  
![新規ファイル作成画面](../images/S1_NewFileCraft.png)  
1. 作成したファイルに次のコードを記載する。  
    ```python
    import shrike
    print(shrike)
    ```
1. 上部のタブから**実行**を押下する。  
![ツール押下画面](../images/S1_ThonnyRun.png)  
1. **現在のスクリプトを実行**を押下し、実行する。  
![実行選択画面](../images/S1_NowScriptRun.png)  
    > 確認：下部のシェルに<module 'shrike' from 'shrike.py'> と表示されれば書き込み成功  
    ![書き込み確認画面](../images/S1_ShrikeOK.png)  

## 2. REPLの使い方
本章では、 Thonny の Shell(シェル) を使用して、 Shrike-lite に Python のコマンドを送信し、実行結果を確認する REPL の使い方について説明する。  
### 2.1 使用するツールと接続方法
本節では、 REPL を使用するために必要なツールと、 Shrike-lite と PC の接続方法について説明する。  
| 使用するツール | 接続方法 |
| ------ | ------ |
| Thonny | USB接続 |

#### 接続手順
1. Shrike-liteとPCをUSBケーブルで接続する。  
1. Thonnyを起動する。  
1. 上部のタブから**ツール**を押下する。  
![ツール押下画面](../images/S1_ThonnyTool.png)  
1. 開かれた項目から**オプション**を押下する。  
![オプション押下画面](../images/S1_ThonnyOption.png)  
1. Thonny オプションの**インタプリタ**を押下する。  
![インタプリタ押下画面](../images/S1_ThonnyInterpreter.png)  
1. Thonnyのインタプリタが「MicroPython (Raspberry Pi Pico)」に設定されていることを確認する。  
![インタプリタ選択画面](../images/S1_OptionSelectCode.png)  
1. 接続先のポートが　Shrike-lite　に対応する COM ポートになっていることを確認する。  
対応する COM ポートの確認は[1.3 ボードの接続](#13-ボードの接続)を参照。
![ポート選択画面](../images/S1_PortSelect.png)
1. Thonnyのシェルに「>>>」が表示されていることを確認できれば完了。  


### 2.2 通信設定(ポート、ボーレートなど)
本節では、 Shrike-lite と PC 間の通信に使用する設定項目について説明する。  
REPL を使用する際は、 Thonny のインタプリタと接続先の COM ポートを適切に設定する必要がある。  
| 設定項目 | 設定内容 |
| ------- | ------- |
| インタプリタ | MicroPython（Shrike-Lite対応）|
| ポート | Shrike-Lite に割り当てられた COM ポート |
| ボーレート | -(※) |
| 通信方式 | USB 経由のシリアル通信 |
※ Thonny では、ボーレートを手動で設定する必要はない。通信速度はインタプリタ側で管理される。  

### 2.3 動作確認
本節では、 REPL を使用して Shrike-lite にコマンドを送信し、正常に実行されることを確認する。  
Thonny のシェルに以下のコマンドを入力する。  
```python
print("Hello Shrike-lite")
```
> 確認：入力後、Enterキーを押下し、 Hello Shrike-lite が表示されると成功  
![REPL確認画面](../images/S1_REPLCheck.png)  

## 3. プロジェクト作成

### 3.1 新規プロジェクトの作成
### 3.2 プロジェクトの構成(フォルダ・ファイルの役割)

## 4. コーディング・ビルド・書込み(RTOSなし)

<!-- 各題材の節は「作るもの → 配線 → プログラム → ビルドと書込み → 動作確認」の順で書く -->

### 4.1 題材1:LED点滅
### 4.2 題材2:ボタン入力
### 4.3 題材3:UART出力
### 4.4 題材4:タイマ割込み

## 5. コーディング・ビルド・書込み(FreeRTOS)

### 5.1 FreeRTOSの導入・設定
### 5.2 題材5:複数タスク
### 5.3 題材6:タスク間通信

## 6. デバッグ

### 6.1 デバッガの接続(配線図・写真)
### 6.2 デバッグの開始
### 6.3 ブレークポイント・ステップ実行
### 6.4 変数・メモリ・レジスタの確認

## 7. トラブルシューティング

| 症状 | 原因 | 対処 |
|---|---|---|
| | | |

## 8. 参考資料

- 

<!-- ボード独自の追加章がある場合は、ここから「## 9. 〜」として追加する -->

<!--
  記述ルール(書きぶりを全章でそろえる)
  1. 見出し:章は「## 1. 開発環境構築」、節は「### 1.1 ツールのインストール」の形にする。
  2. 手順:番号付きリストで書き、1つの番号に1つの操作。文末は「〜する。」にそろえる。
  3. 確認:手順のあとに「> 確認:〜れば成功」と、成功したかどうかの判断方法を書く。
  4. 注意:まちがえやすい所は「> 注意:〜」と書く。
  5. 画像:画面操作にはスクリーンショットを付ける。images/ に置き、名前は担当番号から始める(例:p1_ext.png)。
  6. 図:処理の流れやタスクの関係は Mermaid で書く。配線は図か写真で、ピン番号と信号名を書く。
  7. プログラム:手順書の中にコードブロックで書き、日本語のコメントを付ける。
  8. バージョン:ツールと SDK のバージョンを書く。
  9. 用語:同じものは同じことばで書く。英数字は半角で、日本語との間に半角スペースを入れる。
  10. つまずいた点:第7章に必ず記録する。
-->
