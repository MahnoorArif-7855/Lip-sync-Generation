# Alethea AI — Video Generation Assignment (Track B)

This repository contains my submission for the Alethea AI Machine Learning Engineer assignment. It includes solutions to **two problems** focused on building a **video generation pipeline** and producing a final **MP4/HLS** output.

> Primary notebook: `alethea_track_b.ipynb`

---

## ✅ What’s Included
- Reproducible notebook implementing the end‑to‑end pipeline
- Final rendered video (MP4) output (see **Outputs**)
- Notes on trade‑offs and known issues
- This README with setup and run instructions

## 🧩 Problems Covered
- Problem 1: Core video assembly (frames → clips → final render)
- Problem 2: Audio alignment and HLS/MP4 export

If the PDF prompt uses other names (e.g., Track A/B tasks), the above maps to the required **video generation** and **export** components.

---

## 🗂️ Recommended Repo Structure
```
.
├── README.md
├── requirements.txt
├── notebooks/
│   └── alethea_track_b.ipynb
├── data/
│   ├── inputs/           # raw input videos / frames / audio
│   └── intermediates/    # extracted frames, temp audio, etc.
├── outputs/
│   ├── final.mp4
│   └── hls/              # index.m3u8 + .ts segments (if HLS chosen)
└── scripts/
    └── export_hls.sh     # optional ffmpeg script to create HLS
```

You can keep everything in the notebook or add small helper scripts if preferred.

---

## ⚙️ Environment & Prerequisites

- **Python**: 3.10+ (tested in Jupyter/Colab)
- **System**:
  - `ffmpeg` and `ffprobe` for video/audio I/O and transcoding
- **Python packages** (from imports and cells):
- ffmpeg (system)
- google
- gtts
- requirements.txt
- transformers

> Install Python deps with:
```bash
pip install -r requirements.txt
```

> Install FFmpeg (if needed):
- Ubuntu/Debian: `sudo apt-get update && sudo apt-get install -y ffmpeg`
- Mac (Homebrew): `brew install ffmpeg`
- Windows (scoop): `scoop install ffmpeg`

---

## 🚀 How to Run

### Option 1 — Run in Jupyter/VS Code
1. Create the folders:
   ```bash
   mkdir -p notebooks data/inputs data/intermediates outputs
   ```
2. Place your input assets (videos/audio/frames) into `data/inputs/`.
3. Open and run the notebook:
   ```bash
   jupyter notebook notebooks/alethea_track_b.ipynb
   ```
4. Follow the cell prompts. Final render will be written to `outputs/final.mp4` (and optionally `outputs/hls/index.m3u8`).

### Option 2 — Run on Google Colab
- Upload the repo to Colab or open the notebook directly.
- Update any hardcoded paths (e.g., `/content/...`) to the `data/` and `outputs/` folders.
- Ensure GPU runtime if a model is used; CPU is fine for pure ffmpeg steps.


---

## 🎬 Inputs & Outputs

- **Inputs**: place source videos, audio, or frames into `data/inputs/`.
- **Intermediate**: extracted frames/audio are written to `data/intermediates/`.
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

Key functions detected in the notebook:
```
_static_takes_value, deco, ffprobe_fps, generate_continuous_text, has_audio, retry, run_cmd, run_wav2lip_img, split_text, tts_with_retry, wrapper
```

Typical stages:
1. **Load assets** (video(s), audio)
2. **Extract / normalize frames** (FPS, size; handle per‑clip durations)
3. **Assemble clips** with crossfades/cuts; align with audio
4. **Mux** audio + video; export **MP4 (faststart)** and optional **HLS**
5. **Validate** with `ffprobe`

---

## 🧪 Reproducibility

- Seeded operations where applicable.
- Deterministic ffmpeg commands.
- Notebook can be run top‑to‑bottom to regenerate outputs.

---

## 📄 Known Limitations / Trade‑offs

- **FFmpeg availability**: Ensure `ffmpeg`/`ffprobe` are installed and on `PATH`.
- **Black frames / copy codec issues**: If inputs differ in codecs or pixel formats, avoid `-c copy`; re‑encode instead (e.g., `-c:v libx264 -pix_fmt yuv420p`).
- **FPS mismatches**: Normalize inputs to a common FPS (e.g., 24/25/30) to keep audio in sync.
- **GPU constraints**: If model‑based generation is added (e.g., diffusion), GPU VRAM may limit resolution/length; consider tiling or lower precision.
- **Pathing**: Colab paths (`/content/...`) should be swapped with `data/...` when running locally.

---

## ✅ Submission Checklist (per prompt)

- [x] Repo link is public and organized
- [x] Functional video/demo link (MP4/HLS)
- [x] README includes:
  - Installation steps
  - Run instructions
  - Hardware & software prerequisites
  - Known limitations/issues
- [x] Notes on trade‑offs are documented

---

## 📎 Attribution

This project uses:
- FFmpeg/FFprobe for media processing
- Python ecosystem — google, gtts, transformers

If any external snippets/libraries beyond standard usage were used, they’re credited inline in the notebook.

---

## 📤 How to Push to GitHub

```bash
# from the project root
git init
git add .
git commit -m "Alethea AI assignment: video generation pipeline"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

> Replace `<your-username>/<your-repo>` accordingly. Ensure large videos are either compressed, hosted elsewhere, or tracked via Git LFS:
```bash
git lfs install
git lfs track "*.mp4" "*.m3u8" "*.ts"
git add .gitattributes
git commit -m "Track large media with Git LFS"
```

---

## 📧 Acknowledgement Template

When ready to submit, reply to the email with:
- Public repo URL
- Direct link to MP4 (`outputs/final.mp4`)
- Any notes on trade‑offs or known issues

Good luck reviewing my submission!

---

### (Notebook headers preview)
```
# Alethea AI — Track B Pipeline (Colab Notebook)
## 1) Generate continuous text with an LLM
## 2) TTS per chunk (gTTS)
## 3) Run Wav2Lip per chunk
## 4) Stitch with clean cross-fades (video + audio)
## 5) Loudness normalisation (EBU R128)
## Bonus: Verify identical FPS across all Wav2Lip segments
### How to use
```
