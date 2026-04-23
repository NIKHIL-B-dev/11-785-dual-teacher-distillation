# Claude Prompt for Professional PPT

Use this prompt in Claude exactly (edit placeholders in brackets if needed):

```text
You are an expert AI presentation designer and ML storyteller.
Create a beautiful, clean, professional PowerPoint deck for a technical audience (professor + grad students) based on the project below.

PROJECT CONTEXT
Title: Dual-Teacher Distilled Wav2Vec2 with Temporal Aggregation for Speaker Verification and Deepfake Detection
Core idea: One wav2vec2 student model learns from two frozen teachers:
1) ReDimNet teacher for speaker verification (SV)
2) ASSIST teacher for deepfake detection (DF)
Loss:
- loss1 = cosine loss between student SV projection and ReDimNet embedding
- loss2 = cosine loss between student DF projection and ASSIST embedding
- Total = loss1 + loss2 (or alpha*loss1 + beta*loss2, with alpha=beta=1.0)

WHAT TO DELIVER
- Generate a complete slide-by-slide PPT structure (12 to 16 slides).
- For each slide provide:
  - Slide title
  - Key bullet points (concise, high signal)
  - Suggested visual (diagram/chart/table/icon/photo)
  - Speaker notes (what to say in 30-60 seconds)
- Include exact wording for important equations and architecture labels.
- Keep language technically correct but presentation-friendly.

DESIGN REQUIREMENTS
- Visual style: modern academic + industry polish, minimal clutter.
- Color palette: navy, slate, white, with one accent color (teal or orange).
- Typography: clean sans-serif, strong hierarchy.
- Layout: generous whitespace, consistent margins, no crowded slides.
- Animations: subtle (fade/wipe), only where they improve understanding.
- Output should feel like a conference-quality deck.

MANDATORY CONTENT SECTIONS
1. Problem motivation:
- Why unified audio representation for both SV and deepfake matters.
- Practical use cases and risk context.

2. Data pipeline:
- VoxCeleb1 for SV (streaming/chunked handling due size)
- ASVspoof 2019 LA for deepfake
- Chunk-wise training pipeline (load -> train -> release memory)

3. Model architecture:
- Student: wav2vec2-base
- Temporal module: BiLSTM aggregator
- Two projection heads: SV head + DF head
- Teacher A: ReDimNet (frozen)
- Teacher B: ASSIST (frozen)
- Clear architecture diagram with tensor flow.

4. Objective function:
- Show loss1, loss2, total loss equation clearly.
- Explain why cosine similarity is used in embedding alignment.

5. Training strategy:
- Phase 1 distillation
- Phase 2 selective fine-tuning
- Important hyperparameters and rationale

6. Results:
- Training loss trends (total, SV, DF)
- Qualitative/quantitative observations
- Interpretation of what improved and why

7. Validation and sanity checks:
- Embedding similarity checks
- Robustness considerations
- Failure cases / limitations

8. Conclusion and future work:
- Key achievements
- Next improvements (better teachers, data balancing, stronger evaluation metrics)

OUTPUT FORMAT
- First provide a slide table (Slide #, Title, Purpose).
- Then provide detailed slide content one by one.
- Include a final section: "How to build this deck in PowerPoint quickly" with practical build steps (theme, master slide, chart styles, icon sources).
- Add a compact appendix suggestion (extra backup slides).

TONE
- Professional, clear, confident, non-hype.
- Avoid generic filler text.
- Every slide must have a purpose.
```
