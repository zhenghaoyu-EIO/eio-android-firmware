
# EIO Android Firmware – 开发环境快速搭建指南（中文）

[贡献指南](CONTRIBUTING_CN.md)

本指南适用于 **Windows 11/10** 与 **Linux（Ubuntu 22.04/24.04）**。  
目标：5‑15 分钟内跑起 *字幕 + 视频预览 Demo*，并基于 **main / feature** 分支模式开展协作。

> ⚠ 目前仅包含本地开发环境步骤，CI / OTA 等后续再补。

---

## 1. 环境总览

| 组件 | 版本 | Windows | Linux |
|------|------|---------|-------|
| JDK  | 17  | Android Studio 自带 | `sudo apt install openjdk-17-jdk` |
| Android Studio | Iguana / Hedgehog 稳定版 | `.exe` 安装 | Snap / tar.gz |
| Android SDK | **Android 9 (API 28)** 平台 + Build‑Tools 28.0.3 | SDK Manager 勾选 | 同左 |
| 硬件模拟 | 自定义 AVD 640×480 @160 dpi | HAXM / Hyper‑V | KVM |

---

## 2. 安装 Android Studio 与 SDK

### 2.1 Windows

1. 下载安装包并勾选：  
   * Android 9 (API 28) Platform  
   * Google APIs Intel x86_64 System Image  
   * Android Emulator  
2. SDK 默认路径：  
   `C:\Users\<name>\AppData\Local\Android\Sdk`

### 2.2 Linux

```bash
sudo snap install android-studio --classic
# 首次启动按提示装 SDK、工具
```

SDK Manager 中确认已安装：

* Android 9 Platform
* Build‑Tools 28.0.3
* Emulator, Platform‑Tools

---

## 3. 创建 640×480 AR_Panel AVD

1. Android Studio ▸ **Device Manager → Create Device → Phone → New Hardware Profile**  
2. 填写  
   * Resolution：`640 × 480`  
   * Density：`160 dpi`  
   * Screen size：`2.0 inch`  
   * Default orientation：`Landscape`  
3. 保存为 **AR_Panel_640x480**，系统镜像选 *API 28 Google APIs (x86_64)*  
4. 关闭 AVD 后编辑

```ini
# config.ini
hw.lcd.density=160
hw.initialOrientation=landscape
skin.dynamic=no

# hardware-qemu.ini
lcd.density = 160
```

5. 重启 AVD → 工具栏 **View ▸ Zoom ▸ 1:1** 验证像素比例正确。

---

## 4. 克隆仓库并运行 Demo

```bash
git clone https://github.com/zhenghaoyu-EIO/eio-android-firmware.git
cd eio-android-firmware
./gradlew :app:installDebug   # 或 IDE ▶ 运行
```

> 仓库目前为空，请将 Demo 代码复制进 `app/` 再编译。  
> 成功后会看到：中央 320×240 循环播放 `sample.mp4`，字幕 5 秒后淡入“Welcome / EIO Glasses User!”

---

## 5. 项目结构示例

```
app/
 ├─ src/main/java/com/example/eio_simulator/
 │   ├─ MainActivity.kt         # 入口
 │   ├─ VideoPreviewView.kt     # 循环播放 res/raw/sample.mp4
 │   ├─ SubtitleSwitcher.kt     # 字幕淡入淡出
 │   └─ LoadingBox.kt
 ├─ res/raw/sample.mp4
 └─ AndroidManifest.xml
```

---

## 6. 分支协作约定

* **main**：始终可构建演示 APK  
* **feature/<功能或任务名>**：开发分支  
  * 提交 PR → Code Review → squash 合并回 **main**  
  * 合并后删除 feature 分支

> 初期不设 dev 分支，保持流程简单。

---

## 7. Windows 常见问题

| 问题 | 解决方案 |
|------|----------|
| 模拟器提示 VT‑x 不可用 | 关闭 Hyper‑V，或使用 Windows 11 WSA 驱动 |
| ADB 找不到设备 | `adb kill-server && adb start-server` |
| Gradle 编译慢 | 把项目放 SSD，开启 Settings ▸ Gradle ▸ Parallel & Cache |

---

## 8. WSL 使用建议

* 代码可放 WSL：`\wsl$\Ubuntu\home\user\repo`  
* IDE（Windows 端）共享 SDK；Emulator 也在 Windows 跑  
* 构建由 Windows 端完成，可避免图形加速问题

---

## 9. 下一步

1. 将 `VideoPreviewView` 换成 `CameraPreviewView`，接入真实摄像头  
2. 按 Star1s 文档 §1 添加物理按键监听 (`dispatchKeyEvent`)  
3. 接入 §3 Microphone & §4 Speaker 示例，实现语音录制 & TTS  

如需更多帮助，请在仓库提 Issue 或使用企业 IM 联系平台组。
