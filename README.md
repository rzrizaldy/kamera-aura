# Kamera Aura

Live "aura" viewfinder. The camera and mic read three signals (facial expression, body movement, voice), fuse them into two scores (valence: positive vs negative, arousal: calm vs energetic), and render that as a colored glow around your silhouette.

It is a vibe meter for fun. It reads expressions, not what someone actually feels.

## Run it

Camera and mic only work on `https` or `localhost`.

- **Phone:** open the GitHub Pages link for this repo in Chrome.
- **Laptop:** `python3 -m http.server 8000` in this folder, then open `http://localhost:8000`.

Opening `index.html` straight from the folder (`file://`) will usually be blocked.

## How it works (v1)

| Stage | What it does |
|---|---|
| Face | MediaPipe Face Landmarker, 52 blendshapes (smile, frown, brow down, eye wide, jaw open) + head motion |
| Body | MediaPipe Pose Landmarker (lite), posture (ear vs shoulder height), arm openness, crossed arms, movement speed, plus a segmentation mask for the glow |
| Voice | Web Audio: RMS energy, autocorrelation pitch, pitch variability, simple voice activity detection |
| Calibration | First ~4 s of each signal sets a per-person baseline; all scores are deltas from it |
| Fusion | Weighted average (face 0.5, body 0.25, voice 0.25) scaled by each signal's confidence, smoothed with an EMA |
| Render | Valence x arousal mapped to a color circumplex: Cerah (gold), Adem (teal), Mendung (indigo), Panas (red), Netral (pearl) |

Everything runs in the browser. Video and audio never leave the device; only the model files are downloaded (MediaPipe 0.10.21 from jsDelivr, models from Google storage).

## Known limits

- Weights are hand-picked starting values, not tuned on data.
- Facial expressions are an unreliable proxy for emotion (see Barrett et al., 2019). Resting faces, culture, and posed smiles all fool it.
- Voice scoring is the weakest signal: pitch and energy only.
- Not yet tested on a real phone for fps.

See [ROADMAP.md](ROADMAP.md) for what comes next.
