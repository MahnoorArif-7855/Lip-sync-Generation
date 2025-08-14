# Alethea AI — Video Generation Assignment (Track A)

This repository contains my **Track A** submission for Alethea AI's Machine Learning Engineer assignment. It demonstrates a video generation pipeline with clean, reproducible steps and a final **MP4** output.

> Primary notebook: `alethea_track_A.ipynb`

---

## ✅ What’s Included
- A runnable notebook implementing the Track A pipeline
- Final rendered output video (MP4/HLS) (see **Outputs**)
- Documentation of trade-offs and known issues
- This README with installation and run instructions

## 🧩 Problems Covered
- Problem 1 per the PDF: input → generation → export

---

## 🗂️ Recommended Repo Structure
```
.
├── README.md
├── requirements.txt
├ alethea_track_A.ipynb
├── inputs/
├── outputs/
│   ├── final.mp4
```

---

## ⚙️ Environment & Prerequisites

- **Python**: 3.10+ (Jupyter/Colab supported)
- **System**: `ffmpeg` and `ffprobe` required
- **Python packages**:
- ffmpeg (system)
- TTS
- face-alignment==1.3.5
- numpy==1.26.4
- torch

Install deps:
```bash
pip install -r requirements.txt
```

Install FFmpeg (if required):
- Ubuntu/Debian: `sudo apt-get update && sudo apt-get install -y ffmpeg`
- macOS (Homebrew): `brew install ffmpeg`
- Windows (scoop): `scoop install ffmpeg`

---

## 🚀 How to Run
### Colab
- Open the notebook in Colab.
- Adjust any `/content/...` paths to `data/` and `outputs/` if needed.
- Enable GPU if generation uses models; CPU is fine for pure ffmpeg stages.
---

## 🎬 Inputs & Outputs
- **Inputs**: `inputs/` (place assets here)
- **Outputs**: `outputs/final.mp4` and optional HLS at `outputs/hls/`

 Paths in notebook:
```
/content/Wav2Lip/results/final.mp4
/content/final.mp4
/content/image_1.jpg
/content/speech.wav
/content/speech_long.wav
Saved -> /content/final.mp4
```

---

## 🧠 Pipeline Overview

Key functions (auto-detected):
```
(pipeline implemented directly in cells)
```

Typical stages:
1. Prepare inputs (frames/audio/models)
2. Generate or assemble frames at a consistent FPS
3. Compose clips, apply transitions, and align audio
4. Encode final MP4 (faststart)
5. Validate streams with ffprobe

---

## 🧪 Reproducibility
- Deterministic ffmpeg commands where applicable
- Notebook can be run top-to-bottom to regenerate the output

---

## 📄 Known Limitations / Trade-offs
- Ensure `ffmpeg`/`ffprobe` availability on `PATH`
- Normalize FPS/size/codecs to avoid desync or black frames
- If models are used, VRAM may constrain resolution/length; lower precision or tiling helps
- Colab path differences (`/content/...`) vs local `data/...`

---

## ✅ Submission Checklist
- [x] Public repo with organized structure
- [x] Functional video/demo link [(MP4)](https://drive.google.com/file/d/1U1aEEBHrA0pOhu0bwqne2CNABrMQGHUB/view?usp=sharing)
- [x] README with installation, usage, prereqs, and known issues
- [x] Trade-offs documented

---

## 📎 Attribution
- Python packages used in the notebook: TTS, torch

---

### (Notebook headers preview)
```
# Track A — Single Image → Lip‑Synced Video
## Weights: wav2lip_gan.pth
## Input Image
### Make the audio ~30–45s (optional)
### Notes / Known Issues
```
