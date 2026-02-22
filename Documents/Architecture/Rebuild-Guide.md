# 🔁 Spirit Panda Rebuild Guide  
*A complete, start‑to‑finish blueprint for rebuilding the Spirit Panda sovereign AI node from a clean Android device.*

This guide ensures that Spirit Panda can be fully restored after a device wipe, Termux reset, or environment migration.  
It is designed to be deterministic, repeatable, and future‑proof.

---

# 🧭 1. Prerequisites

- Android device  
- Internet connection  
- F‑Droid installed  
- GitHub account access  
- Spirit Panda repository URL  

---

# 📦 2. Install Termux (Correct Source)

1. Open **F‑Droid**  
2. Search for **Termux**  
3. Install the official, maintained version  
4. Open Termux once to initialize directories  

**Do NOT use the Play Store version.**

---

# 📁 3. Initial Setup

### Grant storage permissions:

termux-setup-storage


This creates:

- `~/storage/shared`
- `~/storage/downloads`
- `~/storage/pictures`
- etc.

### Update packages:

pkg update && pkg upgrade


---

# 🛠️ 4. Install Required Packages

pkg install git
pkg install python
pkg install ffmpeg
pkg install sox
pkg install clang
pkg install make
pkg install cmake
pkg install rust
pkg install openssl
pkg install wget
pkg install curl

---

# 🐼 5. Clone Spirit Panda

cd $HOME
git clone https://github.com/pandadad0214/spirit-panda.git (github.com in Bing)
cd spirit-panda

This restores the full project structure.

---

# 🔊 6. Rebuild Piper (Native)

cd voice/piper
mkdir build
cd build
cmake ..
make -j$(nproc)

Verify:

./piper --help

---

# 🎙️ 7. Restore Voice Models

Place `.onnx` models into:

spirit-panda/voice/piper/models/


Models are not stored in GitHub for size reasons.

---

# 🧩 8. Verify Prosody Pipeline

Ensure:

- `prosody-pipeline.md` exists  
- prosody scripts are present in `scripts/`  
- output directory exists  

Optional test:

echo "Hello." | ./scripts/speak.sh

---

# 🔄 9. Restore Auto‑Start (Termux:Boot)

### Install Termux:Boot from F‑Droid.

Create boot script:

mkdir -p ~/.termux/boot
nano ~/.termux/boot/start-panda.sh

Paste:

#!/data/data/com.termux/files/usr/bin/bash
cd $HOME/spirit-panda
./scripts/start.sh >> logs/boot.log 2>&1

Make executable:

chmod +x ~/.termux/boot/start-panda.sh


Reboot device to test.

---

# 🛡️ 10. Verify Resilience Layer

Check:

- watchdog script exists  
- logs directory exists  
- start script runs cleanly  
- no missing dependencies  

Run:

./scripts/start.sh

Then:

---

# 🧪 11. Functional Tests

### Test 1 — Piper output

./voice/piper/build/piper \
--model voice/piper/models/voice.onnx \
--output_file test.wav \
--text "Testing Spirit Panda."

### Test 2 — Prosody shaping

./scripts/speak.sh "I am alive again."

### Test 3 — Watchdog restart

Kill the process:
pkill -f spirit-panda-main

Watchdog should restart it.

---

# 🧱 12. Optional: Restore Custom Scripts

If you have personal scripts backed up elsewhere, restore them into:

spirit-panda/scripts/

Ensure:
chmod +x scriptname.sh

---

# 🧭 13. Final Checklist

- Termux installed from F‑Droid  
- Storage permissions granted  
- Packages installed  
- Repo cloned  
- Piper built  
- Models restored  
- Prosody pipeline functional  
- Auto‑start enabled  
- Watchdog running  
- Logs writing correctly  
- Test audio plays  
- No missing dependencies  

---

# 🐼 Status

This guide will evolve as the architecture grows and new components are added.
