# Pseudocode: Dual-Teacher Distillation (SV + Deepfake)

```text
INPUTS:
  audio batch x
  student encoder S (wav2vec2)
  teacher_sv T1 (ReDimNet, frozen)
  teacher_df T2 (ASSIST, frozen)
  temporal aggregator A (BiLSTM)
  projection heads P1 (SV), P2 (DF)
  weights alpha=1.0, beta=1.0

FOR each training step:
  # Student forward
  h_frames = S(x).last_hidden_state            # frame sequence
  h_seq    = A(h_frames)                       # temporal modeling
  h_pool   = temporal_pool(h_seq)              # utterance-level summary

  # Map student features into each teacher space
  s_sv = P1(h_pool)
  s_df = P2(h_pool)

  # Teacher targets (no gradients)
  z_sv = T1(x)
  z_df = T2(x)

  # Cosine losses
  loss1 = 1 - cosine_similarity(s_sv, z_sv).mean()   # SV alignment
  loss2 = 1 - cosine_similarity(s_df, z_df).mean()   # DF alignment

  # Total objective
  total_loss = alpha * loss1 + beta * loss2

  # Update student + aggregator + projection heads
  optimizer.zero_grad()
  total_loss.backward()
  optimizer.step()
END
```

## Two-Phase Training Logic
1. Distillation phase:
- Teachers frozen.
- Train student + BiLSTM + projection heads.

2. Fine-tuning phase:
- Keep teachers frozen.
- Unfreeze selected top layers of student.
- Continue optimizing same combined loss.
