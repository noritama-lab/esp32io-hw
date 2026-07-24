# ESP32-S3 IO Device – Hardware Repository

このリポジトリは、ESP32-S3 を産業用途で扱うための  
**入出力回路（KiCad）** と **筐体データ（FreeCAD / 3MF / STEP）** を公開するものです。

本プロジェクトは、電子工作レベルではなく、  
**工場の 24V 機器と安全に接続できる小型 I/O デバイス**  
として利用できることを目標に設計しています。

> ⚠️ 本リポジトリは個人開発によるものであり、認証・第三者検証を受けたものではありません。  
> 実運用前には、必ずご自身の環境・負荷条件に対する検証を行ってください。詳しくは [免責事項](#️-免責事項--disclaimer) を参照してください。

このハードウェアは、以下のソフトウェア群と組み合わせて使用することを想定しています。  
This hardware is designed to work together with the following software components.

- 🔧 HTTP版ファームウェア: [esp32io-firmware](https://github.com/noritama-lab/esp32io-firmware) — WiFi/HTTP経由での制御版
- 🐍 HTTP版 Pythonクライアント: [esp32io](https://pypi.org/project/esp32io/) — PyPI公開
- 🔧 MQTT版ファームウェア: [esp32io-mqtt-firmware](https://github.com/noritama-lab/esp32io-mqtt-firmware) — MQTT + Home Assistant対応ファームウェア
- 🐍 MQTT版 Pythonクライアント: [esp32io-mqtt](https://pypi.org/project/esp32io-mqtt/) — PyPI公開

---

# ESP32-S3 IO Device – Hardware Repository (English)

This repository provides the **hardware design (KiCad)** and **mechanical data (FreeCAD / 3MF / STEP)**  
for an ESP32-S3 based I/O device intended for **industrial 24V environments**.

The goal of this project is to create a compact I/O module that can be safely connected  
to factory equipment, rather than a hobby-level electronics board.

> ⚠️ This is an individually developed, open-source project. It has not been certified or independently verified.  
> Please validate the design against your own environment and load conditions before deploying it in production. See [Disclaimer](#️-disclaimer) below.

This hardware is designed to be used together with:

- 🔧 HTTP firmware: [esp32io-firmware](https://github.com/noritama-lab/esp32io-firmware) — WiFi/HTTP control variant
- 🐍 HTTP Python client: [esp32io](https://pypi.org/project/esp32io/) — Published on PyPI
- 🔧 MQTT firmware: [esp32io-mqtt-firmware](https://github.com/noritama-lab/esp32io-mqtt-firmware) — MQTT-based firmware with Home Assistant support
- 🐍 MQTT Python client: [esp32io-mqtt](https://pypi.org/project/esp32io-mqtt/) — Published on PyPI

---

## 含まれるデータ / Included Data

### ■ KiCad（回路図・基板） / KiCad (Schematics & PCB)
- `ESP32-S3-IO-DEVICE.kicad_sch` – 回路図 / Schematic  
- `ESP32-S3-IO-DEVICE.kicad_pcb` – 基板データ / PCB layout  
- `ESP32-S3-IO-DEVICE.kicad_pro` – プロジェクト設定 / Project settings  
- `sym-lib-table` / `fp-lib-table` – ライブラリ設定 / Library tables  

KiCad プロジェクトを開くだけで、  
**回路図 → 基板 → ガーバー** まで完全に再現できます。  
Opening the KiCad project allows full reproduction from **schematic → PCB → Gerber**.

基板自体はHTTP版・MQTT版どちらのファームウェアでも共通してお使いいただけます。  
The same PCB can be used with either the HTTP or MQTT firmware.

---

### ■ FreeCAD / 3D データ（筐体・取付板）  
### FreeCAD / 3D Data (Enclosure & Mounting Parts)
- `.FCStd` – FreeCAD モデル / FreeCAD models  
- `.3mf` – 3D プリント用 / For 3D printing  
- `.step` – 汎用 CAD 形式 / Standard CAD format  

ケース、基板カバー、取付板など、  
**デバイスとして使える形にするための筐体データ**を含みます。  
Includes mechanical parts required to assemble the device.

> ℹ️ 基板取付板は現行基板でもそのままご利用いただけます。  
> 基板カバーのみ旧基板に合わせた設計のため、現行基板とは適合しない場合があります。  
> 最新基板に対応した基板カバーは今後更新予定です。
>
> ℹ️ The board mounting plate is compatible with the current PCB as-is.  
> Only the board cover was designed for an older board revision and may not fit the current PCB.  
> An updated cover matching the latest board will be published in the future.

---

## 🏭 工場で使うための設計方針  
## Industrial Design Principles

本デバイスは、一般的な電子工作向けではなく、  
**工場・設備で使われる 24V 系信号を安全に扱うための回路構成**を採用しています。  
This device is designed for **industrial 24V signals**, not hobby electronics.

### ✔ DIO_OUT：NPN シンク出力 / NPN Sink Output  
PLC と同じ方式で外部ソース負荷を駆動可能。  
Compatible with PLC-style sourcing loads.

### ✔ DIO_IN：ソース入力（NPN シンク出力センサ対応）  
### Source Input (for NPN sensors)  
**トランジスタ＋抵抗＋Zener** による保護回路を採用。  
Protected using **transistor + resistors + Zener**.

### ✔ ADC：0〜10V アナログ入力 / 0–10V Analog Input  
工場のアナログ信号に合わせて **分圧＋可変抵抗＋Zener** を使用。  
Uses **voltage divider + trim resistor + Zener** for safe conversion.

> ℹ️ DI/DOはフォトカプラによりMCU側と絶縁されていますが、ADC（アナログ入力）は非絶縁です。  
> ℹ️ DI/DO channels are isolated from the MCU via optocouplers. The ADC (analog input) is **not** isolated.

### ✔ PWM：3.3V ロジック出力 / 3.3V Logic PWM  
外部ドライバ・SSR の制御信号として使用。  
Used as a control signal for external drivers or SSRs.

---

## ⚙️ 想定負荷 / Intended Load

DIO_OUT（NPNシンク出力）は、以下のような **小型DC24Vリレー** の直接駆動を想定して設計・検証しています。  
The DIO_OUT (NPN sink output) is designed and verified for driving **small DC24V relays**, such as:

- オムロン LYシリーズ相当（コイル電流 目安 20〜40mA程度） / Omron LY-series relays or equivalent (coil current approx. 20–40mA)

より大きな消費電流のソレノイド・コンタクタ・AC駆動機器を直接駆動することは想定していません。  
それらを制御する場合は、SSRやパワーリレーなど適切な中間段を介してください。  
Larger solenoids, contactors, or AC-driven devices are **not** intended to be driven directly.  
Use an appropriate intermediate stage (e.g. SSR or power relay) for such loads.

複数チャンネルを同時にON にする場合、COM+ライン全体に流れる電流（チャンネル数 × コイル電流）が、  
使用する電源・配線の許容電流を超えないことをご確認ください。  
When switching multiple channels simultaneously, verify that the total current on the COM+ line  
(number of channels × coil current) does not exceed the current rating of your power supply and wiring.

---

## ディレクトリ構成 / Directory Structure
/KiCad
/3D_print

> 📷 組み立て後の完成写真、および使用部品表（BOM）は準備中です。追って追加予定です。  
> 📷 Assembly photos and a bill of materials (BOM) are planned and will be added soon.

---

## 必要なソフトウェア / Required Software

- KiCad 7+  
- FreeCAD 0.21+  
- 3D プリンタ（任意） / Any 3D printer (FDM recommended)

---

## ⚠️ 免責事項 / Disclaimer

本リポジトリで公開する回路・筐体データは、個人による設計・検証であり、  
特定の安全規格・産業規格への適合を保証するものではありません。  
実際の設備・機器へ接続する場合は、電圧・電流・絶縁・保護協調などについて、  
利用者ご自身の責任において十分な検証を行ってください。  
本データの利用によって生じたいかなる損害についても、作者は責任を負いません。

The circuit and enclosure data provided in this repository are individually designed and verified,  
and do not guarantee compliance with any specific safety or industrial standard.  
Before connecting this device to actual equipment, users are solely responsible for verifying  
voltage, current, isolation, and protection coordination appropriate to their environment.  
The author assumes no liability for any damage resulting from the use of this data.

---

## ライセンス / License

- 回路図・基板データ（KiCad）: MIT License  
- 3D モデル（FreeCAD / STEP / 3MF）: CC BY-SA 4.0  

- Schematic & PCB (KiCad): MIT License  
- 3D Models (FreeCAD / STEP / 3MF): CC BY-SA 4.0