# [ESP32CAM](https://docs.m5stack.com/#/en/unit/esp32cam) ファームウェア

[English](https://github.com/m5stack/m5stack-cam-psram/blob/NoPsram/README.md) | [中文](https://github.com/m5stack/m5stack-cam-psram/blob/NoPsram/README_zh_CN.md) | 日本語

## ファームウェア概要

[esp32-camera](https://github.com/espressif/esp32-camera.git)をベースにしたこのリポジトリは[ESP32CAM](https://docs.m5stack.com/#/ja/unit/esp32cam)のファームウェアです。さらに、キャプチャされたフレームデータをより一般的な`BMP`および`JPEG`形式に変換するためのツールを提供します。

## ノート

現在、M5Stack向けには、4タイプ５種類のカメラユニットが存在します。 それぞれ [ESP32CAM](https://docs.m5stack.com/#/ja/unit/esp32cam)、[M5Camera (A Model)](https://docs.m5stack.com/#/ja/unit/m5camera)、[M5Camera (B Model)](https://docs.m5stack.com/#/ja/unit/m5camera)、M5CameraX、[M5CameraF](https://docs.m5stack.com/#/ja/unit/m5camera_f)です。

それぞれのカメラユニットの主な仕様の違いは **メモリ**, **インターフェース**, **レンズ**, **オプションのハードウェア**、そして**ケース**です。

**各カメラユニットに対応したリポジトリのブランチは以下の通りです:**

- [master](https://github.com/m5stack/m5stack-cam-psram/tree/master) -> M5Camera (B Model) / M5CameraX

- [ModeA](https://github.com/m5stack/m5stack-cam-psram/tree/ModeA) -> M5Camera (A Model)

- [NoPsram](https://github.com/m5stack/m5stack-cam-psram/tree/NoPsram) -> ESP32CAM

- [FishEye](https://github.com/m5stack/m5stack-cam-psram/tree/FishEye) -> M5CameraF

### カメラユニットの比較

比較表を以下に示します。

- **直接みる**場合は[こちら](https://shimo.im/sheets/gP96C8YTdyjGgKQC)。

- **download**する場合は[こちら](https://github.com/m5stack/M5-Schematic/blob/master/Units/m5camera/M5%20Camera%20Detailed%20Comparison.xlsx)。

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/camera_main_comparison_en.png">

####  A Model と B Model

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/diff_A_B.png">

### インターフェース比較

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/CameraPinComparison_en.png">

#### インターフェース差異（異なる部分のみ抜粋）

`Interface Comparison`の中からピン配列が異なるものだけ抜粋して、以下に示します。

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/CameraPinDifference_en.png">

## 重要なポイント

- JPEGまたはCIF以下の解像度の場合を除き、ドライバでPSRAMをアクティブにする必要があります。
- PSRAMへの書き込みは高速ではないため、YUVまたはRGBを使用するとチップに大きな負担がかかります。その結果、画像データが失われる可能性があります。WiFiが有効になっている場合、特に顕著です。RGBデータが必要な場合は、先にJPEGとしてキャプチャしてから、`fmt2rgb888`/`fmt2bmp`/`frame2bmp`を使用してRGBに変換することをお勧めします。
- 1フレームバッファを使用する場合、ドライバは現在のフレームが終了するのを待って（VSYNC）、I2S DMAを開始します。フレームが取得された後、I2Sは停止し、フレームバッファはアプリケーションに返されます。この方法ではシステムをより細かく制御できますが、フレームを取得する時間が長くなります。
- 2つ以上のフレームバッファが使用されている場合、I2Sは連続モードで動作しており、各フレームはアプリケーションがアクセスできるキューにプッシュされます。この方法では、CPU/メモリに負担がかかりますが、フレームレートを2倍にすることができます。 注意点としてJPEGのみで使用してください。

## インストール手順

- あなたのESP-IDFプロジェクトのコンポーネントフォルダーにリポジトリをクローンまたはダウンロードして解凍します。
- `Make`