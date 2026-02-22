# 🛡️ Resilience & Self‑Healing Patterns  
*A complete reference for Spirit Panda’s watchdog logic, auto‑restart behavior, and stability philosophy.*

This document captures the patterns, scripts, and engineering principles that keep Spirit Panda stable on Android — even under storage revocations, app kills, low‑memory events, or unexpected shutdowns.

The goal is simple:  
**If the device is on, Spirit Panda is alive.**

---

## 🎯 1. Purpose of the Resilience Layer

Android is hostile to long‑running local services.  
Termux sessions die.  
Permissions get revoked.  
Apps get killed in the background.  
Storage paths change.  
Processes get frozen.

The resilience layer ensures:

- automatic recovery  
- watchdog monitoring  
- clean restarts  
- log‑based debugging  
- graceful degradation  
- zero‑touch operation  

This is the backbone of a sovereign AI node.

---

## 🔄 2. Boot‑Time Auto‑Start Pattern

Using **Termux:Boot**, any script placed in:

~/.termux/boot/


runs automatically when the device powers on.

### **Example boot script:**

#!/data/data/com.termux/files/usr/bin/bash
cd $HOME/spirit-panda
./scripts/start.sh >> logs/boot.log 2>&1


This ensures Spirit Panda starts **without user interaction**.

---

## 🐾 3. The Watchdog Loop (Conceptual)

The watchdog is a lightweight loop that:

1. Checks if the main service is running  
2. If not, restarts it  
3. Logs the event  
4. Sleeps briefly  
5. Repeats forever  

### **Pseudocode:**

while true; do
if ! pgrep -f "spirit-panda-main"; then
echo "$(date) — Restarting Spirit Panda" >> logs/watchdog.log
./scripts/start.sh
fi
sleep 5
done


This prevents silent failures.

---

## 🧹 4. Crash Recovery Pattern

When a crash is detected:

- watchdog restarts the service  
- logs capture the failure  
- memory state is preserved  
- partial outputs are cleaned  
- the system returns to a known‑good state  

### **Crash log example:**

2026-02-21 14:32:10 — Crash detected: exit code 139
2026-02-21 14:32:10 — Restarting Spirit Panda


---

## 🧪 5. Health Checks

The system performs lightweight checks such as:

- verifying model files exist  
- verifying storage is mounted  
- verifying Termux has permissions  
- verifying Piper binary is executable  
- verifying logs directory is writable  
If a critical resource is missing, the system refuses to start and logs the reason.

---

## 🧱 6. Log‑Based Debugging

All resilience components write to:

### **Example check:**

[ -f "$SPANDA_HOME/voice/piper/models/voice.onnx" ] || exit 1

spirit-panda/logs/


Typical logs:

- `boot.log`  
- `watchdog.log`  
- `errors.log`  
- `piper.log`  
- `prosody.log`  

Logs are rotated manually or during rebuilds.

---

## 🧰 7. Graceful Shutdown Pattern

Before stopping:

- flush memory buffers  
- close file handles  
- stop Piper cleanly  
- write a shutdown entry to logs  

This prevents corruption and partial writes.

---

## 🧬 8. Stability Philosophy

The system follows these principles:

### **1. Fail fast**  
If something is wrong, exit immediately and let the watchdog handle recovery.

### **2. Log everything**  
Silent failures are the enemy.

### **3. Keep scripts simple**  
Complexity increases fragility.

### **4. Avoid external dependencies**  
Local‑first means local‑reliable.

### **5. Prefer deterministic behavior**  
Same input → same output → easier debugging.

---

## 🧭 9. Notes for Future Rebuild

- Keep watchdog logic minimal and readable  
- Add a heartbeat file for external monitoring  
- Consider adding a “panic mode” with reduced features  
- Expand health checks as the system grows  
- Document all resilience changes in the dev log  

---

## 🐼 Status

This document will evolve as Spirit Panda gains more autonomous recovery capabilities.
