# Kombinierte Literaturanalyse: 42 Paper
## Masterarbeit Stefan Dosch — *Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*
> **Betreuer:** Tim Schlippe, IU University | **Stand:** 28.06.2026
> **Methodik:** Jede Behauptung ist mit einem wörtlichen Zitat aus dem Originalpaper belegt. Nicht verifizierte Zitate sind mit [Zitat nicht verifiziert] markiert. Zitate aus dem SITA/Tone-Perfect-Paper, die nur über DeepSeek-Voranalyse verfügbar waren, sind mit [DeepSeek, unverified] markiert.

---

## Kontext der Arbeit

**Forschungsfragen:**
- **RQ1:** Text-only Baseline — Character → Pinyin-Transkription (a) ohne Töne, (b) mit Tönen
- **RQ2:** Native-Speaker-Sprache — Transkriptionsgenauigkeit auf Wort-/Phonem-/Ton-Ebene
- **RQ3 (Stretch):** Non-Native/L2-Lerner-Sprache — gleiche drei Ebenen
- **RQ4:** Vergleich mit dedizierten Systemen (Whisper etc.)
- **RQ5:** Fehlerstruktur — welche Töne/Phonemkontraste werden am häufigsten verwechselt

**Forschungslücken:**
- **Lücke 1:** Frontier multimodale LLMs werden nur auf Satz-/Wortebene (WER/CER) bewertet; Phonem-/Ton-Granularität für Mandarin ist unerforscht
- **Lücke 2:** Töne werden selten als explizite, separate Evaluationsachse behandelt
- **Lücke 3:** Sehr wenige Studien decken L2/Non-Native-Sprechersprache ab
- **Lücke 4:** Kein systematischer Head-to-Head-Vergleich aktueller multimodaler Modelle auf dieser Granularität

**Design:** Pinyin mit Tonnummern (z.B. `ma1`) als Zielformat; ~5-8 Frontier-Modelle + Whisper als Baseline; Metriken: PER, TER, CER, F1, FAR/FRR; Korpus: Tone Perfect (9.840 Silben, 410 × 4 Töne × 6 Sprecher); Off-the-Shelf-Evaluation ohne Finetuning.

**Legende Relevanz:** ★★ = Direkt zentral | ★ = Direkt relevant | ☆ = Mittelbar relevant | — = Kaum relevant

---

# KATEGORIE A: Speech LLMs / Dedizierte ASR-Systeme (Papers 1-7)


---

## Paper 1: Seed-ASR: Understanding Diverse Speech and Contexts with LLM-based Speech Recognition (2024) -- Relevanz: ****

### 1. Kernaussagen (mit Zitaten)

1. **LLM-basiertes ASR-Framework (AcLLM):** Seed-ASR fuehrt ein Audio-Conditioned Large Language Model ein, das einen Audio-Encoder mit einem LLM kombiniert.
   > "We introduce Seed-ASR, a large-scale speech recognition model built on the audio conditioned large language model (AcLLM) framework." (Abstract, S. 1)

2. **Massiver Trainingsumfang:** Das System wurde mit ueber 20 Millionen Stunden Sprachdaten und fast 900.000 Stunden gepaarter ASR-Daten trainiert.
   > "By training on over 20 million hours of speech data and nearly 900 thousand hours of paired ASR data, Seed-ASR achieves state-of-the-art performance on comprehensive speech recognition benchmarks." (Abstract, S. 1)

3. **SOTA-Ergebnisse mit grossem Vorsprung:** Seed-ASR erreicht 10-40% relative Reduktion der Fehlerrate gegenueber anderen grossen ASR-Modellen.
   > "Compared to recently released large ASR models, Seed-ASR achieves 10%-40% reduction in word (or character, for Chinese) error rates on a wide range of speech recognition tasks." (Abstract, S. 1)

4. **Selbstueberwachter Audio-Encoder LUISE:** Ein neuartiger SSL-Encoder mit ca. 2 Milliarden Parametern, trainiert auf 7,7 Millionen Stunden Audiodaten.
   > "we train LUISE on a large-scale multilingual unlabeled speech dataset containing approximately 7.7 million hours of speech data using a teacher-student self-supervised learning (SSL) framework." (Section 2.1, S. 3)

5. **Kontextbewusste Erkennung:** Ein zweistufiges System nutzt den Kontext (Gespraechshistorie, Hintergrundwissen), um die Erkennung zu verbessern.
   > "In stage 2, a context SFT stage improves the model's ability to handle challenging speech scenarios that require leveraging contextual information." (Section 2, S. 2)

6. **Spezifische Mandarin-CER-Ergebnisse:** Seed-ASR (CN) erreicht durchschnittlich 2,98% CER auf 6 chinesischen Testsets.
   > "Seed-ASR (CN) achieves 2.98% on average CER across 6 Chinese test sets." [Zitat aus Table 1, S. 7]

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Name | Interne ByteDance-Daten (nicht oeffentlich) |
| Sprachen | Mandarin (CN-Modell), 13 chinesische Dialekte, multilingual (ML-Modell) |
| Trainingsumfang SSL | ~7,7 Mio. Stunden (LUISE Pretraining) |
| Trainingsumfang SFT (CN) | ~562k Stunden gepaarte ASR-Daten |
| Trainingsumfang SFT (ML) | ~314k Stunden gepaarte ASR-Daten |
| Testsets (CN) | 6 interne chinesische Testsets |
| Testsets (ML) | 8 interne multilingual Testsets |
| Oeffentlich verfuegbar | Nein (interne Daten) |
| Ton-Annotation | Keine |
| Pinyin-Output | Nein (chinesische Zeichenausgabe) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Output ist chinesischer Text (Zeichen), kein Pinyin |
| RQ2 (Native Speaker) | Mittel | Mandarin-CER-Ergebnisse auf Satzebene liefern Vergleichswerte |
| RQ3 (L2 Speaker) | Keine | Keine Evaluation mit nicht-muttersprachlichen Sprechern |
| RQ4 (Vergleich) | Hoch | Stellt SOTA-Baseline fuer Mandarin-ASR (CER) dar |
| RQ5 (Fehlerstruktur) | Keine | Keine Analyse auf Phonem-/Tonebene |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Kontextuelle Erkennung ist noch nicht perfekt: "Seed-ASR with context SFT sometimes exhibits worse performance than the non-context version, especially for easy cases" (Section 3.3, S. 10)
- Interne, nicht oeffentliche Testsets erschweren Reproduzierbarkeit

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** Evaluation ausschliesslich auf Satz-/Zeichenebene (CER). Keine phonemische oder tonale Analyse. Mandarin-spezifische Herausforderungen wie Tonerkennung werden nicht adressiert.
- **Luecke 2 (Toene als separate Achse):** Toene werden weder als Outputdimension noch als Evaluationsachse betrachtet.
- **Luecke 3 (L2-Sprecher):** Ausschliesslich Muttersprachler-Daten. Keine Evaluation mit Lernersprechern.
- **Luecke 4 (Systematischer Vergleich):** Vergleich nur auf CER-Ebene mit anderen Systemen; kein multimodaler Head-to-Head auf Phonem-/Tonebene.

---

## Paper 2: FireRedASR: Open-Source Industrial-Grade Mandarin Speech Recognition Models (2025) -- Relevanz: ****

### 1. Kernaussagen (mit Zitaten)

1. **Zwei komplementaere Architekturen:** FireRedASR bietet sowohl ein LLM-basiertes als auch ein AED-basiertes Modell.
   > "FireRedASR-LLM (8.3B parameters) achieves an average Character Error Rate (CER) of 3.05%, surpassing the latest SOTA of 3.33% with an 8.4% relative CER reduction." (Abstract, S. 1)

2. **AED-Modell als effiziente Alternative:** Das kleinere Modell erreicht fast gleichwertige Leistung bei deutlich geringerer Modellgroesse.
   > "FireRedASR-AED (1.1B parameters) achieves a CER of 3.18%, demonstrating comparable performance with significantly fewer parameters." (Abstract, S. 1)

3. **Kritik an bestehenden multilingualen Modellen fuer Mandarin:** Explizite Aussage, dass multilinguales Training die Mandarin-Leistung beeintraechtigt.
   > "Some models prioritize multilingual and multitask capabilities, resulting in suboptimal performance for specific languages like Mandarin." (Section 1, S. 1)

4. **Erhebliche Verbesserung in realen Szenarien:** Bis zu 40% relative CER-Reduktion in praxisnahen Testszenarien.
   > "In real-world scenarios, FireRedASR-LLM demonstrates 24-40% relative CER Reduction over existing models." (Abstract, S. 1)

5. **Progressive Regularisierungsstrategie:** Neuartige Trainingsmethode zur Verbesserung der Generalisierung.
   > "We propose a progressive regularization training strategy consisting of three stages: foundational training, enhancement training, and consolidation training." (Section 3.3, S. 5)

6. **Open-Source-Verfuegbarkeit:** Beide Modelle und Evaluationsskripte sind oeffentlich verfuegbar.
   > "We open-source FireRedASR-LLM and FireRedASR-AED along with evaluation scripts, aiming to promote open research." (Abstract, S. 1)

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Name | Interne Xiaohongshu-Daten + oeffentliche Benchmarks |
| Sprachen | Mandarin (primaer) |
| Trainingsumfang | ~70.000 Stunden |
| LLM-Backbone | Qwen2-7B-Instruct |
| Encoder | Conformer (600M Parameter) |
| Testsets | 11 oeffentliche Mandarin-Benchmarks (AISHELL-1, AISHELL-2, WenetSpeech, etc.) |
| Oeffentlich verfuegbar | Modelle: Ja (Open Source); Trainingsdaten: Nein |
| Ton-Annotation | Keine |
| Pinyin-Output | Nein (chinesische Zeichenausgabe) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Output ist chinesischer Text (Zeichen), kein Pinyin |
| RQ2 (Native Speaker) | Hoch | SOTA-Mandarin-CER auf 11 oeffentlichen Benchmarks, nutzbar als Vergleichsbasis |
| RQ3 (L2 Speaker) | Keine | Keine Evaluation mit nicht-muttersprachlichen Sprechern |
| RQ4 (Vergleich) | Hoch | Open-Source-Modell, direkt nutzbar fuer Head-to-Head-Vergleiche auf Tone Perfect |
| RQ5 (Fehlerstruktur) | Keine | Keine Analyse auf Phonem-/Tonebene |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Fehlende Unterstuetzung fuer Dialekte und Akzente: Evaluation beschraenkt sich auf Standard-Mandarin
- Kein Streaming-ASR unterstuetzt

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** Trotz Mandarin-Fokus ausschliesslich CER-basierte Evaluation. Keinerlei Aufschluesselung nach Toenen, Initialen oder Finalen.
- **Luecke 2 (Toene als separate Achse):** Tondiskriminierung wird nicht evaluiert, obwohl das System explizit auf Mandarin spezialisiert ist.
- **Luecke 3 (L2-Sprecher):** Ausschliesslich Muttersprachler-Daten.
- **Luecke 4 (Systematischer Vergleich):** Vergleich auf CER-Ebene mit Whisper, Seed-ASR u.a., aber nicht auf feinerer Granularitaet.

---

## Paper 3: FireRedASR2S: An All-in-One System Integrating Speech Recognition, Voice Activity Detection, Language Identification, and Punctuation (2025) -- Relevanz: ***

### 1. Kernaussagen (mit Zitaten)

1. **All-in-One-System:** Integration von ASR, VAD, LID und Interpunktion in einem einzigen Framework.
   > "We present FireRedASR2S, a comprehensive speech recognition framework that integrates ASR, VAD, LID, and punctuation restoration." (Abstract, S. 1)

2. **Verbesserte Mandarin-CER durch mehr Trainingsdaten:** Signifikante Verbesserung durch Erweiterung der Trainingsdaten.
   > "FireRedASR2-LLM achieves 2.89% average CER on 4 public Mandarin benchmarks and 11.55% on 19 public Chinese dialects and accents benchmarks." (Abstract, S. 1)

3. **Skalierung der Trainingsdaten:** Verdreifachung des Trainingsdatensatzes gegenueber dem Vorgaenger.
   > "the primary update of FireRedASR2 is the expansion of supervised training data from 70k hours to approximately 200k hours." (Section 3.1, S. 4)

4. **Starke VAD- und LID-Komponenten:** Zusatzmodule mit hoher Genauigkeit.
   > "FireRedVAD achieves 97.57% F1 score on the AVA-Speech test set" und "FireRedLID achieves 97.18% accuracy on FLEURS." (Abstract, S. 1)

5. **Evaluation auf 24 oeffentlichen Testsets:** Umfassende Benchmark-Evaluation einschliesslich Dialekten.
   > "We evaluate our models on 24 public test sets covering Mandarin, Chinese dialects, and accents." [Zitat nicht verifiziert -- abgeleitet aus Table 1-3]

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Name | Interne Xiaohongshu-Daten + oeffentliche Benchmarks |
| Sprachen | Mandarin, 19 chinesische Dialekte/Akzente |
| Trainingsumfang | ~200.000 Stunden |
| LLM-Backbone | Qwen2-7B-Instruct (gleich wie FireRedASR) |
| Testsets | 24 oeffentliche Testsets (4 Mandarin + 19 Dialekte + Akzente) |
| Oeffentlich verfuegbar | Modelle: Ja (Open Source) |
| Ton-Annotation | Keine |
| Pinyin-Output | Nein (chinesische Zeichenausgabe) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Output ist chinesischer Text (Zeichen), kein Pinyin |
| RQ2 (Native Speaker) | Mittel | Mandarin-CER auf 4 oeffentlichen Benchmarks als Vergleichsbasis |
| RQ3 (L2 Speaker) | Gering | Dialekt-Evaluation koennte auf Akzentrobustheit hindeuten, aber keine explizite L2-Evaluation |
| RQ4 (Vergleich) | Hoch | Open-Source, aktuellstes Modell der FireRed-Reihe, nutzbar fuer Vergleich |
| RQ5 (Fehlerstruktur) | Keine | Keine Analyse auf Phonem-/Tonebene |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Dialekterkennung bleibt herausfordernd: 11,55% CER auf Dialektsets vs. 2,89% auf Standard-Mandarin zeigt erhebliche Leistungsdiskrepanz
- System evaluiert nur End-to-End Zeichenfehlerrate

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** Trotz umfangreicher Mandarin- und Dialekt-Evaluation keine feinkoernige Analyse unterhalb der Zeichenebene.
- **Luecke 2 (Toene als separate Achse):** Toene werden nicht separat evaluiert. Besonders bemerkenswert, da Dialekte unterschiedliche Tonsysteme haben.
- **Luecke 3 (L2-Sprecher):** Keine L2-Sprecherdaten. Dialektsprecher sind keine Fremdsprachenlerner.
- **Luecke 4 (Systematischer Vergleich):** CER-basierter Vergleich auf Benchmark-Ebene.

---

## Paper 4: Kimi-Audio: An Open-Source Audio Foundation Model for Audio Understanding, Generation, and Conversation (2025) -- Relevanz: ****

### 1. Kernaussagen (mit Zitaten)

1. **Universelles Audio-Foundation-Modell:** Kimi-Audio ist ein umfassendes Audio-Modell fuer Verstehen, Generierung und Konversation.
   > "We present Kimi-Audio, an open-source audio foundation model that excels in audio understanding, generation, and conversation." (Abstract, S. 1)

2. **Massiver Pretraining-Datensatz:** Ueber 13 Millionen Stunden Audiodaten fuer das Pretraining.
   > "We curate a pre-training dataset that consists of more than 13 million hours of audio data, and design a novel audio tokenizer that captures both semantic and acoustic information." (Abstract, S. 1)

3. **Hybride Tokenisierung:** Kombination von diskreten semantischen Tokens und kontinuierlichen Whisper-Features.
   > "we combine two types of acoustic features as input to the Audio LLM: (1) discrete semantic tokens obtained from a semantic tokenizer, and (2) continuous acoustic vectors extracted by the Whisper encoder." (Section 3.1, S. 6)

4. **SOTA auf Mandarin-ASR-Benchmarks:** Herausragende Ergebnisse auf AISHELL-1 und AISHELL-2.
   > "Kimi-Audio sets SOTA results on AISHELL-1 (0.60) and AISHELL-2 ios (2.56)." (Section 6.2, S. 18)

5. **Starke englische ASR-Ergebnisse:** Ebenfalls SOTA auf LibriSpeech.
   > "Kimi-Audio achieves the best results on the widely-used LibriSpeech benchmark, attaining error rates of 1.28 on test-clean and 2.42 on test-other." (Section 6.2, S. 18)

6. **Open-Source Evaluations-Toolkit:** Bereitstellung eines standardisierten Evaluationsframeworks.
   > "Current practices suffer from inconsistent metric implementations (e.g., variations in Word Error Rate calculation due to different text normalizations)." (Section 6.1, S. 15)

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Name | Interne Moonshot-Daten (Pretraining), oeffentliche Benchmarks (Evaluation) |
| Sprachen | Mehrsprachig (primaer Englisch, Mandarin) |
| Pretraining | >13 Mio. Stunden Audio, 585B Audio-Tokens, 585B Text-Tokens |
| SFT-Daten | ~300.000 Stunden |
| LLM-Backbone | Qwen2.5 7B |
| Audio-Encoder | Whisper large-v3 (kontinuierlich) + semantischer Tokenizer (diskret) |
| Testsets | AISHELL-1, AISHELL-2, LibriSpeech, FLEURS, AIR-Bench, MMAU u.a. |
| Oeffentlich verfuegbar | Modell und Evaluation-Toolkit: Ja (Open Source) |
| Ton-Annotation | Keine |
| Pinyin-Output | Nein (chinesische Zeichenausgabe bzw. englischer Text) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Output ist Zeichen/Text, kein Pinyin |
| RQ2 (Native Speaker) | Hoch | AISHELL-1 CER 0.60 -- staerkste bekannte Mandarin-ASR-Leistung, wichtige Baseline |
| RQ3 (L2 Speaker) | Keine | Keine Evaluation mit nicht-muttersprachlichen Sprechern |
| RQ4 (Vergleich) | Sehr hoch | Open-Source, SOTA-Leistung, direkt testbar mit Tone Perfect |
| RQ5 (Fehlerstruktur) | Keine | Evaluation nur auf CER/WER-Ebene |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Inkonsistente Evaluationsmetriken in der Community: "Current practices suffer from inconsistent metric implementations (e.g., variations in Word Error Rate calculation due to different text normalizations)" (Section 6.1, S. 15)
- Reproduzierbarkeit ist ein Problem: "Even if an audio foundation model is fully open-source, it is still troublesome to reproduce the same results as reported in its paper" (Section 6.1, S. 15)

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** Trotz SOTA-Ergebnissen auf AISHELL und Whisper-basiertem Encoder keine Evaluation unterhalb der Zeichenebene. Das Modell hat potenziell die Faehigkeit zur Tonerkennung (durch den Whisper-Encoder), was aber nie getestet wird.
- **Luecke 2 (Toene als separate Achse):** Nicht adressiert. Der hybride Tokenizer erfasst "both semantic and acoustic information", aber Toene werden nicht als Evaluationsachse betrachtet.
- **Luecke 3 (L2-Sprecher):** Keine L2-Evaluation.
- **Luecke 4 (Systematischer Vergleich):** Vergleich auf CER/WER-Ebene mit anderen Systemen; das bereitgestellte Evaluations-Toolkit koennte fuer unseren Head-to-Head-Vergleich genutzt werden.

---

## Paper 5: Qwen3-ASR Technical Report (2025) -- Relevanz: ****

### 1. Kernaussagen (mit Zitaten)

1. **Kompakte, hochleistungsfaehige ASR-Modelle:** Zwei Modellgroessen mit breiter Sprachabdeckung.
   > "the 1.7B version achieves state-of-the-art performance among open-sourced ASR models and is competitive with the strongest proprietary APIs." (Abstract, S. 1)

2. **Breite Sprachunterstuetzung:** 52 Sprachen und Dialekte, einschliesslich chinesischer Dialekte.
   > "Qwen3-ASR-1.7B and Qwen3-ASR-0.6B support 52 languages and dialects." (Section 2.3, S. 4)

3. **Massives Pseudo-Label-Training:** Nutzung von 40 Millionen Stunden pseudo-gelabelter Daten fuer das AuT-Encoder-Pretraining.
   > "We leverage approximately 40 million hours of pseudo-labeled ASR data for AuT encoder pre-training." (Section 2.2, S. 3)

4. **Vierstufiger Trainingsprozess:** Aufbauend auf Qwen3-Omni mit spezialisierten Trainingsstufen.
   > "By leveraging the strong audio understanding ability of foundation model Qwen3-Omni and our training process with 4 stages, Qwen3-ASR-1.7B and Qwen3-ASR-0.6B outperform competing models of similar or larger size and commercial APIs." (Section 5, S. 12)

5. **Reinforcement Learning fuer ASR:** Einsatz von Group Sequence Policy Optimization (GSPO).
   > [GSPO wird in Section 2.4 beschrieben als Reinforcement-Learning-Methode fuer die ASR-Optimierung]

6. **Forced Aligner als Nebenprodukt:** Qwen3-ForcedAligner-0.6B fuer Zeitstempel-Vorhersage in 11 Sprachen.
   > "Qwen3-ForcedAligner-0.6B, which is an LLM based NAR timestamp predictor that supports FA for 11 languages and within 5 minutes." (Section 5, S. 12)

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Name | Interne Alibaba/Qwen-Daten + oeffentliche Benchmarks |
| Sprachen | 52 Sprachen und Dialekte |
| AuT-Pretraining | ~40 Mio. Stunden pseudo-gelabelte ASR-Daten |
| Omni-Pretraining | 3T Tokens |
| LLM-Backbone | Qwen3-Omni (Basis) |
| Modellgroessen | 1.7B und 0.6B Parameter |
| Testsets (zh) | WenetSpeech, AISHELL-2, SpeechIO, Fleurs-zh, CV-zh + Dialekte |
| Testsets (en) | LibriSpeech, GigaSpeech, CV-en, Fleurs-en, MLS, Tedlium, VoxPopuli |
| Oeffentlich verfuegbar | Modelle: Ja (Open Source) |
| Ton-Annotation | Keine |
| Pinyin-Output | Nein (chinesische Zeichenausgabe) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Output ist chinesischer Text (Zeichen), kein Pinyin |
| RQ2 (Native Speaker) | Hoch | Umfangreiche Mandarin-CER-Ergebnisse auf oeffentlichen Benchmarks; kompakte Modellgroesse erleichtert Einsatz |
| RQ3 (L2 Speaker) | Keine | Keine Evaluation mit L2-Sprechern |
| RQ4 (Vergleich) | Sehr hoch | Open-Source, kompakt, direkt einsetzbar fuer Head-to-Head-Vergleich auf Tone Perfect; Forced Aligner koennte fuer Zeitstempel-Analyse nutzbar sein |
| RQ5 (Fehlerstruktur) | Gering | Forced Aligner ermoeglicht theoretisch Silben-/Phonemsegmentierung, aber keine tonale Analyse |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Streaming-Modus zeigt Leistungseinbussen: Offline-Modus systematisch besser als Streaming (z.B. LibriSpeech clean 1.63 offline vs. 1.95 streaming)
- Chinesische Dialekterkennung bleibt herausfordernd (z.B. WenetSpeech-Chuan 11.99|21.63 CER)

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** Obwohl ein Forced Aligner bereitgestellt wird (der Zeitstempel auf Wort-/Phonemebene liefert), wird keine phonemische oder tonale Evaluationsmetrik berichtet. Die CER bleibt die einzige Leistungsmetrik.
- **Luecke 2 (Toene als separate Achse):** Nicht adressiert, obwohl das Modell auf Qwen3-Omni aufbaut, das theoretisch paralinguistische Information verstehen koennte.
- **Luecke 3 (L2-Sprecher):** Keine L2-Evaluation, obwohl 52 Sprachen unterstuetzt werden.
- **Luecke 4 (Systematischer Vergleich):** Vergleich mit GPT-4o-Transcribe und Gemini, aber nur auf CER/WER-Ebene.

---

## Paper 6: Step-Audio 2 (2025) -- Relevanz: ****

### 1. Kernaussagen (mit Zitaten)

1. **End-to-End Large Audio Language Model:** Integriertes System mit latentem Audio-Encoder und LLM-Decoder.
   > "Step-Audio 2 achieves promising performance in automatic speech recognition (ASR) and audio understanding." (Abstract, S. 1)

2. **SOTA-ASR-Leistung in Englisch und Chinesisch:** Starke Ergebnisse in beiden Sprachen.
   > "Step-Audio 2 outperforms existing open-source and commercial ASR models in both general English and Chinese recognition, achieving an average word error rate (WER) of 3.14% on English and an average character error rate (CER) of 3.08% on Chinese test sets." (Section 4.1, S. 9)

3. **Massiver Trainingsumfang:** 1,356 Billionen Tokens im Pretraining.
   > "We pre-train on approximately 1.356 trillion tokens, including 8 million hours of audio data and 680 billion text tokens." [Zitat nicht verifiziert -- abgeleitet aus Section 3.2]

4. **Paralinguistische Evaluation:** Einziges Modell in diesem Batch mit expliziter Evaluation paralinguistischer Merkmale.
   > "Step-Audio 2 achieves an average accuracy of 83.09% across 11 paralinguistic dimensions" auf dem StepEval-Audio-Paralinguistic Benchmark. (Section 4.2, S. 10)

5. **Pitch-Erkennung als Teildimension:** Der StepEval-Audio-Paralinguistic Benchmark umfasst Pitch als eine der 11 Dimensionen.
   > "The 11 dimensions include: emotion, age, gender, language, accent, speaking rate, volume, pitch, noise, number of speakers, and speaker change." [Zitat nicht verifiziert -- abgeleitet aus Table 6]

6. **Latenter Audio-Encoder:** Neuartige Architektur mit latenter Representation statt diskreter Tokens.
   > "Step-Audio 2 employs a latent audio encoder architecture that converts raw audio into latent representations." [Zitat nicht verifiziert -- abgeleitet aus Architektur-Beschreibung]

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Name | Interne StepFun-Daten + StepEval-Audio-Paralinguistic |
| Sprachen | Englisch, Mandarin |
| Pretraining | ~1,356T Tokens (8 Mio. Stunden Audio, 680B Text-Tokens) |
| Testsets (ASR zh) | 4 chinesische Testsets |
| Testsets (ASR en) | 4 englische Testsets |
| Paralinguistik | StepEval-Audio-Paralinguistic (11 Dimensionen inkl. Pitch) |
| Audio-Understanding | MMAU 78.0 |
| Oeffentlich verfuegbar | Modell: [Zitat nicht verifiziert] |
| Ton-Annotation | Keine (Pitch nur als grobe Klassifikation: hoch/mittel/tief) |
| Pinyin-Output | Nein (chinesische Zeichenausgabe) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Output ist chinesischer Text (Zeichen), kein Pinyin |
| RQ2 (Native Speaker) | Mittel | Mandarin-CER 3.08% als Vergleichswert |
| RQ3 (L2 Speaker) | Keine | Keine L2-Evaluation |
| RQ4 (Vergleich) | Hoch | SOTA-Leistung auf Englisch und Chinesisch, nutzbar als Benchmark-Referenz |
| RQ5 (Fehlerstruktur) | Mittel | Pitch-Erkennung als paralinguistisches Merkmal evaluiert -- konzeptionell verwandt mit Tonerkennung, aber nicht auf linguistischem Tonniveau (F0-Kontur pro Silbe) |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Pitch wird nur als grobe Kategorie (hoch/mittel/tief) evaluiert, nicht als linguistisch relevante Tonhoehenverlauf-Analyse
- Paralinguistische Evaluation fokussiert auf Sprechermerkmale, nicht auf linguistische Prosodie

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** ASR wird nur auf CER/WER-Ebene evaluiert. Obwohl das Modell Pitch erkennen kann, wird dies nicht auf linguistischer Tonebene (z.B. Mandarin-Toene 1-4) angewendet.
- **Luecke 2 (Toene als separate Achse):** Pitch ist als paralinguistisches Merkmal enthalten, aber die Bruecke zu linguistischen Toenen wird nicht geschlagen. Dies ist besonders bemerkenswert: Das Modell *kann* Tonhoehe wahrnehmen, evaluiert aber nicht die Faehigkeit, Mandarin-Toene zu differenzieren.
- **Luecke 3 (L2-Sprecher):** Keine L2-Evaluation.
- **Luecke 4 (Systematischer Vergleich):** Breiter Vergleich, aber nur auf CER/WER-Ebene.

---

## Paper 7: A Survey on Speech Large Language Models for Speech Understanding (Peng et al., 2024/2025) -- Relevanz: *****

### 1. Kernaussagen (mit Zitaten)

1. **Erste umfassende Survey zu Speech LLMs fuer Verstehen:** Einzigartige Perspektive mit Fokus auf Speech Understanding statt Generation.
   > "This paper presents the first comprehensive survey of Speech LLMs from the perspective of speech understanding." (Section X Conclusion, S. 26)

2. **Dreidimensionale Taxonomie:** Systematische Einordnung nach informationaler, funktionaler und Format-Dimension.
   > "we propose the first systematic conceptualization of speech understanding, along with a task-oriented taxonomy that spans informational, functional, and format dimensions." (Section X, S. 26)

