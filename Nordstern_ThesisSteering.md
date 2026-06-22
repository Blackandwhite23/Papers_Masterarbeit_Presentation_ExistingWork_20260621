# Nordstern — Thesis Steering Document

**Thesis:** *Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*
**Author:** Stefan Dosch (IU, M.Sc. Data Science) · **Tutor:** Tim Schlippe

*Purpose: single source of truth on research direction, gap, and scope. Use it to steer literature extraction and synthesis. Judge every paper's relevance against Sections 3–8.*

---

## 1. Core research question
How accurately can current multimodal foundation models transcribe spoken Mandarin into a phonetic representation that captures both segmental information (phonemes) **and** tones — and is this transcription reliable enough, out of the box, to underpin pronunciation feedback for learners?

The vocabulary-trainer application is the **motivation**, not the object of study. The **transcription step itself** is the object of study.

## 2. Operational research questions
- **RQ1 — text-only baseline.** `[CORE]` Character → phonetic transcription, (a) without tones, (b) with tones. Isolates the model's linguistic knowledge from acoustic perception.
- **RQ2 — native speech.** `[CORE]` Transcription accuracy at (a) word/character, (b) phoneme, (c) tone level. Upper bound under favorable conditions.
- **RQ3 — non-native learner speech.** `[STRETCH — time-permitting]` Same three levels; the actual target scenario for a learning tool.
- **RQ4 — vs. dedicated systems.** `[CORE]` How do general-purpose multimodal LLMs compare to Whisper and established pronunciation-assessment baselines?
- **RQ5 — error structure.** `[CORE]` Which tones / phoneme contrasts are most often misrecognized; what characteristic confusion patterns emerge.

## 3. The research gap (what the literature leaves open)
1. Frontier multimodal / audio LLMs are benchmarked mostly at **sentence/word level (WER/CER)**; **isolated syllable / phoneme / tone granularity for Mandarin is largely unexamined.**
2. **Tones are rarely evaluated as an explicit, separate axis** — classical ASR often omits them entirely.
3. **Very few studies cover non-native / L2 learner speech**, which is exactly the feedback-relevant case.
4. **No systematic head-to-head** of the best current general-purpose models, against each other and against dedicated systems, at this granularity.

## 4. How this thesis closes the gap (contribution)
**Committed core — addresses gaps 1, 2, 4.** A systematic, prompt-based evaluation of a selection of current frontier multimodal models at word / phoneme / tone granularity, progressing **text-only (RQ1) → native speech (RQ2)**, with **explicit tone-level evaluation**, benchmarked against dedicated baselines such as Whisper (RQ4), plus an error-structure analysis (RQ5). No model surgery — systems are used off the shelf and compared/combined. The "can anyone do this with an off-the-shelf model?" angle is treated as a **research question** (how good are they, really?), not a product pitch. This contribution stands on its own even if Phase 2 is not reached.

**Planned extension, time-permitting — addresses gap 3.** Non-native / L2 learner speech (RQ3), the feedback-relevant case. If reached, this becomes a distinctive part of the contribution; if not, it is positioned as immediate future work. The thesis does **not** depend on it to make a valid contribution.

## 5. Key design constraints (use these to judge relevance)
- **Representation:** Pinyin with tone numbers (e.g. `ma1`) as the phoneme+tone target. Must be **fixed in the prompt** so models don't each pick a different scheme (Pinyin vs. IPA vs. numbers), which would inflate error rates artificially.
- **Target-leakage / auto-correction problem ("linguistic trap" / lexicon confound):** general LLMs tend to **normalize** learner errors — when the intended/canonical word is available, they transcribe what *should* have been said, not what *was* said. Prompts and evaluation must prevent the model from seeing the target word. This is a core methodological constraint, not a side note.
- **Models:** ~5–8 current frontier multimodal models + 2–3 dedicated baselines (Whisper etc.).
- **Metrics:** PER, tone error rate (TER), CER, F1, FAR/FRR; per-tone and per-contrast breakdowns for RQ5.

## 6. Scope (in / out)
**Phase 1 — committed:**
- Text-only baseline (RQ1) and native-speaker speech (RQ2) at tone / phoneme / word levels.
- Corpus: **Tone Perfect** (isolated monosyllables, native speakers, tone-labelled) as the primary data spine. Note: monosyllabic, so "word level" here is effectively single-character/syllable level.
- Comparison against dedicated baselines incl. **Whisper** (RQ4); error-structure analysis (RQ5).

**Phase 2 — stretch, time-permitting:**
- Non-native / L2 learner speech (RQ3).
- A small self-collected L2 set, following the meeting's 5-field data structure (target word · target Pinyin phoneme sequence · learner audio · produced/confused word · produced Pinyin phoneme sequence).

**Out of scope (future work):** full vocabulary-trainer build; UI; longitudinal learning-outcome studies; model fine-tuning or architectural changes.

## 7. Related Work skeleton (map each paper to one slot)
- **2.2** Traditional methods (2.2.1 pre-Transformer: HMM, statistical LM, CRF · 2.2.2 Transformer-based: HuBERT, BERT…)
- **2.3** LLMs/FMs in language learning (2.3.1 vocabulary · 2.3.2 pronunciation)
- **2.4** LLMs/FMs for Chinese speech processing (2.4.1 speech recognition · 2.4.2 tone recognition)
- **2.5** Evaluation of pronunciation and tone recognition
- **2.6** Corpora for Chinese pronunciation evaluation
- **2.7** Learnings & research gaps

## 8. What makes a paper "central" vs. "context"
- **Central:** Mandarin **+** phoneme/tone granularity **+** (LLM/foundation model **or** dedicated MDD/APA) **+** ideally L2 speakers or explicit tone evaluation.
- **Context:** general audio-LLM surveys, sentence-level Chinese ASR, other tonal languages, corpus descriptions.
- Tag each paper as *central* or *context* and assign it a Related-Work slot (§7) during extraction.
