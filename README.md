
# EIO Android Firmware – Development Environment Setup

[中文文档](README_CN.md)
[Contributing Guide](CONTRIBUTING_EN.md)

This guide shows **Windows 11/10** and **Linux (Ubuntu 22.04/24.04)** developers how to
set up exactly the same Android 9 (API 28) tool‑chain that Star1s firmware uses.
After finishing the steps you will be able to run the **Subtitle + Video demo**
and start new *feature/* branches on top of **main**.

> *CI / OTA / BLE topics will be documented later.*

---

## 1. Prerequisites

| Item | Version | Windows | Linux |
|------|---------|---------|-------|
| JDK  | 17      | OpenJDK 17 (bundled with Android Studio) | `sudo apt install openjdk-17-jdk` |
| Android Studio | Iguana / Hedgehog | `.exe` installer | Snap / Tarball |
| Android SDK | **Android 9 (API 28)** Platform + Build‑Tools 28.0.3 | install via SDK Manager | same |
| Disk space | ≥ 15 GB | | |

---

## 2. Install Android Studio & SDK

### 2.1 Windows 11 / 10

1. Download Android Studio installer  
2. During “Component Setup” tick **Android SDK, SDK Platform 28, Google APIs, Android Emulator**  
3. Finish wizard – the SDK is placed at  
   `C:\Users\<you>\AppData\Local\Android\Sdk`

### 2.2 Linux (Ubuntu 22.04 / 24.04)

```bash
sudo snap install android-studio --classic
android-studio    # first launch – accept SDK licence
```

Inside SDK Manager install:

* **Android 9 (API 28) Platform**
* **Android SDK Build‑Tools 28.0.3**
* **Android Emulator**, **Platform‑Tools**

---

## 3. Create Star1s AVD (640 × 480 @160 dpi)

1. **Device Manager → Create Device → Phone → New Hardware Profile**  
2. Fill in  
   * Resolution `640 × 480`  
   * Screen size `2.0 inch`  
   * **dpi `160`**  
   * Default orientation `Landscape`  
3. Save as **AR_Panel_640x480** (API 28 system image, Google APIs, x86_64)  
4. *Shut down* the AVD once so that config files are created, then edit  
   `~/.android/avd/AR_Panel_640x480.avd/config.ini`

   ```ini
   hw.lcd.density=160
   hw.initialOrientation=landscape
   skin.dynamic=no
   ```

   and **hardware‑qemu.ini**

   ```ini
   lcd.density = 160
   ```

5. Restart AVD → Toolbar → **View ▷ Zoom ▷ 1 : 1** – you should see a true‑size
   640 × 480 panel.

---

## 4. Clone & run the demo project

```bash
git clone https://github.com/zhenghaoyu-EIO/eio-android-firmware.git
cd eio-android-firmware
# first build (Studio will suggest to 'Sync'):
./gradlew :app:installDebug   # or press ▶️ Run in IDE
```

*The repository is currently empty – copy the provided demo code into `app/` and commit.*

### What you should see

* Grey translucent background  
* 320 × 240 black rectangle in the centre playing **`res/raw/sample.mp4`** on loop  
* Subtitle area below the video:  
  * displays “Initializing…/Please Wait…” for 5 s  
  * fades to “Welcome / EIO Glasses User!”

---

## 5. Project layout (when you copy demo inside)

```
app/
 ├─ src/main/java/com/example/eio_simulator/
 │   ├─ MainActivity.kt
 │   ├─ VideoPreviewView.kt
 │   ├─ SubtitleSwitcher.kt
 │   └─ LoadingBox.kt
 ├─ res/raw/sample.mp4
 └─ AndroidManifest.xml
```

All Compose code lives under that package.

---

## 6. Branch workflow

* **main** – always buildable demo / integration target  
* **feature/<ticket or topic>** – each task / bug‑fix lives here  
  * open Pull Request → review → squash merge into **main**  
  * delete feature branch when merged

> There is **no `dev` branch** in this simplified early‑stage flow.

---

## 7. Windows–specific tips

| Issue | Fix |
|-------|-----|
| Emulator says **“VT‑x not available”** | Disable Hyper‑V or use Windows 11 WSA‑driver hypervisor |
| ADB device not found | `adb kill-server && adb start-server` |
| Slow Gradle on HDD | Enable **Gradle ▸ Settings ▸ Parallel & Caching**, keep project on SSD |

---

## 8. Linux / WSL notes

*You can develop entirely in WSL2 using Windows IDE:*

1. Store source under WSL (`\wsl$\Ubuntu\home\user\repo`)  
2. Set Android SDK path to Windows side to reuse packages  
3. Build inside IDE; emulator runs on Windows (HAXM/Hyper‑V)

---

## 9. Next steps

* Replace `VideoPreviewView` with real CameraX preview when hardware is available  
* Integrate Event‑Key listener sample (Star1s doc §1)  
* Add Microphone & Speaker APIs (doc §3/§4)  

> Keep feature branches small and focused – merge early & often.  