3. **Evaluation auf ASR nur mit WER/CER:** Die Survey bestaetigt, dass ASR-Evaluation auf diese Metriken beschraenkt ist.
   > "Automatic Speech Recognition (ASR): Word Error Rate (WER), Character Error Rate (CER), Match Error Rate (MER)" (Section VII.A, S. 18)

4. **Variabilitaet der Modellleistung ueber Tasks:** Speech LLMs zeigen inkonsistente Leistung.
   > "Despite the promising multitask capabilities of Speech LLMs, there is significant variability in their performance across different tasks." (Section VII.B.2.c, S. 21)

5. **Degradation semantischer Faehigkeiten:** Multimodale Fusion kann die Textverstaendnis-Faehigkeiten des LLM beeintraechtigen.
   > "integrating the speech modality leads to less satisfactory end-to-end transcription results" und "although Speech LLMs can align speech inputs with textual outputs through fine-tuning, their original textual reasoning capacity may be weakened, likely due to modality alignment constraints and shared model capacity." (Section VIII.B, S. 25)

6. **Instruktionssensitivitaet als kritische Herausforderung:** Modellleistung variiert stark je nach Instruktionsformulierung.
   > "two critical challenges emerge in real-world scenarios where the same or similar task requirements are expressed using varied instructions: 1) Instruction following failure [...] 2) Instruction-induced performance instability" (Section VIII.A, S. 23)

### 2. Datensatz-Profil

| Merkmal | Details |
|---|---|
| Typ | Survey-Paper (kein eigenes Modell/Datensatz) |
| Abgedeckte Modelle | 30+ Speech LLMs (inkl. Qwen-Audio, SALMONN, Kimi-Audio u.a.) |
| Evaluierte Tasks | ASR, Speech Translation, Emotion Recognition, Sound Event Classification |
| Evaluierte Benchmarks | LibriSpeech, CoVoST2, MELD, VocalSound, MMAU, MMAR, AIR-Bench |
| RPS-Metrik | Eigene "Relative Performance to SOTA" Metrik vorgeschlagen |
| Sprachen (Evaluation) | Primaer Englisch, teilweise Mandarin |
| Ton-Annotation | Keine (in keinem der evaluierten Benchmarks) |

### 3. Verwendbarkeit fuer unsere Arbeit (RQ-Mapping)

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-to-Pinyin) | Keine | Pinyin wird in keinem der abgedeckten Modelle/Tasks diskutiert |
| RQ2 (Native Speaker) | Mittel | Vergleichstabelle (Table V) mit ASR-Leistung verschiedener Speech LLMs liefert Kontextwerte |
| RQ3 (L2 Speaker) | Keine | L2-Sprecher werden nicht thematisiert |
| RQ4 (Vergleich) | Hoch | Systematischer Vergleich von Speech LLMs auf standardisierten Benchmarks; RPS-Metrik koennte adaptiert werden |
| RQ5 (Fehlerstruktur) | Gering | Hinweis auf "fine-grained auditory understanding" als offene Herausforderung, aber keine konkreten Phonem-/Ton-Analysen |

### 4. Forschungsluecken

**Explizit (aus dem Paper):**
- Instruktionssensitivitaet: "variation in instruction can lead to operational inefficiencies and reduced reliability" (Section VIII.A, S. 23)
- Degradation semantischer Faehigkeiten: "their original textual reasoning capacity may be weakened" (Section VIII.B, S. 25)
- Feinkoerniges auditives Verstehen als offenes Problem: "they often struggle to reliably capture and reason over acoustic information, leading to restricted performance in tasks that require fine-grained auditory understanding" (Section VIII.B, S. 25)
- Akustische Informationsextraktion als Zukunftsrichtung: "Another critical avenue for future work lies in extracting richer and finer acoustic information from speech. Although current models can process speech to some extent, their ability to capture the full range of acoustic cues, such as speaker emotion, intonation, and environmental noise, remains limited." (Section IX, S. 25)

**Implizit (Mapping auf unsere Luecken):**
- **Luecke 1 (Phonem/Ton-Granularitaet):** Die Survey bestaetigt explizit, dass ASR-Evaluation auf WER/CER beschraenkt ist (Section VII.A). Kein einziger der 30+ abgedeckten Speech LLMs wird auf Phonem- oder Tonebene evaluiert. Dies validiert direkt unsere Luecke 1.
- **Luecke 2 (Toene als separate Achse):** In der gesamten Survey wird "intonation" nur einmal als Zukunftsrichtung erwaehnt, nie als Evaluationsachse. Mandarin-Toene werden nicht thematisiert. Dies validiert direkt unsere Luecke 2.
- **Luecke 3 (L2-Sprecher):** Nicht-muttersprachliche Sprecher werden in der gesamten Survey nicht erwaehnt. Dies validiert direkt unsere Luecke 3.
- **Luecke 4 (Systematischer Vergleich):** Die Survey bietet einen systematischen Vergleich, aber nur auf Task-Ebene (ASR, ST, ER, SEC). Ein Head-to-Head auf feinerer Granularitaet fehlt. Die RPS-Metrik ist ein nuetzlicher methodischer Beitrag.

---

## Querschnittsanalyse: Zentrale Befunde fuer die Masterarbeit

### Konsistente Bestaetigung aller vier Luecken

Ueber alle 7 analysierten Arbeiten hinweg zeigt sich ein einheitliches Bild:

1. **Keine einzige Arbeit evaluiert Toene als separate Achse.** Selbst Step-Audio 2, das Pitch als paralinguistisches Merkmal erkennt, brueckt nicht zu linguistischen Mandarin-Toenen.

2. **Keine einzige Arbeit verwendet Pinyin als Output-Repraesentation.** Alle Mandarin-ASR-Systeme geben chinesische Zeichen aus.

3. **Keine einzige Arbeit testet mit L2/nicht-muttersprachlichen Sprechern.** Selbst Systeme mit 52 Sprachen (Qwen3-ASR) evaluieren nur Muttersprachler.

4. **Evaluation bleibt auf Satz-/Zeichenebene (CER/WER).** Phonem-Level-Analysen fehlen durchgehend.

### Relevanteste Modelle fuer Head-to-Head-Vergleich (RQ4)

| Modell | CER (Mandarin) | Open Source | Empfehlung |
|---|---|---|---|
| Kimi-Audio | 0.60 (AISHELL-1) | Ja | Hoechste Prioritaet -- SOTA Mandarin |
| FireRedASR-LLM | 3.05 (avg. 11 sets) | Ja | Hohe Prioritaet -- Mandarin-spezialisiert |
| FireRedASR2-LLM | 2.89 (avg. 4 sets) | Ja | Hohe Prioritaet -- aktuellstes Modell |
| Qwen3-ASR-1.7B | 2.71 (AISHELL-2) | Ja | Hohe Prioritaet -- kompakt, multilingual |
| Seed-ASR | 2.98 (avg. 6 sets) | Nein | Nur als Referenzwert (nicht Open Source) |
| Step-Audio 2 | 3.08 (avg. zh) | Teilweise | Mittel -- interessant wegen Pitch-Erkennung |

### Schluesselzitat fuer die Einleitung/Motivation der Masterarbeit

Aus der Peng-Survey (Section IX, S. 25):
> "Another critical avenue for future work lies in extracting richer and finer acoustic information from speech. Although current models can process speech to some extent, their ability to capture the full range of acoustic cues, such as speaker emotion, **intonation**, and environmental noise, remains limited."

Dieses Zitat bestaetigt direkt, dass die feinkoernige Analyse akustischer Information -- einschliesslich Intonation/Toene -- eine anerkannte offene Forschungsfrage ist.

---

# KATEGORIE B: Mandarin Töne & Pinyin/ASR (Papers 8-14)


---

## Paper 8 -- Du et al. (2025): Zipformer-RNN-T fuer Mandarin-Phonemerkennung

**Referenz:** Du, Y. et al. (2025). *Mandarin Chinese phoneme recognition using Zipformer-based models with pruned RNN-T/CTC.* PLOS ONE.

### Kernaussagen

1. **Tonale Bedeutung:** Mandarin-Toene sind semantisch entscheidend -- gleiche Silben haben je nach Ton voellig unterschiedliche Bedeutungen.
   > "tones play a crucial role in the semantic structure of Chinese, with the same syllable having completely different meanings depending on the tone" (S. 2)

2. **66 distinkte Phoneme:** Die Autoren unterteilen die Mandarin-Aussprache in 66 distinkte Phoneme, die als Modellierungseinheiten dienen.
   > "Mandarin pronunciation is subdivided into 66 distinct phonemes" (S. 17)

3. **State-of-the-Art-Ergebnisse:** Das Zipformer-RNN-T(Pruned)-Modell erreicht extrem niedrige Fehlerraten auf zwei Datensaetzen.
   > "achieving a Word Error Rate (WER) of 1.92% (Dev) and 2.12% (Test) on the AISHELL1-PHONEME dataset, and 4.28% (Dev) and 4.51% (Test) on the ST-CMDS-PHONEME dataset" (Abstract)

4. **Effizienz:** Das Modell balanciert Leistung und Effizienz mit nur 61,1M Parametern.
   > "the model requires only 61.1M parameters, striking a balance between performance and efficiency" (Abstract)

5. **Substitutionsfehler dominieren:** Die Fehleranalyse zeigt, dass Substitutionsfehler den groessten Anteil ausmachen, was auf phonetische Verwechslungen hinweist.
   > "Substitution errors remain the dominant error type across all experimental conditions" (S. 14) [Zitat nicht verifiziert]

6. **Hybrid-Verlustfunktion:** Eine hybride Pruned-RNN-T/CTC-Verlustfunktion mit CTC-Gewicht 0,085 liefert optimale Ergebnisse.
   > "The hybrid Pruned RNN-T/CTC Loss with CTC weight 0.085 achieves optimal performance" (S. 12) [Zitat nicht verifiziert]

7. **Rauschaugmentation:** MUSAN-Datensatz wird zur Rauschaugmentation verwendet, was die Robustheit verbessert.
   > "We apply noise augmentation using the MUSAN dataset to improve model robustness" (S. 8) [Zitat nicht verifiziert]

### Datensatz-Profil

| Merkmal | AISHELL1-PHONEME | ST-CMDS-PHONEME |
|---|---|---|
| Sprache | Mandarin (Muttersprachler) | Mandarin (Muttersprachler) |
| Sprecherzahl | k.A. | k.A. |
| Stunden (Train/Dev/Test) | 150h / 18h / 10h | 85h / 10h / 5h |
| Tonannotation | Implizit (ueber Phoneme) | Implizit (ueber Phoneme) |
| Phonemanzahl | 66 | 66 |
| Aufnahmebed. | Studio/kontrolliert | Studio/kontrolliert |
| Offen verfuegbar | Ja (basiert auf AISHELL-1) | Ja (basiert auf ST-CMDS) |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | Kein direkter Bezug (kein Text-zu-Pinyin) | -- |
| RQ2 | **Stark:** Muttersprachliche Phonemerkennung auf AISHELL-1 und ST-CMDS | +++ |
| RQ3 | Kein Bezug (keine L2-Sprecher) | -- |
| RQ4 | **Relevant:** Zipformer als dediziertes System, Vergleichsmassstab fuer LLM-basierte Ansaetze | ++ |
| RQ5 | **Relevant:** Substitutionsfehleranalyse zeigt systematische phonetische Verwechslungsmuster | ++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Keine Evaluation auf nicht-muttersprachlichen Daten
- Keine multimodale Eingabe (nur Audio)
- Toene werden nur implizit als Teil der 66 Phoneme modelliert, nicht separat evaluiert

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): Zipformer ist ein dediziertes System; ob LLMs vergleichbare Phonemerkennung leisten, bleibt offen
- **Luecke 2** (Nicht-Muttersprachler): Ausschliesslich Muttersprachler-Daten verwendet
- **Luecke 3** (Fehlerstruktur): Substitutionsfehleranalyse liefert Grundlage, aber keine systematische Tonverwechslungsmatrix
- **Luecke 4** (Multimodale Modelle): Rein akustisches System, kein multimodaler Ansatz

---

## Paper 9 -- Zou et al. (2025): Systematische Uebersicht ML fuer Mandarin-Tonerkennung

**Referenz:** Zou, Y. et al. (2025). *Machine learning for Mandarin tone recognition: A systematic review.* Preprints.

### Kernaussagen

1. **Deep Learning ueberlegen:** Deep-Learning-Modelle uebertreffen traditionelle Ansaetze bei der Mandarin-Tonklassifikation signifikant.
   > "Deep learning models outperform traditional approaches in Mandarin tone classification (mean accuracy 88.8% vs. 83.1%)" (Abstract)

2. **CNNs auf isolierten Silben:** Convolutional Neural Networks erreichen bis zu 99,16% Genauigkeit bei isolierten Silben.
   > "Convolutional Neural Networks (CNNs) achieve up to 99.16% accuracy for isolated syllables" (Abstract)

3. **Ton 3 am schwierigsten:** Ton 3 ist konsistent der am schwierigsten zu klassifizierende Ton wegen starker Koartikulation und Tonsandhi.
   > "Tone 3 remains consistently the most difficult to classify due to heavy coarticulation and tonal sandhi effects" (S. 12)

4. **Leistungsbeeintraechtigende Faktoren:** Hintergrundgeraeusche, gestoerte Sprache und neutrale Toene beeinflussen die Leistung negativ.
   > "Performance is affected by Tone 3 variability, neutral tones, and challenging conditions like background noise and disordered speech" (Abstract)

5. **Datensatzluecken:** Es fehlen diverse Datensaetze, schwache Prosodie- und Dialektmodellierung sowie unzureichende Validierungsstrenge.
   > "the lack of diverse datasets, weak prosody and dialect modeling, and insufficient validation rigor" (Abstract)

6. **L2-Korpora unterrepraesentiert:** L2-Lernerdaten wie iCALL existieren, erhalten aber wenig Aufmerksamkeit.
   > "L2 corpora like iCALL (142 hours, 305 learners) exist but receive less attention than native-speaker datasets" (S. 10)

7. **Multimodale Korpora gefordert:** Spezialzweck-Korpora mit multimodalen Daten werden fuer effiziente Leichtgewichtsmodelle benoetigt.
   > "Multimodal special-purpose corpora are needed to develop efficient lightweight models" (Abstract)

8. **61 Artikel analysiert:** Die systematische Uebersicht umfasst 61 Artikel nach PRISMA-Richtlinien.
   > "This systematic review examines 61 peer-reviewed articles" (S. 2) [Zitat nicht verifiziert]

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Studientyp | Systematische Uebersicht (PRISMA) |
| Anzahl analysierter Artikel | 61 |
| Abgedeckte Datensaetze | Diverse (AISHELL, THCHS-30, iCALL u.a.) |
| DL Mean Accuracy | 88,82% (SD 7,21%) |
| Traditionell Mean Accuracy | 83,06% (SD 14,04%) |
| Beste Einzelergebnisse | CNN: 99,16% (isolierte Silben); BiLSTM: 7,03% TER (kont. Sprache) |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | Kein direkter Bezug (kein Text-zu-Pinyin) | -- |
| RQ2 | **Stark:** Umfassende Uebersicht ueber Tonerkennung bei Muttersprachlern | +++ |
| RQ3 | **Relevant:** Erwaehnung von L2-Korpora, aber wenig analysiert | + |
| RQ4 | **Stark:** Systematischer Vergleich dedizierter Systeme (CNN, BiLSTM, Transformer) | +++ |
| RQ5 | **Stark:** Detaillierte Analyse der Tonverwechslungsmuster, insb. Ton 3 | +++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Mangel an diversen Datensaetzen
- Schwache Prosodie- und Dialektmodellierung
- Unzureichende Validierungsstrenge
- L2-Korpora erhalten zu wenig Aufmerksamkeit
- Multimodale Korpora fehlen

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): Kein einziger der 61 Artikel verwendet LLMs oder Foundation Models fuer Tonerkennung
- **Luecke 2** (Nicht-Muttersprachler): Explizit als unterrepraesentiert benannt
- **Luecke 3** (Fehlerstruktur): Ton-3-Problematik detailliert, aber keine LLM-spezifische Fehleranalyse
- **Luecke 4** (Multimodale Modelle): Explizit gefordert, aber nicht realisiert

---

## Paper 10 -- Bu et al. (2025): Siamesisches Netzwerk fuer L2-Tonbewertung

**Referenz:** Bu, Y. et al. (2025). *A ResNet-based Siamese network approach for L2 Mandarin tone discrepancy evaluation.* Scientific Reports.

### Kernaussagen

1. **Subjektive Bewertung:** Das Siamesische Netzwerk erreicht bei subjektiver Bewertung MSE 2,295 und RMSE 1,515 im Vergleich zu Expertenbewertungen.
   > "In subjective evaluations, our model achieved a Mean Squared Error (MSE) of 2.295 and a Root Mean Squared Error (RMSE) of 1.515 compared to expert assessments" (Abstract)

2. **Objektive Bewertung:** Bei objektiver Bewertung werden MSE 0,189 und RMSE 0,435 erreicht.
   > "We recorded an MSE of 0.189 and an RMSE of 0.435 in objective evaluations" (Abstract)

3. **ResNet-18 beste Architektur:** ResNet-18 zeigt die staerkste Faehigkeit, Tonmerkmale zu verstehen und zu differenzieren.
   > "ResNet-18 emerges as the top performer, demonstrating a robust ability to understand and differentiate these features" (S. 16)

4. **Ton 3 am leichtesten unterscheidbar:** Im Gegensatz zu Erkennungsaufgaben ist Ton 3 bei der Abweichungsbewertung am einfachsten zu unterscheiden.
   > "Tone3 is easier for the models to learn and has distinctive recognition features" (S. 17)

5. **Limitation -- Vereinfachung der Tonmerkmale:** Die Methode basiert auf berechneten Tonmerkmalen, die die Komplexitaet moeglicherweise vereinfachen.
   > "the method still relies on the computed tone features, which may oversimplify the complexity of tone characteristics, presenting a limitation of the current approach" (S. 17)

6. **Five-Degree-Tone-Model:** Zur Normalisierung der F0-Konturen wird das Fuenf-Stufen-Tonmodell (Skala 0--5) verwendet.
   > "The Five-Degree Tone Model normalizes F0 contours to a 0-5 scale for standardized tone representation" (S. 5) [Zitat nicht verifiziert]

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Sprache | Mandarin (L2-Lerner: Tibetisch L1) |
| Sprecherzahl | 40 L2-Lerner (27 weiblich, 13 maennlich) |
| Alter | 10--22 Jahre |
| Gesamtdaten | 120.022 Sprachproben, 145h Audio, 79.278 zeitgestempelte Silben |
| Standard-Mandarin | Professionelle Sprecher + THCHS-30, AIShell-1/2/3 |
| Tonmerkmale | 1D (40D-Vektor) und 2D (40x50 Binaerbild) |
| Offen verfuegbar | Selbstkonstruiertes Korpus (Verfuegbarkeit unklar) |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | Kein Bezug (keine Text-zu-Pinyin-Konversion) | -- |
| RQ2 | **Indirekt:** Standard-Mandarin als Referenz | + |
| RQ3 | **Stark:** Explizite L2-Lernerevaluation (tibetische Muttersprachler) | +++ |
| RQ4 | **Relevant:** ResNet-18 als dediziertes Tonbewertungssystem | ++ |
| RQ5 | **Relevant:** Tonspezifische Analyse (Ton 3 am leichtesten, Ton 2 geringste Expertenabweichung) | ++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Vereinfachung der Tonmerkmale durch berechnete Features
- Nur eine L1-Gruppe (Tibetisch) untersucht
- Bewertung statt Erkennung -- keine Transkription

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): Kein LLM-Einsatz; Siamesisches Netzwerk ist spezialisiertes System
- **Luecke 2** (Nicht-Muttersprachler): Direkt relevant, aber nur eine L1-Gruppe
- **Luecke 3** (Fehlerstruktur): Tonspezifische Analyse vorhanden, aber auf Bewertung beschraenkt
- **Luecke 4** (Multimodale Modelle): Rein akustische Merkmale, kein multimodaler Ansatz

---

## Paper 11 -- Li et al. (2024): PY-GEC -- Pinyin-enhanced LLM-basierte ASR-Fehlerkorrektur

**Referenz:** Li, Y. et al. (2024). *Large language model should understand Pinyin for Chinese ASR error correction.* arXiv:2409.13262.

### Kernaussagen

1. **Pinyin verbessert konsistent:** Die Integration von Pinyin verbessert konsistent die Character Error Rates und Entity Recalls.
   > "incorporating Pinyin consistently improves the character error rates (CERs) and entity recalls" (S. 1)

2. **Beste Ergebnisse:** CER von 10,53% und Entity Recall von 72,93% werden mit Multitask + PY-GEC erreicht.
   > "the average CER and entity recall reach 10.53% and 72.93%, respectively" (S. 3)

3. **Keine direkte Verbindung Aussprache-Schrift:** Im Chinesischen besteht keine direkte Verbindung zwischen Aussprache und Schriftform.
   > "there is no direct connection between the pronunciation and the written form of Chinese characters" (S. 1)

4. **Pinyin-Feature-Projektion:** Der Ansatz projiziert Pinyin-Features erfolgreich in einen Merkmalraum, der dem der Textfeatures am aehnlichsten ist.
   > "our approach successfully projects Pinyin features into a feature space most similar to that of the text features" (S. 2)

5. **Cosine-Similarity-Verbesserung:** Die Cosinus-Aehnlichkeit zwischen Text und Pinyin steigt von 0,26 (Basis) auf 0,82 (Multitask + PY-GEC).
   > "cosine similarity between text and Pinyin increases from 0.26 (base) to 0.82 (Multitask + PY-GEC)" (S. 3) [Zitat nicht verifiziert]

6. **Synthetische Trainingsdaten:** 136.597 synthetische Trainingsbeispiele werden generiert -- rein textbasiert, ohne ASR-Modelle.
   > "We generate 136,597 synthetic training samples using text-only data without requiring ASR models" (S. 2) [Zitat nicht verifiziert]

7. **Zukuenftige Forschung:** Erweiterung auf groessere LLMs und multimodale LLMs wird angestrebt.
   > "For future research, we aim to extend our experiments to larger-scale LLMs and multi-modal LLMs" (S. 4)

### Datensatz-Profil

| Merkmal | Aishell-1 Test | Common Voice Test |
|---|---|---|
| Sprache | Mandarin | Mandarin |
| Testsamples | 7.176 | 8.273 |
| ASR-Modelle | Whisper-Small, Whisper-Large-v2 | Whisper-Small, Whisper-Large-v2 |
| Trainingssamples | 136.597 (synthetisch) | 136.597 (synthetisch) |
| LLM-Backbone | LLaMA-3-8B-Chinese | LLaMA-3-8B-Chinese |
| LoRA-Rank | 32 | 32 |
| Multitask-Training | Direkte Korrektur, PY-GEC, Pinyin-Text-Konversion | dito |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | **Stark:** Explizites Text-Pinyin-Alignment in LLMs, Multitask-Training mit Pinyin-Text-Konversion | +++ |
| RQ2 | **Relevant:** Korrektur von ASR-Ausgaben fuer Muttersprachler (Aishell-1) | ++ |
| RQ3 | Kein Bezug (keine L2-Daten) | -- |
| RQ4 | **Stark:** Vergleich LLM (LLaMA-3) mit Whisper; LLM als Post-Processing-System | +++ |
| RQ5 | **Relevant:** Fehlerkorrektur basiert auf phonetischer Aehnlichkeit (Homophone) | ++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Nur LLaMA-3-8B getestet; groessere LLMs und multimodale LLMs ausstehend
- Nur textbasierte Korrektur, kein direkter Audioeingang
- Keine Tonevaluation separat

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): Pinyin-Verstaendnis in LLMs direkt relevant, aber Toene nicht separat evaluiert
- **Luecke 2** (Nicht-Muttersprachler): Keine L2-Evaluation
- **Luecke 3** (Fehlerstruktur): Homophonfehler adressiert, aber keine systematische Tonverwechslungsanalyse
- **Luecke 4** (Multimodale Modelle): Explizit als zukuenftige Arbeit genannt

---

## Paper 12 -- Liang & Zhang (2025): PERL -- Pinyin Enhanced Rephrasing Language Model

**Referenz:** Liang, J. & Zhang, B. (2025). *PERL: Pinyin Enhanced Rephrasing Language Model for Chinese ASR N-best Error Correction.* arXiv:2412.03230v2.

### Kernaussagen

1. **Pinyin-Luecke in ASR-Korrektur:** Existierende chinesische ASR-Korrekturmethoden haben Pinyin-Informationen nicht effektiv genutzt.
   > "Existing Chinese ASR correction methods have not effectively utilized Pinyin information, a unique feature of the Chinese language." (Abstract)

2. **CER-Reduktion auf Aishell-1:** PERL erreicht eine 29,11% Reduktion der Character Error Rate auf Aishell-1.
   > "achieving a 29.11% reduction in Character Error Rate on Aishell-1 and around 70% CER reduction on domain-specific datasets" (Abstract)

3. **Ueberlegenheit gegenueber LLMs:** PERL uebertrifft GPT-4o, DeepSeek und Qwen2.5 bei der ASR-Korrektur, da generative Modelle Laengenbeschraenkungen schlecht handhaben.
   > "LLMs perform worse than PERL on DoAD. We attribute this to the inherent limitations of generative models when handling tasks with length constraints, where noisy inputs often mislead them into producing erroneous outputs." (S. 3)

4. **Phonetische Fehlerkorrektur:** PERL nutzt Pinyin-Wissen, um phonetisch aehnliche Zeichen korrekt zu ersetzen (z.B. song -> zong).
   > "PERL could calculate the correct length and incorporate Pinyin knowledge to recover the wrong token [...] since they share similar Pinyin." (S. 4)

5. **Niedrige Latenz:** PERL fuehrt nur minimalen Overhead ein (3,09 ms) verglichen mit LLMs (Hunderte von ms).
   > "The PERL pipeline introduces only minor overhead (3.09 ms) while lowering CER, offering a strong balance between speed and accuracy." (S. 4)

6. **Laengenvorhersage kritisch:** Die Ablationsstudie zeigt, dass das Entfernen des Laengenpraediktors die groesste Leistungsverschlechterung verursacht.
   > "removing the length predictor during correction results in even more significant performance degradation, as the language model needs to rephrase the sentence into shorter or longer sequences." (S. 4)

7. **Domaenenspezifischer Datensatz DoAD:** Ein neuer N-best-ASR-Datensatz wird eingefuehrt, der Recht, Medizin und amtliche Dokumente abdeckt.
   > "We construct DoAD, a domain-specific N-best ASR dataset covering legal, medical, and official document domains." (S. 1)

### Datensatz-Profil

| Merkmal | CHP/Aishell-1 | DoAD-Law | DoAD-Med | DoAD-Odw |
|---|---|---|---|---|
| Saetze (Train/Test) | 120.099 / 7.176 | 4.040 / 1.010 | 10.978 / 1.820 | 5.137 / 1.480 |
| Mittlere Satzlaenge | 14,41 / 14,60 | 13,69 / 13,64 | 11,30 / 11,55 | 12,45 / 12,41 |
| Gleiche Laenge (1-best=Ref) | 113.039 / 6.699 | 2.469 / 263 | 5.878 / 1.032 | 2.502 / 696 |
| ASR-System | Whisper (diverse) | Belle-distil-whisper-large-v2-zh | dito | dito |
| Baseline-CER | 5,84% | 12,64% | 16,09% | 15,94% |
| PERL-CER | 4,10% | 3,41% | 3,91% | 4,87% |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | **Stark:** Pinyin-Encoder lernt Text-Pinyin-Mapping; Pinyin als phonetische Bruecke | +++ |
| RQ2 | **Relevant:** ASR-Korrektur fuer Muttersprachler | ++ |
| RQ3 | Kein Bezug (keine L2-Daten) | -- |
| RQ4 | **Stark:** Direkter Vergleich PERL vs. GPT-4o, DeepSeek, Qwen2.5 -- PERL deutlich besser | +++ |
| RQ5 | **Relevant:** Phonetisch aehnliche Zeichenverwechslungen als Hauptfehlertyp identifiziert | ++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Nur BERT als semantischer Encoder getestet
- N-best optimal bei n=5; groessere n verschlechtern Ergebnisse
- Keine Evaluation auf nicht-chinesischen Sprachen

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): PERL zeigt, dass LLMs ohne Pinyin-Integration schlechter abschneiden -- Pinyin-Verstaendnis als Schluessel
- **Luecke 2** (Nicht-Muttersprachler): Keine L2-Evaluation
- **Luecke 3** (Fehlerstruktur): Phonetische Verwechslungen (Homophone) als Hauptfehler, aber keine tonspezifische Analyse
- **Luecke 4** (Multimodale Modelle): Rein textbasierte Pipeline, kein multimodaler Ansatz

---

## Paper 13 -- Zhengjie & Cheng (2025): PYG-ASR -- Pinyin-Guided Chinese Speech Recognition

**Referenz:** Zhengjie, J. & Cheng, G. (2025). *Pinyin-Guided Chinese Speech Recognition with Large Language Model.* Interspeech 2025.

### Kernaussagen

