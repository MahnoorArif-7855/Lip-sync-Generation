# Alethea AI — Video Generation Assignment (Track C)

This repository contains my submission for the Alethea AI Machine Learning Engineer assignment. It implements a **text‑to‑video lip‑sync pipeline** that produces a final MP4 (and optional HLS) output using TTS → Loudness → Wav2Lip → Stitch.

**Primary notebook:** `notebooks/Alethea_Track_C.ipynb`

---

## ✅ What’s Included
- Reproducible notebook implementing the end‑to‑end pipeline
- Final rendered video (MP4) output (see **Outputs**) [Direct link](https://drive.google.com/file/d/1CMi7kWkMG_x_u8QBml5SEisMZ7UxWn7C/view?usp=sharing)
- **HLS** (HTTP Live Streaming) export for web playback
- Notes on trade‑offs and known issues
- This README with setup and run instructions

---

## 🧩 Problems Covered
- **Problem C:** Text → TTS (Coqui) → **Wav2Lip** lip‑sync video generation from a single image
- **Audio & Export:** Loudness normalization (EBU R128) and MP4/HLS export
---

## 🗂️ Recommended Repo Structure
```
.
├── README.md
├──Alethea_Track_C.ipynb

├── face.jpg                # example portrait input
└── outputs/
    ├── tmp_XXXXXXXX/seg_000.mp4  # per‑segment MP4s (tmp folder changes each run)
    ├── final.mp4                  # stitched final
    └── hls/index.m3u8            # optional HLS playlist
```

---

## ⚙️ Environment & Prerequisites
**Python:** 3.10+ (tested in Colab, Python 3.11)  
**System:** `ffmpeg`/`ffprobe` for media I/O  
**GPU:** NVIDIA (Colab T4/A100 OK). CPU works but is slow for Wav2Lip.

**Python packages (as used in the notebook):**
- `torch==2.3.1+cu121`, `torchvision==0.18.1+cu121`, `torchaudio==2.3.1+cu121`
- `TTS==0.22.0` (Coqui TTS)
- `opencv-python-headless==4.10.0.84`
- `numpy==1.26.4`, `scipy>=1.13.0`, `pandas==2.2.2`
- `librosa` (works with a 1‑line patch in `Wav2Lip/audio.py`)


---

## 🚀 How to Run
### Run on Google Colab (recommended)
1. Open `notebooks/Alethea_Track_C.ipynb` in a **GPU** runtime.
2. Run the **Install** cell that pins versions:
   ```python
   !pip -q install "numpy==1.26.4" "scipy>=1.13.0" "opencv-python-headless==4.10.0.84" "pandas==2.2.2"
   !pip -q install torch==2.3.1+cu121 torchvision==0.18.1+cu121 torchaudio==2.3.1+cu121 -i https://download.pytorch.org/whl/cu121
   !pip -q install -U TTS==0.22.0
   !git clone -q https://github.com/Rudrabha/Wav2Lip.git /content/Wav2Lip
   ```
3. Apply the **librosa** compatibility patch (newer librosa requires keyword args):
   ```python
   from pathlib import Path
   ap = Path("/content/Wav2Lip/audio.py")
   ap.write_text(ap.read_text().replace(
       "librosa.filters.mel(hp.sample_rate, hp.n_fft, n_mels=hp.num_mels,",
       "librosa.filters.mel(sr=hp.sample_rate, n_fft=hp.n_fft, n_mels=hp.num_mels,"
   ))
   print("Patched librosa mel() call.")
   ```
4. Put your portrait at `/content/face.jpg` and run the pipeline cells in order:
   - **Segment text → TTS → Loudness**
   - **Wav2Lip per segment** (your code runs: `--static True --pads 0 10 0 0 --nosmooth`)
   - **Validate/Conform → Stitch**
   - **(Optional) Export HLS** by setting `CFG["io"]["serve"] = "hls"`

> **Note:** The notebook writes segments to a **random tmp folder per run** (`/content/outputs/tmp_XXXXXXXX`). If you prefer a stable path, set `TMP = OUT / "run_latest"`.

---

## 🎬 Inputs & Outputs
- **Inputs:** place the portrait at `/face.jpg` (or `/content/face.jpg` in Colab). Provide your input text via cell.
- **Outputs:** final MP4 at `outputs/final.mp4`. Optional HLS in `outputs/hls/`.

---

## 🧠 Pipeline Overview
**Key helpers (as in the notebook):**  
`auto_segment`, `tts_coqui`, `loudnorm_batch`, `wav2lip_batch`, `validate_conform`, `xfade_stitch`, `export_hls`.

**Typical stages:**
1. Load assets (portrait image, input text)
2. Split text into segments, synthesize TTS (Coqui)
3. Loudness normalization with FFmpeg (EBU R128)
4. Run **Wav2Lip** per segment to create lip‑synced MP4s
5. Validate/conform segments (fps/size/pix_fmt), then stitch with cross‑fades
6. (Optional) Export **HLS** (`index.m3u8` + segments) for web playback

---

## 🧪 Notes, Trade‑offs & Known Issues
- **S3FD weights:** The current code does **not enforce** a `torch.load` verification step. If face detection fails on your environment, place a valid `s3fd.pth` at `Wav2Lip/face_detection/detection/sfd/s3fd.pth`, or try a clearer/larger portrait.  
- **Top‑right mouth overlay:** Usually caused by invalid detection boxes (e.g., missing/corrupt S3FD or malformed `--box`). In this code, we rely on stock S3FD; increasing `--pads` (e.g., `0 15 0 5`) and using a frontal image helps.  
- **Librosa incompatibility:** Newer librosa requires keyword args for `filters.mel(...)` — the 1‑line patch above fixes it.  
- **`temp/result.avi` message:** If you see `temp/result.avi: No such file or directory`, it’s harmless. Running from the repo directory or making `/content/Wav2Lip/temp` suppresses it.  
- **Quality vs. speed:** `--resize_factor 1` yields best visual quality; `2` is faster but softer.  
- **Scope:** Single‑image lip‑sync (no head motion/blinks).

---

## ✅ Submission Checklist
- Public **repo link** is accessible and organized
- Functional **video/demo** link (direct MP4 and/or HLS `index.m3u8`)
- README includes:
  - Installation steps
  - Run instructions
  - Hardware & software prerequisites
  - Known limitations / trade‑offs

---

## 📎 Attribution
This project uses:
- **Wav2Lip** (Rudrabha et al.)
- **Coqui TTS** (Coqui.ai)
- **librosa**, **OpenCV**, **PyTorch**

Please review and comply with upstream licenses.

---

### (Notebook section preview)
- **1) Segment text & TTS (Coqui)**
- **2) Loudness normalization (EBU R128)**
- **3) Wav2Lip per segment**
- **4) Stitch with cross‑fades**
- **5) Export HLS**
