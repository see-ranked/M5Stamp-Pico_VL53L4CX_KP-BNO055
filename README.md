# M5Stamp Pico + VL53L4CX + KP-BNO055 KiCad Project

このプロジェクトは、M5Stamp-Pico、ToF距離センサ「VL53L4CX」、9軸センサモジュール「KP-BNO055」で構成された回路設計のKiCadプロジェクトです。

## プロジェクト構成

```
m5stamp-pico_VL53L4CX_KP-BNO055/
├── Circuit/              # KiCad回路設計ファイル
│   ├── Circuit.kicad_pro # プロジェクトファイル
│   ├── Circuit.kicad_sch # 回路図ファイル
│   ├── Circuit.kicad_pcb # PCBレイアウトファイル
│   ├── Circuit.kicad_dru # デザインルールファイル（JLCPCB用）
│   ├── sym-lib-table     # シンボルライブラリテーブル
│   └── fp-lib-table      # フットプリントライブラリテーブル
└── Library/              # カスタムライブラリ
    ├── M5Stamp-Pico-Board/
    ├── KP-BNO055/
    ├── A295-CTRPB-1/     # USB Type-Cコネクタ
    ├── TCA9548A/         # I2Cマルチプレクサ
    └── SS12D00G3/        # スライドスイッチ
```

## 使用コンポーネント

- **M5Stamp-Pico**: ESP32-PICO-D4搭載の小型開発ボード
- **VL53L4CX**: STMicroelectronics製ToF距離センサ
- **KP-BNO055**: Bosch製9軸センサモジュール（加速度・ジャイロ・地磁気）
- **TCA9548A**: I2Cマルチプレクサ（複数のI2Cデバイス管理用）
- **A295-CTRPB-1**: USB Type-Cコネクタ（秋月電子通商）
- **SS12D00G3**: スライドスイッチ

## 必要なソフトウェア