1. **Implizite Ausrichtung versagt:** Die implizite Ausrichtung in LLM-ASR-Modellen kann phonetische Beziehungen im Chinesischen oft nicht erfassen, was zu Aussprache-Konfusionen und Homophon-Fehlern fuehrt.
   > "its implicit alignment often fails to capture phonetic relationships in Chinese, leading to pronunciation confusion and homophone errors." (Abstract)

2. **Keine direkte Korrespondenz:** Es gibt keine direkte Korrespondenz zwischen der Aussprache und der Schriftform chinesischer Zeichen.
   > "there is no direct correspondence between the pronunciation and the written form of Chinese characters." (S. 1)

3. **CER-Reduktion um 25%:** PYG-ASR reduziert die CER um 25% auf dem AISHELL-1-Testset (von 4,0% auf 3,0%).
   > "PYG-ASR reduces CER by 25% on the AISHELL-1 test set." (Abstract)

4. **Pinyin-Error-Rate:** Die PYG-ASR-Modelle erreichen niedrige Pinyin-Fehlerraten von 1,9% (Test), was die phonetische Genauigkeit belegt.
   > "The PYG-ASR1 model achieved a Pinyin error rate of 1.6%/1.9%" (S. 4)

5. **Kontextuelle Verzerrung (Contextual Biasing):** PYGEC-CB erreicht eine B-CER von 12,55% und einen F1-Score von 88,62% durch Pinyin-Matching mit Bias-Phrasen.
   > "PYGEC-CB achieved a B-CER of 12.55% and an F1-score of 88.62%." (S. 4)

6. **CER-Reduktion fuer Bias-Phrasen:** 49,2% relative CER-Reduktion fuer Bias-Phrasen nach kontextueller Verzerrung.
   > "our approach shows a 49.2% CER reduction relatively for bias phrases on the AISHELL-1 test set after contextual bias." (Abstract)

7. **Zwei Ausgabestrategien:** Sequentielle (PYG-ASR1) und iterative (PYG-ASR2) Pinyin-Text-Ausgabe werden verglichen; sequentielle Ausgabe ist ohne PYGEC besser.
   > "The sequential output configuration consistently outperformed the iterative output when PYGEC was not applied." (S. 4)

8. **Kein Finetuning fuer Fehlerkorrektur:** Die Fehlerkorrekturphase nutzt DeepSeek-v3 als Text-LLM ohne Finetuning.
   > "our method attains even better performance without finetuning or additional specialized designs." (S. 4)

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Datensatz | AISHELL-1 |
| Sprache | Mandarin (Muttersprachler) |
| Umfang | 170h (150h Train / 10h Val / 5h Test) |
| Speech Encoder | HuBERT (315,8M Parameter) |
| LLM | Qwen2-7B |
| LoRA-Rank | 16 |
| Text-LLM (Korrektur) | DeepSeek-v3 |
| Trainierbare Parameter | 20,35M |
| Bias-Phrasen-Listgroesse | 186 |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | **Stark:** LLM generiert simultan Pinyin und Text aus Sprache; explizites Pinyin-Text-Mapping | +++ |
| RQ2 | **Stark:** Muttersprachliche ASR auf AISHELL-1 mit LLM-Backbone | +++ |
| RQ3 | Kein Bezug (keine L2-Daten) | -- |
| RQ4 | **Stark:** Vergleich LLM-ASR (Qwen2-7B + HuBERT) vs. Conformer-AED; LLM-basiert besser | +++ |
| RQ5 | **Relevant:** Homophon-Fehler als Hauptproblem; Pinyin-Matching zur Fehlererkennung | ++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Nur AISHELL-1 evaluiert; groessere Experimente geplant
- Tiefere Integration von Pinyin in LLM-ASR als zukuenftige Arbeit
   > "For future works, we intend to conduct larger-scale experiments and explore a more profound integration of Pinyin information into LLM-ASR models." (S. 4)

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): Direkt relevant -- LLM generiert Pinyin mit Toenen, aber Tongenauigkeit nicht separat evaluiert
- **Luecke 2** (Nicht-Muttersprachler): Keine L2-Evaluation
- **Luecke 3** (Fehlerstruktur): Homophon-Fehler identifiziert, aber keine systematische Tonverwechslungsmatrix
- **Luecke 4** (Multimodale Modelle): Multimodal (Audio + Text-LLM), aber kein visueller Kanal

---

## Paper 14 -- Chen et al. (2025): SCCM -- Syllable-Character Collaborative Model

**Referenz:** Chen, Z., Zhong, C. & Chen, D. (2025). *A syllable-character collaborative model for enhanced Pinyin and Chinese recognition.* PLOS ONE.

### Kernaussagen

1. **Komplexe Abbildung Sprache-Text:** Im Chinesischen ist die Beziehung zwischen Text und Aussprache komplexer als in anderen Sprachen, da Chinesisch staerker an die Bedeutung der Zeichen als an ihre Aussprache gebunden ist.
   > "Chinese is more closely tied to the meaning of the characters rather than their pronunciation" (S. 1) [Zitat nicht verifiziert]

2. **Inspiriert vom Lernprozess:** Der Ansatz ist vom Lernprozess chinesischer Anfaenger inspiriert, die zuerst Initiale, Finale und Pinyin lernen, bevor sie Zeichen lernen.
   > "Inspired by the learning process of Chinese beginners, who first master initials, finals, and pinyin before learning characters, we propose the Syllable-Character Collaborative Model (SCCM)" (Abstract)

3. **45,7% relative CER-Reduktion:** SCCM erreicht eine 45,7% relative Reduktion der Character Error Rate gegenueber der AISHELL-1-Baseline.
   > "achieves a 45.7% relative reduction in Character Error Rate (CER) over the AISHELL-1 baseline" (Abstract)

4. **Beste Ergebnisse:** SCCM erreicht Pinyin-CER 2,41% und Text-CER 6,50% auf AISHELL-1 und uebertrifft alle Baselines.
   > "Our proposed model consistently outperforms all baselines, achieving the lowest CER (6.4%) and pinyin CER (3.1%)." (S. 9) [Zitat nicht verifiziert]

5. **Pinyin-Ensemble (PE) Modul:** Das PE-Modul integriert Ausgaben aus drei Decodern (Pinyin, Initiale/Finale, Zeichen) mittels Ensemble-Lernen mit 10 Basisklassifikatoren.
   > "We design Pinyin-Ensemble module using the idea of ensemble learning, which integrates pinyin, initials and finals, and Chinese characters to produce more accurate predicted pinyin." (S. 2)

6. **Drei Trainingsphasen:** SCCM wird in drei Phasen trainiert: (1) unabhaengiges CTC-Training der Pinyin/IF-Decoder, (2) Joint-Training mit Zeichen-Decoder, (3) Volles End-to-End-Training mit iterativem Feedback.
   > "The training of SCCM consists of three phases." (S. 4)

7. **Pinyin-Embedding-Fusion:** Pinyin-Embeddings werden ueber CNN extrahiert und mit den Shared Features fusioniert, was phonetische Beschraenkungen beim Decoding verstaerkt und Homophon-Fehler reduziert.
   > "This fusion reinforces phonetic constraints during decoding, thereby mitigating homophone errors and improving text generation quality" (S. 6)

8. **Pycorrector als Post-Processing:** Ein chinesisches Textkorrektur-Tool (Pycorrector) wird als Post-Processing eingesetzt, um verbleibende Homophon-Fehler zu reduzieren (Text-CER: 6,53% -> 6,50%).
   > "we incorporate a Chinese text correction tool called Pycorrector to refine the decoder's output" (S. 7)

9. **Ablation -- PE-Modul kritisch:** Die Pinyin-Ensemble reduziert Pinyin-Fehlerrate um 28,0% (3,54% -> 2,55%) und Text-CER um 14,0% (7,83% -> 6,73%).
   > "by integrating both pinyin and initials/finals information through our Pinyin-Ensemble (PE) module, we achieve a 28.0% reduction in pinyin error rate (from 3.54% to 2.55%) and a 14.0% reduction in text CER (from 7.83% to 6.73%)." (S. 10)

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Datensatz | AISHELL-1 |
| Sprache | Mandarin (Muttersprachler) |
| Format | 16 kHz WAV |
| Features | 80-dim Filterbank, 10ms Shift, 25ms Fenster |
| Augmentation | SpecAugment |
| Encoder | 6 Conformer-Bloecke |
| Decoder | 6 Transformer-Schichten (Zeichen) + CTC (Pinyin, IF) |
| Epochen | 60 |
| Batch-Groesse | 32 |
| Pinyin-CER | 2,41% |
| Text-CER | 6,50% |

### RQ-Mapping

| RQ | Bezug | Staerke |
|---|---|---|
| RQ1 | **Stark:** Explizites Multi-Decoder-Design fuer Pinyin, Initiale/Finale und Zeichen; Pinyin als Bruecke zwischen Sprache und Text | +++ |
| RQ2 | **Stark:** Muttersprachliche ASR auf AISHELL-1 | +++ |
| RQ3 | Kein Bezug (keine L2-Daten) | -- |
| RQ4 | **Relevant:** Vergleich mit Conformer, wav2vec2.0, SpeechTransformer, Dual-Decoder | ++ |
| RQ5 | **Relevant:** Homophon-Fehler als Hauptproblem identifiziert; Pycorrector adressiert diese | ++ |

### Forschungsluecken

**Explizit im Paper genannt:**
- Nur AISHELL-1 evaluiert
- Pycorrector kann bei ungewoehnlichen Woertern Fehler einfuehren
- Keine Tonevaluation separat

**Implizites Mapping auf Luecken der Masterarbeit:**
- **Luecke 1** (LLM-Tonerkennung): Kein LLM-Einsatz; konventionelles Conformer-basiertes System -- zeigt aber, wie Pinyin-Integration ASR verbessert
- **Luecke 2** (Nicht-Muttersprachler): Ausschliesslich Muttersprachler
- **Luecke 3** (Fehlerstruktur): Homophon-Fehler adressiert, aber keine tonspezifische Verwechslungsmatrix
- **Luecke 4** (Multimodale Modelle): Kein multimodaler Ansatz

---

## Uebergreifende Synthese: Batch 2

### Zentrale Themen

1. **Pinyin als phonetische Bruecke:** Papers 11--14 zeigen uebereinstimmend, dass die explizite Integration von Pinyin-Informationen die chinesische ASR-Leistung signifikant verbessert. Die fehlende direkte Korrespondenz zwischen Aussprache und Schriftform macht Pinyin-Guidance unentbehrlich.

2. **Ton 3 als Problemton:** Paper 9 (Uebersicht ueber 61 Studien) bestaetigt, dass Ton 3 konsistent der schwierigste Ton ist, waehrend Paper 10 (L2-Bewertung) interessanterweise zeigt, dass Ton 3 bei der Abweichungsbewertung am leichtesten zu unterscheiden ist -- ein aufschlussreicher Widerspruch.

3. **LLMs vs. dedizierte Systeme:** Paper 12 (PERL) zeigt, dass GPT-4o, DeepSeek und Qwen2.5 bei der ASR-Korrektur schlechter abschneiden als spezialisierte Systeme. Paper 13 (PYG-ASR) zeigt jedoch, dass LLMs mit Pinyin-Guidance deutlich besser werden.

4. **Homophon-Fehler als Hauptproblem:** Alle Pinyin-fokussierten Papers (11--14) identifizieren Homophon-Verwechslungen als den dominanten Fehlertyp in der chinesischen ASR.

### Luecken-Matrix

| Luecke | Paper 8 | Paper 9 | Paper 10 | Paper 11 | Paper 12 | Paper 13 | Paper 14 |
|---|---|---|---|---|---|---|---|
| L1: LLM-Tonerkennung | x | x | x | ~ | ~ | ~ | x |
| L2: Nicht-Muttersprachler | x | ~ | Direkt | x | x | x | x |
| L3: Fehlerstruktur | ~ | Direkt | ~ | ~ | ~ | ~ | ~ |
| L4: Multimodale Modelle | x | x | x | Erwaehnt | x | ~ | x |

Legende: x = nicht adressiert, ~ = teilweise/indirekt adressiert, Direkt = direkt adressiert, Erwaehnt = als zukuenftige Arbeit erwaehnt

### Relevanz fuer Stefan Doschs Masterarbeit

Diese sieben Papers bilden eine wichtige Grundlage fuer die Masterarbeit:

- **Papers 8 + 9** liefern den Stand der Technik bei dedizierter Phonemerkennung und Tonklassifikation, gegen den LLM-Leistung gemessen werden kann (RQ4).
- **Paper 10** ist die einzige Studie mit expliziter L2-Evaluation (RQ3), wenn auch auf eine L1-Gruppe beschraenkt.
- **Papers 11--14** demonstrieren die Bedeutung von Pinyin-Integration fuer LLM-basierte Systeme (RQ1) und zeigen, dass ohne explizite phonetische Modellierung LLMs bei chinesischer ASR systematisch scheitern.
- **Keine der sieben Arbeiten** evaluiert systematisch, ob multimodale Foundation Models (wie GPT-4o Audio, Gemini) Mandarin-Toene erkennen koennen -- genau die zentrale Luecke der Masterarbeit (Luecke 1 + 4).

---

# KATEGORIE C: Aussprache, MDD & Chinese-Specific (Papers 15-21)

---

## Paper 15: Wang, Shi & Wang (2024) -- Pitch-Aware RNN-T for Mandarin Chinese Mispronunciation Detection and Diagnosis

**Relevanz: 3/5 Sterne**

### Kernaussagen

1. **Pitch-Information verbessert MDD in tonalen Sprachen.** Die Autoren stellen ein zustandsloses (stateless) RNN-T-Modell vor, das HuBERT-Features mit extrahierter F0-Pitch-Information durch einen Pitch Fusion Block kombiniert. Zitat: "this method did not explicitly extract pitch information from speech, which has been shown to enhance the performance of MDD models in tonal languages" (Section 1, S. 1).

2. **Datenmangel fuer L2-Mandarin-MDD ist ein zentrales Problem.** Die Autoren betonen, dass die Knappheit annotierter nicht-muttersprachlicher Daten das Training robuster Modelle einschraenkt. Zitat: "The scarcity of Mandarin MDD datasets limits model training" (Abstract, S. 1). Konkret: "the only publicly available L2 Mandarin Chinese dataset for training is the relatively small LATIC dataset" (Section 1, S. 1).

3. **Bestes Modell: PER 26.69, FRR 0.257, FAR 0.077.** Mit dem Pitch Fusion Block bei 40ms F0-Hop-Size und Pitch Embedding erreicht das Modell die beste Gesamtleistung (Table 2, S. 4). Im Vergleich zur wav2vec2.0-CTC-Baseline (PER 27.55, FRR 0.266, FAR 0.083) zeigt sich eine relative Verbesserung von 3% in PER und 7% in FAR.

4. **Tonale Phoneme als Evaluationseinheit.** Das System verwendet ein Phonem-Set mit 214 tonalen Phonem-Tokens plus Blank-Token (Section 3.2, S. 3): "The vocabulary size in our work is 215, including 214 tonal phonemes tokens and a blank token."

### Datensatz-Profil

| Merkmal | AISHELL-1 | LATIC |
|---|---|---|
| Typ | L1-Muttersprachler-Korpus | L2-Lerner-Korpus |
| Sprecher | 340 (Train), 40 (Dev), 20 (Test) | 4 |
| Dauer | 150h (Train), 10h (Dev), 5h (Test) | k.A. (2579 Aeusserungen) |
| L1 der Sprecher | Mandarin | Russisch, Koreanisch, Franzoesisch, Arabisch |
| Annotation | Zeichenebene | Mispronunciation-Annotation |
| Oeffentlich verfuegbar | Ja | Ja |
| Tonmarkierung | Ueber tonale Phoneme (Initial-Final-Ton-System) | Ueber tonale Phoneme |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht direkt relevant; System arbeitet mit Audio, nicht mit Text-zu-Pinyin.
- **RQ2 (Muttersprachler-Transkription):** Indirekt relevant. AISHELL-1 liefert L1-Trainingsdaten; das System trainiert auf Muttersprachler-Daten und evaluiert auf L2-Daten. Zeigt, dass L1-feingetuntes HuBERT tonale Phoneme mit PER 26.69 erkennen kann.
- **RQ3 (L2-Lerner):** Direkt relevant. LATIC-Evaluation mit 4 L2-Sprechern zeigt FAR/FRR-Trade-off bei der Fehlererkennung. Die geringe Sprecherzahl (4) ist eine wesentliche Einschraenkung.
- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant als Referenzpunkt. wav2vec2.0-CTC und HuBERT werden verglichen; allerdings kein Vergleich mit Whisper oder multimodalen LLMs.
- **RQ5 (Fehlerstruktur):** Teilweise relevant. Diagnostic Error Rate (DER) wird berichtet (Table 3), aber keine detaillierte Analyse welche Toene/Phonemkontraste am haeufigsten verwechselt werden.

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- "Data sparsity, highlighting the scarcity of annotated non-native speech data, is a critical issue in Mandarin Chinese MDD." (Section 1, S. 1)

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Das Paper evaluiert auf Phonem-/Ton-Ebene (PER, FRR, FAR, DER), aber nur fuer dedizierte ASR-Modelle, nicht fuer multimodale LLMs.
- **Luecke 2 (Toene selten separater Evaluationsaxis):** Das Paper verwendet tonale Phoneme als Grundeinheit, trennt aber Ton-Erkennung nicht systematisch von Segment-Erkennung.
- **Luecke 3 (Wenige L2-Studien):** Bestaetigt direkt -- nur 4 L2-Sprecher im Testset.
- **Luecke 4 (Kein Head-to-Head):** Kein Vergleich mit Whisper oder multimodalen LLMs.

---

## Paper 16: Wu & Shen (2024) -- The Assessment of Automated Rating of L2 Mandarin Prosody in Lexical Tone Recognition and Pauses

**Relevanz: 3/5 Sterne**

### Kernaussagen

1. **Tonale Diagnosegenauigkeit (DA) ueber 70% fuer alle Toene.** Die automatisierten Systeme erreichen Diagnostic Accuracy (DA) ueber 70% fuer alle vier lexikalischen Toene (Table 5, S. 253): Tone 1: 75%, Tone 2: 85%, Tone 3: 84%, Tone 4: 73%, Tone 0: 78%, Tone Sandhi: 81%. Zitat: "the overall accuracy of tonal diagnosis reached 80%" (Abstract, S. 250).

2. **False Rejection Rate uebersteigt False Acceptance Rate.** Die Autoren stellen fest, dass korrekt ausgesprochene Einheiten haeufiger faelschlich zurueckgewiesen als falsch ausgesprochene faelschlich akzeptiert werden. Zitat: "the automated model should be less stringent in identifying phoneme errors, thereby increasing tolerance for pronunciation variations among L2 learners" (Section 3.2, S. 253).

3. **Korrelation Mensch-Maschine bei Fluenz hoeher als bei Genauigkeit.** Die Korrelation zwischen menschlichen Bewertern (H) und dem automatisierten System A1 ist bei Fluenz (0.60) hoeher als bei Genauigkeit (0.31) und Gesamtbewertung (0.51) (Table 4, S. 252).

4. **Automatisierte Systeme fuer Unterricht bedingt geeignet.** Zitat: "while automated engines may effectively replace human raters in standardized tests, their applicability to L2 classrooms remains questionable" (Section 4, S. 253).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Typ | L2-Lerner-Aufnahmen (Vorleseaufgaben) |
| Lerner | 12 Studierende (6m/6w), elementares L2-Mandarin |
| L1 | Indonesisch (8), Englisch (2), Japanisch (1), Nepali (1) |
| Aufnahmen | 146 Eintraege |
| Silben | 1.388 |
| Tonverteilung | T1: 20, T2: 22, T3: 33, T4: 49, T0: 20, Sandhi: 7 |
| Material | 15 kurze Saetze, 145 Zeichen, 4 Themen |
| Oeffentlich verfuegbar | Nein (proprietaere Aufnahmen) |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht relevant; reine Audio-Bewertung.
- **RQ2 (Muttersprachler-Transkription):** Nicht direkt relevant; Fokus liegt auf L2-Bewertung, nicht Muttersprachler-Transkription.
- **RQ3 (L2-Lerner):** Direkt relevant. Zeigt DA-Werte fuer alle vier Toene plus Sandhi bei L2-Lernern. Tone 2 hat hoechste DA (85%), Tone 4 niedrigste (73%) -- wichtige Referenz fuer Fehlerstruktur.
- **RQ4 (Vergleich mit dedizierten Systemen):** Teilweise relevant. Zwei automatisierte Engines (A1, A2) werden mit menschlichen Bewertern verglichen, aber keine spezifischen Modellnamen (Whisper, LLM) genannt.
- **RQ5 (Fehlerstruktur):** Relevant. Table 5 zeigt FRR, FAR und DA pro Ton. Tone 3 hat hoechste FRR (30%), Tone 2 niedrigste FRR (19%).

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- "this study did not explore the relationship between speech rate in the fluency descriptor of automated models and the fluency scores assigned by human raters" (Section 4, S. 253)
- "additional audio recordings with more evenly distributed L1 background will be collected to study whether this influences the accuracy of scoring" (Section 4, S. 253)

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Keine Verwendung von LLMs; nur proprietaere automatisierte Engines.
- **Luecke 2 (Toene selten separater Evaluationsaxis):** Paper separiert Ton-Evaluation explizit -- zeigt, dass dies moeglich und informativ ist.
- **Luecke 3 (Wenige L2-Studien):** Bestaetigt. Nur 12 Lerner, hauptsaechlich indonesische L1.
- **Luecke 4 (Kein Head-to-Head):** Keine Identifikation der automatisierten Engines; kein Vergleich mit State-of-the-Art ASR.

---

## Paper 17: Huang, Xu, Bei & Huang (2025) -- Perception-Production of Second-Language Mandarin Tones Based on Interpretable Computational Methods: A Review

**Relevanz: 4/5 Sterne**

### Kernaussagen

1. **T2/T3-Kontrast bleibt persistente Schwierigkeit.** Die systematische Uebersicht ueber 54 Studien (2015-2025) identifiziert den Kontrast zwischen Tone 2 (rising) und Tone 3 (dipping/low) als zentrale Herausforderung. Zitat: "the contrast between Tone 2 (rising) and Tone 3 (dipping/low) remains the persistent difficulty" (Abstract, S. 1).

2. **Bestehende Modelle liefern nur grobe Ton-Labels.** Die Autoren kritisieren, dass die meisten audio-only Systeme nur kategoriale Outputs liefern und nicht die Parameter schaetzen, die Lehrende korrigieren. Zitat: "most of them assume i.i.d. tokens and output only a coarse tone label (T1-T4). Very few attempt to estimate the parameters that teachers correct -- slope, turning-point timing in milliseconds from rhyme onset, or effective F0 range" (Section 2.3, S. 14).

3. **Kein P-P-Framework erfuellt alle Kriterien.** Trotz vier identifizierter Forschungsstraenge (A: konventionelle Evaluationen, B: physiologisch-verhaltensbezogen, C: Audio-Sprachanalyse, D: relational-erklaerbar) erfuellt keine eingeschlossene Studie die vollstaendigen Kriterien fuer ein unterrichtstaugliches Framework. Zitat: "none of the included studies yet satisfy our full criteria for a classroom-ready, item-matched perception-production (P <-> P) framework" (Section 2, S. 8).

4. **Vier komplementaere Forschungsstraenge.** Das Review organisiert die Literatur in Strand A (konventionelle Evaluationen und Aufgaben), Strand B (physiologische/verhaltensbezogene Instrumentierung), Strand C (audio-only Sprachanalyse), Strand D (relational-aware und erklaerbare Modellierung). Strand D ist noch entstehend (nur 2 Studien).

5. **GNN + XAI als vielversprechender Ansatz.** Die Autoren schlagen ein Framework basierend auf Graph Neural Networks mit heterogenem relationalem Graphen und Explainable AI vor (Section 4, S. 17-20).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Typ | Systematisches PRISMA-Review |
| Suchzeitraum | 2015-2025 |
| Datenbanken | Google Scholar, CNKI |
| Identifizierte Studien | 245 |
| Eingeschlossene Studien | 54 |
| Screening-Reliabilitaet | kappa = 0.94 (Titel/Abstract), kappa = 0.93 (Volltext) |
| Tone Perfect erwaehnt | Ja, als Referenz [66]: "Tone Perfect corpus (isolated syllables)" (Table 2, S. 9) |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht direkt relevant; Fokus auf Audio-basierter Tonerkennung.
- **RQ2 (Muttersprachler-Transkription):** Indirekt relevant. Das Review referenziert Tone Perfect und wav2vec 2.0 fine-tuned Modelle fuer Mandarin mit "public-corpus tone error rates around 5%" (Section 4, S. 23).
- **RQ3 (L2-Lerner):** Hoch relevant. Zentrales Thema des Reviews. Dokumentiert L1-spezifische Transfereffekte, T2/T3-Verwechslungen, und Perception-Production-Asymmetrien.
- **RQ4 (Vergleich mit dedizierten Systemen):** Teilweise relevant. Vergleicht CNN/RNN/CTC vs. SSL-Modelle, aber keine multimodalen LLMs.
- **RQ5 (Fehlerstruktur):** Hoch relevant. Identifiziert T2/T3-Kontrast als persistente Schwierigkeit; dokumentiert Konturform, Slope und Turning-Point-Timing als diagnostische Parameter.

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- "none of the included studies yet satisfy our full criteria for a classroom-ready, item-matched perception-production (P <-> P) framework" (Section 2, S. 8)
- "most of them assume i.i.d. tokens and output only a coarse tone label (T1-T4). Very few attempt to estimate the parameters that teachers correct" (Section 2.3, S. 14)
- "These pipelines provide necessary baselines, but they are commonly token-wise and label-focused, which limits their usefulness for instruction and for perception-production inference." (Section 5.2, S. 24)

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Bestaetigt indirekt -- kein einziges multimodales LLM im Review eingeschlossen.
- **Luecke 2 (Toene selten separater Evaluationsaxis):** Das Review fordert explizit parameteralignierte Outputs (Slope, Turning-Point, F0 Range) ueber grobe Ton-Labels hinaus.
- **Luecke 3 (Wenige L2-Studien):** Dokumentiert umfassend die existierende L2-Literatur, zeigt aber Heterogenitaet und methodische Schwaechen.
- **Luecke 4 (Kein Head-to-Head):** Kein multimodales LLM in den 54 eingeschlossenen Studien.

---

## Paper 18: Zhu, Samir, Chodroff & Mortensen (2025) -- ZIPA: A Family of Efficient Models for Multilingual Phone Recognition

**Relevanz: 4/5 Sterne**

### Kernaussagen

1. **Kleine ZIPA-Modelle uebertreffen groessere Baselines.** Selbst die 64M-Parameter ZIPA-Modelle uebertreffen 300M-Parameter Transformer-Baselines. Zitat: "Even the small ZIPA models with only 64M parameters can outperform the 300M transformer baselines" (Section 6, S. 8).

2. **Phone-Recognition-Modelle glaetten phonetische Variation.** Die Fehleranalyse zeigt einen systematischen Bias: Modelle tendieren dazu, die Standardaussprache statt der tatsaechlichen Aussprachvariation vorherzusagen. Zitat: "phone recognition models tend to smooth out the phonetic variation during inference" (Section 7, S. 8). Konkret bei L2-Sprache: "given the exact same L2 speech, ZIPA predictions tend to better match the standard dictionary pronunciation than the actual pronunciation" (Section 7, S. 8).

3. **Vokalsubstitutionen als haeufigster Fehler.** Zitat: "the top errors are the substitution of vowels that are close in the vowel space" (Section 7, S. 8). Table 5 zeigt die haeufigsten Substitutionsfehler, z.B. a->a, i->e, r->e.

4. **IPAPACK++ als grosser multilingualer Korpus.** 17.132 Stunden Audio in 88 Sprachen mit G2P-generierten IPA-Transkriptionen (Section 3, S. 3). Mandarin (cmn) ist enthalten mit Aishell-1 und Fleurs-Daten.

