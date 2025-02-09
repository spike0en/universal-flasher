# Flashing Tool Documentation

## User Guide

**Prerequisites**  
- Python 3.8+ installed (only if u are running python file)
- Unlocked bootloader on target device
- USB debugging enabled
- Windows/Linux/MacOS system

### Installation
```bash
# Clone repository
git clone https://github.com/PHATWalrus/universal-flasher.git
cd nothing-flasher

# Install requirements
pip install -r requirements.txt
```

### Requirements.txt
```txt
requests
colorama
packaging
pyinstaller# Only needed for EXE conversion
```

### Basic Usage
1. Place firmware files in project root:
   - boot.img
   - vendor_boot.img
   - dtbo.img
   - etc
2. Run script:
```bash
python flash.py
```
3. Follow on-screen prompts for:
   - Slot selection (A/B or current)
   - Data wipe confirmation
   - Partition flashing

**Key Features**  
- Automatic platform-tools setup
- Dual slot (A/B) support
- Log generation (`flash_log_*.txt`)
- Safety checks for bootloader status

## Developer Guide

### Device Adaptation
Modify these sections in `flash.py` for other devices:

**1. Partition Configuration**
```python
self.boot_partitions = ["boot", "vendor_boot", "dtbo"]
self.firmware_partitions = ["abl", "modem", "tz"]  # Add device-specific partitions
```

**2. Bootloader Requirements**
```python
def check_prerequisites(self):
    # Update device-specific checks
    checks = [
        ("Is your device bootloader unlocked?", True),
        ("Device in fastboot mode?", True)
    ]
```

**3. Device Detection**
```python
def device_checks(self):
    # Modify for device-specific identifiers
    result = self.run_command([self.fastboot_path, "devices"])
    if "your_device_codename" not in result.stdout:
        raise Exception("Unsupported device")
```

### Build as EXE with Auto PY to EXE

1. Install converter:
```bash
pip install auto-py-to-exe
```

2. Conversion settings:
```
Script command: pyinstaller --noconfirm --onefile --console  "C:\Users\harsh\Downloads\admin\v1.py"
Icon: custom.ico (optional)
```

3. Post-build considerations:
- Include platform-tools directory with EXE
- Test on clean system without Python
