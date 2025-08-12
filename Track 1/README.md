# Alethea AI — Video Generation Assignment (Track A)

This repository contains my **Track A** submission for Alethea AI's Machine Learning Engineer assignment. It demonstrates a video generation pipeline with clean, reproducible steps and a final **MP4/HLS** output.

> Primary notebook: `alethea_track_A.ipynb`

---

## ✅ What’s Included
- A runnable notebook implementing the Track A pipeline
- Final rendered output video (MP4/HLS) (see **Outputs**)
- Documentation of trade-offs and known issues
- This README with installation and run instructions

## 🧩 Problems Covered
- Problem 1 and Problem 2 per the PDF: input → generation → export

If the PDF uses different labels, the above covers the required **generation + export** components.

---

## 🗂️ Recommended Repo Structure
```
.
├── README.md
├── requirements.txt
├── notebooks/
│   └── alethea_track_A.ipynb
├── data/
│   ├── inputs/
│   └── intermediates/
├── outputs/
│   ├── final.mp4
│   └── hls/
└── scripts/
    └── export_hls.sh
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

### Local (Jupyter/VS Code)
1. Create folders:
   ```bash
   mkdir -p notebooks data/inputs data/intermediates outputs
   ```
2. Place inputs (video/audio/frames) in `data/inputs/`.
3. Open and execute:
   ```bash
   jupyter notebook notebooks/alethea_track_A.ipynb
   ```
4. Final outputs are saved to `outputs/final.mp4`.

### Colab
- Open the notebook in Colab.
- Adjust any `/content/...` paths to `data/` and `outputs/` if needed.
- Enable GPU if generation uses models; CPU is fine for pure ffmpeg stages.

### Optional: Export HLS
```bash
bash scripts/export_hls.sh outputs/final.mp4 outputs/hls
```

---

## 🎬 Inputs & Outputs
- **Inputs**: `data/inputs/` (place assets here)
- **Intermediates**: `data/intermediates/`
- **Outputs**: `outputs/final.mp4` and optional HLS at `outputs/hls/`

Detected paths in notebook:
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
4. Encode final MP4 (faststart) and optional HLS
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
- [x] Functional video/demo link (MP4/HLS)
- [x] README with installation, usage, prereqs, and known issues
- [x] Trade-offs documented

---

## 📎 Attribution
- FFmpeg/FFprobe for media I/O and transcoding
- Python packages used in the notebook: TTS, torch

---

## 📤 GitHub Push (quick start)
```bash
git init
git add .
git commit -m "Alethea AI Track A submission"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

Use Git LFS for large media:
```bash
git lfs install
git lfs track "*.mp4" "*.m3u8" "*.ts" "*.wav" "*.mp3"
git add .gitattributes
git commit -m "Track large media with LFS"
git push
```

---

### (Notebook headers preview)
```
# Track A — Single Image → Lip‑Synced Video
## Weights: wav2lip_gan.pth
## Input Image
### Make the audio ~30–45s (optional)
### Notes / Known Issues
```
