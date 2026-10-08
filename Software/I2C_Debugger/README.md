# I²C Debugger：状態監視と記録

Python / PyQt5で構成した監視アプリです。応答の有無だけでなく、各ノードの識別、動作状態、指示と設定値、フィードバック、通信断、電源状態を確認します。

## 内部の分担

| ファイル | 主な役割 |
|---|---|
| mainwindow.py | 画面更新、タイマ、モード、セッションの調整 |
| readers.py | 実I²Cと模擬レジスタの読み取り |
| debugger_model.py | 最新観測、通信失敗の扱い、変化検出 |
| node_logic.py | 状態フラグから表示状態を選ぶ |
| db_logger.py | SQLiteへの保存 |
| power_monitor.py / power_calc.py | 電源測定の取得と換算 |
| power_config.py | 測定チャンネルと論理ポートの対応 |

読み取り、状態判定、画面、保存を分けています。詳細なファイル構成は[英語版](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Software/I2C_Debugger/README.md)で確認できます。

## 通信状態と表示状態

既定では0x10–0x19を周期的に読みます。設定値やフィードバックなどの任意レジスタは個別に扱い、一部が未実装でもノード自体を確認できるようにしています。

状態フラグの表示優先度はESTOP → ERROR → WARN → BUSY → READY → UNKNOWNです。これは画面上の代表状態であり、元のフラグを置き換えるものではありません。

| 通信の観測結果 | 表示 |
|---|---|
| 少数の連続読み取り失敗で、現在状態を確定できない | UNKNOWN |
| 一度も応答していないアドレスで失敗しきい値に達した | No Device |
| 以前は応答したノードで失敗しきい値に達した | Signal Lost |

以前から未使用だったアドレスと、動作中に応答しなくなったノードを区別します。Signal Lostだけでは断線、電源喪失、ファームウェア停止などの原因までは確定しません。

## 記録する情報

| テーブル | 用途 |
|---|---|
| event_logs | イベントや状態変化 |
| node_snapshots | 定期的なノード観測 |
| state_segments | 状態が継続した区間 |
| monitor_sessions | 監視実行の条件と開始・終了 |
| run_sessions | 実行関連セッション |
| power_port_snapshots | 電源測定の定期記録 |

起動ごとの監視セッションで記録に文脈を与えます。最初の読み取りは初期状態として扱い、架空の前状態からの遷移として記録しません。

## realとmock

realはSMBus経由で実機を読み、mockは状態、通信失敗、電源値を模擬します。同じアプリ側インターフェースで扱うため、画面や判定処理を実機なしで確認できます。実機バスを開けない場合にmockへ戻る挙動もあるため、検証時は現在のモードを確認します。

mockは電気的・機械的な成立を検証するものではありません。

## 電源測定の扱い

INA219の値を電圧・電流・電力へ換算し、測定ポート単位で表示・保存します。一部の値を読めなければ欠測として扱い、電力は電圧と電流の両方がある場合に算出します。

電源読み取り失敗をノードのSignal Lostなどへ直接反映せず、実測値からノードWARNへ自動対応付ける処理も現在はありません。主幹画面の合計電流・電力の計算と、基板が備える主幹測定チャンネルは、具体的な実装と設定を区別して確認します。

## 周期と制約

短いポーリング周期は観測遅延を減らしますが、通信、保存、画面更新の負荷が増えます。長い周期では短時間の変化を見逃す場合があります。監視はハードリアルタイム制御ではありません。

詳細：[状態モデルの実装](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Software/I2C_Debugger/debugger_model.py)／[保存実装](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Software/I2C_Debugger/db_logger.py)／[実機・模擬読み取り](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Software/I2C_Debugger/readers.py)

[ソフトウェア概要へ戻る](../README.md)
