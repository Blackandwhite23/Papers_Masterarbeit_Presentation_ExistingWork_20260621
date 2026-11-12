# PowerPoint — Datensätze in der Literatur
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Vocabulary*

> **Zweck:** Folien zum Thema „Welche Datensätze verwenden die 42 Papers?" — mit Fokus auf Mandarin, Töne, Lautsprache und Audio→LLM-Pipeline.
>
> **Legende:** ★★ = direkt zentral · ★ = direkt relevant · ☆ = mittelbar relevant

---

# Folie D1: Datensätze — Überblick

**Folientitel:** Datasets Used in 42 Papers: Scale and Scope

| Kategorie | Papers | Typische Datensätze | Stunden | Töne? |
|---|---|---|---|---|
| **Speech LLMs** (7) | Seed-ASR, FireRedASR, FireRedASR2S, Kimi-Audio, Qwen3-ASR, Step-Audio 2, Peng Survey | Proprietär + AISHELL-1/2, WenetSpeech, LibriSpeech, CommonVoice | 70k–40M h | Nie getestet |
| **Pronunciation/MDD** (7) | Wang MDD, Wu L2, Huang Review, Zhu ZIPA, Kang, Wang MSPB, Chirkova | AISHELL-1, LATIC, Custom, IPAPACK++ | 3–17k h | Meist ja |
| **ASR Error Correction** (4) | Li PY-GEC, Liang PERL, Zhengjie PYG, Chen SCCM | AISHELL-1, CommonVoice | 170–200 h | Pinyin-Ebene |
| **Benchmarks** (9) | Wei ASR-EC, ContextASR, VocalBench-zh, TELEVAL, MMAU, Dynamic-SUPERB, VoxEval, AudioBench, WildSpeech | Eigen-konstruiert (TTS + real) | Variiert | Selten |
| **Surveys** (10) | Latif, Gaido, Cui, Arora, Luo, AI Index, Ahlawat, Kaur, Yang×2 | (Überblick) | — | Kaur: Fokus |
| **Tone Perfect** (2) | Ryu (2021), Xu SITA (2026) | **Tone Perfect** (9.840 Silben) | ~2 h | **Kern** |

> **Sprecher-Notizen:**
> - Die 7 Speech LLMs trainieren auf **20M+ bis 40M Stunden** Audio — aber evaluieren nur mit CER auf Zeichenebene, nie auf Ton-/Phonemebene
> - Nur 10 von 42 Papers evaluieren Töne explizit
> - Kein einziges Paper evaluiert ein multimodales LLM auf Tontranskription
> - AISHELL-1 ist der mit Abstand häufigste Benchmark (in 15+ Papers)

---

# Folie D2: Trainingsdaten-Skala der Speech LLMs

**Folientitel:** Training Data Scale: The Big Players

```
Training Data (Hours, log scale)

Qwen3-ASR        ████████████████████████████████████  40M h (pseudo-labeled)
Seed-ASR (SSL)    ██████████████████████████████████   20M h (unlabeled)
Kimi-Audio        ████████████████████████████████     13M h (pre-training)
Step-Audio 2      █████████████████████████            8M h (audio+synth)
Seed-ASR (SFT)    █████████████                        900k h (labeled)
Qwen3-ASR (SFT)   ███████████                         ~300k h
FireRedASR2S      ████████                             200k h
FireRedASR        █████                                70k h
AISHELL-1         ▌                                    170 h
Tone Perfect      ▏                                    ~2 h
```

**Kernaussage:** *Millionen Stunden Training — aber NIE auf Tonebene evaluiert.*

> **Sprecher-Notizen:**
> - Seed-ASR: "training on over 20 million hours of speech data and nearly 900 thousand hours of paired ASR data" (Abstract, S. 1)
> - Qwen3-ASR: "approximately 40 million hours of pseudo-labeled ASR data" (S. 3)
> - Kimi-Audio: "approximately 13 million hours of raw audio" (S. 6)
> - Diese Modelle haben Mandarin-Töne hunderte Millionen Mal "gehört" — aber niemand hat geprüft, ob sie die Töne auch VERSTEHEN
> - Tone Perfect: 9.840 Silben, ~2 Stunden — winzig, aber PERFEKT für isolierte Tonevaluation

