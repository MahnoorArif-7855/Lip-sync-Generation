# Alethea AI — Video Generation Assignment (Track B)

This repository contains my submission for the Alethea AI Machine Learning Engineer assignment. It includes solutions to **two problems** focused on building a **video generation pipeline** and producing a final **MP4** output.

> Primary notebook: `alethea_track_b.ipynb`

---

## ✅ What’s Included
- Reproducible notebook implementing the end‑to‑end pipeline
- Final rendered video (MP4) output (see **Outputs**) or [(MP4](https://drive.google.com/file/d/1t9_TeGQtq0XquFX5AHm5A_5U16YYmMV1/view?usp=drive_link)
- Notes on trade‑offs and known issues
- This README with setup and run instructions

## 🧩 Problems Covered
- Problem 1: Core video assembly (frames → clips → final render)
- Problem 2: Audio alignment and MP4 export
---

## 🗂️ Recommended Repo Structure
```
.
├── README.md
├── requirements.txt
├── alethea_track_b.ipynb
├── inputs/          # raw input videos / frames / audio
├── outputs/
│   ├── final.mp4            # index.m3u8 + .ts segments (if HLS chosen)
```

---

## ⚙️ Environment & Prerequisites

- **Python**: 3.10+ (tested in Jupyter/Colab)
- **Python packages** (from imports and cells):
- google
- gtts
- requirements.txt
- transformers

> Install Python deps with:
```bash
pip install -r requirements.txt
```

## 🚀 How to Run
###  Run on Google Colab
- Upload the repo to Colab or open the notebook directly.
- Update any hardcoded paths (e.g., `/content/...`) to the `data/` and `outputs/` folders.
- Ensure GPU runtime if a model is used; CPU is fine for pure ffmpeg steps.


---

## 🎬 Inputs & Outputs

- **Inputs**: place source videos, audio, or frames into `inputs/`.
- **Output**: final video at `outputs/final.mp4`.

Detected in the notebook (paths preview):
```
/content/face.jpg
/content/outputs/final_faststart.mp4
/content/outputs/final_windows.mp4
/content/outputs/seg_000.mp4
/content/outputs/seg_000.wav
/content/outputs/seg_003.mp4
/content/outputs/seg_003.wav
/content/outputs/stitched_raw.mp4
/content/your_image.jpg
final_faststart.mp4
```

---

## 🧠 Pipeline Overview
Typical stages:
1. **Load assets** (video(s), audio)
2. **Extract / normalize frames** (FPS, size; handle per‑clip durations)
3. **Assemble clips** with crossfades/cuts; align with audio
5. **Validate** with `ffprobe`

---

## 📄 Known Limitations / Trade‑offs

- **FPS mismatches**: Normalize inputs to a common FPS (e.g., 24/25/30) to keep audio in sync.
- **GPU constraints**: If model‑based generation is added (e.g., diffusion), GPU VRAM may limit resolution/length; consider tiling or lower precision.
- **Pathing**: Colab paths (`/content/...`) should be swapped with `data/...` when running locally.

---

## ✅ Submission Checklist (per prompt)

- [x] Repo link is public and organized
- [x] Functional video/demo link (MP4)
- [x] README includes:
  - Installation steps
  - Run instructions
  - Hardware & software prerequisites
  - Known limitations/issues
- [x] Notes on trade‑offs are documented

---

## 📎 Attribution

This project uses:
- Python ecosystem — google, gtts, transformers
---

## 📧 Acknowledgement Template

When ready to submit, reply to the email with:
- Public repo URL
- Direct link to MP4 (`outputs/final.mp4`)
- Any notes on trade‑offs or known issues

---

### (Notebook headers preview)
```
# Alethea AI — Track B Pipeline (Colab Notebook)
## 1) Generate continuous text with an LLM
## 2) TTS per chunk (gTTS)
## 3) Run Wav2Lip per chunk
## 4) Stitch with clean cross-fades (video + audio)
## 5) Loudness normalisation (EBU R128)
```
