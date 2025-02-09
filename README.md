# Universal Android ROM Flasher 🔄

![Python Version](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
[![Build and Release](https://github.com/PHATWalrus/universal-flasher/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/PHATWalrus/universal-flasher/actions/workflows/main.yml)

Next-gen Android flashing tool with multi-device support and enhanced safety features

## Features ✨

- **Multi-Device Support** via `devices.json` configuration
- **A/B Slot Management** with automatic partition handling
- **Platform-Tools Auto-Setup** (bundled or system)
- **Verified Boot (AVB) Control** with disable options
- **Windows Admin Privilege Handling** with UAC elevation
- **Cross-Platform Support** (Windows/Linux/macOS)
- **Comprehensive Logging** with timestamped records
- **Interactive Menu System** with color-coded UI
- **Smart Partition Resizing** for logical partitions
- **Device Compatibility Verification** via board checks

## Installation 📦

```bash
# Clone repository
git clone https://github.com/PHATWalrus/universal-flasher.git
cd universal-flasher

# Install requirements
pip install -r requirements.txt

# Download platform-tools (Windows)
python flash.py --setup
```

## Requirements 📋
```
requests
colorama
packaging
pyinstaller #(only if you want to make an compiled binary)
```

## Usage 🚀

1. **Prepare Firmware Files**:
   ```
   # Required base files
   boot.img
   vendor_boot.img
   dtbo.img
   
   # Optional firmware files
   abl.img modem.img tz.img
   ```

2. **Run Flasher**:
   ```
   python flash.py
   ```

3. **Follow Prompts**:
   - Select device from supported list
   - Choose slot strategy (A/B or current)
   - Confirm partition flashing
   - Handle AVB verification
   - Manage data wipe

![Menu Demo (soon)](https://www.youtube.com/watch?v=XfELJU1mRMg)

## Building from Source 🔨

**Create EXE with PyInstaller**:
```
pyinstaller --noconfirm --onefile --console --icon "icon.ico" --name flasher flash.py
```

**Build Automation (GitHub Actions)**:
```
# .github/workflows/build.yml
jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [windows-latest, ubuntu-latest]
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
      - run: pip install -r requirements.txt
      - run: pyinstaller --onefile --name flasher flash.py
```

## Device Configuration ⚙️

**`devices.json` Structure**:
```json
{
    "devices": [
      {
        "model": "Nothing Phone 2",
        "codename": "pong",
        "board": "taro",
        "partitions": {
          "boot": ["boot", "vendor_boot", "dtbo", "recovery"],
          "firmware": ["abl", "aop", "aop_config", "bluetooth", "cpucp", "devcfg", "dsp", "featenabler", "hyp", "imagefv", "keymaster", "modem", "multiimgoem", "multiimgqti", "qupfw", "qweslicstore", "shrm", "tz", "uefi", "uefisecapp", "xbl", "xbl_config", "xbl_ramdump"],
          "logical": ["system", "system_ext", "product", "vendor", "vendor_dlkm", "odm"],
          "vbmeta": ["vbmeta_system", "vbmeta_vendor"]
        },
        "slot_specific": ["boot", "vendor_boot", "dtbo", "recovery"]
      }
   ]
}
```
### How to find "board" variable
- **when in bootloader run the following command**
```bash
fastboot getvar product
```
- **copy the value after product:**
## Troubleshooting 🔧

**Common Issues**:
- **Missing .img Files**: Ensure required partitions exist in working directory
- **Admin Rights**: On Windows, run as Administrator for system PATH updates
- **Device Not Detected**:
  - Verify fastboot mode
  - Check USB drivers
  - For Linux: Configure udev rules
- **Board Mismatch**: Double-check device configuration in `devices.json`

**Log Analysis**:
```bash
# Check generated log files
tail -f flash_log_20250209_1600.txt
```

## Disclaimer ⚠️
Use at your own risk. Always maintain backup of critical data before flashing. The authors are not responsible for any bricked devices.

---

📝 **License**: MIT | 💻 **Requirements**: Python 3.10+ | 📦 **Dependencies**: See requirements.txt