---

# Folie D3: Die wichtigsten Mandarin-Datensätze

**Folientitel:** Key Mandarin Datasets Across 42 Papers

| Datensatz | Stunden | Sprecher | Verwendet in | Töne? | Für uns? |
|---|---|---|---|---|---|
| **AISHELL-1** | 170 h | 400 Native | 15+ Papers (fast alle) | Nein (CER only) | Vergleichs-Benchmark |
| **AISHELL-2** | 1.000 h | — | 8+ Papers | Nein | Vergleich |
| **WenetSpeech** | 10.000+ h | — | 6+ Papers (net/meeting) | Nein | Vergleich |
| **CommonVoice ZH** | 100+ h | Crowd | 4+ Papers | Nein | Vergleich |
| **LATIC** | 4 h | 4 L2 | Wang MDD (1 Paper) | T1-T5 | L2-Referenz |
| **THCHS-30** | 30 h | — | 2 Papers | Nein | — |
| **★ Tone Perfect** | ~2 h | 6 Native | 2 Papers (Ryu, SITA) | **T1-T4** | **Unser Korpus** |
| **MSPB** | — | 1+27 | Wang MSPB (1 Paper) | Prosodie | — |
| **Custom Baima** | 3,1 h | 3 | Chirkova (1 Paper) | 6 Töne | IPA-Vergleich |

> **Sprecher-Notizen:**
> - AISHELL-1 ist der "MNIST der chinesischen ASR" — überall verwendet, aber nur CER, nie Töne
> - Tone Perfect ist der EINZIGE öffentlich verfügbare Datensatz mit isolierten Silben × 4 Töne × 6 Sprecher — ideal für kontrollierte Tonevaluation
> - "The Tone Perfect corpus contains 9,840 recordings of 410 Mandarin syllables, each spoken in all 4 tones by 6 Beijing native speakers" (Ryu 2021)
> - LATIC (4 Stunden, 4 L2-Sprecher) ist der einzige öffentliche L2-Mandarin-Datensatz mit Tonannotation — extrem klein

---

# Folie D4: Audio→LLM Pipeline — Wer kann Audio DIREKT verarbeiten?

**Folientitel:** Audio Processing: Who Takes Audio Directly?

| Pipeline-Typ | Modelle | Töne evaluiert? |
|---|---|---|
| **Encoder→Adapter→LLM** (direkt) | Seed-ASR (MoE 10B+), FireRedASR (Qwen2-7B), FireRedASR2S, Qwen3-ASR (Qwen3-1.7B) | ❌ Nie |
| **Hybrid (diskret+kont.)→LLM** | Kimi-Audio (Qwen2.5-7B), Step-Audio 2 (Custom 130B) | ❌ Nie ("tone" erwähnt, nicht evaluiert) |
| **Audio→Encoder→LLM→Pinyin+Text** | Zhengjie PYG-ASR (HuBERT→Qwen2-7B) | ⚠️ Pinyin-ERR (1,9%), aber OHNE separaten Ton-Test |
| **Multimodal (Audio+Text Prompt)** | Wang MSPB: GPT-4o, Gemini-1.5-Pro, Qwen2-Audio-7B | ⚠️ Satzprosodie, nicht lexikalische Töne |
| **ASR→Text→LLM** (kaskadiert) | Li PY-GEC (Whisper→LLaMA-3-8B), Liang PERL (Whisper→BERT) | ❌ (Text-only) |
| **Klassisch (kein LLM)** | Wang MDD (HuBERT), Chirkova (Wav2Vec2), Du (Zipformer) | ✅ Aber kein LLM |

**Kernaussage:** *Viele Modelle können Audio direkt verarbeiten — aber KEINES wird auf Tontranskription evaluiert.*

