# ハードウェア

自作PCBは、ホスト側の配線、電源分配、Picoによる制御、装置との電気的接続を分担しています。基板だけで工程が動くわけではなく、ファームウェアと工程制御を組み合わせて使用します。

## 制御信号と電源の経路

制御・通信の経路は、Raspberry Pi 5 → Pi5 Wiring Auxiliary → Power Monitor Board → Controller Board → モータ・サーボ・センサ基板です。

アクチュエータの電源は、外部5 V電源 → Power Monitor Board → 各分岐 → 接続した負荷という経路です。ロジック電源とアクチュエータ電源は関連しますが、役割と経路を区別して確認します。

## 基板ごとの役割

| 基板 | 役割 | 詳細 |
|---|---|---|
| Pi5 Wiring Auxiliary | Pi 5側のI²C、電源、MUXリセットなどの配線を整理する | [英語版](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Hardware/Pi5_Wiring_Auxiliary/README.md) |
| Power Monitor Board | 外部5 Vの受電、分配、主幹と分岐の測定 | [日本語の詳細](./Power_Monitor_Board/README.md) |
| Controller Board | Pico、共通バス、装置接続、アドレス設定、停止入力 | [日本語の詳細](./Controller_Board/README.md) |
| DC Motor Board | DCモータの駆動回路を提供する | [英語版](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Hardware/DC_Motor_Board/README.md) |
| Servo Board | サーボの接続と電源経路を提供する | [英語版](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Hardware/Servo_Board/README.md) |
| Sensor Board | 選定したフォトリフレクタ回路と検出信号を接続する | [英語版](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Hardware/Sensor_Board/README.md) |

Controller Boardは共通の制御装置、周辺基板は負荷ごとの電気的インターフェースです。どの装置として動くかは、接続基板と読み込むファームウェアの組み合わせで決まります。

## ハードウェア・ファームウェア・ソフトウェアの境界

ハードウェアは配線、電源、ドライバ、保護部品、基板レイアウトを担当します。[ファームウェア](../Firmware/README.md)は指示の解釈、GPIO・PWM・ADC、状態と停止処理を担当します。[ソフトウェア](../Software/README.md)は工程の判断、観測、換算、記録、可視化を担当します。

## 配線を変更するときに確認すること

I²Cの配線変更は、コネクタだけでなく配線距離、容量、プルアップ、ノード数、電源経路に影響します。現在の基板は卓上PoCに合わせた構成で、長距離通信や産業用の接続規格への適合を前提としていません。

実機で判明したハーネスと操作性の問題は[実際に作って分かったこと](../Docs/実際に作って分かったこと.md)で説明しています。

## 回路図・製作資料の確認

[製作と調達](./Manufacturing/README.md)から回路図・Gerber・部品情報の確認方法へ進めます。日本語版は技術説明を提供し、回路図や製造データは英語版で管理します。

詳細：[英語版ハードウェア概要](https://github.com/hidem1129-oss/Modular-Factory-System/blob/main/Hardware/README.md)

+[READMEへ戻る](../README.md)
