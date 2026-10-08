# 共通ファームウェア

通信や状態管理を各ノードで重複させず、装置固有の処理を呼び出す共通基盤です。

## 指示から完了まで

ホストは動作パラメータを書き、LATCH_APPLYで適用し、受付結果を確認してRUNを送ります。ノードはBUSYへ進み、装置固有の処理を実行し、フィードバックと完了情報を公開してREADYへ戻ります。

設定の準備と適用を分けるのは、書き込み途中のパラメータで動作を始めないためです。16ビット値は上位・下位バイトの順で書き、下位バイトの書き込み時に値を確定します。

## 状態と更新情報

| 情報 | 用途 |
|---|---|
| READY / BUSY / ESTOP | 待機、実行、非常停止のライフサイクル |
| WARN / ERROR | 警告・異常に関する状態フラグ |
| 完了情報 | 動作の完了と理由 |
| DATA_READY / UPDATE_CNT | 関連情報が更新されたことの通知 |
| 指示の受付結果 | 受理と拒否、拒否理由の区別 |

DATA_READYとUPDATE_CNTは動作段階そのものではありません。通信成功と指示受理も別です。動作中、設定未適用、非常停止などにより指示が拒否される場合があります。

## 装置固有処理の責任

共通部分は「実行してよいか」「いつ開始・停止するか」を管理します。ノード固有部分はGPIO初期化、装置の駆動、パラメータ検証、取得値、完了条件、停止時の装置処理を担当します。

固有処理はコールバックと公開APIを通して接続し、共通ライフサイクルを直接変更しない設計です。

## 詳細と実装

- [レジスタ定義](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Docs/Register_Map/Common_Register_Map.md)
- [指示・設定の仕様](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Docs/Register_Map/Command_and_Setpoint.md)
- [停止・復帰を含む状態モデル](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/docs/State_Model.md)
- [拡張API](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/docs/Node_Extension_API.md)
- [ノード追加手順](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/docs/Adding_New_Node.md)
- [レジスタと公開APIの定義](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/include/node_core.h)
- [指示・状態管理の実装](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Firmware/common/core/node_core.c)

非常停止の電気的経路は[Controller Board](../../Hardware/Controller_Board/README.md)、処理と復帰はファームウェア側で確認します。

[ファームウェア概要へ戻る](../README.md)