> **Sprecher-Notizen:**
> - Kimi-Audio erwähnt "tone" explizit als wichtige paralinguistische Information: "text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)" (Section 8, S. 21) [VERIFIZIERT]
> - Zhengjie ist das NÄCHSTE an unserer Arbeit: Audio→LLM→Pinyin — aber "there has been little exploration of leveraging LLMs to map audio features directly to Pinyin representations" (Introduction, S. 1) [VERIFIZIERT]
> - Wang MSPB ist das einzige Paper, das multimodale LLMs (GPT-4o, Gemini, Qwen2-Audio) auf Audio-Prosodie testet — aber nur Satzprosodie (Frage/Aussage), nicht lexikalische Töne (ma1/ma2/ma3/ma4)

---

# Folie D5: Tone Perfect — Unser Datensatz

**Folientitel:** Tone Perfect: Why This Dataset?

```
Tone Perfect (Ryu et al. 2021, Michigan State University)
├── 410 einzigartige Silben (alle Standard-Mandarin-Silben)
├── × 4 Töne (T1 ˉ, T2 ˊ, T3 ˇ, T4 ˋ)
├── × 6 Sprecher (3m, 3f, alle Beijing-Muttersprachler)
├── = 9.840 Aufnahmen
├── Format: WAV, einzelne Silben, professionelle Aufnahme
├── Lizenz: CC-BY 4.0 (Open Access)
└── Labels: Pinyin + Tonnummer (z.B. "ma1", "ma2", "ma3", "ma4")
```

**Warum Tone Perfect?**
1. **Kontrolliert:** Isolierte Silben → Ton-Evaluation ohne Kontexteffekte
2. **Vollständig:** Alle 410 Mandarin-Silben × 4 Töne
3. **Balanciert:** Gleiche Anzahl pro Ton, pro Sprecher
4. **Reproduzierbar:** Öffentlich, CC-BY 4.0
5. **Ground Truth:** Professionelle Annotation (Pinyin + Tonnummer)

> **Sprecher-Notizen:**
> - Bisher nur in 2 Papers verwendet: Ryu (2021) als Datensatz-Paper und Xu SITA (2026) für Mandarin-Transfer
> - SITA fand "tone collapse" in Whisper-Embeddings: "embeddings from Whisper large-v3 exhibit a notable degree of tone collapse" — Töne werden im Embedding-Raum nicht gut unterschieden
> - Für unsere Arbeit: Wir nutzen Tone Perfect als kontrollierten Evaluationsdatensatz — jedes Sample hat exakt EINE Silbe mit exakt EINEM Ton → kein Tone Sandhi, keine Satzprosodie, reiner Tonerkennungstest
> - Limitation: Nur isolierte Silben, keine connected speech — das ist GEWOLLT für den ersten Test, aber muss als Limitation diskutiert werden

---

# Folie D6: Gap-Analyse — Was fehlt in den Datensätzen?

**Folientitel:** Dataset Gaps: What's Missing?

| Was haben wir | Was fehlt |
|---|---|
| AISHELL-1/2: 170-1.000h Mandarin | Aber: nur CER auf Zeichenebene evaluiert |
| 20M+ Stunden Trainingsdaten | Aber: nie auf Tonebene getestet |
| 15+ Papers nutzen AISHELL-1 | Aber: kein Paper testet Silbe+Ton separat |
| Tone Perfect: perfektes Ton-Korpus | Aber: nur 2 Papers nutzen es |
| LATIC: einziger L2-Datensatz mit Tönen | Aber: nur 4h, 4 Sprecher, 1 Paper |

**3 Lücken:**
1. **Kein Datensatz** wird für LLM-Tonevaluation genutzt
2. **Kein Paper** evaluiert multimodale LLMs auf Tone Perfect
3. **Kein L2-Mandarin-Datensatz** wurde mit LLMs getestet

> **Sprecher-Notizen:**
> - Die Literatur hat einen blinden Fleck: Millionen Stunden Training, aber die fundamentalste Eigenschaft des Mandarin — die 4 Töne — wird nie evaluiert
> - Zou (2025) empfiehlt TER (Tone Error Rate) als Metrik: "Tone Error Rate (TER) analogous to Word Error Rate is preferred" — aber KEIN Paper wendet TER tatsächlich an!
> - Unsere Arbeit schließt alle 3 Lücken: Tone Perfect + multimodale LLMs + TER als Metrik
