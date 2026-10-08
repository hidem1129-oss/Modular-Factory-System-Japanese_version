# ファームウェア

Raspberry Pi Pico上で動き、I²Cの共通指示をモータ・サーボ・センサの具体的な動作へ変換します。工程全体の順序はホスト側が決め、装置ごとの処理はノード側が担当します。

## 構成

| 要素 | 担当 |
|---|---|
| [common](./common/README.md) | 通信、状態、設定適用、指示検証、停止・完了 |
| motor_node | DCモータ固有の駆動・完了処理 |
| servo_node | サーボ固有の動作・完了処理 |
| sensor_node | センサの取得とフィードバック |

ファームウェアイメージは、共通部分、選択したノード実装、ノードプロファイルを組み合わせて作ります。ノード固有部分は独立した別アーキテクチャではなく、共通部分を拡張する構成です。

## ランタイム

Pico SDKを用いる協調型ポーリングで、共通処理はおおむね1 ms、ノード固有の周期処理はおおむね10 msごとに呼ばれます。これは実装上の周期目安で、最悪応答時間やハードリアルタイムの保証ではありません。I²Cは100 kHzです。

## 技術確認の順序

1. [共通処理](./common/README.md)で状態と指示の流れを理解する。
2. [状態モデル](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/docs/State_Model.md)で停止・完了・復帰条件を確認する。
3. [拡張API](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/docs/Node_Extension_API.md)でノード固有処理との境界を確認する。
4. [英語版ファームウェア一覧](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/README.md)から対象ノードのコードへ進む。

レジスタの仕様と実装に差がある場合は、現在のnode_core.hとnode_core.cを確認します。ホストのデモコードも別途確認し、仕様だけで対応状況を判断しないようにします。

[READMEへ戻る](../README.md)
