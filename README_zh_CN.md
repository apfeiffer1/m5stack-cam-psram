# [ESP32CAM](https://docs.m5stack.com/#/en/unit/esp32cam) 固件

[English](https://github.com/m5stack/m5stack-cam-psram/blob/NoPsram/README.md) | 中文 | [日本語](https://github.com/m5stack/m5stack-cam-psram/blob/NoPsram/README_ja.md)

## 固件说明

这个基于[esp32-camera](https://github.com/espressif/esp32-camera.git)的存储库是[ESP32CAM](https://docs.m5stack.com/#/zh_CN/unit/esp32cam)的固件。此外，它还提供了一些工具，可以将捕获的帧数据转换为更常见的BMP和JPEG格式。

## 注意

现在，M5Stack有四种类型的摄像机单元，分别有[ESP32CAM](https://docs.m5stack.com/#/zh_CN/unit/esp32cam)，[M5Camera (A Model)](https://docs.m5stack.com/#/zh_CN/unit/m5camera)，[M5Camera (B Model)](https://docs.m5stack.com/#/zh_CN/unit/m5camera)，M5CameraX，[M5CameraF](https://docs.m5stack.com/#/zh_CN/unit/m5camera_f)。

这些相机之间的主要区别是**内存**，**接口**，**镜头**，**可选硬件**和**相机外壳**。

**不同的分支对应于不同版本的硬件:**

- [master](https://github.com/m5stack/m5stack-cam-psram/tree/master) -> M5Camera (B Model) / M5CameraX

- [ModeA](https://github.com/m5stack/m5stack-cam-psram/tree/ModeA) -> M5Camera (A Model)

- [NoPsram](https://github.com/m5stack/m5stack-cam-psram/tree/NoPsram) -> ESP32CAM

- [FishEye](https://github.com/m5stack/m5stack-cam-psram/tree/FishEye) -> M5CameraF

### 不同版本相机的比较

下图是他们的对照表。 （注意：因为接口有很多不同的引脚，所以我做了一个单独的表进行比较。）

- 如果你想**查看**与它们的详细区别，请点击[这里](https://shimo.im/sheets/gP96C8YTdyjGgKQC)。

- 如果您想**下载**细节差异，请点击[这里](https://github.com/m5stack/M5-Schematic/blob/master/Units/m5camera/M5%20Camera%20Detailed%20Comparison.xlsx)。

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/camera_main_comparison_en.png">

#### A模型和B模型的图片

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/diff_A_B.png">

### 界面比较

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/CameraPinComparison_en.png">

#### 界面差异

下表显示了基于`Interface Comparison`表的相机电路板之间的接口差异。

<img src="https://m5stack.oss-cn-shenzhen.aliyuncs.com/image/m5-docs_table/camera_comparison/CameraPinDifference_en.png">

## 重要的是要记住

- 除了使用带有JPEG或CIF更低分辨率外，驱动程序还需要安装和激活PSRAM。
- 使用YUV或RGB会给芯片带来很大的压力，因为写入PSRAM并不是特别快。 结果是图像数据可能丢失。 如果启用WiFi，则尤其如此。 如果您需要RGB数据，建议使用`fmt2rgb888`或`fmt2bmp`/`frame2bmp`捕获JPEG然后转换为RGB。
- 当使用1帧缓冲区时，驱动程序将等待当前帧完成（VSYNC）并启动I2S DMA。 获取帧后，I2S将停止，帧缓冲区返回到应用程序。 这种方法可以更好地控制系统，但会导致获得帧的时间更长。
- 当使用2个或更多帧缓冲时，I2S以连续模式运行，每个帧被推送到应用程序可以访问的队列。 这种方法会给CPU/Memory带来更大的压力，但允许帧速率加倍。 请仅使用JPEG。

## 安装说明

- 克隆或下载并将存储库解压缩到ESP-IDF项目的components文件夹中
- `Make`