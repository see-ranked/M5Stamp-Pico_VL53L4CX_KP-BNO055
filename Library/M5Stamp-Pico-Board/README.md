# M5Stamp Pico ピン配置とJLCPCB発注ガイド

## 概要

M5Stamp Picoは18mm × 24mmの超小型ESP32開発ボードです。このドキュメントでは、ピンソケット/ピンヘッダーを使用してM5Stamp Picoを基板に実装するためのフットプリント情報とJLCPCB発注ガイドを提供します。

## フットプリント

### PinHeaders_for_M5Stamp_Pico.kicad_mod
- **基板側**: ピンヘッダー(オス)を実装
- **M5Stamp Pico側**: ピンソケット(メス)を半田付け
- **用途**: M5Stamp Picoを上から差し込む構成

## ピン配置図

```
        USB側 (上)
    ┌─────────────┐
 1  │●            │     (●=Pin1マーカー)
 2  │             │
 3  │             │
 4  │             │ 15
 5  │             │ 14
 6  │             │ 13
 7  │             │ 12
 8  │             │ 11
 9  │             │ 10
    └─────────────┘
    LED/ボタン側 (下)

ピン間隔: 2.54mm (0.1インチ)
左右列間隔: 12.7mm (5 × 2.54mm)
ピン配置: 反時計回り (1番ピンから)
```

## ピン機能一覧

| ピン番号 | 信号名 | 機能 | 備考 |
|---------|--------|------|------|
| 1 | G26 | GPIO26 | SPI_MOSI, ADC, DAC2 |
| 2 | G36 | GPIO36 | SPI_MISO, ADC |
| 3 | G18 | GPIO18 | SPI SCK |
| 4 | G19 | GPIO19 | SPI MISO |
| 5 | G21 | GPIO21 | I2C SDA |
| 6 | G22 | GPIO22 | I2C SCL |
| 7 | G25 | GPIO25 | ADC2_CH8, DAC1 |
| 8 | 5V | 電源入力 | 5V入力(最大500mA) |
| 9 | GND | グランド | 0V基準 |
| 10 | GND | グランド | 0V基準 |
| 11 | G0 | GPIO0 | Boot制御 |
| 12 | EN | Enable | Low=リセット |
| 13 | G3 | GPIO03 | UART0 RX, PWM |
| 14 | G1 | GPIO01 | UART0 TX, PWM |
| 15 | 3V3 | 電源出力 | 3.3V出力(内部DC/DC) |

## 重要な注意事項

> [!WARNING]
> **入力専用ピン**
> - GPIO36 (Pin 2)は入力専用です。出力として使用できません。

> [!CAUTION]
> **Boot制御ピン**
> - GPIO0 (Pin 11)はBoot時の動作モード制御に使用されます
> - Boot時にLowにするとダウンロードモードに入ります
> - 通常動作時はプルアップしておくことを推奨します

> [!IMPORTANT]
> **電源仕様**
> - 入力電圧: 5V (Pin 8)
> - 出力電圧: 3.3V (Pin 15)
> - 最大消費電流: 500mA
> - GNDピンは2つあります(Pin 9, 10)

## JLCPCB発注ガイド

### 基板仕様の推奨設定

| 項目 | 推奨値 | 備考 |
|------|--------|------|
| 基板厚 | 1.6mm | 標準的な厚さ |
| 銅箔厚 | 1oz (35μm) | 一般的な用途に適合 |
| 表面処理 | HASL (有鉛) | コスト重視 |
| 表面処理 | ENIG (無鉛) | 高品質、長期保存向け |
| ソルダーマスク | 緑 | 最も安価 |
| シルク印刷 | 白 | 標準 |

### Gerberファイル生成手順 (KiCad)

1. **PCB Editorでプロジェクトを開く**
2. **File → Fabrication Outputs → Gerbers (.gbr)...**
3. **設定**:
   - Plot format: Gerber
   - Include layers: すべての必要なレイヤーを選択
   - Use Protel filename extensions: チェック推奨
4. **Generate Drill Files...**をクリック
5. **Drill file format**: Excellon
6. **すべてのファイルをZIP圧縮**

### JLCPCBアップロード手順

1. [JLCPCB](https://jlcpcb.com/)にアクセス
2. **Order Now**をクリック
3. **Add gerber file**でZIPファイルをアップロード
4. 基板仕様を確認・変更
5. **Save to Cart**で注文

### コスト最適化のヒント

- **基板サイズ**: 100mm × 100mm以下で5枚 $2程度(送料別)
- **色**: 緑色が最安
- **枚数**: 最小5枚から(追加コストで増量可能)
- **納期**: 標準納期(5-7日)が最安

## 基板設計のベストプラクティス

### 1. 電源デカップリング
```
M5Stamp Picoの3V3ピン近くに:
- 100nF セラミックコンデンサ (0603または0805)
- 10μF 電解またはセラミックコンデンサ
```

### 2. リセット回路
```
ENピンに:
- 10kΩ プルアップ抵抗 → 3V3
- タクトスイッチ → GND (オプション)
```

### 3. Boot制御回路
```
GPIO0 (Pin 1)に:
- 10kΩ プルアップ抵抗 → 3V3
- タクトスイッチ → GND (プログラミング用、オプション)
```

### 4. 配線の推奨事項
- **電源配線**: 最低0.5mm幅(1A以下の場合)
- **信号配線**: 0.2mm〜0.3mm幅
- **GNDプレーン**: 可能な限りベタGNDを使用
- **クリアランス**: 最低0.2mm(JLCPCBの最小値)

### 5. 実装時の注意
- M5Stamp Picoの高さを考慮して周辺部品を配置
- ピンソケット実装時は基板から約8.5mm高くなります
- M5Stamp Pico本体の高さは約4.6mm

## トラブルシューティング

### Q1: KiCadでフットプリントが見つからない
**A**: フットプリントライブラリに手動で追加してください:
1. Preferences → Manage Footprint Libraries
2. Project Specific Librariesタブ
3. フォルダアイコンをクリック
4. esp32フォルダを選択

### Q2: ピンの向きがわからない
**A**: 
- Pin 1は常に四角いパッド(他は丸)
- シルクスクリーンに"1-G0"と表記
- Pin 1インジケーター(小さな円)がPin 1の外側に表示

### Q3: JLCPCBでエラーが出る
**A**: よくあるエラー:
- **Drill too small**: 最小穴径0.3mm以上必要
- **Clearance too small**: 最小0.15mm必要(0.2mm推奨)
- **Track too thin**: 最小0.127mm(0.2mm推奨)

## 参考リンク

- [M5Stack公式ドキュメント](https://docs.m5stack.com/en/core/m5stamp_pico)
- [ESP32-PICO-D4データシート](https://www.espressif.com/sites/default/files/documentation/esp32-pico-d4_datasheet_en.pdf)
- [JLCPCB製造能力](https://jlcpcb.com/capabilities/Capabilities)
- [KiCad公式ドキュメント](https://docs.kicad.org/)

## ライセンス

このフットプリントはMITライセンスの下で提供されます。商用・非商用問わず自由に使用できます。

---

**作成日**: 2026-01-14  
**バージョン**: 1.0  
**対応KiCadバージョン**: 6.0以降