- **KiCad 9.0以降**: [https://www.kicad.org/](https://www.kicad.org/)

## 回路図の確認方法

1. **KiCadを起動**
   - KiCadを開きます

2. **プロジェクトを開く**
   - `File` → `Open Project...` を選択
   - `Circuit/Circuit.kicad_pro` を開きます

3. **回路図エディタを開く**
   - プロジェクトマネージャーで `Circuit.kicad_sch` をダブルクリック
   - または、`Schematic Editor` アイコンをクリック

4. **回路図を確認**
   - マウスホイールでズーム
   - マウス中ボタンドラッグでパン（移動）
   - 各コンポーネントの接続を確認できます

5. **電気ルールチェック（ERC）の実行**（オプション）
   - `Inspect` → `Electrical Rules Checker...` を選択
   - `Run ERC` をクリックして回路のエラーをチェック

## PCBレイアウトの確認方法

1. **PCBエディタを開く**
   - プロジェクトマネージャーで `Circuit.kicad_pcb` をダブルクリック
   - または、`PCB Editor` アイコンをクリック

2. **PCBレイアウトを確認**
   - マウスホイールでズーム
   - マウス中ボタンドラッグでパン（移動）
   - 基板の外形、部品配置、配線を確認できます

3. **3Dビューアで確認**（オプション）
   - `View` → `3D Viewer` を選択
   - 基板の3Dモデルを確認できます
   - マウスドラッグで回転、ホイールでズーム

4. **デザインルールチェック（DRC）の実行**（オプション）
   - `Inspect` → `Design Rules Checker...` を選択
   - `Run DRC` をクリックして製造可能性をチェック
   - JLCPCB用のデザインルール（`Circuit.kicad_dru`）が適用されています

## JLCPCBへの発注手順

### 1. ガーバーファイルの生成

1. **PCBエディタを開く**
   - `Circuit.kicad_pcb` を開きます

2. **ガーバーファイルを出力**
   - `File` → `Fabrication Outputs` → `Gerbers (.gbr)...` を選択
   - 出力先ディレクトリを指定（例: `Circuit/gerber/`）
   - 以下のレイヤーが選択されていることを確認:
     - `F.Cu` (表面銅箔)
     - `B.Cu` (裏面銅箔)
     - `F.Paste` (表面ペースト)
     - `B.Paste` (裏面ペースト)
     - `F.Silkscreen` (表面シルク)
     - `B.Silkscreen` (裏面シルク)
     - `F.Mask` (表面レジスト)
     - `B.Mask` (裏面レジスト)
     - `Edge.Cuts` (基板外形)
   - `Plot` をクリック

3. **ドリルファイルを出力**
   - 同じダイアログで `Generate Drill Files...` をクリック
   - `Drill File Format`: `Excellon` を選択
   - `Map File Format`: `Gerber` を選択（オプション）
   - `Generate Drill File` をクリック
   - ダイアログを閉じる

### 2. ガーバーファイルのZIP圧縮

1. **ガーバーファイルをまとめる**
   - 生成されたすべての `.gbr` ファイルと `.drl` ファイルを選択
   - ZIPファイルに圧縮（例: `Circuit_gerber.zip`）

### 3. JLCPCBでの発注

1. **JLCPCBサイトにアクセス**
   - [https://jlcpcb.com/](https://jlcpcb.com/) を開く

2. **ガーバーファイルをアップロード**
   - `Order Now` または `Quote Now` をクリック
   - `Add gerber file` ボタンをクリック
   - 作成したZIPファイルをアップロード

3. **基板仕様を確認・設定**
   - アップロード後、自動的に基板サイズが検出されます
   - 以下の項目を確認・設定:
     - **PCB Qty**: 基板枚数（最小5枚）
     - **Layers**: 2層
     - **PCB Thickness**: 1.6mm（標準）
     - **PCB Color**: お好みの色（緑が最安）
     - **Surface Finish**: HASL（標準）または ENIG（推奨）
     - **Copper Weight**: 1oz
     - **Remove Order Number**: Yes（基板に注文番号を印字しない場合）

4. **SMT実装サービス（オプション）**
   - 部品実装を依頼する場合:
     - `SMT Assembly` を `Yes` に設定
     - BOMファイルとCPLファイル（部品配置ファイル）が必要
     - ※このプロジェクトではBOM/CPLファイルは含まれていません

5. **カートに追加**
   - 仕様を確認後、`Save to Cart` をクリック

6. **チェックアウト**
   - カートを確認し、`Secure Checkout` をクリック
   - 配送先住所、配送方法、支払い方法を入力
   - 注文を確定

### 4. 製造・配送

- **製造期間**: 通常2-3営業日
- **配送期間**: 配送方法により異なる（DHL: 3-5日、OCS: 7-15日など）
- **追跡**: 注文後、追跡番号がメールで送られます

## デザインルールについて

このプロジェクトには、JLCPCB 2層基板用のデザインルール（`Circuit.kicad_dru`）が含まれています。

- **最小トレース幅**: 0.127mm (5mil)
- **最小クリアランス**: 0.127mm (5mil)
- **最小ビア径**: 0.3mm（ドリル径: 0.2mm）

これらのルールはJLCPCBの標準仕様に準拠しています。

## ライブラリについて

`Library/` ディレクトリには、このプロジェクトで使用するカスタムコンポーネントのKiCadライブラリが含まれています。各ライブラリには個別のREADMEファイルがあり、詳細な情報が記載されています。

- **M5Stamp-Pico-Board**: M5Stamp Pico用のシンボルとフットプリント
- **KP-BNO055**: 9軸センサモジュール用のライブラリ
- **A295-CTRPB-1**: USB Type-Cコネクタ（秋月電子通商）
- **TCA9548A**: I2Cマルチプレクサ用のライブラリ
- **SS12D00G3**: スライドスイッチ用のライブラリ

## トラブルシューティング

### ライブラリが見つからない

- `sym-lib-table` と `fp-lib-table` が正しく設定されているか確認
- ライブラリパスが `${KIPRJMOD}/../Library/` を使用して相対パスで指定されているか確認

### DRCエラーが出る

- JLCPCB用のデザインルール（`Circuit.kicad_dru`）が読み込まれているか確認
- `File` → `Board Setup` → `Design Rules` で設定を確認

### ガーバーファイルが正しく生成されない

- すべての必要なレイヤーが選択されているか確認
- ドリルファイル（`.drl`）も生成されているか確認

## ライセンス

このプロジェクトのライセンスについては、プロジェクトオーナーにお問い合わせください。

## 貢献

バグ報告や改善提案は、GitHubのIssuesでお願いします。

---

**注意**: このREADMEは、KiCad 9.0を基準に作成されています。他のバージョンでは手順が異なる場合があります。
