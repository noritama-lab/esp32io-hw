# ESP32-S3 IO Device – Hardware Repository

このリポジトリは、ESP32-S3 を産業用途で扱うための  
**入出力回路（KiCad）** と **筐体データ（FreeCAD / 3MF / STEP）** を公開するものです。

本プロジェクトは、電子工作レベルではなく、  
**工場の 24V 機器と安全に接続できる小型 I/O デバイス**  
として利用できることを目標に設計しています。

---

# ESP32-S3 IO Device – Hardware Repository (English)

This repository provides the **hardware design (KiCad)** and **mechanical data (FreeCAD / 3MF / STEP)**  
for an ESP32-S3 based I/O device intended for **industrial 24V environments**.

The goal of this project is to create a compact I/O module that can be safely connected  
to factory equipment, rather than a hobby-level electronics board.

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

---

### ■ FreeCAD / 3D データ（筐体・取付板）  
### FreeCAD / 3D Data (Enclosure & Mounting Parts)
- `.FCStd` – FreeCAD モデル / FreeCAD models  
- `.3mf` – 3D プリント用 / For 3D printing  
- `.step` – 汎用 CAD 形式 / Standard CAD format  

ケース、基板カバー、取付板など、  
**デバイスとして使える形にするための筐体データ**をすべて含みます。  
Includes all mechanical parts required to assemble the device.

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

### ✔ PWM：3.3V ロジック出力 / 3.3V Logic PWM  
外部ドライバ・SSR の制御信号として使用。  
Used as a control signal for external drivers or SSRs.

---

## ディレクトリ構成 / Directory Structure

```
/KiCad
/3D_print
```

---

## 必要なソフトウェア / Required Software

- KiCad 7+  
- FreeCAD 0.21+  
- 3D プリンタ（任意） / Any 3D printer (FDM recommended)

---

## ライセンス / License

- 回路図・基板データ（KiCad）: MIT License  
- 3D モデル（FreeCAD / STEP / 3MF）: CC BY-SA 4.0  

- Schematic & PCB (KiCad): MIT License  
- 3D Models (FreeCAD / STEP / 3MF): CC BY-SA 4.0

---