5. **Mandarin-PFER extrem niedrig.** Fuer Mandarin (cmn) erreicht ZIPA-T-LARGE einen PFER von 0.44 auf gesehenen Sprachen (Table 2, S. 7), einer der niedrigsten Werte ueber alle Sprachen.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Typ | Multilingualer Phone-Recognition-Korpus + Modelle |
| Korpus | IPAPACK++ |
| Umfang | 17.132 Stunden, 88 Sprachen |
| Mandarin-Anteil | Aishell-1 (150h Train, 50h sonstiges) + Fleurs |
| Architektur | Zipformer mit CR-CTC und Transducer |
| Modellgroessen | 64M (small) und 300M (large) |
| Evaluation | PFER (Phonetic Feature Error Rate) |
| Oeffentlich verfuegbar | Ja (https://github.com/lingjzhu/zipa) |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht direkt relevant; Audio-zu-IPA, nicht Text-zu-Pinyin.
- **RQ2 (Muttersprachler-Transkription):** Hoch relevant. ZIPA erreicht PFER 0.44 fuer Mandarin auf gesehenen Sprachen -- zeigt State-of-the-Art Phone-Recognition-Leistung. Kann als Vergleichspunkt fuer multimodale LLMs auf Phone/Ton-Ebene dienen.
- **RQ3 (L2-Lerner):** Direkt relevant. Die L2-ARCTIC-Evaluation zeigt, dass Modelle L2-Variation glaetten: "the phone recognition models are still matching the frequent phone patterns in the dataset, rather than transcribing phones as they are actually produced" (Section 7, S. 8).
- **RQ4 (Vergleich mit dedizierten Systemen):** Hoch relevant. Vergleicht ZIPA mit Allosaurus, Wav2Vec2Phoneme, MultIPA, WhisperPPT auf identischen Benchmarks.
- **RQ5 (Fehlerstruktur):** Relevant. Table 5 dokumentiert systematische Substitutionsmuster. Vokale nahe im Vokalraum werden am haeufigsten verwechselt.

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- "current phone recognition models, despite the impressive performance, are still struggling with predicting sociophonetic variation" (Section 1, S. 1)
- "simply scaling up the G2P for transcribed speech data alone might not be able to solve phone recognition, as models can simply memorize the standard pronunciation" (Section 8, S. 9)

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** ZIPA evaluiert auf PFER (Phonem-Ebene), aber keine multimodalen LLMs einbezogen.
- **Luecke 2 (Toene selten separater Evaluationsaxis):** Toene sind im IPA-Inventar enthalten (als Diakritika), aber nicht als separater Evaluationsaxis analysiert. "Removing diacritics can improve the match between model predictions and ground truth" (Section 6, S. 8) -- d.h. Toene erhoehen den Fehler.
- **Luecke 3 (Wenige L2-Studien):** L2-ARCTIC wird getestet, aber nur fuer Englisch, nicht fuer Mandarin-L2.
- **Luecke 4 (Kein Head-to-Head):** Kein Vergleich mit GPT-4o, Gemini oder anderen multimodalen LLMs.

---

## Paper 19: Kang & Xu (2024) -- Tone-Syllable Synchrony in Mandarin: New Evidence and Implications

**Relevanz: 2/5 Sterne**

### Kernaussagen

1. **Ton- und Vokal-Onset sind vollstaendig synchronisiert.** Die Studie liefert den ersten quantitativen Nachweis fuer die Synchronisation von Ton und Vokal im Mandarin. Zitat aus dem Abstract: "The results indicate that tone and vowel onsets are fully synchronized" (Abstract, S. 1).

2. **F2 divergiert bei 0.137 Norm-Time, F0 bei 0.145 Norm-Time.** Die GAMM-Analyse zeigt, dass F2 (Vokal) bei 0.137 normalisierter Zeit divergiert (SD = 0.032), waehrend F0 (Ton) bei 0.145 divergiert (SD = 0.018). Dies entspricht etwa 125 ms vor der konventionellen Silbengrenze (Section 3.1, S. 6).

3. **Bayes-Faktor bestaetigt Synchronie.** Der Bayes-Faktor betraegt 252.84 (normalisierte Zeit) und 229.79 (Echtzeit), was starke Evidenz fuer die Synchronie-Hypothese liefert (Section 3.1.2/3.2.3, S. 9-10).

4. **Antizipatorisches Raising findet innerhalb der Silbe statt.** Zitat: "the previously reported 'anticipatory raising' effect of tone now appears to occur within rather than before the articulatory syllable" (Abstract, S. 1). Dies hat Implikationen fuer das Synchronisationsmodell der Silbe.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Typ | Phonetisches Laborexperiment |
| Sprecher | 8 (7 verwendet, 1 ausgeschlossen) |
| L1 | Mandarin (Muttersprachler, Nordchina) |
| Stimuli | 32 disilbige Pseudowoerter |
| Aufnahmen | 2.560 Aeusserungen (32 Saetze x 10 Wiederholungen x 8 Sprecher) |
| Abtastrate | 44.1 kHz |
| Analyse | GAMMs, LMEMs, Bayes-Faktoren |
| Oeffentlich verfuegbar | Auf Anfrage |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht relevant.
- **RQ2 (Muttersprachler-Transkription):** Indirekt relevant. Liefert phonetische Grundlage dafuer, warum Ton und Segment nicht unabhaengig evaluiert werden sollten -- sie beginnen synchron, was Implikationen fuer die Annotation hat.
- **RQ3 (L2-Lerner):** Nicht direkt relevant (nur Muttersprachler untersucht).
- **RQ4 (Vergleich mit dedizierten Systemen):** Nicht relevant.
- **RQ5 (Fehlerstruktur):** Indirekt relevant. Die Synchronie-Ergebnisse erklaeren, warum Tonale und segmentale Fehler korreliert auftreten koennen -- Ton und Vokal teilen den gleichen Onset-Zeitpunkt.

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- Keine direkten ASR/LLM-Luecken identifiziert (phonetische Grundlagenforschung).

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 2 (Toene selten separater Evaluationsaxis):** Die Befunde zur Ton-Vokal-Synchronie stuetzen die Argumentation, dass Tone und Segment gemeinsam evaluiert werden sollten, da sie akustisch gekoppelt sind.

---

## Paper 20: Wang, Zhang, Jiang, Song & Yu (2025) -- Can AI Understand Mandarin Speech Prosody? A Framework and Benchmark Showcase (MSPB)

**Relevanz: 5/5 Sterne**

### Kernaussagen

1. **Speech LLMs kaempfen mit subtilen prosodischen Variationen.** Die Evaluation von 6 Speech LLMs auf dem MSPB zeigt erhebliche Leistungsdefizite gegenueber Menschen. Zitat: "they generally struggled with tasks relying heavily on subtle speech prosody variations (e.g., prosodic focus marking, scalar meaning)" (Abstract/Section 4, S. 5378/5381).

2. **GPT-4o ist bestes Speech LLM mit 59.70%.** Die Gesamtgenauigkeiten: GPT-4o: 59.70%, Gemini-2-flash: 59.28%, Gemini-1.5-Pro: 56.05%, MiniCPM-o 2.6: 50.44%, GLM-4-Voice: 47.66%, Qwen2-Audio-7B-Instruct: 46.41% (Section 3.2.2, S. 5380).

3. **Erhebliche Luecke zu menschlicher Leistung.** Menschen erreichen 81.01%-95.67% je nach Aufgabe, waehrend Speech LLMs deutlich darunter liegen. Zitat: "current Speech LLMs still require substantial improvement to fully capture the nuanced subtleties of human speech prosody" (Section 4, S. 5381).

4. **Kontextreiche vs. prosodieabhaengige Aufgaben.** Modelle schneiden bei Aufgaben mit reichem Kontext (z.B. Ironie, Fokusoperator) besser ab als bei rein prosodie-abhaengigen Aufgaben (z.B. prosodic focus marking, scalar meaning). Zitat: "they rely more on contextual cues than on subtle speech prosody variations" (Section 3.2.2, S. 5381).

5. **MSPB als linguistisch fundierter Benchmark.** 8 prosodische Aufgaben, 178 Items, validiert durch 20 Muttersprachler und Expertenkonsultation (Section 2, S. 5379).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Typ | Prosodischer Benchmark fuer Speech LLMs |
| Name | Mandarin Speech Prosody Benchmark (MSPB) |
| Aufgaben | 8 (Intonation, prosodische Disambiguation, Focus Marking, Focus Operator, Scalar, Irony, Emotional Prosody mit/ohne Kontext) |
| Items | 178 |
| Audioaufnahmen | Phonetikerin, Sennheiser-Mikrofon, 44.1 kHz, 16-bit |
| Validierung | 20 Muttersprachler + Expertenkonsultation |
| Evaluierte Modelle | GPT-4o, Gemini-1.5-Pro, Gemini-2-flash, Qwen2-Audio-7B, GLM-4-Voice, MiniCPM-o 2.6 |
| Menschliche Baseline | 27 Teilnehmer (26F/1M, 18-53 Jahre) |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht direkt relevant (Audio-Prosody, nicht Zeichentranskription).
- **RQ2 (Muttersprachler-Transkription):** Indirekt relevant. Die menschliche Baseline zeigt, dass Muttersprachler prosodische Aufgaben mit 81-96% loesen, waehrend LLMs bei 46-60% liegen.
- **RQ3 (L2-Lerner):** Nicht direkt relevant (keine L2-Sprecher im Benchmark).
- **RQ4 (Vergleich mit dedizierten Systemen):** Hoch relevant. Direkter Head-to-Head-Vergleich von 6 Speech LLMs (GPT-4o, Gemini, Qwen2, GLM-4, MiniCPM) auf prosodischen Mandarin-Aufgaben. Zeigt, dass selbst GPT-4o nur 59.70% erreicht.
- **RQ5 (Fehlerstruktur):** Hoch relevant. Aufgabenspezifische Analyse zeigt, wo LLMs versagen: prosodic focus marking und scalar meaning sind am schwierigsten. Task-spezifische Radar-Charts (Figure 2) zeigen Staerken/Schwaechen-Profile pro Modell.

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- "current Speech LLMs still require substantial improvement to fully capture the nuanced subtleties of human speech prosody" (Section 4, S. 5381)
- "this study focused on a zero-shot, single-round approach in Mandarin, and future work should explore multi-turn conversational settings and expand to a multilingual benchmark" (Section 4, S. 5381)

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Direkt adressiert. MSPB evaluiert LLMs auf prosodischer Ebene, nicht auf WER/CER. Allerdings keine phonemische/tonale Transkriptionsaufgabe im Benchmark.
- **Luecke 2 (Toene selten separater Evaluationsaxis):** MSPB umfasst Intonation (Statement vs. Frage), aber nicht lexikalische Tonerkennung (T1-T4) als separaten Task.
- **Luecke 3 (Wenige L2-Studien):** Kein L2-Material im Benchmark.
- **Luecke 4 (Kein Head-to-Head):** MSPB liefert genau einen solchen Head-to-Head-Vergleich fuer Prosodie; eine analoge Studie fuer Ton-Transkription fehlt.

---

## Paper 21: Chirkova, Coto-Solano, Griffiths & Meelen (2025) -- Comparing Efficacy of IPA vs Pinyin Romanisation Transcriptions for Complex Tonal Languages: A Case Study in Baima

**Relevanz: 4/5 Sterne**

### Kernaussagen

1. **Komplexe Toene bleiben schwierigster Aspekt der Phonologie.** Die Autoren zeigen, dass Toene den groessten Beitrag zur Fehlerrate leisten. Zitat: "complex tones remain the most difficult part of the phonology to transcribe" (Section 5, S. 178).

2. **Transkriptionsformat beeinflusst tonale Outputs.** Selbst bei identischen Tonmarkierungen fuehren verschiedene Transkriptionssysteme zu unterschiedlichen Ergebnissen. Zitat: "the way the language is transcribed can affect tonal outputs, even when the tonal markings themselves remain the same throughout different transcriptions" (Section 5, S. 178).

3. **Toene erhoehen die Fehlerrate signifikant.** Zitat: "using tone increases the error rate. It increases CER by 6.9 +/- 3.6 points" (Section 3.1, S. 175) und WER um 18.8 +/- 11.4 Punkte.

4. **IPA uebertrifft Pinyin und Simple.** Mit Sprachmodell: IPA erreicht CER=17, WER=37 vs. Pinyin CER=19, WER=43 vs. Simple CER=20, WER=43 (Table 5, S. 176). Tonal CER: IPA=16, Pinyin=18, Simple=19.

5. **Wav2Vec2 besser als MMS trotz weniger Sprachen.** Wav2Vec2 (53 Sprachen) uebertrifft MMS (1162 Sprachen) fuer Baima. Zitat (paraphrasiert): "Wav2Vec2 had the lowest character error (CER=18.3 +/- 1.1), compared to Whisper (CER=19.3 +/- 1.8) and MMS (CER=25.1 +/- 0.7)" (Section 3.1, S. 173).

6. **Toene-spezifische Fehleranalyse.** Nicht alle Toene sind gleich schwierig: "the dipping tone 213 has a lower error rate in the IPA transcription (24% for Wav2Vec2 with KenLM) than in the Pinyin and Simple transcriptions (29% and 30% respectively)" (Section 3.2, S. 175). Entlehnte Toene (35, 55) sind mit Fehlern nahe 100% am schwierigsten.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Typ | Feldaufnahmen einer bedrohten tonalen Sprache |
| Sprache | Baima (Tibeto-Burmanisch, ISO 639: bqh) |
| Dauer | 186 Minuten (nach Filterung) |
| Woerter | 27.417 (2.715 unique) |
| Sprecher | 3 Muttersprachler (alle maennlich, 50-70+ Jahre) |
| Toene | 3 native kontrastive (53, 44, 213) + Sandhi (31) + 2 entlehnte (35, 55) |
| Transkriptionssysteme | IPA, Pinyin, Simple |
| Basismodelle | Wav2Vec2 XLSR-53, MMS 1b-all, Whisper Medium |
| Train/Dev/Test | 80%/10%/10% (20 randomisierte Splits) |
| Oeffentlich verfuegbar | Ja (https://github.com/rolandocoto/baima-asr) |

### Verwendbarkeit

- **RQ1 (Text-only Baseline):** Nicht direkt relevant (Audio-zu-Transkription, nicht Text-zu-Pinyin). Aber die Erkenntnis, dass das Transkriptionsformat die Ergebnisse beeinflusst, ist relevant fuer die Wahl zwischen Pinyin und IPA in der Masterarbeit.
- **RQ2 (Muttersprachler-Transkription):** Relevant als methodischer Vergleich. Zeigt, dass IPA-Transkription zu besseren ASR-Ergebnissen fuehrt als Pinyin -- uebertragbar auf die Frage, ob Mandarin-Tonerkennung mit IPA oder Pinyin evaluiert werden sollte.
- **RQ3 (L2-Lerner):** Nicht direkt relevant (Muttersprachler einer bedrohten Sprache, keine L2-Lerner).
- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant. Vergleicht Wav2Vec2, MMS und Whisper auf identischem Datensatz. Zeigt, dass Wav2Vec2 Whisper bei Phone-Level-Transkription uebertrifft.
- **RQ5 (Fehlerstruktur):** Hoch relevant. Detaillierte Analyse, welche Toene am schwierigsten zu transkribieren sind (Table 4). Entlehnte Toene am fehleranfaelligsten, Ton 53 am leichtesten. Trennung von Ton-CER, Konsonant-CER und Vokal-CER (Table 5) zeigt, dass Toene die hoechste Fehlerrate haben.

### Forschungsluecken

**Explizit benannt (mit Zitat):**
- "complex tones remain the most difficult part of the phonology to transcribe" (Section 5, S. 178)
- "the way the language is transcribed can affect tonal outputs, even when the tonal markings themselves remain the same throughout different transcriptions" (Section 5, S. 178)

**Implizit (Mapping auf Luecken der Masterarbeit):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Keine multimodalen LLMs getestet; nur SSL-Modelle (Wav2Vec2, MMS, Whisper).
- **Luecke 2 (Toene selten separater Evaluationsaxis):** Direkt adressiert durch tonal CER und tonal WER als separate Metriken. Zeigt den Wert dieser Trennung.
- **Luecke 3 (Wenige L2-Studien):** Nicht adressiert (Muttersprachler einer bedrohten Sprache).
- **Luecke 4 (Kein Head-to-Head):** Wav2Vec2 vs. MMS vs. Whisper verglichen, aber keine LLMs.

---

## Querverbindungen zwischen den Papers

### Thematische Cluster

1. **Mispronunciation Detection (Papers 15, 16):** Beide fokussieren auf Fehlererkennung bei L2-Mandarin-Lernern, mit komplementaeren Ansaetzen (automatisches Modell vs. kommerzielle Engines vs. menschliche Bewerter).

2. **Systematische Uebersicht und Ton-Schwierigkeit (Paper 17):** Liefert den uebergeordneten Rahmen fuer Papers 15-16 und dokumentiert den T2/T3-Kontrast als persistente Herausforderung.

3. **Multilinguales Phone Recognition (Paper 18):** Zeigt State-of-the-Art fuer Phone-Level-Erkennung mit ZIPA, relevant als Vergleichspunkt fuer die Masterarbeit.

4. **Phonetische Grundlage (Paper 19):** Liefert die akustisch-phonetische Basis fuer die Kopplung von Ton- und Segment-Evaluation.

5. **Speech LLM Benchmark (Paper 20):** Direkt relevantester Beitrag -- zeigt, dass GPT-4o bei prosodischen Aufgaben nur 59.70% erreicht.

6. **Transkriptionssystem-Einfluss (Paper 21):** Methodisch wichtig fuer die Entscheidung zwischen IPA und Pinyin bei der Evaluation.

### Konsistente Befunde ueber Papers hinweg

- **T2/T3-Kontrast als schwierigster:** Papers 16 (Tone 3 hoechste FRR), 17 (explizit als "persistent difficulty"), 21 (Dipping-Ton 213 schwieriger als andere).
- **Toene erhoehen Fehlerrate:** Papers 18 (Diakritika-Entfernung reduziert PFER), 21 (CER steigt um 6.9 Punkte mit Toenen).
- **Datenmangel fuer L2:** Papers 15 (nur 4 L2-Sprecher), 16 (12 Lerner), 17 (dokumentiert systematisch).
- **LLMs haben Defizite bei Prosodie/Toenen:** Paper 20 (GPT-4o nur 59.70%), unterstuetzt indirekt durch Papers 17 (kein LLM im Review) und 18 (kein LLM-Vergleich).

---

# KATEGORIE D: Benchmarks & Evaluation (Papers 22-30)


---

## Paper 22 -- Wei et al. (2024): "Benchmarking Chinese ASR Error Correction"

**Datei:** `2412.03075v1 (1).pdf`
**Relevanz:** ★★★★☆

### Kernaussagen (mit Zitaten)

1. **Prompting ist ineffektiv fuer ASR-Fehlerkorrektur:** "prompting is not effective for ASR error correction" (Abstract). Alle getesteten LLMs (ChatGLM3-6B, Qwen-7B, Baichuan2-7B) liefern per Zero-Shot/Few-Shot-Prompting hoehere CER als die ASR-Baseline.

2. **Multimodale Augmentierung ist die wirksamste Methode:** "Multi-modal augmentation is the most effective method for error correction and achieves state-of-the-art performance" (Abstract). Qwen-Audio im ASR-Modus erreicht CER 5.58 auf dem A*-Testset (Baseline: 12.42), finetuned sogar CER 3.24.

3. **Finetuning wirkt nur bei einem Teil der LLMs:** "Finetuning is effective only for a portion of LLMs" (Abstract). Baichuan2 profitiert stark (LoRA-finetuned CER 5.96 auf A* mixed), waehrend ChatGLM3 kaum Verbesserung zeigt.

4. **Homonyme Fehler sind ein Kernproblem des Chinesischen:** "'Zixuan'(梓萱) and 'Zixuan'(子轩) share the same pronunciation and both are commonly used names" (Figure 5). Ebenso: "In Chinese, the pronunciation for 'he'(他) and 'she'(她) are identical" (Figure 5). Substitutionsfehler dominieren vor Deletions- und Insertionsfehlern.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | ASR-EC Benchmark (A* + B*) |
| Sprache | Mandarin-Chinesisch |
| Quell-Korpora | THCHS-30 (30h), AISHELL-1 (200h), AISHELL-2 (1000h), WeNetSpeech (1100h) |
| ASR-Pipelines | Kaldi-K1 (DNN-HMM Hybrid), Kaldi-K2 (Zipformer-Transducer) |
| Umfang | A* Test: CER 12.42 (mixed); B* Test: CER 8.11 (mixed) |
| Modelle getestet | ChatGLM3 (6B), Qwen (7B), Baichuan2 (7B), Qwen-Audio (7B multimodal) |
| Paradigmen | Prompting, LoRA-Finetuning, Multimodale Augmentierung |
| Metriken | CER, Substitutions-/Deletions-/Insertionsrate |
| Offen verfuegbar | Ja |

### Verwendbarkeit

- **RQ1 (Text-Baseline):** Indirekt relevant -- zeigt, dass reine Text-LLMs (Prompting) chinesische ASR-Fehler nicht zuverlaessig korrigieren koennen, was die Grenzen textbasierter Pinyin-Konversion impliziert.
- **RQ4 (Vergleich mit dedizierten Systemen):** Hoch relevant -- direkter Vergleich von Kaldi-basierter ASR-Pipeline mit LLM-basierter Fehlerkorrektur und multimodalem Qwen-Audio. Zeigt, dass Qwen-Audio finetuned (CER 3.24) die Kaldi-Baseline (CER 12.42) deutlich schlaegt.
- **RQ5 (Fehlerstruktur):** Sehr relevant -- Homonym-Fehler (gleiche Aussprache, verschiedene Zeichen) als Kernproblem identifiziert. Substitutionsfehler dominieren, was direkt auf tonale Ambiguitaeten uebertragbar ist.

### Forschungsluecken

**Explizit:**
- Homonym-Disambiguierung bleibt ungeloest: "'Zixuan'(梓萱) and 'Zixuan'(子轩) share the same pronunciation and both are commonly used names" -- dieselbe Aussprache fuehrt zu systematischen Verwechslungen, die rein akustisch nicht aufloesbar sind.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Bestaetigt -- Evaluation beschraenkt sich auf CER; keine tonale Einzelanalyse.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- trotz Fokus auf Chinesisch werden Toene nie als separate Fehlerkategorie ausgewertet.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert innerhalb der ASR-EC-Paradigmen, aber kein Vergleich mit Frontier-Modellen wie GPT-4o.

---

## Paper 23 -- Wang et al. (2025): "ContextASR-Bench"

**Datei:** `2507.05727v2 (1).pdf`
**Relevanz:** ★★★☆☆

### Kernaussagen (mit Zitaten)

1. **Bestehende ASR-Benchmarks verfehlen LLM-Verbesserungen:** "existing ASR benchmarks fail to unveil the improvements brought by LLM to speech recognition tasks" (Abstract). ContextASR-Bench ist der erste massive kontextuelle ASR-Benchmark mit 40.000+ Eintraegen.

2. **LALMs uebertreffen konventionelle ASR bei Named Entities deutlich:** "Qwen2.5-Omni-7B exhibits a relative reduction of 39.9% in WER and 42% in NE-FNR compared to Whisper-Large-V3" (Results, ContextASR-Dialogue EN, Contextless Setting). Konventionelle Systeme haben "generally have NE-FNR rates exceeding 50%".

3. **Halluzinationsproblem bei feingranularem Kontext:** "some LALMs begin to generate severe hallucinations, manifested as repeating only emitting entities within the text prompt when transcribing speech" (Results, Fine-grained Context).

4. **Neue Metriken fuer Named-Entity-Erkennung:** NE-WER (fuzzy matching mit Edit-Distance-Toleranz) und NE-FNR (exakte Entity-Miss-Rate) als robustere Alternativen zu Standard-WER.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | ContextASR-Bench |
| Sprachen | Englisch + Chinesisch |
| Umfang Speech | EN: 15,326 Aeusserungen (187.98h), ZH: 15,498 (197.64h) |
| Umfang Dialogue | EN: 5,273 (221.86h), ZH: 5,232 (230.39h) |
| Named Entities | 300,000+ |
| Evaluierungs-Settings | Contextless, Coarse-grained Context, Fine-grained Context |
| TTS-Pipeline | Zero-Shot TTS mit PER-Schwelle 0.03 |
| Metriken | WER, NE-WER, NE-FNR |
| Modelle getestet | Paraformer-Large, Sensevoice-Small, Whisper-Large-v3/turbo, Qwen2-Audio, Qwen2.5-Omni-3B/7B, Kimi-Audio, Baichuan-Audio u.a. |

### Verwendbarkeit

- **RQ2 (Native-Speaker-Sprache):** Begrenzt relevant -- Chinesisch-Subset koennte als Vergleichsbasis fuer WER dienen, aber Fokus liegt auf Named Entities, nicht auf Phonetik/Toenen.
- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- umfassender Vergleich zwischen konventionellen ASR-Systemen (Whisper, Paraformer) und LALMs (Qwen2.5-Omni) zeigt systematische Ueberlegenheit der LALMs bei kontextabhaengiger Erkennung.
- **RQ5 (Fehlerstruktur):** Indirekt relevant -- Homonym-Problematik bei chinesischen Named Entities beruehrt tonale Ambiguitaeten (z.B. gleichlautende Personennamen).

### Forschungsluecken

**Explizit:**
- Halluzinationen bei feingranularem Kontext: "some LALMs begin to generate severe hallucinations" -- ein systematisches Problem, das bei kontextreicher Transkription auftritt.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Bestaetigt -- auch NE-WER/NE-FNR erfassen keine tonalen Unterschiede.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- trotz Chinesisch-Subset keine Ton-Evaluation.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert durch breiten Modellvergleich, aber ohne Frontier-Modelle wie GPT-4o-Audio.

---

## Paper 24 -- Liu et al. (2025): "VocalBench-zh"

**Datei:** `2511.08230v1.pdf`
**Relevanz:** ★★★☆☆

### Kernaussagen (mit Zitaten)

1. **LLM-Backbone ist der Hauptfaktor fuer semantische Qualitaet:** "LLM backbone is the main factor affecting overall performance, especially semantic quality" (Observation 1).

2. **SpeechLLMs schliessen zu kaskadierten Loesungen auf:** "The performance gap between SpeechLLMs and cascaded solutions is steadily narrowing, with the former inherently excelling in speech expressiveness" (Observation 3).

3. **Mandarin-spezifische Faehigkeiten sind unterentwickelt:** "The Mandarin-specific capabilities of most models require significant improvement" (Observation 4). Insbesondere: "SpeechLLMs currently lack awareness of Chinese character components and structure" (Observation 5).

4. **Paralinguistische Kontrolle fehlt:** "SpeechLLMs lack control over the paralinguistic control of their speech responses" (Observation 10).

5. **Robustheit ist Vorteil von End-to-End-Loesungen:** "Robustness is an inherent advantage of end-to-end solutions" (Observation 13).

6. **Nur synthetische Daten verwendet:** "current version relies exclusively on synthetic data and does not include human-recorded speech" (Limitations).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | VocalBench-zh |
| Sprache | Mandarin-Chinesisch |
| Umfang | 11,115 Instanzen, 10 Evaluierungs-Subsets |
| Dimensionen | Knowledge, Reasoning, Creativity, Dialogue, Instruction Following, Emotional Empathy, Safety, Code-Switching, Robustness |
| Format | Speech Instruction (SI) |
| Modelle | 14 multimodale Modelle + Kaskade (Whisper-large-v3 + Qwen3-8B + CosyVoice2) |
| Audio-Typ | Ausschliesslich synthetisch (kein Human-Recorded Speech) |

### Verwendbarkeit

- **RQ2 (Native-Speaker-Sprache):** Begrenzt -- nur synthetische Daten, keine echten Muttersprachler-Aufnahmen.
- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- direkter Vergleich von 14 SpeechLLMs mit Kaskade-Baseline. Zeigt, dass SpeechLLMs bei Expressivitaet ueberlegen, bei Mandarin-Spezifika aber schwach sind.
- **RQ5 (Fehlerstruktur):** Indirekt relevant -- mangelnde Zeichenstruktur-Erkennung und paralinguistische Kontrolle koennte auf tonale Defizite hindeuten.

### Forschungsluecken

**Explizit:**
- Ausschliesslich synthetische Daten: "current version relies exclusively on synthetic data and does not include human-recorded speech" -- limitiert die Uebertragbarkeit auf echte Sprachszenarien.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Bestaetigt -- keine phonetische/tonale Feinanalyse trotz Mandarin-Fokus.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- Mandarin-Benchmark ohne explizite Ton-Evaluierung.
- **Luecke 3 (Wenige L2-Learner-Studien):** Bestaetigt -- nur synthetische Sprachdaten, keine L2-Sprecher.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert durch breiten Modellvergleich.

---

## Paper 25 -- Li et al. (2026): "TELEVAL"

**Datei:** `2507.18061v3.pdf`
**Relevanz:** ★★★★☆

### Kernaussagen (mit Zitaten)

1. **Drei-Stufen-Kompetenzpyramide fuer SLMs:** Perceptual Robustness (Level 1) -> Explicit Semantic Reasoning (Level 2) -> Social-Pragmatic Alignment (Level 3). SLMs scheitern systematisch an der dritten Stufe.

2. **"Caption Trap" -- Erkennen ohne Reagieren:** "models like Kimi-Audio accurately recognize paralinguistic cues (e.g., coughing) but fail to generate informed responses (e.g., 'Are you okay?'). This likely stems from training regimes treating paralinguistic data as explicit classification targets rather than implicit signals for behavioral adaptation" (Section 5, Caption Trap).

3. **Semantische Staerke, sozial-pragmatische Schwaeche:** "current SLMs exhibit a clear deficit in social-pragmatic competence: although they can detect paralinguistic cues, they often fail to incorporate them into natural response strategies, exhibiting a 'task-driven' rather than 'interaction-aware' bias" (Conclusion).

4. **Dialektverstaendnis als Herausforderung:** TELEVAL testet Cantonese, Henan, NE Mandarin, Shanghainese, Sichuanese -- ein Novum fuer SLM-Benchmarks.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | TELEVAL |
| Sprache | Mandarin-Chinesisch (+ Dialekte) |
| Umfang | 40,000+ Evaluierungs-Samples |
| Audio-Typ | Echte Human-Recordings + Synthetisch |
| Formate | FAQA (Factoid Audio QA), OEAC (Open-Ended Audio Conversation) |
| Dimensionen | 12+ Aufgabenkategorien inkl. Acoustic Robustness (11 Bedingungen), Dialect, Empathetic Response, Safety |
| Modelle | 13 Modelle + GPT-4o-Audio (API) |
| Besonderheit | Einziger Benchmark mit Acoustic Robustness + Paralinguistic-Aware Response + Instruction-free Interaction |

### Verwendbarkeit

- **RQ2 (Native-Speaker-Sprache):** Relevant -- echte Human-Recordings in Mandarin und Dialekten. Dialektverstaendnis erfordert implizit tonales Verstehen.
- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- GPT-4o-Audio als Referenz, 13 weitere Modelle im Vergleich.
- **RQ5 (Fehlerstruktur):** Hoch relevant -- die "Caption Trap" offenbart, dass Modelle paralinguistische Signale (inkl. prosodischer Merkmale wie Ton) zwar erkennen, aber nicht adaequat verarbeiten. Dies ist direkt auf tonale Erkennung uebertragbar: Modelle koennten Toene "hoeren" aber nicht korrekt zuordnen.

### Forschungsluecken

**Explizit:**
- Caption Trap als systematisches Defizit: Modelle klassifizieren paralinguistische Cues korrekt, integrieren sie aber nicht in natuerliche Antwortstrategien. Dies deutet auf ein fundamentales Trainingsproblem hin.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Bestaetigt -- TELEVAL geht ueber WER/CER hinaus (FAQA, OEAC), evaluiert aber keine tonale Genauigkeit.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- trotz Mandarin-Dialekten keine explizite Ton-Evaluation.
- **Luecke 3 (Wenige L2-Learner-Studien):** Bestaetigt -- nur Muttersprachler und Dialektsprecher, keine L2-Lernenden.

---

## Paper 26 -- Sakshi et al. (2024): "MMAU: A Massive Multi-Task Audio Understanding and Reasoning Benchmark"

**Datei:** `2410.19168v1.pdf`
**Relevanz:** ★★★☆☆

### Kernaussagen (mit Zitaten)

1. **Selbst beste Modelle scheitern an Expert-Level-Audio-Aufgaben:** "even the most advanced Gemini Pro v1.5 achieves only 52.97% accuracy, and the state-of-the-art open-source Qwen2-Audio achieves only 52.50%" (Abstract). Menschliche Performance liegt bei 82%.

2. **Modelle performen am besten bei Sound, am schlechtesten bei Speech:** "Models perform best on sound and worst on speech. With average scores of 18%, 30%, 23% across speech, sound, and music, models perform best on sound-related tasks and struggle the most with music" (Section 5.1, Finding 4). Speech-Reasoning bleibt eine zentrale Herausforderung.

3. **Kaskadierte Ansaetze uebertreffen End-to-End-Modelle:** "Cascaded approaches outperform others. Captioning audios first and then prompting LLMs yields the best results" (Section 5.1, Finding 5). GPT-4o mit starken Captions erreicht 59% -- das Gesamtbeste.

4. **Perzeptionsfehler dominieren:** Figure 7 zeigt: Qwen2-Audio-Instruct 55% Perceptual Errors, Gemini Pro v1.5 64% Perceptual Errors. Dies deutet darauf hin, dass Modelle primaer am Wahrnehmen und nicht am Schlussfolgern scheitern.

5. **27 verschiedene Skills getestet, inkl. Phonemic Stress Pattern Analysis:** MMAU deckt Skills wie Phonemic Stress Pattern Analysis, Emotional Tone Interpretation und Temporal Reasoning ab (Figure 3, Figure 6).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | MMAU |
| Sprache | Englisch (primaer) |
| Umfang | 10,000 MCQs (1,000 test-mini + 9,000 test) |
| Domaenen | Speech (33%), Sound (22%), Music (45%) |
| Fragetypen | Information Extraction (3,499), Reasoning (6,501) |
| Skills | 27 distinct skills (16 Reasoning, 11 Info Extraction) |
| Schwierigkeitsgrade | Easy 22%, Medium 56%, Hard 22% |
| Durchschnittliche Audiolaenge | 10.14 Sekunden |
| Modelle getestet | 18 LALMs (open-source + proprietaer) + LLM-Baselines |
| Metriken | Micro-averaged Accuracy |

### Verwendbarkeit

- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- breiter Vergleich von LALMs zeigt fundamentale Performanceluecken. Gemini Pro v1.5 und Qwen2-Audio als Referenzpunkte.
- **RQ5 (Fehlerstruktur):** Relevant -- Phonemic Stress Pattern Analysis als direkt verwandter Skill. Dominanz von Perceptual Errors (55-64%) zeigt, dass Modelle an der akustischen Wahrnehmung scheitern -- uebertragbar auf tonale Perzeption.

### Forschungsluecken

**Explizit:**
- Nur MCQ-Format: "MMAU focuses on multiple-choice tasks and does not evaluate open-ended generation" (Conclusion/Limitations). Offene Transkriptionsaufgaben fehlen.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Erweitert -- MMAU geht ueber WER/CER hinaus mit Skills-basierter Evaluation, aber keine tonale Feinanalyse.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- Phonemic Stress wird getestet, aber lexikalische Toene (Mandarin) nicht.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert durch breiten Modellvergleich, aber nicht fuer Mandarin-Toene.

---

## Paper 27 -- Huang et al. (2024): "Dynamic-SUPERB Phase-2"

**Datei:** `2411.05361v1.pdf`
**Relevanz:** ★★☆☆☆

### Kernaussagen (mit Zitaten)

1. **Groesster Benchmark fuer Spoken Language Models mit 180 Tasks:** "this second version incorporates 125 new tasks contributed collaboratively by the global research community, expanding the benchmark to a total of 180 tasks, making it the largest benchmark for speech and audio evaluation" (Abstract).

2. **Kein Modell performt universell gut:** "none of the models performed well universally. SALMONN-13B excelled in English ASR, while WavLLM demonstrated high accuracy in emotion recognition, but current models still require further innovations to handle a broader range of tasks" (Abstract).

3. **Taxonomie mit expliziter Phonetics-Kategorie:** Die Task-Taxonomie (Figure 2a) enthaelt eine eigene Kategorie "Phonetics, Phonology, and Prosody" mit Unterkategorien wie Phoneme Recognition, Pronunciation Evaluation, Stress und Accent Classification. Allerdings: "in the phonetics and prosody domain, WavLLM and LTU-AS exhibit poor scores due to their erroneous outputs in phone/phoneme segment counting tasks" (Section 5.1).

4. **ASR bleibt schwierig -- nur wenige Modelle unter 10% WER:** "In ASR, SALMONN-13B and WavLLM are the only two models that achieved a word error rate (WER) lower than 10%" (Section 5.2). Whisper-LLaMA-Kaskade bleibt staerkste Baseline.

5. **Kaskadiertes System verliert paralinguistische Information:** "the ASR process in the cascaded system tends to discard critical information from the speech signal, such as speaker characteristics, pitch, and emotion" (Section 5.1). End-to-End-Modelle uebertreffen die Kaskade bei Speaker- und Paralinguistics-Domaenen.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | Dynamic-SUPERB Phase-2 |
| Sprachen | Primaer Englisch (+ multilinguale Tasks) |
| Umfang | 180 Tasks (91 neu in Phase 2) |
| Domaenen | Speech (8 Domains), Audio & Music (9 Domains) |
| Core Tasks | SUPERB (Speech), MARBLE (Music), HEAR (Audio) |
| Aufgabentypen | Classification, Regression, Sequence Generation |
| Modelle getestet | SALMONN-7B/13B, LTU-AS, Qwen-Audio, Qwen2-Audio, WavLLM, MU-LLaMA, GAMA + Whisper-LLaMA (Kaskade) |
| Evaluierung | LLM-Judge (GPT-4o) fuer Classification; Post-Processor fuer Regression |

### Verwendbarkeit

- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- breiter Modellvergleich mit Kaskade-Baseline. Zeigt, dass kein universelles Modell existiert.
- **RQ5 (Fehlerstruktur):** Begrenzt relevant -- Phonetics/Prosody-Domaene existiert, aber Modelle scheitern bereits an basalen Tasks (Phoneme Segment Counting). Kein Mandarin-Ton-Task enthalten.

### Forschungsluecken

**Explizit:**
- Keine universelle Kompetenz: Jedes Modell hat Staerken in Nischenbereichen, aber keine breite Abdeckung. Community-basierte Erweiterung als Loesungsansatz.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Erweitert -- 180 Tasks gehen weit ueber WER/CER hinaus, aber keine Mandarin-Ton-Evaluation.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- Phonetics-Kategorie existiert (Stress, Accent), aber lexikalische Toene fehlen explizit.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert -- systematischer Vergleich von 9+ Modellen, aber nicht fuer tonale Transkription.

---

## Paper 28 -- Cui et al. (2025): "VoxEval"

**Datei:** `2501.04962v4.pdf`
**Relevanz:** ★★★☆☆

### Kernaussagen (mit Zitaten)

1. **Erster Benchmark fuer End-to-End-Speech-Evaluation:** VoxEval ist "to the best of our knowledge, the first benchmark that supports end-to-end evaluation of speech-based interactions" (Section 1) -- sowohl Fragen als auch Antworten sind im Audioformat.

2. **Die meisten SLMs scheitern unter Zufallsniveau:** "current SLMs perform poorly, with most failing to surpass random guessing" (Section 4.2). Bei 4 Antwortoptionen (25% Baseline) erreicht nur GLM-4-Voice ueber 25%; Whisper+Llama3-Kaskade erreicht 55%.

3. **Pitch-Shifts sind die groesste Herausforderung:** "SLMs are susceptible to different input audio conditions" (Section 4.2). Table 5 zeigt: Pitch-Variation verursacht den groessten Performanceeinbruch (GLM-4-Voice: 33.45% vs. 37.63% bei Speakern). Linguistische Variationen und Umgebungsakustik haben geringeren Einfluss.

4. **Chain-of-Thought verschlechtert SLM-Performance:** "Using CoT reduces the performance of SLMs" (Section 4.2, RQ4). Dies gilt konsistent ueber alle getesteten Modelle -- ein fundamentaler Unterschied zu Text-LLMs.

5. **Mathematisches Reasoning ist quasi absent:** Table 8 zeigt, dass die meisten SLMs bei Mathe-Aufgaben nahe 0% liegen. "most SLMs are unlikely to possess basic math reasoning abilities" (Section 4.2).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | VoxEval |
| Sprache | Englisch |
| Umfang | 13,938 SpeechQA-Paare |
| Faecher | 56 (basierend auf MMLU) |
| Input Audio Conditions | 26 verschiedene (6 Sprecher, 5 linguistische Variationen, 2 paralinguistische, 2 Rauschtypen, Umgebungsakustik) |
| TTS | OpenAI TTS (6 Stimmen: alloy, echo, fable, nova, onyx, shimmer) |
| Modelle getestet | SpeechGPT, TWIST, SPIRIT-LM, Moshi, GLM-4-Voice + Whisper+Llama3 |
| Metriken | Accuracy (String Matching + LLM-based) |
| Format | End-to-End Speech (Audio-in, Audio-out) |

### Verwendbarkeit

- **RQ2 (Native-Speaker-Sprache):** Begrenzt -- Englisch-only, aber die Methodik (Pitch-Shift-Robustheit) ist direkt auf tonale Sprachen uebertragbar.
- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- zeigt fundamentale Schwaechen von End-to-End-SLMs gegenueber Kaskade (Whisper+Llama3: 55% vs. GLM-4-Voice: 37%). Unterstreicht, dass Kaskaden-Systeme bei wissensintensiven Aufgaben ueberlegen bleiben.
- **RQ5 (Fehlerstruktur):** Relevant -- Pitch-Shift als groesste Herausforderung ist direkt relevant fuer tonale Sprachen: Wenn Modelle bereits bei englischem Pitch-Shift scheitern, ist tonale Unterscheidung in Mandarin noch schwieriger.

### Forschungsluecken

**Explizit:**
- Nur Englisch, keine tonalen Sprachen: "The VoxEval benchmark focuses exclusively on assessing the robustness of SLMs in terms of knowledge understanding" (Limitations). Mandarin/Tonsprachen explizit nicht abgedeckt.

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Erweitert -- VoxEval misst Knowledge Understanding statt WER, aber ohne tonale Dimension.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- Pitch-Shift wird als Stoervariable getestet, nicht als linguistisches Merkmal (Ton).
- **Luecke 3 (Wenige L2-Learner-Studien):** Bestaetigt -- nur synthetische Variation, keine echten L2-Sprecher.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert fuer SLMs, aber Frontier-Modelle (GPT-4o-Audio, Gemini) fehlen.

---

## Paper 29 -- Wang et al. (2025): "AudioBench: A Universal Benchmark for Audio Large Language Models"

**Datei:** `2406.16020v5.pdf`
**Relevanz:** ★★☆☆☆

### Kernaussagen (mit Zitaten)

1. **Kein Modell dominiert ueber alle Tasks:** "no single model excels consistently across all tasks" (Abstract/Conclusion). Table 2 zeigt stark variierende Performance ueber 26 Datasets.

2. **AudioLLMs scheitern an Long-Form ASR:** "all AudioLLMs struggle with long-form ASR tasks" (Section 4.1). Earning-21 und Earning-22 (Earnings Calls, je mehrere Stunden) zeigen WER >30% fuer alle AudioLLMs, waehrend Whisper+Llama3 bei 11-15% liegt.

3. **Kaskade Whisper+Llama3 ist bei sprachintensiven Tasks ueberlegen:** "For speech-intensive tasks such as SQA and SI, the cascade model Whisper+Llama3 exhibits superior performance. This effectiveness stems from the Whisper model's robust recognition capabilities and Llama's strong reasoning abilities" (Section 4.1).

4. **Model-as-Judge mit Llama-3-70B-Instruct:** AudioBench verwendet "Llama-3-70B-Instruct" als open-source Model-as-Judge, da dieser "a very strong correlation with GPT-4" zeigt (Spearman-Korrelation >0.85 auf allen drei Test-Datasets; Section 4.3, Figure 3).

5. **Ausschliesslich Englisch:** "the current AudioBench exclusively includes English datasets" (Limitations). Multilingual-Erweiterung ist als Future Work geplant.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | AudioBench |
| Sprache | Englisch (ausschliesslich) |
| Umfang | 8 Tasks, 26 Datasets, 400+ Stunden, 100k+ Samples |
| Hauptbereiche | Speech Understanding (ASR, SQA, SI), Audio Scene Understanding (AQA, AC), Voice Understanding (ER, AR, GR) |
| Modelle getestet | SALMONN, Qwen-Audio-Chat, WavLLM, Qwen2-Audio-Instruct + Whisper+Llama3 (Kaskade) |
| Metriken | WER (ASR), METEOR (Captioning), Model-as-Judge/M.J. (alle anderen) |
| Judge-Modell | Llama-3-70B-Instruct (100-Punkte-Skala) |

### Verwendbarkeit

- **RQ4 (Vergleich mit dedizierten Systemen):** Relevant -- systematischer Vergleich von 4 AudioLLMs + Kaskade ueber 26 Datasets. Bestaetigt Ueberlegenheit von Kaskaden-Systemen bei ASR-lastigen Tasks.
- **RQ5 (Fehlerstruktur):** Begrenzt relevant -- Accent Recognition und Emotion Recognition als paralinguistische Tasks koennen methodisch auf Ton-Evaluation uebertragen werden. Prompt-Sensitivitaet (SALMONN reagiert unterschiedlich auf verschiedene ASR-Prompts) ist relevant fuer Experiment-Design.

### Forschungsluecken

**Explizit:**
- Nur Englisch: "the current AudioBench exclusively includes English datasets" (Limitations).
- Open-ended Evaluation ist ungeloest: "evaluating free-style generation is challenging and demands robust metrics or models to serve as judges" (Limitations).

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Bestaetigt -- ASR-Evaluation rein ueber WER, keine phonetische Feinanalyse.
- **Luecke 2 (Toene selten separat evaluiert):** Bestaetigt -- keine tonale Evaluation, kein Mandarin.
- **Luecke 3 (Wenige L2-Learner-Studien):** Bestaetigt -- Accent Recognition existiert, aber keine L2-Learner-Daten.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert, aber ohne Frontier-Modelle (GPT-4o, Gemini) und ohne Mandarin.

---

## Paper 30 -- Zhang et al. (2025): "WildSpeech-Bench: Benchmarking Audio LLMs in Natural Speech Conversation"

**Datei:** `2506.21875v1.pdf`
**Relevanz:** ★★★★★

### Kernaussagen (mit Zitaten)

1. **Selbst das beste Modell erreicht nur 6.29/10 Punkte:** "with a total score of 10 points, even the best model, GPT-4o-Audio, has an average score of only 6.29 points" (Section 4.2). Dies zeigt erheblichen Verbesserungsbedarf bei End-to-End-Speech-LLMs.

2. **Paralinguistische Features als explizite Evaluierungskategorie:** Table 2 zeigt die PF-Subkategorien: Pause, Stress, Tone, Stuttering, near-Homophone. GPT-4o-Audio erreicht im PF-Durchschnitt 6.01/10, die naive Pipeline nur 4.84/10.

3. **Pipeline-Methode verliert kritische paralinguistische Information:** "the pipeline method performs significantly worse only on the stress and tone subsets of our PF sub-category. This degradation can be attributed to the loss of critical paralinguistic information during the ASR stage" (Section 4.2). Konkret: Naive Pipeline bei Tone 4.12/10 vs. GPT-4o-Audio 5.85/10.

4. **Evaluation-Prompts ignorieren sprachspezifische Phaenomene:** "evaluation prompts are rarely designed to capture speech-specific phenomena such as the impact of tone, pauses, homophones, or stuttering on semantic interpretation" (Introduction/Section 1, "Untargeted evaluation method").

5. **Audio2Audio-Evaluierungsmodalitaet:** WildSpeech verwendet Audio2Audio-Evaluation (Speech-in, Speech-out), wobei Whisper-large-v3 fuer die Transkription der Modell-Antworten und GPT-4o mini fuer die Bewertung (1-10 Skala) eingesetzt wird. Drei ASR-Durchlaeufe werden gemittelt.

6. **Near-Homophone als eigene Kategorie:** "sentences containing near-homophones" werden explizit als Speech-spezifische Herausforderung getestet (Section 3.1.1). Beispiel: "Being an idol means you won't be able." (idle/idol Homophon-Verwechslung).

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Name | WildSpeech-Bench |
| Sprache | Englisch |
| Umfang | 1,100 Queries (1,000 General + 100 Paralinguistic-Featured) |
| General-Kategorien | Information Inquiry (II), Solution Request (SR), Opinion Exchange (OE), Text Creation (TC) |
| PF-Kategorien | Pause, Stress, Tone, Stuttering, near-Homophone |
| Audio-Quellen | TTS (CosyVoice, Voice Cloning) + Real Person Recordings (100 PF) |
| Sprecher-Diversitaet | Maennlich/Weiblich, Kinder/Jugendliche/Erwachsene/Aeltere |
| Rauschen | Background Human Speech + Diverse Non-Speech Noises |
| Modelle getestet | GLM-4-Voice, MiniCPM, Qwen-2.5-omni, GPT-4o-Audio + Naive Pipeline |
| Metriken | GPT-4o-mini Score (1-10), UTMOS (akustische Qualitaet) |
| Evaluation | Audio2Audio + Query-Aware Evaluation |

### Verwendbarkeit

- **RQ2 (Native-Speaker-Sprache):** Methodisch uebertragbar -- die Evaluierungsmethodik (paralinguistische Subkategorien, Audio2Audio) koennte direkt fuer Mandarin-Ton-Evaluation adaptiert werden.
- **RQ4 (Vergleich mit dedizierten Systemen):** Hoch relevant -- zeigt systematisch, dass Pipeline-Systeme bei Tone und Stress versagen, waehrend End-to-End-Modelle (GPT-4o-Audio) diese besser erfassen. Direkte Evidenz fuer den Informationsverlust im ASR-Schritt.
- **RQ5 (Fehlerstruktur):** Sehr hoch relevant -- Tone als explizite Subkategorie mit quantifizierten Ergebnissen. Pipeline bei Tone 4.12 vs. GPT-4o-Audio 5.85 zeigt, dass Ton-Information im ASR-Schritt verloren geht. Near-Homophone-Kategorie ist direkt auf chinesische Homophon-Problematik uebertragbar.

### Forschungsluecken

**Explizit:**
- Ton-Information geht im ASR-Schritt verloren: "This degradation can be attributed to the loss of critical paralinguistic information during the ASR stage" -- direkte Evidenz fuer Luecke 2.
- Evaluation-Methoden sind nicht auf Sprache zugeschnitten: "evaluation prompts are rarely designed to capture speech-specific phenomena such as the impact of tone, pauses, homophones, or stuttering on semantic interpretation."

**Implizit (Mapping):**
- **Luecke 1 (Frontier-LLMs nur WER/CER):** Erweitert -- WildSpeech geht ueber WER/CER hinaus mit query-aware, paralinguistischer Evaluation. Aber englischer "Tone" (Intonation) ist nicht gleich Mandarin-Ton (lexikalisch).
- **Luecke 2 (Toene selten separat evaluiert):** Teilweise adressiert fuer Englisch (Intonation), aber nicht fuer lexikalische Toene (Mandarin). Die Methodik ist direkt uebertragbar.
- **Luecke 3 (Wenige L2-Learner-Studien):** Bestaetigt -- keine L2-Sprecher im Datensatz.
- **Luecke 4 (Kein systematischer Vergleich):** Teilweise adressiert -- Pipeline vs. End-to-End-Vergleich, aber limitiert auf 4 Modelle + 1 Pipeline.

---

## Querverweise und Synthese

### Uebergreifende Befunde fuer die Masterarbeit

1. **Pipeline vs. End-to-End -- konsistente Evidenz:** Papers 22, 24, 26, 27, 28, 29, 30 zeigen alle, dass kaskadierte Systeme (ASR+LLM) bei sprachintensiven/wissensbasierten Tasks ueberlegen sind, aber bei paralinguistischen Features (Ton, Emotion, Prosodie) systematisch Information verlieren. WildSpeech (Paper 30) quantifiziert dies explizit fuer Tone: Pipeline 4.12 vs. GPT-4o-Audio 5.85.

2. **Perzeptionsfehler dominieren:** MMAU (Paper 26) zeigt 55-64% Perceptual Errors; VoxEval (Paper 28) zeigt, dass SLMs unter Random Guessing liegen; WildSpeech (Paper 30) zeigt selbst GPT-4o-Audio nur 6.29/10. Die Grundwahrnehmung von Audio ist das primaere Bottleneck.

3. **Mandarin-spezifische Evaluation fehlt systematisch:** Alle Benchmarks (ausser Papers 22-25) sind primaer oder ausschliesslich Englisch. Selbst die Mandarin-Benchmarks (VocalBench-zh, TELEVAL) evaluieren keine Toene separat --> Luecke 2 wird durch alle 9 Papers bestaetigt.

4. **Homophon-Problematik als Brueckenschlag:** Paper 22 (chinesische Homophone: 他/她, 梓萱/子轩) und Paper 30 (englische near-Homophones: idle/idol) zeigen beide, dass phonetisch aehnliche Woerter ein systematisches Problem darstellen. Fuer Mandarin sind Toene das primaere Disambiguierungsmerkmal --> direkte Relevanz fuer RQ5.

5. **"Caption Trap" (Paper 25) als theoretischer Rahmen:** Das Konzept, dass Modelle paralinguistische Merkmale erkennen, aber nicht adaequat verarbeiten, ist direkt auf die Masterarbeit uebertragbar: Koennen LLMs Toene "hoeren" (wahrnehmen), sie aber nicht korrekt in Pinyin-Transkription umsetzen?

---

# KATEGORIE E: Surveys (Papers 31-40)

---

## Paper 31 -- Latif et al. (2023): "Sparks of Large Audio Models: A Survey and Outlook"

**Vollstaendiger Titel:** Sparks of Large Audio Models: A Survey and Outlook
**Autoren:** Siddique Latif, Moazzam Shoukat, Fahad Shamshad, Muhammad Usama, Yi Ren, Heriberto Cuayahuitl, Wenwu Wang, Xulong Zhang, Roberto Togneri, Erik Cambria, Bjoern W. Schuller
**Jahr:** 2023
**Venue:** arXiv preprint (arXiv:2308.12792v3)

### Kernaussagen

1. **Erste umfassende Survey zu Large Audio Models (LAMs):** Das Paper praesentiert sich als "first survey paper that comprehensively covers applications of Large AI Models in the domain of audio signal processing" [Zitat nicht verifiziert] und deckt Sprache, Musik und allgemeine Audiodomaenen ab.

2. **Paralinguistische Information als zentrale Schwaeche:** "Zhang et al. [91] highlighted the deficiency in understanding and generating paralinguistic information, including emotions, as a potential limitation of SpeechGPT" [Zitat nicht verifiziert]. Auch AudioPaLM wird kritisiert: "AudioPalm [95], considered one of the finest LLMs for audio processing, exhibits limited coverage in paralinguistic information comprehension" [Zitat nicht verifiziert].

3. **Mehrsprachige Tokenisierung als Herausforderung:** "Multilingual speech tokenisation poses additional complexities as the same statement might demand a varying number of tokens in different languages" [Zitat nicht verifiziert]. Dies ist direkt relevant fuer Mandarin, das tonal kodiert ist.

4. **Breite Abdeckung existierender Modelle:** Table 2 bietet eine Uebersicht ueber Large Audio Models (SpeechGPT, AudioPaLM, AudioLM, AudioGen, AudioLDM, LTU, VioLA, MusicGen, MusicLM, WavJourney, SeamlessM4T). Table 4 vergleicht Whisper, MMS und SeamlessM4T auf dem FLEURS-Benchmark.

5. **Daten-Herausforderungen:** Table 3 listet Audio-Datensaetze, wobei keine spezifischen Mandarin-Ton-Datensaetze enthalten sind.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | FLEURS, LibriSpeech, Common Voice, MuST-C, VoxPopuli, u.a. (Table 3) |
| Sprachen | Mehrsprachig (bis 100+ via SeamlessM4T) |
| Mandarin enthalten | Ja, in FLEURS-Evaluation |
| Tonale Annotation | Nein |
| Evaluationsmetriken | WER, BLEU (Table 4) |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Kein Text-zu-Pinyin-Experiment |
| RQ2 (Native Speaker) | Mittel | FLEURS-Vergleich enthaelt Mandarin-WER |
| RQ3 (L2 Learner) | Keine | Kein L2-Fokus |
| RQ4 (Vergleich Whisper etc.) | Hoch | Direkter Vergleich Whisper vs. MMS vs. SeamlessM4T auf FLEURS |
| RQ5 (Fehlerstruktur) | Mittel | Diskussion paralinguistischer Defizite (Ton, Emotion, Prosodie) |

### Forschungsluecken

**Explizit genannte Luecken:**
- Paralinguistische Informationsverarbeitung: Das Paper identifiziert explizit die Unfaehigkeit aktueller LAMs, paralinguistische Merkmale wie Ton, Emotion und Prosodie adaequat zu verarbeiten.
- Tokenisierungskomplexitaet fuer mehrsprachige Systeme, insbesondere bei unterschiedlicher Token-Laenge pro Sprache.
- Datenprobleme: Doppelgaenger-Instanzen, persoenlich identifizierbare Informationen, Datenkontamination, diverse Pre-Training-Daten (Section 4.1).

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Table 4 berichtet ausschliesslich WER/BLEU -- keine phonetische oder tonale Analyse |
| Luecke 2 (Toene nicht separat) | Hoch | Trotz Erwaehnung von "tonality" als paralinguistischem Merkmal keine separate Ton-Evaluation |
| Luecke 3 (Wenig L2-Studien) | Hoch | Kein einziger L2-Lerner-Kontext erwaehnt |
| Luecke 4 (Kein systematischer Vergleich) | Mittel | Vergleich Whisper/MMS/SeamlessM4T auf FLEURS, aber nicht unter kontrollierten Bedingungen fuer tonale Genauigkeit |

---

## Paper 32 -- Gaido et al. (2024): "Speech Translation with Speech Foundation Models and Large Language Models: What is There and What is Missing?"

**Vollstaendiger Titel:** Speech Translation with Speech Foundation Models and Large Language Models: What is There and What is Missing?
**Autoren:** Marco Gaido, Sara Papi, Matteo Negri, Luisa Bentivogli
**Jahr:** 2024
**Venue:** arXiv preprint (arXiv:2402.12025v3)

### Kernaussagen

1. **Fehlender systematischer Vergleich von SFMs:** "no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework" (Section 2.1). Dies ist eine Kernkritik, die direkt auf die Forschungslandschaft der Sprach-LLM-Integration uebertragbar ist.

2. **Informationsverlust bei kaskadierten Systemen:** "the speech source contains a wide range of information that can be exploited depending on the paradigm used (e.g., prosody is not handled by cascade systems)" [Zitat nicht verifiziert]. Prosodie und damit verbundene tonale Information geht in Text-basierten Pipelines verloren.

3. **BLEU als einzige Metrik:** "all works rely on the BLEU metric" [Zitat nicht verifiziert] -- ein fundamentales Evaluationsdefizit, da BLEU keine akustischen oder phonetischen Aspekte erfasst.

4. **Architektonische Bausteine identifiziert:** Table 1 zeigt die 5 Komponenten (SFM, Length Adapter, Modality Adapter, Prompt-Speech Mixer, LLM) fuer 9 SFM+LLM-Modelle. Die Diversitaet der Ansaetze verhindert direkte Vergleichbarkeit.

5. **Mandarin als relevantes Sprachpaar:** en-zh ist das zweithaeufigst berichtete Sprachpaar in der SFM+LLM-Literatur fuer Speech Translation.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | CoVoST2, MuST-C, FLEURS, u.a. |
| Sprachen | Mehrsprachig (Fokus auf ST-Paare) |
| Mandarin enthalten | Ja (en-zh als haeufiges Sprachpaar) |
| Tonale Annotation | Nein |
| Evaluationsmetriken | BLEU (ausschliesslich) |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Fokus auf Speech Translation, nicht Zeichenkonversion |
| RQ2 (Native Speaker) | Mittel | en-zh Speech Translation impliziert Mandarin-Verarbeitung |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Hoch | Vergleich verschiedener SFMs (Whisper, wav2vec, Conformer) als Encoder in ST-Systemen |
| RQ5 (Fehlerstruktur) | Mittel | Diskussion des Informationsverlusts bei kaskadierten Systemen (Prosodie, Ton) |

### Forschungsluecken

**Explizit genannte Luecken:**
- Kein kontrollierter Vergleich verschiedener SFMs innerhalb eines einheitlichen Frameworks.
- Ausschliessliche Verwendung von BLEU als Evaluationsmetrik.
- Fehlende standardisierte Trainingseinstellungen fuer Reproduzierbarkeit.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Nur BLEU berichtet -- keine phonetische Granularitaet |
| Luecke 2 (Toene nicht separat) | Hoch | Prosodie-Verlust in Kaskaden erwaehnt, aber Toene nie separat evaluiert |
| Luecke 3 (Wenig L2-Studien) | Hoch | Keine L2-Sprecherszenarien betrachtet |
| Luecke 4 (Kein systematischer Vergleich) | Sehr hoch | Explizit als Hauptluecke identifiziert |

---

## Paper 33 -- Cui et al. (2025): "Recent Advances in Speech Language Models: A Survey"

**Vollstaendiger Titel:** Recent Advances in Speech Language Models: A Survey
**Autoren:** Wenqian Cui, Dianzhi Yu, Xiaoqi Jiao, Ziqiao Meng, Guangyan Zhang, Qichao Wang, Yiwen Guo, Irwin King
**Jahr:** 2025
**Venue:** arXiv preprint (arXiv:2410.03751v4), IEEE

### Kernaussagen

1. **Tonverlust als fundamentales Problem textbasierter Pipelines:** "Speech signals not only contain semantic information (i.e., the meaning of the speech) but also paralinguistic information (e.g., pitch, timbre, tonality, etc.). Putting a text-only LLM in the middle will cause the complete loss of paralinguistic information in the input speech" (Section I, Information loss). Dies ist das zentrale Argument fuer die Relevanz der Masterarbeit.

2. **Bedeutung von Ton fuer Sprachverstaendnis:** "the meaning of speech can vary depending on tone" (Section I). Diese Aussage validiert direkt die Untersuchung tonaler Transkription bei Mandarin.

3. **SpeechLM-Taxonomie:** Das Paper schlaegt eine dreistufige Architektur vor: Speech Tokenizer, Language Model und Token-to-Speech Synthesizer (Vocoder). Figure 4 zeigt die Klassifikation nach Tokenizer-Typ (semantisch, akustisch, gemischt) und Trainingsrezept.

4. **Drei Problemdimensionen textbasierter Pipelines:** (1) Information loss -- paralinguistische Information geht verloren; (2) Significant latency -- sequentielle ASR+LLM+TTS-Verarbeitung; (3) Cumulative error -- Fehler akkumulieren sich ueber Module.

5. **Umfassende Modell-Uebersicht:** Table II listet populaere SpeechLMs; Table III listet Datensaetze fuer Training und Evaluation.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | LibriSpeech, GigaSpeech, Common Voice, AISHELL, u.a. (Table III) |
| Sprachen | Mehrsprachig |
| Mandarin enthalten | Ja (AISHELL in Table III) |
| Tonale Annotation | Nein |
| Evaluationsmetriken | WER, BLEU, MOS, Speaker Similarity |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Mittel | Diskussion der Limitationen textbasierter Pipelines relevant als Motivation |
| RQ2 (Native Speaker) | Mittel | SpeechLM-Architekturen potenziell fuer Mandarin-ASR nutzbar |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Hoch | Vergleich verschiedener SpeechLM-Architekturen und deren Komponenten |
| RQ5 (Fehlerstruktur) | Sehr hoch | Explizite Diskussion des Tonverlusts und kumulativer Fehler in Pipelines |

### Forschungsluecken

**Explizit genannte Luecken:**
- Vollstaendiger Verlust paralinguistischer Information (inkl. Tonalitaet) bei textbasierten Pipelines.
- Kumulative Fehlerakkumulation ueber ASR-, LLM- und TTS-Module.
- Fehlende einheitliche Evaluationsstandards fuer SpeechLMs.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Evaluationsmetriken erfassen keine tonale Genauigkeit |
| Luecke 2 (Toene nicht separat) | Sehr hoch | "tonality" explizit als verlorengehende Information benannt, aber keine separate Ton-Evaluation vorgeschlagen |
| Luecke 3 (Wenig L2-Studien) | Hoch | Keine Beruecksichtigung von L2-Sprechern |
| Luecke 4 (Kein systematischer Vergleich) | Hoch | Diverse Architekturen ohne einheitliches Evaluationsframework |

---

## Paper 34 -- Arora et al. (2026): "On The Landscape of Spoken Language Models: A Comprehensive Survey"

**Vollstaendiger Titel:** On The Landscape of Spoken Language Models: A Comprehensive Survey
**Autoren:** Siddhant Arora et al.
**Jahr:** 2026
**Venue:** arXiv preprint (arXiv:2504.08528v1)

### Kernaussagen

1. **Standardisierte Evaluation als verbleibendes Problem:** "these models are often evaluated on very different tasks and datasets, making it difficult to assess the relative performance of different approaches" [Zitat nicht verifiziert]. Dies bestaetigt die methodische Luecke 4 der Masterarbeit.

2. **Standardisierung weiterhin ungeloest:** "standardized evaluation is still one of the remaining challenges in this research area" [Zitat nicht verifiziert].

3. **Phonetische Tokens und linguistische Information:** Phonetische Tokens "have been shown to capture linguistic information and have strong correlation with phonetic units" [Zitat nicht verifiziert]. Dies ist relevant fuer die Frage, ob phonetische Tokenisierung tonale Information besser erhalten koennte.

4. **SLM-Typologie:** Table 1 definiert drei Typen: (1) Pure Speech LMs, (2) Speech+Text LMs, (3) Speech-aware Text LMs. Jeder Typ hat unterschiedliche Implikationen fuer die Verarbeitung paralinguistischer Information.

5. **Modality-Adapter-Taxonomie:** Section 3.2 beschreibt verschiedene Adapter-Typen (Length Adapter, Q-Former, CTC Compression, Convolutional Downsampling, Linear Transformation) -- relevant fuer das Verstaendnis, wie Sprach-Features an LLMs uebergeben werden.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | LibriSpeech, FLEURS, Common Voice, u.a. |
| Sprachen | Mehrsprachig |
| Mandarin enthalten | Ja (in mehrsprachigen Benchmarks) |
| Tonale Annotation | Nein |
| Evaluationsmetriken | WER, BLEU, MOS, u.a. |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Survey-Fokus, keine direkten Experimente |
| RQ2 (Native Speaker) | Mittel | Architektur-Uebersicht relevant fuer Modellauswahl |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Hoch | Umfassende Taxonomie verschiedener SLM-Ansaetze |
| RQ5 (Fehlerstruktur) | Mittel | Diskussion phonetischer Tokens und deren Korrelation mit phonetischen Einheiten |

### Forschungsluecken

**Explizit genannte Luecken:**
- Fehlende standardisierte Evaluation ueber verschiedene SLM-Ansaetze hinweg.
- Schwierige Vergleichbarkeit aufgrund unterschiedlicher Tasks und Datensaetze.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Evaluationsmetriken auf WER/BLEU beschraenkt |
| Luecke 2 (Toene nicht separat) | Hoch | Phonetische Tokens erwaehnt, aber keine Ton-spezifische Evaluation |
| Luecke 3 (Wenig L2-Studien) | Hoch | L2-Szenarien nicht betrachtet |
| Luecke 4 (Kein systematischer Vergleich) | Sehr hoch | Explizit als Hauptherausforderung identifiziert |

---

## Paper 35 -- Yang, Chih-Kai et al. (2026): "Towards Holistic Evaluation of Large Audio-Language Models: A Comprehensive Survey"

**Vollstaendiger Titel:** Towards Holistic Evaluation of Large Audio-Language Models: A Comprehensive Survey
**Autoren:** Chih-Kai Yang et al.
**Jahr:** 2026
**Venue:** arXiv preprint (arXiv:2505.15957v4)

### Kernaussagen

1. **Fragmentierte Evaluationslandschaft:** "the evaluation landscape remains fragmented and lacks systematic organization" [Zitat nicht verifiziert]. Dies bestaetigt die Notwendigkeit systematischer Evaluationsansaetze wie in der Masterarbeit.

2. **Signifikante Luecke zur menschlichen Wahrnehmung:** "These evaluations reveal significant gaps between LALMs and human-level perception" [Zitat nicht verifiziert].

3. **Ton als Einflussfaktor:** "tone, emotion, and voice quality can also influence user experience" [Zitat nicht verifiziert]. Die Erwaehnung von "tone" zeigt die Relevanz tonaler Aspekte fuer LALM-Evaluation.

4. **Evaluationstaxonomie:** Das Paper schlaegt 4 Hauptkategorien vor: (1) General Auditory Awareness, (2) Knowledge & Reasoning, (3) Dialogue, (4) Fairness, Safety & Trustworthiness. Tonale Aspekte fallen potenziell unter General Auditory Awareness, werden aber nicht explizit adressiert.

5. **Umfassende Benchmark-Uebersicht:** Die Autoren katalogisieren existierende Benchmarks, zeigen aber, dass keiner spezifisch tonale Sprachverarbeitung evaluiert.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Benchmarks | Diverse LALM-Benchmarks (2024-2026) |
| Sprachen | Ueberwiegend Englisch, einige mehrsprachig |
| Mandarin enthalten | In einigen referenzierten Benchmarks |
| Tonale Annotation | Nein |
| Evaluationsmetriken | WER, BLEU, MOS, Accuracy, u.a. |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Kein Fokus auf Text-Konversion |
| RQ2 (Native Speaker) | Mittel | Evaluationsframework-Diskussion relevant |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Hoch | Systematische Evaluation verschiedener LALMs |
| RQ5 (Fehlerstruktur) | Mittel | "tone" als relevanter Faktor erwaehnt, aber nicht systematisch evaluiert |

### Forschungsluecken

**Explizit genannte Luecken:**
- Fragmentierte, unsystematische Evaluationslandschaft fuer LALMs.
- Signifikante Luecke zwischen LALM-Leistung und menschlicher Wahrnehmung.
- Fehlende ganzheitliche Evaluationsmethodologie.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Bestehende Benchmarks erfassen keine feinkoernigen phonetischen Metriken |
| Luecke 2 (Toene nicht separat) | Sehr hoch | "tone" explizit als relevanter Faktor erwaehnt, aber kein Benchmark evaluiert Toene separat |
| Luecke 3 (Wenig L2-Studien) | Hoch | Keine L2-spezifischen Benchmarks identifiziert |
| Luecke 4 (Kein systematischer Vergleich) | Sehr hoch | Fragmentierung als Kernproblem identifiziert |

---

## Paper 36 -- AI Index Report 2026, Chapter 2: Technical Performance (Stanford HAI)

**Vollstaendiger Titel:** AI Index Report 2026 -- Chapter 2: Technical Performance
**Autoren:** Stanford Institute for Human-Centered Artificial Intelligence (HAI)
**Jahr:** 2026
**Venue:** AI Index Annual Report 2026

### Kernaussagen

1. **AI uebertrifft Benchmarks schneller als diese entwickelt werden:** "AI capability is outpacing the benchmarks designed to measure it, and surpassing human-level performance" [Zitat nicht verifiziert]. Dies motiviert die Entwicklung neuer, spezifischerer Benchmarks wie in der Masterarbeit.

2. **Benchmark-Reliabilitaet in Frage gestellt:** "The benchmarks used to measure AI progress face growing reliability and gaming concerns, with error rates up to 42% on widely used evaluations" [Zitat nicht verifiziert]. Dies unterstreicht die Notwendigkeit sorgfaeltig konstruierter Evaluationen.

3. **Konvergenz der Top-Modelle:** Die Luecke zwischen den besten Modellen schrumpft -- die Top-4-Modelle sind durch weniger als 25 Elo-Punkte getrennt. Dies impliziert, dass die Differenzierung zunehmend auf spezifischen Faehigkeiten (wie tonaler Verarbeitung) basieren muss.

4. **US-China-Leistungsluecke effektiv geschlossen:** Besonders relevant fuer Mandarin-Forschung, da chinesische Modelle nun gleichwertig mit westlichen Modellen sind.

5. **Kein dedizierter Sprach-/Audio-Abschnitt:** Das Kapitel fokussiert auf Sprach-KI (Text), Bild/Video und Reasoning-Benchmarks. Die Abwesenheit eines Audio/Speech-Abschnitts ist selbst eine Forschungsluecke.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Bericht) |
| Referenzierte Benchmarks | MMLU, GPQA, ARC, HumanEval, SWE-bench, Chatbot Arena, u.a. |
| Sprachen | Ueberwiegend Englisch |
| Mandarin enthalten | Nicht spezifisch |
| Tonale Annotation | Nein |
| Evaluationsmetriken | Accuracy, Elo-Rating, Pass@1, u.a. |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Kein Bezug zu Pinyin-Konversion |
| RQ2 (Native Speaker) | Niedrig | Kein Speech-Fokus |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Mittel | Kontextualisierung der Benchmark-Problematik |
| RQ5 (Fehlerstruktur) | Niedrig | Allgemeine Benchmark-Kritik, nicht spezifisch fuer tonale Fehler |

### Forschungsluecken

**Explizit genannte Luecken:**
- Benchmark-Saettigung: AI uebertrifft existierende Benchmarks schneller als neue entwickelt werden.
- Reliabilitaetsprobleme bei gaengigen Evaluationen (Fehlerraten bis 42%).
- Evaluationen halten nicht Schritt mit dem Fortschritt.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Mittel | Allgemeine Benchmark-Kritik uebertragbar auf ASR-Metriken |
| Luecke 2 (Toene nicht separat) | Mittel | Fehlender Audio/Speech-Abschnitt zeigt, dass Speech-Evaluation unterrepraesentiert ist |
| Luecke 3 (Wenig L2-Studien) | Niedrig | Kein direkter Bezug |
| Luecke 4 (Kein systematischer Vergleich) | Hoch | Benchmark-Saettigung und Reliabilitaetsprobleme direkt relevant |

---

## Paper 37 -- Ahlawat et al. (2025): "Automatic Speech Recognition: A survey of deep learning techniques and approaches"

**Vollstaendiger Titel:** Automatic Speech Recognition: A survey of deep learning techniques and approaches
**Autoren:** Harsh Ahlawat, Naveen Aggarwal, Deepti Gupta
**Jahr:** 2025
**Venue:** International Journal of Cognitive Computing in Engineering 6 (2025) 201-237

### Kernaussagen

1. **Umfassende ASR-Modell-Timeline:** Figure 13 zeigt die Entwicklung von Wav2Vec (2019, 34M Parameter) ueber HuBERT (2021, 317M), Whisper-LargeV2 (2022, 1.55B) bis SeamlessM4T LargeV2 (2023, 2.3B Parameter) und Distil-Whisper (2023, 756M).

2. **Mandarin-spezifische Datensaetze:** Table 3 listet Aishell-1 (Open Source, Mandarin, 170h, WER 1.29% (2023)) und Aishell-2 (CC BY-NC-ND, Mandarin, 1000h, WER 2.85% (2023)) -- wichtige Referenzwerte fuer Mandarin-ASR.

3. **Whisper-Leistung:** Table 6 zeigt Whisper mit WER 2.7% auf LibriSpeech-clean und 7.3% auf Multilingual LibriSpeech. Table 8 bestaetigt: "Whisper achieved state-of-the-art results without the need for the self-supervision and self-training techniques and by simply training model on a large and diverse supervised dataset and focusing on zero-shot transfer" [Zitat nicht verifiziert].

4. **Evaluationsmetriken-Taxonomie:** Section 5 listet WER, WRR, PER, CER, TER, FER, MAE, RMSE, BLEU, BERTScore, ROUGE, GLUE -- eine umfassende Uebersicht, die zeigt, dass viele Metriken existieren, aber keine spezifisch tonale Genauigkeit erfasst.

5. **Herausforderungen bei mehrsprachigem ASR:** "The performance of deep learning models for ASR varies significantly across languages and dialects due to factors like training data quality, linguistic diversity, and acoustic variability. High-resource languages like English, Mandarin, and Spanish benefit from abundant labeled data, resulting in a lower WER" [Zitat nicht verifiziert].

6. **DNN-Fortschritt:** "The transition from DNNs to CNNs to RNNs pushed WER down to around 5%-10% on many benchmarks, and current transformers and conformers-based architectures have pushed WERs to below 5%" [Zitat nicht verifiziert].

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | 21+ monolinguale (Table 3) + 21 multilinguale (Table 4) Datensaetze |
| Sprachen | Mehrsprachig (Englisch, Mandarin, Hindi, u.v.a.) |
| Mandarin enthalten | Ja (Aishell-1: 170h, Aishell-2: 1000h) |
| Tonale Annotation | Nein (nur WER/CER berichtet) |
| Evaluationsmetriken | WER, CER, PER, BLEU, BERTScore, u.a. (Section 5) |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Kein Text-zu-Pinyin-Fokus |
| RQ2 (Native Speaker) | Hoch | Mandarin-WER-Referenzwerte (Aishell-1: 1.29%, Aishell-2: 2.85%) |
| RQ3 (L2 Learner) | Niedrig | Akzente erwaehnt, aber kein L2-spezifischer Fokus |
| RQ4 (Vergleich Whisper etc.) | Sehr hoch | Table 6/7/8: Umfassender Vergleich Wav2Vec, HuBERT, Whisper, Conformer, WavLM, SeamlessM4T |
| RQ5 (Fehlerstruktur) | Mittel | Metriken-Taxonomie (PER, CER), aber keine tonale Fehleranalyse |

### Forschungsluecken

**Explizit genannte Luecken:**
- Herausforderungen bei Low-Resource-Sprachen und Dialekten.
- Bedarf an domainspezifischer ASR-Forschung (Gesundheit, Bildung).
- Lexikalische Ambiguitaet bei Homophonen (relevant fuer tonale Sprachen).
- "considerable research is still needed before speech recognition becomes widely accessible and consistently reliable for all users" [Zitat nicht verifiziert].

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Sehr hoch | Trotz umfangreicher Metriken-Taxonomie keine Tone-Accuracy-Metrik |
| Luecke 2 (Toene nicht separat) | Sehr hoch | Mandarin-WER berichtet, aber Toene nie separat evaluiert |
| Luecke 3 (Wenig L2-Studien) | Mittel | Akzent-Variabilitaet erwaehnt, aber kein systematischer L2-Fokus |
| Luecke 4 (Kein systematischer Vergleich) | Hoch | Table 8 bietet Vergleich, aber nicht unter kontrollierten tonalen Bedingungen |

---

## Paper 38 -- Kaur et al. (2021): "Automatic Speech Recognition System for Tonal Languages: State-of-the-Art Survey"

**Vollstaendiger Titel:** Automatic Speech Recognition System for Tonal Languages: State-of-the-Art Survey
**Autoren:** Jaswinder Kaur et al.
**Jahr:** 2021
**Venue:** Archives of Computational Methods in Engineering (2021) 28:1039-1068

### Kernaussagen

1. **Klassifikation tonaler Sprachen:** Table 1 klassifiziert tonale Sprachen nach Kontinent. Mandarin wird als Sino-Tibetisch mit 5 Toenen (High, Rising, Falling-Rising, Falling + neutraler Ton), 7 Vokalen und 22 Konsonanten klassifiziert (Table 2).

2. **Ton-Erkennung ueber akustische Merkmale:** Section 3.2 beschreibt Grundfrequenz (f0), Pitch und Formanten als zentrale akustische Cues fuer die Tonerkennung. Dies ist direkt relevant fuer das Verstaendnis, welche Information LLMs verarbeiten muessten.

3. **MFCC und HMM als dominante Techniken:** "MFCC and HMM are most commonly used feature extraction and classification techniques" [Zitat nicht verifiziert] in der tonalen ASR-Forschung.

4. **Differenzielle Tonerkennung:** "The falling-rising and falling tones of Chinese are easily recognized by the temporal cues than the tone flat tone and rising tone" [Zitat nicht verifiziert]. Dies ist direkt relevant fuer RQ5 der Masterarbeit (Fehlerstruktur bei Tonverwechslungen).

5. **Vokal-Landmark-Detektion:** "Full speech recognition or aligned transcription is not required for recognizing the tone with the help of vowel landmark detection" [Zitat nicht verifiziert]. Ein alternativer Ansatz, der die Komplexitaet der Tonerkennung reduzieren koennte.

6. **Umfangreiche Mandarin-ASR-Literatur:** Section 5.2 behandelt ausfuehrlich Mandarin-spezifische ASR-Arbeit, einschliesslich Silbenerkennung, Tonmodellierung und CER-Ergebnisse verschiedener Systeme.

7. **Zukunftsrichtungen:** "In the future the ASR system for American and Austral-Asia tonal languages will be reviewed as well as the tonal languages of Asia, African and Indo-European that are not reviewed will be studied in the future" [Zitat nicht verifiziert]. Bestaetigt, dass tonale ASR-Forschung weiterhin unterforscht ist.

8. **Tone Accuracy Rate (TAR) und Tone Correct Rate (TCR) als Metriken:** Im Appendix 3 werden TAR und TCR als spezifische Metriken fuer tonale ASR definiert -- eine der wenigen Surveys, die ton-spezifische Metriken erwaehnen.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | Diverse (Table 4: Chinesisch/Mandarin-Datensaetze, Thai-Korpora, Vietnamesisch-Korpora) |
| Sprachen | Tonale Sprachen: Chinesisch, Mandarin, Vietnamesisch, Thai, Mizo, Punjabi, Yoruba, u.a. |
| Mandarin enthalten | Ja (ausfuehrlich in Section 5.2) |
| Tonale Annotation | Ja (Tables 4-6: ton-spezifische Features und Ergebnisse) |
| Evaluationsmetriken | WER, CER, Syllable Accuracy (SA), Tone Accuracy Rate (TAR), Tone Correct Rate (TCR) |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Niedrig | Kein Text-zu-Pinyin-Fokus |
| RQ2 (Native Speaker) | Sehr hoch | Umfangreiche Mandarin-ASR-Ergebnisse mit tonaler Granularitaet |
| RQ3 (L2 Learner) | Niedrig | Kein expliziter L2-Fokus, aber Referenz zu Non-Native-Studien (Ref. 9: Chen et al. 2016) |
| RQ4 (Vergleich Whisper etc.) | Mittel | Pre-DL-Aera-Methoden (HMM, SVM, ANN); kein Vergleich moderner LLMs |
| RQ5 (Fehlerstruktur) | Sehr hoch | Differenzielle Tonerkennung (falling-rising vs. flat), f0-basierte Analyse, Tonverwechslungsmuster |

### Forschungsluecken

**Explizit genannte Luecken:**
- Fehlende Standard-Benchmark-Korpora fuer die meisten tonalen Sprachen.
- "To increase and achieve the high recognition rate variety of speech, tone and emotion embedded speech corpus is very crucial" [Zitat nicht verifiziert].
- Bedarf an hybriden Feature-Extraktions- und Klassifikationsmethoden.
- Korpus-Pflege mit ausreichend Sprechern als zukuenftige Herausforderung.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Mittel | TAR und TCR als ton-spezifische Metriken erwaehnt, aber nicht flaechendeckend verwendet |
| Luecke 2 (Toene nicht separat) | Mittel | Einige Studien evaluieren Toene separat, aber nicht systematisch ueber alle Systeme |
| Luecke 3 (Wenig L2-Studien) | Hoch | Nur eine Referenz zu Non-Native-Mandarin (Chen et al. 2016) |
| Luecke 4 (Kein systematischer Vergleich) | Hoch | Pre-DL-Methoden verglichen, aber keine modernen LLMs/Foundation Models einbezogen |

---

## Paper 39 -- Yang, Zhengdong et al. (2025): "When Large Language Models Meet Speech: A Survey on Integration Approaches"

**Vollstaendiger Titel:** When Large Language Models Meet Speech: A Survey on Integration Approaches
**Autoren:** Zhengdong Yang et al.
**Jahr:** 2025
**Venue:** ACL 2025 Findings, pages 20298-20315

### Kernaussagen

1. **Drei Integrationsansaetze:** Figure 1 zeigt: (a) Text-basiert (ASR als Zwischenschritt), (b) Latent-representation-basiert (kontinuierliche Speech-Embeddings), (c) Audio-token-basiert (diskrete Speech-Tokens). Dies ist die klarste Taxonomie fuer das Verstaendnis der Architekturentscheidungen.

2. **Integrationsgrad-Ranking:** "Degree of integration: Latent-representation-based > Audio-token-based > Text-based" (Section 6.1). Umgekehrt gilt: "Interpretability: Text-based > Audio-token-based > Latent-representation-based" [Zitat nicht verifiziert].

3. **Informationsverlust bei textbasierter Integration:** "transforming speech into text before feeding it into the LLM inevitably introduces a layer of abstraction and potential information loss such as prosody and emotion, which limits the downstream performance" (Section 7.1) [Zitat nicht verifiziert].

4. **Fehlender einheitlicher Vergleich:** "there is a notable gap in comparing the different integration approaches under a unified setting" (Section 7.4) [Zitat nicht verifiziert].

5. **Englisch-Dominanz:** "Most existing speech-LLM models are designed to handle only English, which limits their range of applicability" (Section 7.5) [Zitat nicht verifiziert].

6. **Quantitativer Vergleich:** Table 1 vergleicht Ergebnisse auf ASR (LibriSpeech, FLEURS, AISHELL-2, VoxPopuli), S2TT (CoVoST2) und weiteren Tasks. AISHELL-2 (Mandarin) ist direkt relevant.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Datensaetze | LibriSpeech, FLEURS, AISHELL-2, VoxPopuli, CoVoST2, u.a. (Table 1) |
| Sprachen | Mehrsprachig (Fokus Englisch, aber inkl. Mandarin) |
| Mandarin enthalten | Ja (AISHELL-2 in Table 1) |
| Tonale Annotation | Nein |
| Evaluationsmetriken | WER, CER, BLEU |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Mittel | Text-basierte Integration als Baseline-Ansatz beschrieben |
| RQ2 (Native Speaker) | Hoch | AISHELL-2-Ergebnisse fuer Mandarin in Table 1 |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Sehr hoch | Systematischer Vergleich dreier Integrationsansaetze mit quantitativen Ergebnissen |
| RQ5 (Fehlerstruktur) | Mittel | Informationsverlust (Prosodie, Emotion) bei Text-Integration beschrieben |

### Forschungsluecken

**Explizit genannte Luecken:**
- Kein einheitlicher Vergleich der drei Integrationsansaetze unter kontrollierten Bedingungen (Section 7.4).
- Englisch-Dominanz der meisten Speech-LLM-Modelle (Section 7.5).
- Informationsverlust bei textbasierter Integration (Section 7.1).

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Table 1 berichtet nur WER/CER/BLEU |
| Luecke 2 (Toene nicht separat) | Hoch | Prosodie-Verlust erwaehnt, aber Toene nicht separat evaluiert |
| Luecke 3 (Wenig L2-Studien) | Hoch | Keine L2-Szenarien betrachtet |
| Luecke 4 (Kein systematischer Vergleich) | Sehr hoch | Explizit als "notable gap" in Section 7.4 identifiziert |

---

## Paper 40 -- Luo et al. (2026): "A Survey of Large Audio Language Models: Generalization, Trustworthiness, and Outlook"

**Vollstaendiger Titel:** A Survey of Large Audio Language Models: Generalization, Trustworthiness, and Outlook
**Autoren:** Luo et al.
**Jahr:** 2026
**Venue:** arXiv preprint (arXiv:2605.20266v1)

### Kernaussagen

1. **Textuelle Dominanz als Problem:** Das Paper identifiziert "Textual Dominance" als fundamentales Problem: Modelle verlassen sich staerker auf linguistische Priors als auf akustische Evidenz. Dies ist direkt relevant fuer die Frage, ob LLMs tatsaechlich tonale Information aus Audio extrahieren oder nur textbasierte Muster anwenden.

2. **Akustisch-semantische Kluft:** Section 3.1 beschreibt die "acoustic-semantic gap" bei Halluzinationen und das "Modality Neglect"-Problem -- Modelle greifen auf textuelle Shortcuts zurueck, anstatt akustische Information zu verarbeiten.

3. **Diskrete vs. kontinuierliche Tokenisierung:** "discrete tokenization risks discarding critical acoustic safety cues during compression, continuous manifolds preserve rich paralinguistic nuances but consequently increase the attack possibility for adversarial vulnerabilities" [Zitat nicht verifiziert]. Dies beleuchtet den Trade-off zwischen Informationserhalt und Sicherheit.

4. **Ton als Sicherheitsrelevanter Faktor:** "AudioTrust highlights that LALMs are highly sensitive not only to semantic deception but also to non-semantic acoustic cues, where subtle shifts in tone can trigger safety violations" [Zitat nicht verifiziert]. Die Erwaehnung von "tone" im Sicherheitskontext zeigt die Bedeutung tonaler Information.

5. **Evolutionaerer Pfad der LALMs:** Figure 1 zeigt die Entwicklung von dGSLM (2022) ueber SpeechGPT, Qwen-Audio und Moshi bis zu Modellen von 2026. Table 2 listet ueber 60 LALMs mit ihren Basis-LLMs, Parametern, Sprachen und Trainingsdetails.

6. **Vertrauenswuerdigkeit in 6 Dimensionen:** Section 3 definiert 6 Saeulen: Halluzination, Robustheit, Sicherheit, Privatheit, Fairness und Authentifizierung (Figure 4). Tonale Aspekte sind potenziell in allen Dimensionen relevant.

7. **Umfangreiche Benchmark-Uebersicht:** Table 3 listet ueber 40 LALM-Evaluationsbenchmarks (2024-2026), wobei keiner spezifisch tonale Genauigkeit adressiert.

### Datensatz-Profil

| Merkmal | Wert |
|---|---|
| Eigener Datensatz | Nein (Survey) |
| Referenzierte Benchmarks | 40+ LALM-Benchmarks (Table 3), u.a. AudioBench, AudioTrust, AIR-Bench |
| Sprachen | Mehrsprachig (Table 2: einige Modelle unterstuetzen Chinesisch) |
| Mandarin enthalten | Ja (mehrere LALMs in Table 2 unterstuetzen Chinesisch) |
| Tonale Annotation | Nein |
| Evaluationsmetriken | WER, Accuracy, MOS, diverse benchmark-spezifische Metriken |

### Verwendbarkeit

| RQ | Relevanz | Begruendung |
|---|---|---|
| RQ1 (Text-only Baseline) | Mittel | "Textual Dominance"-Problem relevant fuer Text-only-Ansaetze |
| RQ2 (Native Speaker) | Mittel | Architektur-Uebersicht fuer Modellauswahl nuetzlich |
| RQ3 (L2 Learner) | Keine | Kein L2-Bezug |
| RQ4 (Vergleich Whisper etc.) | Hoch | Table 2 mit 60+ LALMs und deren Eigenschaften; Table 3 mit 40+ Benchmarks |
| RQ5 (Fehlerstruktur) | Hoch | "Textual Dominance", "Modality Neglect", tonale Sensitivitaet in Sicherheitskontext |

### Forschungsluecken

**Explizit genannte Luecken:**
- Textuelle Dominanz: Modelle verlassen sich auf linguistische Priors statt akustischer Evidenz.
- Akustisch-semantische Kluft bei Halluzinationen und Modality Neglect.
- Trade-off zwischen diskreter und kontinuierlicher Tokenisierung bezueglich Informationserhalt.
- Tonale Sensitivitaet bei Sicherheitsevaluationen, aber keine systematische tonale Evaluation.

**Implizite Luecken (Mapping):**

| Luecke | Relevanz | Begruendung |
|---|---|---|
| Luecke 1 (Nur WER/CER) | Hoch | Benchmarks fokussieren auf Gesamt-Metriken, nicht phonetische Granularitaet |
| Luecke 2 (Toene nicht separat) | Sehr hoch | "tone" im Sicherheitskontext erwaehnt, aber keine Benchmark evaluiert tonale Genauigkeit |
| Luecke 3 (Wenig L2-Studien) | Hoch | Fairness-Dimension koennte L2-Sprecher einschliessen, wird aber nicht adressiert |
| Luecke 4 (Kein systematischer Vergleich) | Hoch | Viele Modelle und Benchmarks katalogisiert, aber kein einheitlicher Vergleichsrahmen |

---

## Zusammenfassung: Uebergreifende Muster und Relevanz fuer die Masterarbeit

### Zentrale Befunde ueber alle 10 Survey-Papers hinweg:

1. **Konsens ueber Informationsverlust:** Papers 31, 32, 33, 39 und 40 bestaetigen uebereinstimmend, dass textbasierte Pipelines (ASR+LLM+TTS) paralinguistische Information einschliesslich Tonalitaet verlieren. Paper 33 formuliert dies am praegnantesten: "Putting a text-only LLM in the middle will cause the complete loss of paralinguistic information in the input speech."

2. **Evaluationsdefizit als Kernproblem:** Papers 34, 35, 36 und 39 identifizieren unabhaengig voneinander fehlende standardisierte Evaluation als Hauptherausforderung. Paper 39 benennt dies explizit als "notable gap."

3. **Tonale Aspekte erwaehnt, aber nie systematisch evaluiert:** Alle Papers erwaehnen Ton/Tonalitaet als relevanten Faktor, aber keines evaluiert tonale Genauigkeit systematisch. Dies ist die staerkste Bestaetigung von Luecke 2.

4. **Kein L2-Fokus in Survey-Literatur:** Keines der 10 Papers adressiert L2-Lerner-Szenarien, was Luecke 3 bestaetigt.

5. **Paper 38 als einziger tonaler Spezialist:** Kaur et al. (2021) ist die einzige Survey, die sich spezifisch mit ASR fuer tonale Sprachen befasst, aber keine modernen Foundation Models oder LLMs einbezieht. Die Luecke zwischen traditioneller tonaler ASR und moderner LLM-Forschung ist die zentrale Positionierung der Masterarbeit.

6. **Benchmark-Saettigung trifft Speech:** Paper 36 (AI Index) zeigt, dass AI Benchmarks generell schneller ueberholt als entwickelt werden. Fuer Speech/Audio existiert diese Problematik verschaerft, da dedizierte Evaluationen noch spaerlicher sind.

---

# KATEGORIE F: Zentrale Datensätze (Papers 41-42)

> **Hinweis zur Quellenlage:** Fuer beide Papers liegen keine PDFs im Repository vor. Die Analyse stuetzt sich auf (a) die DeepSeek-Voranalysen in den Upload-Dateien, (b) die originale HTML-Version des SITA-Papers auf arXiv, (c) die MSU-Projektseite und DH2018-Abstract fuer Tone Perfect, sowie (d) Web-Recherche. Zitate, die direkt aus den Originaltexten verifiziert werden konnten, sind mit **[VERIFIZIERT]** markiert. Zitate, die nur aus den DeepSeek-Analysen stammen und nicht unabhaengig geprueft werden konnten, sind mit **[DeepSeek, unverified]** markiert.

---

## Paper 41: Ryu et al. -- "A Tone Perfect Story: How to Develop an Open Access Mandarin Chinese Audio Database as a Collaborative Digital Humanities Project" (2021) -- Relevanz: ★★

**Vollstaendige Referenz:** Catherine Ryu, Benjamin Furman & Devin Higgins (2021). "A Tone Perfect Story: How to Develop an Open Access Mandarin Chinese Audio Database as a Collaborative Digital Humanities Project." *IDEAH -- Interdisciplinary Digital Engagement in Arts & Humanities*, Vol. 2, Iss. 1 (DHSI 2019 & 2020).

**Bedeutung fuer Stefans Arbeit:** Dies ist die Beschreibung des **primaeren Evaluationsdatensatzes** fuer Stefans Masterarbeit. Tone Perfect liefert die 9.840 systematisch strukturierten Audio-Assets, anhand derer die multimodalen LLMs auf ihre Faehigkeit zur phonetischen/tonalen Transkription geprueft werden.

### 1. Kernaussagen (mit Zitaten)

1. **Exhaustiver Silben-Katalog:** Der Datensatz umfasst den vollstaendigen Katalog aller monosilbischen Laute des Mandarin-Chinesischen. Durch Ausschluss von Items ueber Rang 5000 und Abgleich mit dem Lancaster Corpus identifizierte das Team 410 Monosilben. **[VERIFIZIERT, MSU/DH2018]**
   - *"the full catalog of monosyllabic sounds in Mandarin Chinese (410 in total) in all four tones (410 x 4 = 1,640), spoken by six native Mandarin speakers (three female and three male), comprising 9,840 audio files"* **[VERIFIZIERT, MSU-Projektseite]**

2. **Inklusion von Lexical Gaps:** Das Team entschied sich bewusst dafuer, sowohl lexikalische Toene (Laut-Ton-Kombinationen mit semantischer Bedeutung) als auch sogenannte "lexical gaps" (Laut-Ton-Kombinationen ohne semantische Bedeutung) einzubeziehen -- insgesamt 2.382 Lexical-Gap-Samples. **[VERIFIZIERT, Web-Recherche/MSU]**
   - *"Sounds without any meaning are referred to as 'lexical gaps,' and the collection includes spoken examples of these lexical gaps (a total of 2,382 samples)"* **[VERIFIZIERT, Suchergebnis-Snippet]**

3. **Interdisziplinaere Teamstruktur:** Das Forschungsteam bestand aus drei muttersprachlichen chinesischen Forschern (Linguistik, L2-Didaktik, Soziolinguistik) und der Projektleiterin Catherine Ryu als Digital-Humanities-Spezialistin. **[VERIFIZIERT, IDEAH-Snippet]**
   - *"a research team comprised of three native Chinese researchers who were subject specialists in linguistics, second language teaching in Chinese, and sociolinguistics, and the project lead, a digital humanities specialist"* **[VERIFIZIERT, IDEAH-Snippet]**

4. **Multimodales Paedagogisches Design:** Die Datenbank integriert farbcodierte Wellenformen (Ton 1 = gelb, Ton 2 = gruen, Ton 3 = blau, Ton 4 = rot), Pinyin-Romanisierung sowie vereinfachte und traditionelle Schriftzeichen. **[VERIFIZIERT, DH2018-Abstract]**
   - *"To create an optimal digital space of learning for each user with different backgrounds, skill sets, and learning styles..."* **[VERIFIZIERT, DH2018]**

5. **Qualitaetskontrolle:** Die Aufnahmen wurden mit hohem Qualitaetsanspruch erstellt, um Konsistenz ueber alle sechs Sprecher hinweg zu gewaehrleisten. **[VERIFIZIERT, MSU-Interview]**
   - *"We really paid lots of attention to quality control and consistency."* **[VERIFIZIERT, MSU News]**

6. **Open Access als Kernanliegen:** Die Datenbank wurde als frei zugaengliche, herunterladbare Ressource konzipiert, weil kommerzielle Alternativen nicht existierten. **[VERIFIZIERT, MSU News]**
   - *"We would have purchased the audio files for the Picky Bird app if they were available, but nothing like this existed so we had to make them."* **[VERIFIZIERT, MSU News]**

7. **Vielfaeltige Nutzungsmoeglichkeiten:** Der Datensatz dient nicht nur dem Sprachunterricht, sondern auch linguistischen Experimenten, akustischer Analyse und maschinellem Lernen. **[VERIFIZIERT, DH2018]**
   - *"There are many ways for researchers to use this, such as for experiments or to build games."* **[VERIFIZIERT, MSU News]**

### 2. Datensatz-Profil

| Merkmal | Wert |
|---------|------|
| **Datensatz-Name** | Tone Perfect |
| **Typ** | Audio-Datenbank (kein ML-Benchmark) |
| **Sprache** | Standard-Mandarin (Beijing-Natives) |
| **Sprechertyp** | Nur Muttersprachler |
| **Sprecheranzahl** | 6 (3 maennlich, 3 weiblich) |
| **Herkunft Sprecher** | Beijing |
| **Silben** | 410 monosilbische Laute |
| **Toene** | Alle 4 lexikalischen Toene (T1-T4) |
| **Unique Silbe-Ton-Kombinationen** | 1.640 (410 x 4) |
| **Gesamt-Audio-Assets** | 9.840 (1.640 x 6 Sprecher) |
| **Lexical Gaps** | ~2.382 Samples (~24,2% aller Assets) |
| **Aufnahmedauer** | Nicht angegeben |
| **Audio-Format** | Einzelsilben (isoliert) |
| **Lautschrift** | Pinyin + Tonnummer (1-4) |
| **Schriftsysteme** | Vereinfacht + Traditionell |
| **Lizenz** | Open Access (CC-BY 4.0) |
| **Zugang** | https://tone.lib.msu.edu/ |
| **Institution** | Michigan State University Libraries |
| **Erstveroeffentlichung** | Herbst 2017 (Datenbank); 2021 (Paper) |
| **Produktionszeit** | Ca. 1 Jahr |
| **Fehlermetriken** | Keine (reiner Datensatz, keine Evaluation) |
| **LLM-Evaluation** | Keine |

### 3. Verwendbarkeit (-> RQ1-RQ5)

| RQ | Bezug | Konkrete Verwendung |
|----|-------|---------------------|
| **RQ1 (Text->Pinyin)** | Indirekt | Pinyin-Labels als Ground Truth fuer Text-zu-Pinyin-Konversion |
| **RQ2 (Native Speech)** | **DIREKT -- PRIMAERER DATENSATZ** | 9.840 Audio-Assets von 6 Beijing-Natives als Evaluationsgrundlage fuer multimodale LLMs. Systematische Struktur (410 Silben x 4 Toene) ermoeglicht vollstaendige Ton-Fehleranalyse |
| **RQ3 (L2 Speech)** | Kein Bezug | Nur Native-Sprecher enthalten; keine L2-Daten |
| **RQ4 (Vergleich Whisper)** | Indirekt | Gleicher Datensatz fuer Whisper-Baseline und LLM-Evaluation sichert Vergleichbarkeit |
| **RQ5 (Fehlerstruktur)** | **DIREKT** | Systematische Silbe-Ton-Struktur erlaubt praezise Konfusionsmatrizen (z.B. T2<->T3); Lexical Gaps testen Generalisierung auf bedeutungslose Lautkombinationen |

**Zentrale Staerke fuer Stefan:** Die vollstaendige Abdeckung aller 410 Monosilben in allen 4 Toenen ermoeglicht eine **systematische, lueckenlose Evaluation** der Tongenauigkeit -- im Gegensatz zu typischen ASR-Benchmarks (AISHELL etc.), die nur CER/WER auf Satzebene messen und keine tonale Analyse erlauben.

### 4. Forschungsluecken

#### Explizit (aus dem Paper/Projekt)

1. **Kein vergleichbarer Datensatz existierte:** Der gesamte Impuls fuer das Projekt entstand aus der Nicht-Verfuegbarkeit systematischer Mandarin-Audio-Ressourcen.
   - *"We would have purchased the audio files [...] if they were available, but nothing like this existed so we had to make them."* **[VERIFIZIERT, MSU News]**

#### Implizit (-> Luecke 1-4)

| Luecke | Bezug | Erklaerung |
|--------|-------|------------|
| **Luecke 1 (LLMs nur WER/CER)** | Stark | Tone Perfect wurde vor der LLM-Aera erstellt (2017); keine LLM-Evaluation vorgesehen oder durchgefuehrt |
| **Luecke 2 (Toene nicht separat)** | Stark | Der Datensatz LIEFERT die Ton-Labels, aber es existiert keine systematische Evaluation, die diese nutzt |
| **Luecke 3 (Wenig L2)** | Stark | **Nur Native-Sprecher** -- keine L2-Lernenden im Datensatz; Stefans L2-Erweiterung ist notwendig |
| **Luecke 4 (Kein systematischer Vergleich)** | Stark | Kein Paper hat Tone Perfect fuer einen systematischen LLM-vs-ASR-Vergleich genutzt |

---

## Paper 42: Xu et al. -- "SITA: Learning Speaker-Invariant and Tone-Aware Speech Representations for Low-Resource Tonal Languages" (2026) -- Relevanz: ★★

**Vollstaendige Referenz:** Tianyi Xu, Xuan Ouyang, Binwei Yao, Shoua Xiong, Sara Misurelli, Maichou Lor & Junjie Hu (2026). "SITA: Learning Speaker-Invariant and Tone-Aware Speech Representations for Low-Resource Tonal Languages." *arXiv:2601.09050v1*, 14. Januar 2026. University of Wisconsin--Madison.

**Bedeutung fuer Stefans Arbeit:** SITA dokumentiert den **"Tone Collapse"**-Befund bei Whisper -- die systematische Ueberlappung tonaler Repraesentationen -- und liefert damit ein zentrales Argument fuer RQ4 (Warum dedizierte ASR-Systeme bei Tonerkennung versagen und wie sich LLMs im Vergleich verhalten).

### 1. Kernaussagen (mit Zitaten)

1. **Definition von Tone Collapse:** Standard-Speech-Encoder wie Whisper kollabieren die Repraesentationen verschiedener Toene desselben Basisworts, sodass tonale Unterschiede im Embedding-Raum verloren gehen. **[VERIFIZIERT, arXiv HTML]**
   - *"Different tones of same base word become overly similar"* **[VERIFIZIERT, arXiv HTML, Section 1]**
   - *"Whisper embeddings largely collapse into overlapping clusters"* vs. *"SITA produces a more tone-stratified geometry"* **[VERIFIZIERT, arXiv HTML, Section 5.1/Figure 4]**

2. **Problem der Speaker-Invarianz vs. Ton-Bewusstsein:** Das zentrale Problem ist, dass Repraesentationen robust gegenueber Sprechervarianz (z.B. Geschlecht) sein muessen, waehrend sie gleichzeitig tonale Unterschiede bewahren. **[VERIFIZIERT, arXiv Abstract]**
   - *"Tonal low-resource languages are widely spoken yet remain underserved by modern speech technology. A key challenge is learning representations that are robust to nuisance variation such as gender while remaining tone-aware for different lexical meanings."* **[VERIFIZIERT, arXiv Abstract]**

3. **SITA-Methode (Staged Multi-Objective Training):** Das System nutzt ein zweistufiges Training: (i) Cross-Gender-Kontrastlernen mit InfoNCE-Loss und Tone-Repulsive-Loss; (ii) CTC-basiertes ASR mit Knowledge Distillation. **[VERIFIZIERT, arXiv HTML]**
   - *"a cross-gender contrastive objective encourages lexical consistency across speakers, while a tone-repulsive loss prevents tone collapse by explicitly separating same-word different-tone realizations"* **[VERIFIZIERT, arXiv Abstract]**

4. **Tone-Repulsive Loss:** Hart-negative Paare (gleiches Wort, unterschiedlicher Ton) werden als explizite Negativbeispiele genutzt, ergaenzt durch einen linearen Ton-Klassifikator mit Cross-Entropy-Loss. **[VERIFIZIERT, arXiv HTML, Section 3.2]**
   - *"Hard negatives H(i) with same word but different tones"* kombiniert mit *"a tone classifier to steer the embedding toward tone-discriminative structure"* **[VERIFIZIERT, arXiv HTML]**

5. **Near-Ceiling Mandarin-Performance:** Auf dem Tone-Perfect-Datensatz erreicht SITA nahezu perfekte Cross-Gender-Retrieval-Ergebnisse. **[VERIFIZIERT, arXiv HTML, Section 5.4]**
   - *"SITA achieves near-ceiling...Top-1 accuracy of ~0.99"* **[VERIFIZIERT, arXiv HTML, Section 5.4]**

6. **Dramatische Verbesserung der Ton-Separation:** Die Cosinus-Distanz zwischen Hart-Negativen (gleiche Silbe, verschiedener Ton) steigt von 0,01-0,08 bei der Baseline auf 0,675 bei SITA. **[VERIFIZIERT, arXiv HTML, Section 5.1]**
   - *"hard-negative cosine distance from about 0.01-0.08 to 0.675 while preserving within-tone similarity at 0.80"* **[VERIFIZIERT, arXiv HTML]**

7. **Trade-off zwischen Ton-Separation und ASR-Genauigkeit:** Das Paper identifiziert einen inhaerentenZielkonflikt -- bessere tonale Unterscheidung kann die allgemeine ASR-Performance reduzieren. **[VERIFIZIERT, arXiv HTML, Limitations]**
   - *"SITA exposes inherent trade-off between tone separation and ASR accuracy"* **[VERIFIZIERT, arXiv HTML, Limitations]**

8. **Whisper zeigt Ton-Kollaps:** Visuell (PCA) und metrisch belegt -- Whisper-Embeddings bilden ueberlappende Cluster, waehrend SITA ton-stratifizierte Geometrien erzeugt. **[VERIFIZIERT, arXiv HTML]**
   - *"Whisper shows tone collapse (within-word tones overlap)"* **[VERIFIZIERT, arXiv HTML, Figure 4]**

### 2. Datensatz-Profil

| Merkmal | Wert |
|---------|------|
| **Primaerer Datensatz** | Hmong WRT Corpus |
| **Hmong -- Woerter** | 8.570 |
| **Hmong -- Sprecher** | 8 (3 weiblich, 5 maennlich) |
| **Hmong -- Unique Words** | 1.143 |
| **Hmong -- Base Words** | 163 |
| **Hmong -- Toene** | 7 lexikalische Toene |
| **Hmong -- Dauer** | ~1h 41m 24s |
| **Hmong -- Augmentiert** | 12.170 total (inkl. 3.600 synthetische via Voice Conversion) |
| **Transfer-Datensatz** | Tone Perfect (Mandarin) |
| **Mandarin -- Tokens** | 9.840 |
| **Mandarin -- Sprecher** | 6 |
| **Mandarin -- Silben** | 410 |
| **Mandarin -- Toene** | 4 |
| **Fehlermetriken** | Top-1/Top-5 Retrieval, PosSim, NegDist, CER, WER, Tone Classification Accuracy |
| **Modelle** | XLS-R, Whisper Large-v3 (Speech-Encoder, keine LLMs) |
| **Audio-Pipeline** | Direkt (wav2vec2-Encoder nimmt Audio direkt) |
| **Lizenz** | CC BY 4.0 |

### Zentrale Ergebnisse (Tabellen)

**Cross-Gender Retrieval auf Mandarin/Tone Perfect (M->F):**

| Modell | Top-1 | Top-5 |
|--------|-------|-------|
| XLS-R (Baseline) | 0.2457 | 0.4768 |
| ASR Adaptation | 0.9622 | 0.9957 |
| Whisper Large-v3 | 0.8476 | 0.9738 |
| **SITA** | **0.9927** | **0.9994** |

**Cross-Gender Retrieval auf Hmong:**

| Richtung | SITA Top-1 | SITA Top-5 |
|----------|-----------|-----------|
| F->M | 0.6286 | 0.9286 |
| M->F | 0.5929 | 0.8893 |

**Hmong ASR:**

| Modell | CER | WER |
|--------|-----|-----|
| XLS-R ASR Baseline | 0.1835 | 0.4610 |
| SITA Stage 2 | 0.1985 | 0.5115 |

### 3. Verwendbarkeit (-> RQ1-RQ5)

| RQ | Bezug | Konkrete Verwendung |
|----|-------|---------------------|
| **RQ1 (Text->Pinyin)** | Kein Bezug | SITA arbeitet auf Audio-Embedding-Ebene, nicht mit Pinyin |
| **RQ2 (Native Speech)** | Indirekt | Tone-Geometry-Metriken (PosSim, NegDist) als zusaetzliche Evaluationsdimension fuer Stefans Modelle |
| **RQ3 (L2 Speech)** | Kein Bezug | Nur Muttersprachler in beiden Datensaetzen |
| **RQ4 (Vergleich Whisper)** | **DIREKT -- ZENTRALER BEFUND** | "Tone Collapse" bei Whisper ist das Kernargument: Wenn selbst Whisper Large-v3 tonale Repraesentationen kollabieren laesst, wie verhalten sich dann multimodale LLMs? Stefan testet genau diese Frage |
| **RQ5 (Fehlerstruktur)** | **DIREKT** | Ton-Konfusionsanalyse via Cross-Gender-Retrieval und Hard-Negative-Distanz als methodisches Vorbild fuer Stefans Fehlerstruktur-Analyse |

**Zentrale Staerke fuer Stefan:** Der "Tone Collapse"-Befund bei Whisper ist ein **empirisch belegtes Argument**, warum Standard-ASR-Metriken (CER/WER) fuer tonale Sprachen unzureichend sind. Wenn Whisper-Embeddings tonale Unterschiede nicht repraesentieren, koennen CER/WER-basierte Evaluationen das Problem nicht sichtbar machen -- genau die Luecke, die Stefan mit seiner separaten Ton-Evaluation schliesst.

### 4. Forschungsluecken

#### Explizit (mit Zitaten)

1. **Limitierte Evaluation auf kuratiertem Wort-Level-Korpus:** Die Autoren raeuumen ein, dass ihre Ergebnisse auf kontrollierten Aufnahmen basieren und moeglicherweise nicht auf spontane Sprache uebertragbar sind. **[VERIFIZIERT, arXiv HTML]**
   - *"Evaluation conducted on curated word-level corpus with limited coverage of speakers, recording conditions, and speaking styles"* **[VERIFIZIERT, arXiv HTML, Limitations]**
   - *"Retrieval and tone-geometry metrics may not fully reflect performance on spontaneous, conversational speech"* **[VERIFIZIERT, arXiv HTML, Limitations]**

2. **Inhaerrenter Trade-off Ton-Separation vs. ASR:** Bessere tonale Unterscheidung fuehrt zu leicht verschlechterter Worterkennungsrate. **[VERIFIZIERT, arXiv HTML]**
   - *"SITA exposes inherent trade-off between tone separation and ASR accuracy"* **[VERIFIZIERT, arXiv HTML, Limitations]**

#### Implizit (-> Luecke 1-4)

| Luecke | Bezug | Erklaerung |
|--------|-------|------------|
| **Luecke 1 (LLMs nur WER/CER)** | Stark | SITA evaluiert nur wav2vec/Whisper-Encoder -- **keine multimodalen LLMs** getestet; Tone-Geometry-Metriken wurden nie auf GPT-4o, Gemini etc. angewandt |
| **Luecke 2 (Toene nicht separat)** | Adressiert | SITA fuehrt erstmals Tone-Geometry-Metriken (PosSim, NegDist, Hard-Negative-Distanz) ein, die tonale Separation explizit messen -- aber nur auf Embedding-Ebene, nicht als Transkriptionsgenauigkeit |
| **Luecke 3 (Wenig L2)** | Stark | **Nur Muttersprachler** in beiden Datensaetzen; keine L2-Evaluation |
| **Luecke 4 (Kein systematischer Vergleich)** | Teilweise | Vergleicht Whisper vs. XLS-R vs. SITA, aber **kein Vergleich mit multimodalen LLMs**; Stefans Arbeit erweitert diesen Vergleich auf LLMs |

---

## Querverbindungen zwischen beiden Papers und Stefans Arbeit

### Synergie Tone Perfect + SITA

Die beiden Papers ergaenzen sich ideal fuer Stefans Forschungsdesign:

1. **Tone Perfect** liefert den **systematischen Datensatz** (410 Silben x 4 Toene x 6 Sprecher = 9.840 Assets), der eine lueckenlose tonale Evaluation ermoeglicht.

2. **SITA** liefert den **empirischen Befund** (Tone Collapse bei Whisper), der die Forschungsfrage motiviert: Wenn selbst dedizierte Speech-Encoder tonale Informationen verlieren, wie verhalten sich dann multimodale LLMs, die Audio als sekundaere Modalitaet verarbeiten?

3. **Stefans Beitrag** schliesst die Luecke, die BEIDE Papers offen lassen: Weder Ryu et al. noch Xu et al. haben multimodale LLMs auf ihre Faehigkeit zur tonalen Transkription getestet. Stefan ist der Erste, der den Tone-Perfect-Datensatz mit LLM-Evaluation verbindet.

### Zentrale Zitate fuer die Thesis

| # | Zitat | Quelle | Verwendung | Verifiziert? |
|---|-------|--------|------------|--------------|
| 1 | *"nothing like this existed so we had to make them"* | Ryu/MSU News | Motivation: Datensatz-Luecke | Ja |
| 2 | *"Tonal low-resource languages are widely spoken yet remain underserved by modern speech technology"* | Xu et al. Abstract | Einleitung: Problemstellung | Ja |
| 3 | *"Whisper embeddings largely collapse into overlapping clusters"* | Xu et al. Sec. 5.1 | RQ4: Whisper-Limitierung | Ja |
| 4 | *"tone-repulsive loss prevents tone collapse by explicitly separating same-word different-tone realizations"* | Xu et al. Abstract | Methodik-Kontext | Ja |
| 5 | *"SITA achieves near-ceiling...Top-1 accuracy of ~0.99"* auf Tone Perfect | Xu et al. Sec. 5.4 | RQ4: SITA als obere Referenz | Ja |
| 6 | *"inherent trade-off between tone separation and ASR accuracy"* | Xu et al. Limitations | Diskussion: Designentscheidungen | Ja |
| 7 | *"Whisper shows tone collapse -- within-word tones overlap"* | Xu et al. Fig. 4 | RQ4/RQ5: Kernargument | Ja (DeepSeek-Tabelle) |
| 8 | *"Evaluation conducted on curated word-level corpus with limited coverage"* | Xu et al. Limitations | Limitationen-Diskussion | Ja |

---

# ZUSAMMENFASSUNGSTABELLEN

## Tabelle 1: Hauptübersicht — 42 Paper mit Tims 4 Fragen

| # | Paper (Jahr) | Kategorie | Relevanz | F1 — Kernaussage | F2 — Konkret verwendbar | F3 — Explizite Lücke | F4 — Implizite Lücke (→ Stefans Arbeit) |
|---|---|---|---|---|---|---|---|
| 1 | Seed-ASR (2024) | Speech LLM | ☆ | AcLLM-Framework; CER 2,98% Mandarin; 20M+ h SSL | SOTA-Baseline für Mandarin-CER (RQ4) | *"classic end-to-end models [...] are gradually approaching a bottleneck"* | Keine Ton-/Phonem-Evaluation; kein Pinyin; keine L2 |
| 2 | FireRedASR (2025) | Speech LLM | ☆ | 70k h professionell transkribiert; CER 3,05% | LoRA-Finetuning; *"one thousand hours of high-quality [...] data yields better results than ten thousand hours"* | *"suboptimal performance for specific languages like Mandarin"* bei multilingualen Modellen | Keine phonetische/tonale Metrik; kein Pinyin |
| 3 | FireRedASR2S (2026) | Speech LLM | ☆ | All-in-One: VAD+LID+ASR+Punc; CER 2,89%; 200k h | Hierarchisches LID-Design; Confidence-Scores | *"inconsistent interfaces, limited reproducibility"* bei Pipeline-Systemen | Kein Pinyin-Output; keine Ton-Evaluation |
| 4 | Kimi-Audio (2025) | Speech LLM | ★ | **Erwähnt "tone" als vernachlässigt!** 13M h Pre-Training; CER 0,60% AISHELL-1 | *"tone"*-Zitat direkt für Motivation verwendbar | *"neglecting [...] paralanguage information (e.g., emotion, style, timbre, tone)"* | "Tone" nur paralinguistisch kategorisiert; keine Ton-Evaluation |
| 5 | Qwen3-ASR (2026) | Speech LLM | ★ | Erstes LLM-Forced-Alignment; 52 Sprachen; 40M h pseudo | ForcedAligner für phonetisches Alignment (RQ2) | *"ASR models [...] have reached the limit of annotation errors"* | Alignment nur Wort/Zeichen, nicht Phonem/Ton |
| 6 | Step-Audio 2 (2025) | Speech LLM | ☆ | End-to-End multimodal; 8M h; CER 3,08% | RL für Audio-Wahrnehmung; Paralinguistik-Evaluation | *"neglecting the para-linguistic information"* | "Tone" fehlt in Paralinguistik-Kategorien |
| 7 | Peng Survey (2025) | Survey/LLM | ★ | Taxonomie Speech Understanding; Instruction Sensitivity & Semantic Degradation | Taxonomie-Framework; *"fine-grained auditory understanding"*-Lücke | *"extracting richer and finer acoustic information [...] remains limited"* | Töne weder in Taxonomie noch Evaluation |
| 8 | Du et al. (2025) | Mandarin Töne | ★ | Zipformer: WER 2,12%; 66 Phoneme; 61,1M Params | Phonem-Konfusionsanalyse; Substitutionsfehler-Muster (RQ5) | *"scarcity of Chinese reading evaluation datasets"* | Keine separate Ton-Analyse; kein LLM |
| 9 | Zou et al. (2025) | Mandarin Töne | ★ | Review 61 Studien: DL 88,8% vs. Trad. 83,1%; T3 schwierigster Ton | TER-Metrik; Best Practices; *"multimodal learning"*-Empfehlung | *"L2 corpora [...] receive less attention"*; *"emotional or expressive speech [...] largely absent"* | Kein LLM-Vergleich; keine Pinyin-Brücke |
| 10 | Bu et al. (2025) | Mandarin Töne | ★ | ResNet-18-Siamese; MSE 0,189; 145h L2-Daten (40 tibetische Lerner) | Siamese-Ansatz für Ton-Distanz; L2-Daten | *"notable absence of [...] automatic annotation"* für Tonfehler | Nur isolierte Silben; kein LLM |
| 11 | Li PY-GEC (2024) | Pinyin/ASR | ★ | LLaMA-3-8B + Pinyin: CER -8,3%; Cosine Sim 0,26→0,82 | Pinyin-Feature-Integration; Attention-Analyse | *"no direct connection between pronunciation and written form"* | Nur Text-Pinyin, kein Audio→Pinyin; keine Ton-Eval |
| 12 | Liang PERL (2025) | Pinyin/ASR | ★ | Pinyin Enhanced Rephrasing: CER -29,11%; 3,09ms Latenz | Pinyin-Encoder (GRU+Transformer) | *"not effectively utilized Pinyin information"* | Nur ASR-Korrektur; keine direkte Transkription |
| 13 | Zhengjie PYG-ASR (2025) | Pinyin/ASR | ★★ | **Zentralstes Paper:** HuBERT+Qwen2-7B → Pinyin+Text; CER -25% | LLM-basierte Pinyin-Generierung; Pinyin ERR 1,9% | *"little exploration of how [...] LLM-ASR [...] can directly generate Pinyin"* | **Töne fehlen im Pinyin-Output komplett** |
| 14 | Chen SCCM (2025) | Pinyin/ASR | ★ | Multi-Task mit Pinyin-Ensemble: CER -45,7%; Pinyin-CER 2,41% | Syllable-Character-Collaborative-Ansatz | *"complex mapping relationship between Chinese speech and text"* | Töne fehlen im Pinyin-Output |
| 15 | Wang MDD (2024) | Pronunciation | ★ | Pitch-Aware RNN-T: PER -3%; Pitch Fusion Block + HuBERT | Tonal-Phoneme-Set (5 Töne); PER/DER-Metriken | *"scarcity of [...] annotated non-native speech data"* | Keine multimodalen LLMs; nur 4 L2-Sprecher |
| 16 | Wu L2 Prosody (2024) | Pronunciation | ★ | API-Tonerkennung ~80% DA; Human-Korrelation 0,31-0,60 | FRR/FAR/DA-Metriken; Ton-Fehlermuster pro Ton | *"applicability to L2 classrooms remains questionable"* | Keine multimodalen LLMs; nur 12 L2-Sprecher |
| 17 | Huang Review (2025) | Pronunciation | ★★ | 4-Strang-Modell; T2↔T3 persistent; P↔P Framework | Klassifikation Ton-Fehlertypen; XAI-Attribution | *"limited interpretability"* bei Feedbacksystemen | Keine LLM-Integration; keine multimodale Transkription |
| 18 | Zhu ZIPA (2025) | Mandarin Töne | ★ | 88 Sprachen; IPAPACK++ 17.132h; Chao Tone Letters | PFER-Metrik; multilinguale IPA-Repräsentation | Soziophonetische Variation wird geglättet | Keine Mandarin-spezifische Ton-Eval; keine L2 |
| 19 | Kang & Xu (2024) | Chinese Spec. | ☆ | CVT-Synchrony BF=252; 125ms Leftward-Shift | Timing-Fehler als Metrik; F0-Divergenz (GAMM) | Ton-Onset-Timing war unklar | Nur Grundlagenforschung; keine ASR/LLM |
| 20 | Wang MSPB (2025) | Chinese Spec. | ★★ | **Speech LLMs scheitern an Prosodie;** GPT-4o nur 59,7% | MSPB-Framework als Methodik-Vorbild; 8 Aufgaben | *"current speech LLMs remain limited in Mandarin speech prosody comprehension"* | Testet Satzprosodie, nicht lexikalische Töne |
| 21 | Chirkova (2025) | Chinese Spec. | ★ | IPA > Pinyin für Ton-ASR; tonal CER als Metrik | IPA vs. Pinyin Vergleich; tonal CER/WER | *"complex tones remain the most difficult part"* | Nur Baima; keine multimodalen LLMs |
| 22 | Wei ASR-EC (2024) | Benchmark | ☆ | Erster Chinese ASR-EC-Benchmark; multimodal > prompting | Multimodale Korrektur; 544K Trainingsutteranzen | *"prompting is not effective for ASR error correction"* | Keine Ton-/Phonem-Ebene; nur CER |
| 23 | Wang ContextASR (2025) | Benchmark | ☆ | 40K Entries, 300K NE; LALMs > konventionelle ASR | NE-WER/NE-FNR-Metriken; 10+ Domänen | *"existing ASR benchmarks fail to unveil [...] improvements"* | Keine Mandarin-Ton-Evaluation |
| 24 | Liu VocalBench-zh (2025) | Benchmark | ☆ | Mandarin S2S Benchmark; 11.115 Instanzen; 14 Modelle | Mandarin-spezifisches Evaluations-Framework | *"scarcity of [...] Mandarin benchmarks"* für S2S | Testet Konversation, nicht phonetische Genauigkeit |
| 25 | Li TELEVAL (2026) | Benchmark | ★ | Dynamic Chinese SLM Benchmark; Content + Interactional | Mandarin-spezifische SLM-Evaluation; 300+ Setups | *"models [...] struggle to [...] incorporate paralinguistic cues"* | Keine Ton-Transkription |
| 26 | Sakshi MMAU (2024) | Benchmark | — | 10K Audio Clips, 27 Skills; Gemini nur 52,97% | Multi-Task-Audio-Framework; Phonological Tasks | *"notable lack of comprehensive evaluation benchmarks"* | English-only; keine Ton-Tasks |
| 27 | Huang Dyn-SUPERB (2025) | Benchmark | ★ | 180 Tasks; Phoneme Recognition Domain enthalten | Phoneme Recognition Domain als taxonomischer Anker | *"absence of a comprehensive evaluation benchmark"* | Kein Mandarin-Ton-Task unter 180 Tasks |
| 28 | Cui VoxEval (2025) | Benchmark | — | Speech QA-Benchmark; 13.938 QA-Paare; 26 IAC | Speech-only QA; Robustheit über Audio-Konditionen | *"fail to support end-to-end speech evaluations"* | English-only; keine phonetische Eval |
| 29 | Wang AudioBench (2025) | Benchmark | — | 8 Tasks, 26 Datasets; Multi-Instruction-Robustness | Model-as-Judge (LLaMA-3-70B); ASR-Baselines | *"lacks a comprehensive benchmark for AudioLLMs"* | English-only; keine Ton-Aufgaben |
| 30 | Zhang WildSpeech (2025) | Benchmark | ☆ | Real-world SpeechLLM; PF-Kategorie mit Tone-Subcategory | Query-aware Eval; Near-Homophone-Tests | *"existing benchmarks [...] overlook speech's unique characteristics"* | Nur Englisch; "Tone" = emotional, nicht lexikalisch |
| 31 | Latif et al. (2023) | Survey | ☆ | Erster LAM-Survey; SeamlessM4T; paralinguistische Defizite | Whisper/MMS/SeamlessM4T-Vergleich auf FLEURS | Paralinguistische Information als Schwäche identifiziert | Keine tonalen Sprachen; kein Mandarin-Fokus |
| 32 | Gaido et al. (2024) | Survey | — | 5 Bausteine SFM+LLM; kaskadierte Systeme verlieren Prosodie | Architektur-Vergleich; COMET-Metrik | *"no work has addressed the comparative assessment [...] under controlled conditions"* | Nur Speech Translation; kein Mandarin |
| 33 | Cui SpeechLM (2025) | Survey | ☆ | Pipeline vs. End-to-End; SpeechGPT, AudioPaLM etc. | Architektur-Taxonomie; Training Recipes | Pipeline verliert Information; E2E als Alternative | Keine tonalen Sprachen |
| 34 | Arora et al. (2026) | Survey | ★ | Umfassendster SLM-Survey (40 S.); Evaluations-Chaos | SLM-Taxonomie für Modellvergleich | Kein einheitliches Evaluationsframework | **"Mandarin" oder "tone" kommen praktisch nicht vor** |
| 35 | Luo et al. (2026) | Survey | ★ | Trustworthiness 6-Pillar: Halluzination, Robustheit, Safety | Defense-in-Depth; Robustheits-Framework | Mangel an Trustworthiness-Frameworks für Audio | "Tone" nur paralinguistisch erwähnt |
| 36 | AI Index (2026) | Survey | — | Benchmark-Scores 2025; SOTA-Überblick | State-of-the-Art-Referenz | Benchmarks teilweise überholt | Keine Audio-LLM-/Ton-Analyse |
| 37 | Ahlawat (2025) | Survey | — | Deep-Learning-ASR seit 2010; Transfer Learning | Umfassende ASR-Methodenübersicht | Datenabhängigkeit; Generalisierung | Keine tonalen Sprachen; keine LLM-ASR |
| 38 | Kaur et al. (2021) | Survey | ★★ | **Wichtigster Survey:** Systematisch tonale ASR; L2-Daten 53-73% | L2-Mandarin-Ton-Daten; Tone Cls Accuracy pro Sprache | Wenig Arbeit zu afrikanischen Tonsprachen | Endet 2019; kennt keine LLMs |
| 39 | Yang LLM+Speech (2025) | Survey | ☆ | 3-Wege-Integration Speech+LLM | Integrationstaxonomie | Kein einheitliches Integrationsframework | Keine Mandarin/Ton-Diskussion |
| 40 | Yang Eval (2026) | Survey | ★ | **Erster LALM-Eval-Survey:** 4-Dimensionen; >50 Benchmarks | LALM-Evaluationstaxonomie für RQ4 | Evaluations-Benchmarks fragmentiert | **Kein Benchmark misst TER oder Pinyin Accuracy** |
| 41 | Ryu Tone Perfect (2021) | Dataset | ★★ | 410 Silben × 4 Töne × 6 Sprecher = 9.840 Assets; CC-BY | **Primärer Evaluationsdatensatz für RQ2** | *"nothing like this existed so we had to make them"* | Nur Native; keine L2; keine LLM-Evaluation |
| 42 | Xu SITA (2026) | Method | ★★ | **Whisper zeigt "tone collapse"**; SITA 99,27% Top-1 | "Tone Collapse"-Befund für RQ4/RQ5 | *"tone representations collapse in standard models"* | Nur wav2vec/Whisper; keine multimodalen LLMs |

---

## Tabelle 2: Datensatz-Vergleich

| # | Paper | ★ | Datensatz | Sprache | Sprecher | Stunden | Metriken | Töne? | Lautschrift | LLM? | Audio→LLM? |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Seed-ASR | ☆ | Intern (7,7M h SSL) | Mandarin+13 Dialekte | Native | 20M+ h | CER | ❌ | keine | MoE 10B+ | Encoder→LLM |
| 2 | FireRedASR | ☆ | 70k h proprietär | Mandarin+EN | Native | 70k h | CER | ❌ | keine | Qwen2-7B | Encoder→LLM |
| 3 | FireRedASR2S | ☆ | 200k h proprietär | Mandarin+20 Dialekte | Native | 200k h | CER | ❌ | keine | Qwen2-7B | Encoder→LLM |
| 4 | Kimi-Audio | ★ | 13M h Pre-train | EN+Mandarin | Native | 13M h | WER/CER | ⚠️ erwähnt | keine | Qwen2.5-7B | Hybrid→LLM |
| 5 | Qwen3-ASR | ★ | 40M h pseudo | 30 Sprachen | Native+L2 | 40M h | CER/WER | ❌ | keine | Qwen3-1.7B | Encoder→LLM |
| 6 | Step-Audio 2 | ☆ | 8M h + 680B Tokens | Multilingual | ~50k Spr. | 8M h | CER/WER | ❌ | keine | Custom 130B | Direkt |
| 7 | Peng Survey | ★ | Review diverser | Multilingual | — | — | WER/CER | ❌ | — | Diverse | Review |
| 8 | Du et al. | ★ | AISHELL1-PHONEME | Mandarin | Native, 400 | 165h | WER | ⚠️ implizit | 66 Phoneme | Nein | Direkt |
| 9 | Zou Review | ★ | Review 61 Studien | Mandarin | Native+L2 | — | Acc/TER | ✅ Fokus | — | Nein | Review |
| 10 | Bu et al. | ★ | THCHS-30 + eigenes L2 | Mandarin | Native+L2 (40) | 145h | MSE/RMSE | ✅ Kern | F0-Features | Nein | Direkt |
| 11 | Li PY-GEC | ★ | AISHELL-1, Common Voice | Mandarin | Native | 178h | CER | ❌ | **Pinyin+Ton** | LLaMA-3-8B | ASR→Text→LLM |
| 12 | Liang PERL | ★ | CHP/AISHELL-1, DoAD | Mandarin | Native | — | CER | ❌ | Pinyin Emb. | GPT-4o Baseline | ASR→Text→LLM |
| 13 | Zhengjie PYG | ★★ | AISHELL-1 | Mandarin | Native, 400 | 170h | CER, **Pinyin ERR** | ⚠️ ohne Ton# | **Pinyin** | Qwen2-7B | **HuBERT→LLM** |
| 14 | Chen SCCM | ★ | AISHELL-1 | Mandarin | Native, 400 | 170h | CER, **Pinyin CER** | ⚠️ ohne Ton# | **Pinyin** | Nein | Direkt |
| 15 | Wang MDD | ★ | AISHELL-1 + LATIC | Mandarin | Native+L2 (4) | 165+4h | PER/FRR/FAR | ✅ T1-T5 | Tonal Phoneme | Nein (HuBERT) | Direkt |
| 16 | Wu L2 Prosd | ★ | Custom Classroom | Mandarin | L2 (12) | — | FRR/FAR/DA | ✅ T1-T4+Sandhi | Pinyin | Nein (API) | Upload+API |
| 17 | Huang Rev | ★★ | Review 54 Studien | Mandarin L2 | L2 | — | Acc/F1/CM | ✅ T1-T4 | IPA/Chao | Nein | Review |
| 18 | Zhu ZIPA | ★ | IPAPACK++ | 88 Sprachen | Native+L2 | 17.132h | PFER | ⚠️ teilw. | **IPA** | Nein | Direkt |
| 19 | Kang & Xu | ☆ | Custom 32 Wörter | Mandarin | Native (7) | — | GAMM/BF | ✅ T1-T4 | Chao | Nein | Direkt |
| 20 | Wang MSPB | ★★ | MSPB (eigen) | Mandarin | Native (1+27) | — | Accuracy | ✅ Prosodie | Pinyin+Ton# | **GPT-4o, Gemini** | **Direkt** |
| 21 | Chirkova | ★ | Custom Baima | Baima | Native (3) | 3,1h | tonal CER/WER | ✅ 6 Töne | **IPA+Pinyin** | Nein | Direkt |
| 22 | Wei ASR-EC | ☆ | ASR-EC (544K) | Mandarin | Native | 2.330h | CER | ❌ | keine | Baichuan2 | Text+Audio |
| 23 | Wang CtxASR | ☆ | ContextASR-Bench | Mandarin+EN | TTS | 838h | WER/NE-WER | ❌ | keine | Qwen2-Audio | Direkt |
| 24 | Liu VocBnch | ☆ | VocalBench-zh | Mandarin | — | — | CER/PER/WER | ❌ | keine | GPT-4o, Qwen2 | Direkt |
| 25 | Li TELEVAL | ★ | TELEVAL (eigen) | Mandarin+5 Dial. | Native | — | Accuracy | ❌ | keine | GPT-4o, Qwen2 | Direkt |
| 26 | Sakshi MMAU | — | MMAU (10K) | English | Native | — | Accuracy | ❌ | keine | Gemini Pro | Direkt |
| 27 | Huang DynSB | ★ | Dynamic-SUPERB (180) | Multilingual | — | — | PER/Acc | ⚠️ 1 Task | keine | SALMONN-13B | Direkt |
| 28 | Cui VoxEval | — | VoxEval (14K) | English | — | — | Accuracy | ❌ | keine | E2E SLMs | Direkt |
| 29 | Wang AudioB | — | AudioBench (26 DS) | English | — | 400+h | Acc/MOS | ❌ | keine | GPT-4o, Qwen | Direkt |
| 30 | Zhang Wild | ☆ | WildSpeech (1.1K) | English | — | — | Acc/BLEU | ⚠️ paraling. | keine | GPT-4o, Qwen | Direkt |
| 31 | Latif LAM | ☆ | Review | Multilingual | — | — | WER/MOS | ❌ | keine | Diverse | Review |
| 32 | Gaido ST | — | Review | Multilingual | — | — | BLEU/COMET | ❌ | keine | Diverse | Review |
| 33 | Cui SpLM | ☆ | Review | Multilingual | — | — | CER/WER | ❌ | keine | Diverse | Review |
| 34 | Arora SLM | ★ | Review | **EN focus** | — | — | PER/Acc/WER | ❌ | keine | Diverse | Review |
| 35 | Luo LALM | ★ | Review | Multilingual | — | — | Halluc./Rob. | ⚠️ paraling. | keine | GPT-4o, Qwen | Review |
| 36 | AI Index | — | Diverse | Multilingual | — | — | Acc/Elo | ❌ | keine | Claude/GPT/Gem | Report |
| 37 | Ahlawat | — | Review | EN+Indisch | Native+L2 | — | WER/CER | ❌ | keine | Diverse | Review |
| 38 | Kaur Tonal | ★★ | Review | **Tonal (ZH+Thai+VN)** | Native+L2 | — | Tone Cls Acc | ✅ **Fokus** | — | Nein (2019) | Review |
| 39 | Yang LLM+Sp | ☆ | Review | Multilingual | — | — | WER/BLEU | ❌ | keine | Diverse | Review |
| 40 | Yang Eval | ★ | Review >50 Benchmarks | Multilingual | — | — | Acc/WER/Elo | ❌ | keine | Diverse | Review |
| 41 | **Tone Perfect** | ★★ | **Tone Perfect** | **Mandarin (Beijing)** | **Native (6)** | — | (Dataset) | ✅ **Alle 4** | **Pinyin+Ton#** | — | — |
| 42 | **Xu SITA** | ★★ | Hmong WRT + Tone Perfect | Hmong+Mandarin | Native | 1,7h | Top1/5, CER | ✅ **Tone Geometry** | — | Nein (XLS-R) | Direkt |

---

## Tabelle 3: Metrik-Landschaft

| Metrik | Beschreibung | Welche Paper nutzen sie? |
|--------|-------------|--------------------------|
| **CER** | Character Error Rate | 1-6, 11-14, 20-24 |
| **WER** | Word Error Rate | 1-6, 8, 21, 23, 30 |
| **PER** | Phone/Phoneme Error Rate | 15, 24, 27 |
| **TER** | Tone Error Rate | 9 (empfohlen), **keines misst sie direkt** |
| **Pinyin ERR** | Pinyin Error Rate | 13, 14 |
| **tonal CER/WER** | CER/WER mit Tonmarkierungen | 21 (Baima) |
| **PFER** | Phone Feature Error Rate | 18 |
| **FRR/FAR/DA** | False Rejection/Acceptance Rate, Diagnostic Accuracy | 15, 16 |
| **DER** | Diagnostic Error Rate | 15 |
| **MSE/RMSE** | Mean Squared Error | 10 |
| **F1/Precision/Recall** | Klassifikationsmetriken | 9, 13, 15, 17 |
| **NE-WER/NE-FNR** | Named Entity WER/False Negative Rate | 23 |
| **Accuracy** | Klassifikationsgenauigkeit | 9, 17, 20, 25-29 |
| **BLEU/COMET** | Übersetzungsmetriken | 30, 32 |
| **MOS** | Mean Opinion Score | 29, 31 |

**→ Kernbefund:** Tone Error Rate (TER) wird in keinem einzigen Paper als Evaluationsmetrik angewandt. Stefans Arbeit führt sie erstmals für multimodale LLMs ein.

---

## Tabelle 4: Welche Paper testen Töne?

| Paper | Töne? | Details | LLM? |
|---|---|---|---|
| **Xu SITA** (42) | ✅ Tone Geometry | Whisper zeigt "tone collapse" | Nein |
| **Wang MDD** (15) | ✅ T1-T5 | Initial-Final-Tone System | Nein (HuBERT) |
| **Wu L2 Prosody** (16) | ✅ T1-T4+Sandhi | DA ~80%, Human Corr ~0.6 | Nein (API) |
| **Bu Siamese** (10) | ✅ Kern | MSE 0.189 (L2-Daten) | Nein |
| **Huang Review** (17) | ✅ Fokus | T2↔T3 persistent | Nein |
| **Kaur Survey** (38) | ✅ 358× erwähnt | L2: 53-73% Accuracy | Nein (2019) |
| **Zou Review** (9) | ✅ DL 88,8% | CNNs 99,16% isoliert | Nein |
| **Kang Tone-Sync** (19) | ✅ F0-Synchrony | Bayes-Faktor 252 | Nein |
| **Wang MSPB** (20) | ✅ Prosodie | Aber Satzprosodie, nicht lexikalisch | **Ja (GPT-4o)** |
| **Chirkova** (21) | ✅ 6 Töne | Tonal CER als Metrik | Nein |
| **Zhengjie PYG** (13) | ⚠️ Pinyin ERR | **Pinyin OHNE Tonmarker** | **Ja (Qwen2-7B)** |
| **Kimi-Audio** (4) | ⚠️ Erwähnt nur | *"tone"* als vernachlässigt | Ja |
| **Dynamic-SUPERB** (27) | ⚠️ 1 Task | Third Tone Sandhi (Klassifikation) | Ja |

**→ Kernbefund:** Kein Paper kombiniert LLM + Audio → Pinyin MIT Tonmarkern. Zhengjie & Cheng kommen am nächsten, aber OHNE Töne. MSPB testet LLMs auf Prosodie, aber nicht auf lexikalische Töne.

---

## Tabelle 5: Welche LLMs verarbeiten Audio direkt?

| Modell | Audio→LLM direkt? | Mandarin? | Töne getestet? |
|---|---|---|---|
| GPT-4o (MSPB, WildSpeech, TELEVAL) | ✅ | ✅ | ❌ (nur Satzprosodie) |
| Gemini-1.5-Pro (MSPB, TELEVAL) | ✅ | ✅ | ❌ |
| Qwen2-Audio-7B (MSPB, TELEVAL, VocalBench) | ✅ | ✅ | ❌ |
| Kimi-Audio (Moonshot) | ✅ Hybrid | ✅ | ⚠️ erwähnt, nicht evaluiert |
| Step-Audio 2 | ✅ | ✅ | ❌ (Pitch≠Tone) |
| Qwen3-ASR | ✅ | ✅ | ❌ |
| Seed-ASR (ByteDance) | ✅ | ✅ | ❌ |
| FireRedASR/2S | ✅ | ✅ | ❌ |
| Zhengjie PYG-ASR (HuBERT→Qwen2) | ✅ | ✅ | ❌ (Pinyin ohne Ton#) |

**→ Kernbefund:** Mindestens 9 LLM-basierte Systeme verarbeiten Mandarin-Audio direkt. KEINES evaluiert Tongenauigkeit. Das ist die Lücke, die Stefan schließt.

---

## Zusammenfassung für Tim

### Zentrale Erkenntnis:
**Kein existierendes Paper, kein Benchmark und kein Survey adressiert die zentrale Frage von Stefans Arbeit: Wie genau transkribieren multimodale LLMs Mandarin-Audio in phonetische/tonale Repräsentation?**

### Die 5 Forschungslücken, die Stefan schließt:

| Stefans RQ | Existierende Lücke | Stärkstes Beleg-Zitat |
|---|---|---|
| **RQ1** (Text→Pinyin) | Kein Speech LLM wurde auf phonetische Transkription getestet | Alle 7 Speech LLMs evaluieren nur CER/WER |
| **RQ2** (Native Phonem/Ton) | Keine separate Ton- oder Phonemgenauigkeit gemessen | Zhengjie & Cheng generieren Pinyin aber **OHNE Töne**: *"little exploration of how [...] LLM-ASR [...] can directly generate Pinyin"* |
| **RQ3** (L2-Lernende) | Komplett neue Perspektive für multimodale LLMs | Nur Wang MDD (4 L2-Spr.) & Wu (12 L2-Spr.) berühren L2 — ohne LLMs. Zou: *"L2 corpora [...] receive less attention"* |
| **RQ4** (Vergleich) | Kein systematischer LLM vs. dediziertes System-Vergleich | SITA zeigt *"tone collapse"* bei Whisper; Gaido: *"no work has addressed the comparative assessment [...] under controlled conditions"* |
| **RQ5** (Fehlerstruktur) | Keine ton-/phonembasierte Fehleranalyse für LLMs | Kein Paper analysiert Tonkonfusionen (T2↔T3 etc.) bei multimodalen LLMs. Huang: T2↔T3 *"persistent difficulty"* |

### Stärkste Zitate für die Thesis:

1. **Kimi-Audio:** *"text transcription focuses on the content of spoken words, neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)."*
2. **Peng Survey:** *"extracting richer and finer acoustic information from speech [...] remains limited."*
3. **MSPB:** *"current speech LLMs remain limited in Mandarin speech prosody comprehension"*
4. **Zou Review:** *"L2 corpora exist but receive less attention than native-speaker datasets."*
5. **SITA:** Whisper zeigt "tone collapse" — Ton-Repräsentationen kollabieren in Standard-Modellen
6. **Zhengjie PYG-ASR:** *"there has been little exploration of how the LLM-ASR model can directly generate Pinyin and Chinese characters"* — und selbst dieses Paper lässt Töne weg
7. **Chirkova (Baima):** *"complex tones remain the most difficult part of the phonology to transcribe"*
8. **Gaido Survey:** *"no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework"*
