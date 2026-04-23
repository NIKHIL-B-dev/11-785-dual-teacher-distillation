# Claude Prompt to Generate PDF (Report + Viva Pack)

Use this exact prompt in Claude and attach these files:
- `DualTeacher_Wav2Vec2.pptx`
- `README.md`
- `PSEUDOCODE.md`
- `project_results/checkpoints/train_meta.json`
- `project_results/images/loss_curves.png`
- `PROF_QA_SHEET.md`

```text
Create a polished, professional PDF document package from my project materials.
Audience: professor and technical evaluators.
Goal: submission-ready PDF that matches my PPT narrative and helps viva defense.

Critical requirement framing (must be explicit and repeated clearly):
1) Teacher ReDimNet for SV with cosine loss1 to student SV projection.
2) Teacher ASSIST for deepfake with cosine loss2 to student DF projection.
3) Total loss = loss1 + loss2.

What to produce:
A) Main Report PDF (8-12 pages)
B) Viva Q&A PDF (question-answer defense sheet)

A) Main Report PDF structure:
1. Title page
2. Abstract (150-200 words)
3. Problem motivation
4. Dataset and pipeline
5. Architecture (student, two teachers, BiLSTM, dual heads)
6. Objective function with equations
7. Training strategy (distillation + fine-tuning)
8. Results summary (include key numeric values from train_meta)
9. Requirement compliance checklist (explicit YES mapping)
10. Limitations and honest caveats
11. Post-presentation improvement plan
12. Conclusion

B) Viva Q&A PDF structure:
- 30 likely professor questions with concise high-quality answers
- Include challenge questions ("why this not that")
- Include 1-minute defense script
- Include rapid-fire one-line answers section

Formatting/style requirements:
- Clean academic style, no fluff, no hype
- Professional headings, table formatting, equation formatting
- Include at least 2 diagrams rendered as clean figure blocks:
  1) model architecture diagram
  2) training/inference flow diagram
- Include one compliance table with columns:
  Requirement | Implemented Component | Evidence | Status
- Include one metrics table from provided JSON

Strict accuracy constraints:
- If there is uncertainty in any claim, label it as "Planned" or "To be validated".
- Do not invent unavailable benchmark metrics.
- Keep statements consistent with provided artifacts.

Output behavior:
1) First provide full markdown content for both PDFs.
2) Then provide a "Copy-ready LaTeX version" for the main report.
3) Then provide a compact "Canva/Word export checklist" so I can quickly export to PDF.

Tone:
- Confident, technical, clean, professor-friendly.
- Prioritize clarity and defensibility over marketing language.
```
