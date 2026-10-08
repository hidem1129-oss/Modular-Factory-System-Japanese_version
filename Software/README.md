# ソフトウェア

現在の観測、記録、履歴表示を分野別に説明します。工程制御はデモ固有のホストプログラムで行い、この監視・可視化の構成とは役割を分けています。

## 読みたい内容から探す

| 内容 | 日本語資料 |
|---|---|
| ノードの観測、状態判定、通信断、模擬動作、ログ | [I²C Debugger](./I2C_Debugger/README.md) |
| SQLiteの記録を使う時系列表示、SQL、分析の制約 | [Grafana](./Grafana/README.md) |

## データの流れ

ノードと電源測定器 → I²C Debugger → 画面とSQLite → Grafana、という流れです。現在状態の表示と過去の分析を分け、Grafanaからハードウェアへ指示を送りません。

ノードの正規の状態はファームウェアが管理し、監視アプリは読み取った状態と通信状況を人が確認できる表示へ変換します。監視画面が制御ノードの停止処理や電気的保護を代行する構成ではありません。

## 工程制御コード

仕分け工程では、搬送・センサ・画像判定・ゲートを連携させます。スタンプ工程では、固定・押印・解除・紙送りを順序制御します。

- [日本語：仕分けデモ](../Docs/仕分けデモ.md)／[工程制御コード](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Use_cases/Amazon-style_Sorting_Demo/warehouse_demo.py)
- [日本語：スタンプ工程](../Docs/スタンプ工程.md)／[工程制御コード](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Use_cases/Stamp_Process_Demo/stamp_press_demo.py)

## 現在の対象範囲

ローカルのRaspberry Pi、SQLite、手動設定したダッシュボードを使う卓上PoCです。産業用PLC・SCADA・安全コントローラの代替を目的としていません。

詳細：[英語版ソフトウェア概要](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Software/README.md)

[READMEへ戻る](../README.md)
