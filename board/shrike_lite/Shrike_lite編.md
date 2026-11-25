<!--
  手順書テンプレート(全ボード共通)
  - このファイルをコピーして、ボードごとのフォルダに保存してから書き始める。
  - 章の番号と名前は変えない。該当しない章は消さずに「本ボードでは作成しない(理由:〜)」と書く。
  - ボード独自の内容は、第9章以降に追加章として書く。
  - このコメントはプレビューには表示されない。消さずに残してよい。
-->

# 【ボード名】開発手順書

| 項目 | 内容 |
|---|---|
| 対象ボード | |
| 開発言語 | |
| IDE・ツール | (名称とバージョン) |
| SDK・ライブラリ | (名称とバージョン) |
| 開発PCのOS | (例:Windows 11 24H2) |
| 最終検証日 | (yyyy/mm/dd) |

## 改版履歴

| 版 | 日付 | 担当者 | 内容 |
|---|---|---|---|
| 0.1 | (yyyy/mm/dd) | | 新規作成 |

---

## 0. 概要

### 0.1 この手順書でできること
<!-- 読者がこの手順書を最後まで進めると何ができるようになるかを書く -->

### 0.2 必要な機材

| 品名 | 数量 | 備考 |
|---|---|---|
| | | |

### 0.3 前提知識・事前準備
<!-- 既存の Git/GitHub 運用・Redmine 運用を参考に、前提とする知識と準備を書く -->

---

## 1. 開発環境構築

### 1.1 ツールのインストール
### 1.2 ドライバのインストール
### 1.3 インストールできたことの確認

## 2. シリアルターミナルの使い方

### 2.1 使用するツールと接続方法
### 2.2 通信設定(ポート、ボーレートなど)
### 2.3 動作確認

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

## 9. FPGAの開発環境とプロジェクト作成
　本章では、 Shrike-lite に内蔵されている FPGA を開発する環境を構築する手順と、 Shrike-lite のピン割り当て、 FPGA に関する基礎知識について説明する。

### 9.1 FPGAの基礎
- 概要
  
  　FPGA(Field Programmable Gate Array) は使用目的に合わせて内部の電子回路を自由に構成できる半導体である。通常の集積回路（IC）は製造時点で内部の回路構成や機能が決まっている。しかし FPGA はユーザーが設計した回路情報を書き込むことで、内部の論理回路の構成を変更できる。

- 特徴
  - 再構成可能な論理回路
  - 並列処理が得意
  - 回路構成を編集することで機能を変更できる
  - Verilog や VHDL などのハードウェア記述言語を用いて開発する

- 内部構造

  FPGA の内部には多数の小さなロジックブロックや配線が並んでいる。

  ```mermaid
  flowchart TB
    IN["外部入力<br/>センサー・スイッチ"]

    subgraph FPGA["FPGA"]
        direction TB

        subgraph COL1["入力列"]
            direction TB
            IN1["IOB<br/>入力ブロック"]
            IN2["IOB<br/>入力ブロック"]
            IN3["IOB<br/>入力ブロック"]
        end

        subgraph COL2["論理回路列"]
            direction TB
            C["LUT"]
            D["LUT"]
            E["LUT"]
            F["LUT"]
            G["LUT"]
            H["LUT"]
            I["LUT"]
            J["LUT"]
            K["LUT"]
        end

        subgraph COL3["出力列"]
            direction TB
            OUT1["IOB<br/>出力ブロック"]
            OUT2["IOB<br/>出力ブロック"]
            OUT3["IOB<br/>出力ブロック"]
        end

        IN1 --> C
        IN2 --> D
        IN3 --> E
        C --> F
        D --> G
        E --> H
        F --> I
        G --> J
        H --> K
        I --> OUT1
        J --> OUT2
        K --> OUT3
    end

    OUT["外部出力<br/>制御信号・LED"]

    IN --> IN1
    IN --> IN2
    IN --> IN3
    OUT1 --> OUT
    OUT2 --> OUT
    OUT3 --> OUT

    style FPGA fill:#f8fafc,stroke:#64748b,color:#111
    style COL1 fill:#e3f2fd,stroke:#2563eb,color:#111
    style COL2 fill:#fff8e1,stroke:#d69e00,color:#111
    style COL3 fill:#e3f2fd,stroke:#2563eb,color:#111
    style IN1 fill:#bfdbfe,stroke:#2563eb,color:#111
    style IN2 fill:#bfdbfe,stroke:#2563eb,color:#111
    style IN3 fill:#bfdbfe,stroke:#2563eb,color:#111
    style OUT1 fill:#bfdbfe,stroke:#2563eb,color:#111
    style OUT2 fill:#bfdbfe,stroke:#2563eb,color:#111
    style OUT3 fill:#bfdbfe,stroke:#2563eb,color:#111
    style C fill:#fde68a,stroke:#d69e00,color:#111
    style D fill:#fde68a,stroke:#d69e00,color:#111
    style E fill:#fde68a,stroke:#d69e00,color:#111
    style F fill:#fde68a,stroke:#d69e00,color:#111
    style G fill:#fde68a,stroke:#d69e00,color:#111
    style H fill:#fde68a,stroke:#d69e00,color:#111
    style I fill:#fde68a,stroke:#d69e00,color:#111
    style J fill:#fde68a,stroke:#d69e00,color:#111
    style K fill:#fde68a,stroke:#d69e00,color:#111
  ```

  - LUT
  - 配線
  - 入出力ブロック


### 9.2 Shrike-liteのピン割り当て

### 9.3 Go Configure Software Hubの入手とインストール
　Go Configure Software Hub は FPGA に搭載する回路データを開発・構成・シミュレーションするための GUI 開発環境である。

### 9.4 SLG47910のプロジェクトの作り方



## 10. FPGAの設計とシミュレーション


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
