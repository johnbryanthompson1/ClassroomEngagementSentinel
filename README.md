# ClassroomEngagementSentinel

Quantitatively compare student engagement across **lecture**, **group projects**, **whole-class discussions**, and **stations** using **audio-first signals** (and optional computer vision), with a privacy-first, edge-capable pipeline.

## Architecture
```
mics/camera → ingest → features(audio, optional vision) → fusion(model) → metrics → dashboard
                                      |                                 |
                                  data/raw                         data/processed
```

- **src/ingest/**: audio capture (multi-mic), optional video capture; segment labeling input
- **src/features/**: VAD, turn-taking, entropy, silence %, distinct voices; optional head/pose features
- **src/fusion/**: feature normalization + Engagement Index model
- **src/metrics/**: per-activity aggregation, statistical comparisons (ANOVA/Kruskal-Wallis), effect sizes
- **src/dashboard/**: FastAPI endpoints + simple UI (live charts, activity toggle, exports)
- **hardware/**: GPIO scripts & wiring guides for the Teacher Activity Selector

## Folder Structure
```
configs/
docs/
hardware/
data/raw/
data/processed/
dashboard/
notebooks/
src/ingest/
src/features/
src/fusion/
src/metrics/
src/dashboard/
tests/
```

## Quickstart (dev, no hardware)
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python src/dashboard/app.py
```

## Metrics & Targets
- VAD F1 ≥ 0.90; voices MAE ≤ 1.0
- Validity vs. human rubric ρ ≥ 0.6
- Segment attribution ≥ 95%
- Optional: near–real time ≤ 1 s/window

## Privacy
- No identification; aggregate features only by default.
- `.env`: `SAVE_RAW=false`, `SAVE_DEBUG=false`, retention windows in seconds.

## Reports
- `/export/weekly.csv` and `/export/summary.csv` compare activities with means, CIs, and significance markers.

## Bill of Materials (core)
- Raspberry Pi 4/5 (or mini PC), power supply, microSD (32–64 GB)
- 2–4 USB microphones or a small USB mic array
- Optional USB/Pi camera
- **Teacher Activity Selector** parts: rotary switch or 4-way DIP, pushbutton, 3× LEDs (G/Y/R), resistors (220–330Ω), perfboard, header pins, jumper wires
- Soldering kit (iron, solder, flux), small project box

## License
