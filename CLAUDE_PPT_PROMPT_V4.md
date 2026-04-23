# Claude Prompt (V3) — Final 5-Minute PPTX Generator

Paste everything below into Claude, and attach these files from this folder:
- DualTeacher_MainReport.pdf
- project_results/checkpoints/train_meta.json
- project_results/images/loss_curves.png
- PSEUDOCODE.md
- README.md
- PROF_QA_SHEET.md

```text
You are generating a FINAL .pptx file for an academic project video submission.
Audience: professor + technical graders.
Goal: build an exact, defensible deck aligned to the provided grading/video guideline.

NON-NEGOTIABLE RULES
1) Do NOT invent facts, metrics, experiments, dates, team info, or citations.
2) If information is missing, keep an explicit placeholder in square brackets.
3) If sources conflict, do NOT guess. Add a visible [CONFIRM: ...] placeholder.
4) Mark unexecuted evaluation as "Planned" (not completed).
5) Output must be concise enough to present in <= 5:00 minutes total.

SOURCE-OF-TRUTH PRIORITY (highest to lowest)
A) train_meta.json for actual run metrics
B) Main report PDF for architecture/dataset narrative
C) pseudocode/readme for implementation details
D) If conflict remains unresolved, use [CONFIRM: ...]

VIDEO CONSTRAINTS TO ENFORCE
- Total video limit: max 5:00 (hard limit; >5:10 rejected).
- Start with title slide containing project title, team name, and all member names.
- Suggested timing blocks to map into slide notes:
  - 20–50 sec: Problem
  - 30 sec: Task
  - 1–2 min: Approach/Methods
  - <= 1 min: Results/Discussion
  - < 30 sec: Conclusion
- Keep the deck focused and non-verbose.

FIXED IDENTITY DETAILS (USE EXACTLY)
- Team Name: 45
- Members:
  - Lucky Gurjar
  - Aryan Singh Chandel
  - Nikhil B
  - Tanuja Bhatt
- Course: 11-785
- Professor / Mentor: Prof. Massa Baali
- Instructor line on title slide: Mentor

PROJECT FACTS TO USE (FROM PROVIDED FILES)
- Topic: Dual-teacher distillation with wav2vec2 student for speaker verification (SV) + deepfake detection (DF).
- Teachers: ReDimNet (SV, frozen) and ASSIST (DF, frozen).
- Losses:
  - loss1 = 1 - cosine(student_sv_proj, redimnet_embed)
  - loss2 = 1 - cosine(student_df_proj, assist_embed)
  - total_loss = alpha*loss1 + beta*loss2, with alpha=1.0 and beta=1.0 (thus loss1 + loss2).
- Student architecture: wav2vec2-base + BiLSTM temporal aggregator + dual projection heads (SV/DF).
- Datasets discussed: VoxCeleb1 and ASVspoof 2019 LA.
- From train_meta.json (actual logged run):
  - best_loss = 0.005285399991112786
  - phase = finetune
  - epoch = 15
  - logged points in history = 46
  - total loss first->last: 1.5596611950848553 -> 0.005285399991112786
  - sv loss first->last: 0.8116466483554324 -> 0.0029728042112814415
  - df loss first->last: 0.7480145451184865 -> 0.002312595779831345

CONFLICTS / PLACEHOLDERS YOU MUST SHOW
- Audio chunk length conflict across artifacts:
  4s
- If EER/min-DCF/AUC values are available from user, include them.
  Else write:
  [EER: TO FILL], [min-DCF: TO FILL], [AUC: TO FILL] and mark as Planned.

WHAT TO BUILD
Create one polished PPTX in 16:9 aspect ratio, optimized for a 5-minute recorded walkthrough.
Target slide count: 8–10 slides max.

Required slide plan (must include all):
1) Title slide
- Project title: [PROJECT TITLE TO FILL]
- Team 45
- Member names (fixed above)
- Course 11-785
- Mentor: Prof. Massa Baali

2) Problem (20–50 sec)
- What problem, why it matters, where current systems are inefficient (two separate pipelines)

3) Task (30 sec)
- Re-implementation/application/novelty statement
- Datasets used + relevance
- Input assumptions/preprocessing summary

4) Approach/Methods (part 1)
- End-to-end architecture diagram
- Student + two frozen teachers
- Data flow and tensor flow labels

5) Approach/Methods (part 2)
- Loss equations and training strategy
- Distillation and fine-tuning phases
- Explicitly state total = loss1 + loss2

6) Results/Discussion (<=1 min)
- Use train_meta and loss_curves
- One chart slide showing total/sv/df trends
- If provided by user, include EER/min-DCF; otherwise leave clear placeholders
- Separate "Observed" vs "Planned" metrics

7) Limitations + validation status
- Clearly mark what is complete vs pending
- Mention at least one limitation/failure-risk

8) Conclusion (<30 sec)
- Main takeaways
- Next steps

Optional 9) Backup / compliance slide
- Requirement checklist mapping professor requirement -> implemented component -> evidence

DESIGN REQUIREMENTS
- Clean academic-professional style, high readability.
- Avoid clutter; 4-6 bullets max per slide.
- Large typography suitable for screen recording.
- Consistent color system and spacing.
- Include simple, clear diagrams (not decorative noise).
- Use subtle animations/transitions only if they help explanation.

SPEAKER NOTES REQUIREMENT
For each slide, include 1 short presenter script with target duration in seconds so total duration <= 300 sec.

OUTPUT FORMAT
1) First, provide a slide-by-slide build plan table:
   - Slide # | Title | Purpose | Duration(sec) | Visual
2) Then generate the actual PPTX file content/structure.
3) Then provide a final Fact-Check Table:
   - Claim in slides | Source file | Status (Verified / Placeholder / Planned)
4) End with a Missing-Info checklist containing only placeholders that still need user input.

QUALITY BAR
- This deck must be defensible in viva.
- No hype language.
- No fabricated benchmark claims.
- Keep statements aligned with attached artifacts.
```


