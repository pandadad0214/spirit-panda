# 🔊 Piper Native Build Notes  
*A complete reference for building and running Piper TTS natively on Android via Termux.*

This document captures the full process, lessons learned, and configuration patterns for compiling and running Piper TTS directly on-device.  
It ensures future rebuilds are reproducible, stable, and optimized for Spirit Panda’s expressive voice pipeline.

---

## 🎯 1. Purpose of the Native Build

Piper is used as the core TTS engine for Spirit Panda because it provides:

- low-latency inference  
- high-quality voices  
- local-first sovereignty  
- full control over prosody  
- modifiable source code  
- predictable performance on Android  

A native build avoids:
- Python overhead  
- slow startup times  
- dependency bloat  
- inconsistent behavior across devices  

---

## 📦 2. Required Dependencies

Install the following packages in Termux:

pkg install git
pkg install clang
pkg install cmake
pkg install make
pkg install rust
pkg install python
pkg install ffmpeg
pkg install sox

Rust is required for model compilation and certain build steps.

---

## 📁 3. Directory Structure

Recommended layout inside the Spirit Panda project:

spirit-panda/
│
├── voice/
│   ├── piper/
│   │   ├── build/
│   │   ├── models/
│   │   └── src/
│   └── output/
│
└── scripts/


- `piper/` contains the cloned repo and build artifacts  
- `models/` stores `.onnx` voice models  
- `output/` is used for debugging audio or prosody tests  

---

## 🧩 4. Cloning Piper

cd $HOME/spirit-panda/voice
git clone https://github.com/rhasspy/piper.git
cd piper

---

## 🛠️ 5. Building Piper (Native)

### **Standard build:**

mkdir build
cd build
cmake ..
make -j$(nproc)


This produces the `piper` binary inside `build/`.

### **Notes:**

- Termux’s Clang works reliably  
- No static linking (Termux does not support static libs)  
- Dynamic linking is automatic and correct  

---

## 🧪 6. Testing the Build

Example test command:

./piper \
--model ../models/en_US-libritts-high.onnx \
--output_file test.wav \
--text "Hello from Spirit Panda."


Verify:

- audio file is created  
- no missing library errors  
- inference speed is acceptable  

---

## 🎙️ 7. Voice Models

Models are stored in:

spirit-panda/voice/piper/models/


Typical model types:

- `en_US-libritts-high.onnx`  
- `en_US-amy-medium.onnx`  
- custom fine-tuned models (future)  

### **Model placement rule:**

Keep models **inside the project**, not in shared storage.

---

## 🎛️ 8. Configuration for Spirit Panda

Piper is wrapped by Spirit Panda’s prosody pipeline.  
Typical invocation pattern:

piper \
--model $SPANDA_HOME/voice/piper/models/voice.onnx \
--output_file $SPANDA_HOME/voice/output/output.wav \
--text "$GENERATED_TEXT"


Prosody shaping happens **before** this step.

---

## 🧩 9. Integration With Prosody Pipeline

The prosody engine outputs:

- modified text  
- SSML-like markers  
- pacing hints  
- emotional cues  

Piper receives the final processed text.

This will be fully documented in `prosody-pipeline.md`.

---

## 🐞 10. Troubleshooting Notes

### **Missing libraries**
If Piper fails with missing `.so` files:

ldd build/piper


Then reinstall:

pkg install libandroid-support
pkg install libandroid-glob


### **Slow inference**
Use a smaller model or enable:

--sentence_silence 0.2


### **Audio artifacts**
Check:
- sample rate  
- ffmpeg version  
- model quality  

---

## 🧭 11. Notes for Future Rebuild

- Always build Piper natively (avoid Python wrapper)  
- Keep models inside the project directory  
- Document any custom flags in the dev log  
- Rebuild after major Termux updates  
- Test with multiple models to verify stability  

---

## 🐼 Status

This document will evolve as the voice stack becomes more expressive and integrated with the prosody engine.
