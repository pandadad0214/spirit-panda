# 🎙️ Adaptive Prosody Pipeline  
*A complete reference for Spirit Panda’s expressive, emotionally intelligent voice system.*

This document captures the design, logic, and shaping patterns behind Spirit Panda’s adaptive prosody engine.  
It ensures future rebuilds preserve the expressive quality, emotional nuance, and natural pacing that define the Panda voice.

---

## 🎯 1. Purpose of the Prosody Pipeline

The prosody engine transforms plain text into expressive, emotionally aligned speech by shaping:

- rhythm  
- pacing  
- emphasis  
- tone  
- emotional coloration  
- pauses  
- breath patterns  

Piper provides the raw voice.  
The prosody pipeline provides the *soul*.

---

## 🧩 2. High-Level Flow

[Input Text]
↓
[Context Analyzer]
↓
[Emotion & Intent Mapper]
↓
[Prosody Shaping Engine]
↓
[SSML-like Markup Generator]
↓
[Piper TTS]
↓
[Final Audio Output]


Each stage is modular and replaceable.

---

## 🧠 3. Context Analyzer

The analyzer inspects the input text for:

- sentence type (statement, question, exclamation)  
- emotional cues  
- pacing hints  
- keywords (e.g., “wait”, “listen”, “okay”, “hmm”)  
- punctuation patterns  
- length and complexity  

Outputs a context profile:

{
"tone": "neutral",
"pace": "medium",
"energy": "low",
"emphasis_targets": ["important", "keywords"]
}


---

## ❤️ 4. Emotion & Intent Mapper

Maps context to emotional states such as:

- warm  
- playful  
- serious  
- excited  
- calm  
- reflective  
- empathetic  

Each emotion has parameters:

emotion_profile = {
"pitch_shift": +0.2,
"speed_factor": 0.95,
"pause_weight": 1.1,
"emphasis_strength": 1.3
}


These values are applied downstream.

---

## 🎛️ 5. Prosody Shaping Engine

This is the core of the system.

### **Shaping includes:**

- inserting micro‑pauses  
- adjusting pacing based on sentence length  
- adding emphasis markers  
- smoothing transitions  
- adding “breath space” between emotional beats  
- modulating pitch and speed  

### **Example transformation:**

**Input:**  
I’m really proud of you.

**Shaped:**  
<prosody pitch="+0.1" rate="0.95">
I’m <emphasis>really</emphasis> proud of you.
</prosody>

---

## 🧪 6. SSML‑Like Markup Generator

Piper does not support full SSML, so the pipeline outputs a simplified markup that the wrapper interprets.

### **Supported markers:**

<break time="200ms">
<emphasis>
<prosody pitch="+0.2" rate="0.9">


These are converted into:

- text pacing  
- silence insertion  
- pitch/speed adjustments  
- emphasis weighting  

---

## 🔊 7. Integration With Piper

Final invocation pattern:

piper \
--model $VOICE_MODEL \
--output_file $OUTPUT \
--text "$SHAPED_TEXT"


The shaped text is fully processed before Piper receives it.

---

## 🎼 8. Example Prosody Profiles

### **Warm:**

pitch +0.1
speed 0.95
pause_weight 1.2


### **Playful:**

pitch +0.3
speed 1.05
pause_weight 0.9


### **Serious:**

pitch -0.1
speed 0.9
pause_weight 1.3


### **Reflective:**

pitch -0.05
speed 0.85
pause_weight 1.5


These profiles evolve over time.

---

## 🐞 9. Troubleshooting Notes

### **Flat delivery**
- emotion profile not applied  
- punctuation missing  
- emphasis targets not detected  

### **Too fast**
- speed_factor too high  
- pause_weight too low  

### **Choppy output**
- excessive break tags  
- mismatched emphasis markers  

---

## 🧭 10. Notes for Future Rebuild

- Keep prosody logic modular  
- Store emotion profiles in JSON for easy tuning  
- Add a “learning layer” later to adapt to user preferences  
- Consider adding breath simulation for realism  
- Document all changes in the dev log  

---

## 🐼 Status

This document will expand as the prosody engine becomes more expressive and gains new emotional capabilities.
