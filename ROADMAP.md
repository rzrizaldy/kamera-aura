# Roadmap

Effort estimates are rough guesses, not measured.

## v1: rule-based prototype (done)
Single `index.html`. MediaPipe face + pose, mic pitch/energy, per-person baseline, confidence-weighted fusion, silhouette glow.

## v1.1: make it solid (about a weekend)
- [ ] Test on Pixel and laptop; note fps with face + pose + mask running together
- [ ] If slow: run pose every 3rd frame, lower camera resolution, or drop the mask on weak devices
- [ ] Debug panel: live raw blendshapes, body features, pitch, and sliders for every weight
- [ ] Tune weights by acting out each quadrant (happy, calm, sad, angry) and checking the dot lands right
- [ ] Split the single file into modules (`face.js`, `body.js`, `voice.js`, `fusion.js`, `render.js`)

## v1.2: collect your own data (1 to 2 weeks, part time)
- [ ] Session logger that saves features only (no video or audio) to local JSON
- [ ] Self-label buttons ("lagi happy", "lagi kesel", "capek") to get ground truth
- [ ] Simple eval: how often the predicted quadrant matches the self-label

## v2: learned models
- [ ] Replace hand weights with a small model (logistic regression or tiny MLP) on the same features, trained on your logged data
- [ ] Face: consider a valence/arousal model trained on AffectNet-style labels
- [ ] Voice: proper speech emotion model (wav2vec2 or emotion2vec family) via ONNX Runtime Web or transformers.js, in a Web Worker
- [ ] Note: acted datasets (RAVDESS, CREMA-D) generalize poorly to real speech

## v3: product
- [ ] Shareable aura card (snapshot + aura name), only with consent
- [ ] Multiple people, each with their own glow
- [ ] Wrap as an Android app (Capacitor or native MediaPipe) if browser performance is not enough

## Guardrails (all versions)
- Keep processing on-device; never store faces or voice
- Consent screen before scanning anyone
- Label it as entertainment, not a judgment of a person
- Emotion recognition in workplaces and schools is banned under the EU AI Act; biometric laws (e.g. Illinois BIPA) apply if it ever stores data
