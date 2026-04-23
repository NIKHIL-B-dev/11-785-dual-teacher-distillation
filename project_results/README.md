---
tags: [audio, speaker-verification, deepfake-detection, knowledge-distillation, wav2vec2, lstm]
---
# Dual-Teacher Distilled Wav2Vec2 + LSTM

Student model: `facebook/wav2vec2-base` + bidirectional LSTM aggregator,
distilled from two specialized teachers.

| Teacher | Task | Model |
|---|---|---|
| Teacher SV | Speaker Verification | ReDimNet-B0 |
| Teacher DF | Deepfake Detection | ASSIST |

## Architecture
- Wav2Vec2 frame encoder -> BiLSTM (512x2) -> projection heads
- LSTM preserves temporal structure that mean-pooling would remove

## Training
- Platform: Google Colab | chunked streaming pipeline
- Phase 1: Distillation (teachers frozen)
- Phase 2: Fine-tuning (selected top student layers)
- Loss: `L = 1.0*(1-cosine(proj_sv, z_sv)) + 1.0*(1-cosine(proj_df, z_df))`

## Files Present in This Archive
- `checkpoints/checkpoint_latest.pt` - latest checkpoint
- `checkpoints/train_meta.json` - training metadata
- `images/loss_curves.png` - loss visualization
