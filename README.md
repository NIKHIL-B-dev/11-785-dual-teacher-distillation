# Dual-Teacher Distillation with Wav2Vec2

A multi-task speech representation project that distills a single `wav2vec2-base` student from two frozen teachers:
- `ReDimNet` for speaker verification (SV)
- `ASSIST` for deepfake / anti-spoofing detection (DF)

The core training objective is:
- `loss_sv = 1 - cosine(student_sv_proj, teacher_sv_embed)`
- `loss_df = 1 - cosine(student_df_proj, teacher_df_embed)`
- `total_loss = loss_sv + loss_df`

## Why this project
Most production voice-security systems run separate pipelines for speaker verification and spoof detection. This project explores a unified student model to reduce inference overhead while preserving task-specific quality.

## Repository Contents
- `dual_teacher_colab_v3.ipynb` - end-to-end training notebook
- `project_results/images/loss_curves.png` - training loss visualization
- `project_results/checkpoints/train_meta.json` - run metadata + loss history
- `PSEUDOCODE.md` - implementation pseudocode
- `DualTeacher_MainReport.pdf` - technical report
- `DualTeacher_VivaQA.pdf` - viva preparation material
- `DualTeacher_Wav2Vec2.pptx` - presentation deck

## Quick Start
1. Open `dual_teacher_colab_v3.ipynb` in Colab.
2. Configure dataset paths/tokens as needed.
3. Run distillation + fine-tuning stages.
4. Inspect `project_results/checkpoints/train_meta.json` and `project_results/images/loss_curves.png`.

## Current Status
- Architecture implemented and trained.
- Distillation objective verified in code.
- Benchmark metrics such as EER/min-DCF can be added as additional evaluation artifacts.

## Team
- Lucky Gurjar
- Aryan Singh Chandel
- Nikhil B
- Tanuja Bhatt

Mentor: Prof. Massa Baali  
Course: 11-785

## Contribution Guide
If you are a project member, please use your own GitHub account and open pull requests from your own branch so individual contributions are visible on GitHub.

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License
This repository is released under the MIT License. See [LICENSE](LICENSE).
