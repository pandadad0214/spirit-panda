# 🧱 Termux Environment Overview  
*A complete reference for the Spirit Panda sovereign AI node environment on Android.*

This document captures the full structure, patterns, and lessons learned from the current Termux setup.  
It exists to ensure that future rebuilds are clean, reproducible, and resilient.

---

## 📦 1. Termux Installation Source

**Source:** F‑Droid (official, maintained version)  
**Reason:**  
- Play Store version is abandoned  
- Missing critical updates  
- Breaks modern packages  
- Causes permission inconsistencies  

---

## 📁 2. Directory Structure (Current Working Layout)

$HOME/
│
├── spirit-panda/
│   ├── models/
│   ├── logs/
│   ├── memory/
│   ├── voice/
│   ├── scripts/
│   └── config/
│
├── .termux/
│   ├── boot/
│   └── properties
│
└── storage/
├── downloads/
├── dcim/
├── pictures/
└── shared/

**Notes:**  
- `$HOME/spirit-panda/` is the main node directory  
- `.termux/boot/` contains auto‑start scripts  
- `storage/` is symlinked to Android shared storage  

---

## 🔐 3. Permissions & Storage Setup

### **Commands used:**

Termux Setup Storage

This creates:

- `~/storage/shared` → `/sdcard/`  
- `~/storage/downloads` → `/sdcard/Download/`  
- and other Android‑linked directories

### **Known quirks:**

- Android sometimes revokes storage permissions after updates  
- Termux may require re‑granting permissions manually  
- Some devices require toggling “Allow access to all files”  

---

## 🔄 4. Auto‑Start & Watchdog Logic

### **Termux:Boot integration**

Scripts placed in:

~/.termux/boot/

run automatically on device boot.

### **Typical boot script pattern:**

#!/data/data/com.termux/files/usr/bin/bash
cd $HOME/spirit-panda
./scripts/start.sh >> logs/boot.log 2>&1

### **Watchdog pattern (conceptual):**

- Loop checks if service is running  
- If not, restart  
- Log failures  
- Avoid infinite restart loops  

This will be fully documented in the resilience file later.

---

## 🧰 5. Installed Packages (Baseline)

A typical Spirit Panda environment includes:

pkg install python
pkg install git
pkg install ffmpeg
pkg install sox
pkg install clang
pkg install make
pkg install cmake
pkg install rust
pkg install openssl
pkg install wget
pkg install curl

Additional packages may be added depending on the build.

---

## 🧱 6. Environment Variables

Common variables used:
export SPANDA_HOME=$HOME/spirit-panda
export PATH=$PATH:$SPANDA_HOME/scripts
export LD_LIBRARY_PATH=$PREFIX/lib

These will be refined during the rebuild.

---

## 🧪 7. Verification Steps

After setup, verify:
python --version
ffmpeg -version
sox --version
clang --version
rustc --version


And confirm:

- storage access works  
- boot scripts run  
- logs are created  
- models load correctly  

---

## 🧭 8. Notes for Future Rebuild

- Always install Termux from F‑Droid  
- Run `termux-setup-storage` immediately  
- Keep all scripts inside `$HOME/spirit-panda/scripts/`  
- Use `.termux/boot/` for auto‑start  
- Avoid storing models in shared storage  
- Keep logs inside the project directory  
- Document every change in the dev log  

---

## 🐼 Status

This document will evolve as the environment is rebuilt and refined.

