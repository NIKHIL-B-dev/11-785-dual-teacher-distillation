# Professor Q&A Prep Sheet (Based on Your Actual PPT)

Use this as viva prep for `DualTeacher_Wav2Vec2.pptx`.

## 1) High-Probability Questions (Most Likely)

Q1. What exactly did you implement from the professor instruction?
A. We implemented two teacher encoders with one student: ReDimNet for SV and ASSIST for deepfake. We compute `loss1` as cosine loss between student SV projection and ReDimNet embedding, `loss2` as cosine loss between student DF projection and ASSIST embedding, and optimize `Total = loss1 + loss2`.

Q2. Show where `Total = loss1 + loss2` is actually done.
A. In the training block: `total = ALPHA * loss_sv + BETA * loss_df` with `ALPHA=1.0` and `BETA=1.0`, so it is exactly `loss1 + loss2`.

Q3. Why cosine loss and not MSE/L2?
A. Teachers produce embeddings where direction matters more than magnitude. Cosine aligns embedding geometry and is scale-invariant, which is more stable for representation distillation.

Q4. Why two projection heads instead of one shared head?
A. SV and deepfake are related but not identical tasks. Two heads allow task-specific alignment while keeping a shared backbone.

Q5. Why use wav2vec2 as student?
A. Strong pretrained speech representation, robust frame features, and practical transfer to both tasks with one backbone.

Q6. Why BiLSTM after wav2vec2?
A. wav2vec2 outputs frame sequences. BiLSTM aggregates temporal cues needed for both speaker identity and spoof artifacts before fixed-size projection.

Q7. Are teachers trainable during distillation?
A. No. Both teachers are frozen; only student + temporal aggregator + projection heads are updated.

Q8. What data supports each task?
A. VoxCeleb1 for speaker verification behavior and ASVspoof 2019 LA for deepfake behavior.

Q9. What is the key training evidence?
A. `train_meta.json` shows total loss from ~1.5597 to ~0.005285, SV loss to ~0.002973, DF loss to ~0.002313, showing convergence of both objectives.

Q10. What is the final practical benefit?
A. One inference pipeline that serves both SV and deepfake representations, reducing duplicated deployment complexity.

## 2) Slide-by-Slide Challenge Questions + Best Answers

Q11. (Slide Motivation) Why not keep two separate models?
A. Two separate pipelines increase latency, memory, and maintenance. Distilling both tasks into one student allows shared feature extraction and simpler deployment.

Q12. (Dataset Slide) How did you avoid memory issues with large data?
A. Chunked/streamed loading strategy instead of full in-memory loading; process manageable segments during training.

Q13. (Architecture Slide) Are teacher outputs in same dimension as student outputs?
A. Student hidden representation is mapped by task-specific projection heads into each teacher space before cosine comparison.

Q14. (Loss Slide) Is cosine computed per sample or batch?
A. Per sample embedding similarity, then averaged over the batch.

Q15. (Training Slide) Why equal weights (1.0, 1.0)?
A. Equal weighting follows professor’s exact requirement for `loss1 + loss2`. Weighted variants are future ablations.

Q16. (Results Slide) You claim strong convergence; how do you justify?
A. Objective values consistently reduce across total, SV, and DF histories; no branch divergence.

Q17. (Compliance Slide) How do you prove requirement compliance quickly?
A. Point to model components and the loss formula in code: `teacher_sv`, `teacher_df`, `loss_sv`, `loss_df`, and summed `total` with alpha=beta=1.

Q18. (Design Slide) Why not mean pooling instead of BiLSTM?
A. Mean pooling discards sequence order. BiLSTM preserves temporal patterns useful for spoof cues and speaker dynamics.

Q19. (Inference Slide) Does model output class labels directly?
A. Main outputs are task embeddings/projections; downstream scoring or classifier heads can be added depending on deployment target.

Q20. (Conclusion Slide) What will you improve after presentation?
A. Add controlled ablations (weights, pooling choice, no-LSTM baseline), stronger metrics reporting, and robustness checks.

## 3) Tough/Trap Questions and Safe Responses

Q21. "Did you really use official ASSIST weights end-to-end?"
A. Primary path is loading ASSIST; notebook also includes fallback stubs for robustness if external loading fails. For final reproducible run, we keep the real model path active.

Q22. "Your slide says 50 epochs. Is that what this run used?"
A. The current stored metadata shows 46 logged total points ending in finetune phase (`epoch=15`). We can present this run as the validated one and keep 50-epoch as planned configuration target.

Q23. "Why no explicit EER/AUC table in this deck?"
A. This deck focuses on architectural compliance and distillation behavior first. Metric table is the next reporting increment and can be added from evaluation scripts after presentation.

Q24. "How do you know tasks are not conflicting?"
A. Both branch losses reduce in parallel; no one branch explodes while the other decreases. We also use separate heads to reduce representational interference.

Q25. "Could one teacher dominate optimization?"
A. Possible in general. Here equal weights were required; later we can tune `lambda1/lambda2` if imbalance appears.

Q26. "Why not train from scratch instead of distillation?"
A. Distillation transfers mature teacher geometry into a compact unified student faster and more stably than pure from-scratch multi-task training.

Q27. "Why not a transformer aggregator instead of BiLSTM?"
A. BiLSTM gives a lower-cost temporal aggregator with clear gains over naive pooling; transformer aggregator is a future extension.

Q28. "How do you prevent catastrophic forgetting in fine-tuning?"
A. Keep teachers frozen, selectively unfreeze top student layers, and reduce LR during fine-tuning.

## 4) 60-Second Defense Script

We followed the professor’s required formulation exactly: one wav2vec2 student distilled from two frozen teachers, ReDimNet for speaker verification and ASSIST for deepfake detection. We compute cosine alignment losses for each branch and optimize their sum, `loss1 + loss2`. The student uses a BiLSTM temporal aggregator and separate SV/DF projection heads so shared features are preserved while each task retains specialization. The training logs show both branch losses and total loss decreasing steadily, confirming stable dual-task distillation. This gives us one deployable student pipeline instead of two separate systems.

## 5) Fast Reference Numbers

- Best total loss: `0.0052854`
- Total loss first -> last: `1.5596612 -> 0.0052854`
- SV loss first -> last: `0.8116466 -> 0.0029728`
- DF loss first -> last: `0.7480145 -> 0.0023126`
- Logged total points: `46`

Source: `project_results/checkpoints/train_meta.json`
