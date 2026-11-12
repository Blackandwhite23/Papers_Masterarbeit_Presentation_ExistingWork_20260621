# Core Information: Systematische Literaturanalyse aller Paper (Improved)

> Erstellt am 2026-06-22. Fuer jedes Paper werden zentrale Learnings mit exakten Originalzitaten, konkrete Verwendbarkeit fuer die Thesis, Forschungsluecken mit woertlichen Zitaten und deren Relevanz fuer die eigene Arbeit dokumentiert.

## Kontext der eigenen Arbeit

**Thesis:** *Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*
**Autor:** Stefan Dosch (IU, M.Sc. Data Science) · **Betreuer:** Tim Schlippe

**Forschungsfragen:**
- **RQ1:** Text-only Baseline — Character → Pinyin-Transkription (a) ohne Toene, (b) mit Toenen
- **RQ2:** Native-Speaker-Sprache — Transkriptionsgenauigkeit auf Wort-/Phonem-/Ton-Ebene
- **RQ3 (Stretch):** Non-Native/L2-Lerner-Sprache — gleiche drei Ebenen
- **RQ4:** Vergleich mit dedizierten Systemen (Whisper etc.)
- **RQ5:** Fehlerstruktur — welche Toene/Phonemkontraste werden am haeufigsten verwechselt

**Zentrale Forschungsluecken, die diese Arbeit adressiert:**
- **Luecke 1:** Frontier multimodale LLMs werden nur auf Satz-/Wortebene (WER/CER) evaluiert; Phonem-/Ton-Granularitaet fuer Mandarin ist weitgehend unerforscht
- **Luecke 2:** Toene werden selten als explizite, separate Evaluationsachse behandelt
- **Luecke 3:** Sehr wenige Studien decken L2/Non-Native-Sprechersprache ab (Stretch-Goal)
- **Luecke 4:** Kein systematischer Head-to-Head-Vergleich aktueller multimodaler Modelle untereinander und gegen dedizierte Systeme auf dieser Granularitaet

**Design:** Pinyin mit Tonnummern (z.B. `ma1`) als Zielformat; ~5-8 Frontier-Modelle + Whisper als Baseline; Metriken: PER, TER, CER, F1, FAR/FRR; Korpus: Tone Perfect (isolierte Monosyllaben); Off-the-Shelf-Evaluation ohne Finetuning.

---


# Paper-Analysen

---

# Batch 1: Papers 1-6 (Seed-ASR, Baima ASR, AI Index, AudioBench, PY-GEC, FireRedASR2S)

---

## Paper 1: Seed-ASR -- Understanding Diverse Speech and Contexts with LLM-based Speech Recognition
**Autoren:** Seed Team, ByteDance
**Jahr:** 2024
**Quelle:** arXiv:2407.04675v2

### 1. Zentrale Learnings

Seed-ASR ist ein LLM-basiertes grossskaliges ASR-Modell, das auf dem Audio Conditioned LLM (AcLLM)-Framework aufbaut. Es nutzt einen Audio-Encoder mit nahezu 2 Milliarden Parametern und ein Mixture-of-Experts (MoE) LLM. Das Modell wird ueber ein mehrstufiges Trainingsverfahren (SSL -> SFT -> Context SFT -> RL) entwickelt und erreicht auf 6 oeffentlichen Mandarin-Testsets eine durchschnittliche CER von 2.98%. Das Modell unterstuetzt Mandarin und 13 chinesische Dialekte in einem einzigen Modell, evaluiert jedoch ausschliesslich auf CER/WER-Ebene ohne phonetische oder tonale Granularitaet.

**Belegende Zitate:**
- "Seed-ASR is developed based on the framework of audio conditioned LLM (AcLLM), leveraging the capabilities of LLMs by inputting continuous speech representations together with contextual information into the LLM."
- "Seed-ASR employs an audio encoder with nearly 2 billion parameters and a Mixture of Experts (MoE) LLM with tens of billions of parameters for modeling."
- "we developed our audio encoder, a conformer-based model that captures both global and local structures stored in audio signals. In this work, we primarily focus on speech signal. Since it is trained on large-scale unsupervised data, we term the trained audio encoder as LUISE, which represents Large-scale Unsupervised Iterative Speech Encoder."
- "Word error rate (WER) is often considered a core metric for evaluating the performance of ASR models, but certain parts of content (e.g. keyword) in a sentence plays a more crucial role in the understanding of the whole sentence."

### 2. Verwendbarkeit fuer unsere Arbeit

- **RQ1 (Pinyin-Konversion):** Seed-ASR zeigt, dass das AcLLM-Framework prinzipiell in der Lage ist, kontinuierliche Sprachrepraesentationen mit LLMs zu verarbeiten. Diese Architektur koennte fuer die Pinyin-Tonnummern-Konversion adaptiert werden, wird aber im Paper nicht dafuer evaluiert.
- **RQ2 (Modellvergleich):** Seed-ASR liefert wichtige Baselines fuer die Mandarin-ASR-Leistung (2.98% CER), gegen die unsere multimodalen LLM-Ergebnisse kontextualisiert werden koennen.
- **RQ3 (Phonem- vs. Tonebene):** Das Paper evaluiert NUR auf CER/WER-Ebene und liefert damit kein phonem- oder tonspezifisches Fehlerprofil -- dies bestaetigt die Luecke, die unsere Arbeit adressiert.

**Belegendes Zitat:** "The evaluation results demonstrate that Seed-ASR (CN) possesses more comprehensive and powerful model capabilities compared to classic end-to-end models and other released models."

### 3. Forschungsluecken (aus dem Paper)

Das Paper evaluiert ausschliesslich auf Zeichenfehlerrate (CER) und Wortfehlerrate (WER). Es gibt keine Aufschluesselung nach phonetischen Komponenten (Initiale, Finale, Ton) und keine Analyse tonaler Fehlermuster. Obwohl das Modell Mandarin und 13 Dialekte unterstuetzt, wird die tonale Dimension der Spracherkennung nicht separat untersucht.

**Belegendes Zitat:** "In future, we will focus on extending Seed-ASR's ability to deal with multiple tasks within a single model, further enhance the long-form ability and increase the number of supported languages."

### 4. Adressierung durch meine Arbeit

- **Gap 1 (Phonem-/Tongranularitaet):** Unsere Arbeit schliesst genau diese Luecke, indem wir multimodale LLMs explizit auf Phonem- und Tonebene evaluieren, statt nur aggregierte CER/WER zu berichten.
- **Gap 2 (Toene als separate Achse):** Waehrend Seed-ASR Toene implizit im CER mitmisst, trennen wir in unserer Evaluation systematisch zwischen segmentalen (Initiale/Finale) und suprasegmentalen (Ton) Fehlern.
- **Gap 3 (Head-to-Head-Vergleich):** Seed-ASR vergleicht sich mit End-to-End-Modellen, aber nicht mit anderen multimodalen LLMs auf phonetischer Ebene -- genau diesen Vergleich fuehren wir durch.

---

## Paper 2: Comparing efficacy of IPA vs Pinyin romanisation transcriptions for complex tonal languages -- A case study in Baima
**Autoren:** Chirkova, Coto-Solano, Griffiths, Meelen
**Jahr:** 2025
**Quelle:** Proceedings of the Eight Workshop on the Use of Computational Methods in the Study of Endangered Languages (ComputEL)

### 1. Zentrale Learnings

Dieses Paper ist methodisch am direktesten relevant fuer unsere Arbeit. Es untersucht die automatische Tontranskription der gefaehrdeten Tonsprache Baima und vergleicht drei Transkriptionssysteme (IPA, Pinyin, Simple Romanisation) mit drei Basismodellen (Wav2Vec2, MMS, Whisper Medium). Zentrale Erkenntnis: Toene sind der schwierigste Teil der Transkription. Das Paper fuehrt die Metriken "tonal CER" und "tonal WER" ein, die Tonnummern isoliert evaluieren. Die Per-Ton-Fehleranalyse zeigt, dass haeufige Toene (Ton 53) leichter erkannt werden und seltene entlehnte Toene (Ton 55) am schwierigsten sind. Die Verwendung eines Language Models reduziert tonale Fehler signifikant.

**Belegende Zitate:**
- "tones remain the most difficult part of the transcription"
- "The tonal CER is the percentage of characters in this transcription that are wrong. The tonal WER is the percentage of words that have a tonal error in them."
- "For tonal CER, IPA has tonal CER=20, compared to Pinyin tonal CER=28 and Simple tonal CER=31"
- "In the case of the LM, the CER shows a pattern where the use of a KenLM LM reduces the error, but it reduces it more for tones than for the other segments."
- "complex tones remain the most difficult part of the phonology to transcribe, despite the complexity of Baima vowel phonology."
- "the way the language is transcribed can affect tonal outputs, even when the tonal markings themselves remain the same throughout different transcriptions."

### 2. Verwendbarkeit fuer unsere Arbeit

- **RQ1 (Pinyin-Konversion):** Das Paper vergleicht direkt IPA vs. Pinyin als Transkriptionsformat und zeigt, dass das Transkriptionssystem die Tongenauigkeit beeinflusst -- direkt relevant fuer unsere Wahl von Pinyin mit Tonnummern als Zielformat.
- **RQ3 (Phonem- vs. Tonebene):** Die Methodik der separaten tonalen CER/WER-Berechnung ist ein direktes Vorbild fuer unsere Evaluationsstrategie, bei der wir Ton- und Segmentfehler getrennt analysieren.
- **RQ4 (Tonale Fehlermuster):** Die Per-Ton-Fehleranalyse (Tabelle 4 im Paper) zeigt, dass Tonfrequenz und Tonsystem die Fehlerrate beeinflussen -- eine Methodik, die wir fuer Mandarin-Toene adaptieren.

**Belegendes Zitat:** "Are tones more difficult to transcribe than other parts of the phonology, like the consonants and the vowels? They are, but mainly when an LM is NOT used."

### 3. Forschungsluecken (aus dem Paper)

Das Paper untersucht nur traditionelle ASR-Modelle (Wav2Vec2, MMS, Whisper Medium), nicht aber multimodale LLMs. Es arbeitet mit einer extrem ressourcenarmen Sprache (Baima, 186 Minuten Daten), nicht mit einer gut-ressourcierten Sprache wie Mandarin. Die Methodik der tonalen Fehleranalyse wird nicht auf groessere Modelle oder im Kontext von off-the-shelf multimodalen LLMs angewendet.

**Belegendes Zitat:** "In this paper we therefore focus on transcription systems and how they might impact the automatic transcription of complex tones, testing different base models as well as the usefulness of adding an LM to the decoding process."

### 4. Adressierung durch meine Arbeit

- **Gap 1 (Phonem-/Tongranularitaet):** Wir uebertragen die Methodik der tonalen CER/WER aus diesem Paper auf multimodale LLMs und Mandarin -- eine Sprache mit wesentlich mehr verfuegbaren Ressourcen und breiterer Relevanz.
- **Gap 2 (Toene als separate Achse):** Die hier eingefuehrte Trennung von tonaler und segmentaler Evaluation adaptieren wir fuer unseren Mandarin-Kontext mit vier lexikalischen Toenen plus neutralem Ton.
- **Gap 3 (Head-to-Head-Vergleich):** Waehrend dieses Paper Wav2Vec2, MMS und Whisper vergleicht, fuehren wir den Vergleich fuer multimodale LLMs (GPT-4o, Gemini, etc.) durch, die im Paper nicht untersucht werden.

---

## Paper 3: AI Index Report 2026, Chapter 2 -- Technical Performance
**Autoren:** AI Index Steering Committee, Stanford HAI
**Jahr:** 2026
**Quelle:** AI Index Report 2026

### 1. Zentrale Learnings

Der AI Index Report 2026 dokumentiert die Konvergenz der Leistungsfaehigkeit von Frontier-Modellen: Die Top-4-Modelle auf dem Arena Leaderboard liegen nur noch innerhalb von 25 Elo-Punkten. Die Luecke zwischen US- und chinesischen Modellen hat sich fast vollstaendig geschlossen. Benchmark-Saettigung bleibt ein zentrales Problem -- Tests, die als schwierig konzipiert wurden, werden innerhalb weniger Jahre uebertroffen. Der Bericht deckt jedoch keine spezifischen ASR- oder phonetischen Benchmarks ab.

**Belegende Zitate:**
- "the gap between top models is shrinking. This narrowing extends geographically, as the distance between top U.S. and Chinese models has closed almost completely."
- "Frontier models became even more tightly clustered over the past year, as several companies moved into a very narrow performance band at the top of the Arena Leaderboard. [...] the top four models are separated by fewer than 25 points."
- "Benchmark saturation, where models reach scores so high that a test can no longer distinguish between them, remains a concern."
- "As leading models become harder to distinguish on benchmark performance, factors such as cost, latency, reliability, and domain-specific optimization may play a greater role in user adoption."

### 2. Verwendbarkeit fuer unsere Arbeit

- **RQ2 (Modellvergleich):** Die dokumentierte Konvergenz der Frontier-Modelle unterstreicht die Notwendigkeit spezifischer, differenzierender Benchmarks -- wie unsere phonetische Transkriptionsevaluation. Wenn Modelle auf generischen Benchmarks kaum unterscheidbar sind, bieten domaenenspezifische Tests wie tonale Transkription einen Mehrwert.
- **RQ3 (Fehlerprofile):** Der Bericht betont, dass Benchmark-Saettigung die Unterscheidbarkeit von Modellen reduziert. Unsere feingranulare Phonem/Ton-Evaluation bietet genau die Art differenzierender Metrik, die der Bericht als noetig identifiziert.

**Belegendes Zitat:** "The overall spread between the top-ranked and 15th-ranked model is just over 4 percentage points, illustrating how competitive the frontier has become on broad knowledge tasks. This tight clustering is also consistent with the convergence pattern described in Section 2.1."

### 3. Forschungsluecken (aus dem Paper)

Der AI Index Report 2026 behandelt keine ASR-spezifischen oder phonetischen Benchmarks. Audio/Sprache wird im Kapitel Technical Performance nicht als separate Evaluationskategorie gefuehrt. Die Konvergenz der Modelle wird fuer Text, Reasoning, Coding und Math dokumentiert, aber nicht fuer Sprachverarbeitung oder tonale Sprachen.

**Belegendes Zitat:** "Several challenges highlighted in previous editions of this report persist. Benchmark saturation, where models reach scores so high that a test can no longer distinguish between them, remains a concern. Tests designed to be harder often remain useful for only a few years before systems surpass them."

### 4. Adressierung durch meine Arbeit

- **Gap 1 (Phonem-/Tongranularitaet):** Der Bericht zeigt implizit, dass es an spezialisierten Benchmarks fuer ASR und phonetische Evaluation fehlt. Unsere Arbeit traegt dazu bei, diese Luecke zu fuellen.
- **Gap 3 (Head-to-Head-Vergleich):** Die Konvergenz auf generischen Benchmarks macht Head-to-Head-Vergleiche auf spezifischen Aufgaben wie tonaler Transkription umso wichtiger, um tatsaechliche Leistungsunterschiede zwischen Modellen aufzudecken.

---

## Paper 4: AudioBench -- A Universal Benchmark for Audio Large Language Models
**Autoren:** Wang, Zou, Lin, Sun, Liu, Zhang, Liu, Aw, Chen
**Jahr:** 2025
**Quelle:** arXiv:2406.16020v5

### 1. Zentrale Learnings

AudioBench ist der erste umfassende Benchmark fuer Audio Large Language Models (AudioLLMs). Es umfasst 8 Aufgaben und 26 Datensaetze, die Sprachverstaendnis, Audio-Szenen-Verstaendnis und Stimmverstaendnis (paralinguistisch) abdecken. Die Evaluation von 5 Modellen zeigt, dass kein einzelnes Modell ueber alle Aufgaben hinweg dominiert. ASR wird nur ueber WER evaluiert, ausschliesslich fuer Englisch. Es gibt keine phonetische oder tonale Evaluationsdimension.

**Belegende Zitate:**
- "We introduce AudioBench, a universal benchmark designed to evaluate Audio Large Language Models (AudioLLMs). It encompasses 8 distinct tasks and 26 datasets"
- "we also evaluated the capabilities of five popular models and found that no single model excels consistently across all tasks."
- "the current AudioBench exclusively includes English datasets. However, multilingual capabilities and code-switching are crucial for comprehensive speech understanding and generation. We plan to expand the benchmark to incorporate these aspects in future iterations."
- "multilingual understanding, which includes speech translation and code-switching, represents a useful application that could be integrated with future work."
- "Expanding the linguistic capabilities of AudioLLMs to handle multiple languages, code-switching, and various dialects is crucial."

### 2. Verwendbarkeit fuer unsere Arbeit

- **RQ2 (Modellvergleich):** AudioBench zeigt, dass kein AudioLLM ueber alle Aufgaben dominiert -- dies motiviert unseren aufgabenspezifischen Vergleich fuer phonetische Mandarin-Transkription.
- **RQ3 (Fehlerprofile):** Der Benchmark evaluiert ASR nur ueber WER -- keine phonem- oder tonspezifische Analyse. Dies bestaetigt die Luecke, die unsere Arbeit adressiert.
- **Methodisch:** Die Model-as-Judge-Evaluationsmethodik und die Verwendung von Llama-3-70B-Instruct als Evaluationsmodell koennen fuer unsere Evaluation informativ sein.

**Belegendes Zitat:** "the modality fusion process in AudioLLMs may distort speech content, highlighting an area for future improvement."

### 3. Forschungsluecken (aus dem Paper)

AudioBench beschraenkt sich auf englische Datensaetze, ohne Mandarin oder andere tonale Sprachen. ASR wird nur ueber WER gemessen, ohne phonemische, tonale oder subwort-granulare Metriken. Multilinguale Faehigkeiten und Code-Switching werden als explizite Luecken benannt.

**Belegendes Zitat:** "First, the current AudioBench exclusively includes English datasets. However, multilingual capabilities and code-switching are crucial for comprehensive speech understanding and generation. We plan to expand the benchmark to incorporate these aspects in future iterations."

### 4. Adressierung durch meine Arbeit

- **Gap 1 (Phonem-/Tongranularitaet):** AudioBench evaluiert ASR nur ueber WER. Unsere Arbeit fuegt genau die fehlende phonem- und tongranulare Evaluationsdimension hinzu, die im Benchmark fehlt.
- **Gap 2 (Toene als separate Achse):** Die im Paper identifizierte Luecke fehlender multilingualer Evaluation fuer tonale Sprachen wird durch unsere Mandarin-fokussierte Evaluation adressiert.
- **Gap 3 (Head-to-Head-Vergleich):** Waehrend AudioBench 5 Modelle auf generischen Aufgaben vergleicht, fuehren wir einen spezialisierten Vergleich multimodaler LLMs auf phonetischer Transkriptionsgenauigkeit durch.

---

## Paper 5: Large Language Model Should Understand Pinyin for Chinese ASR Error Correction
**Autoren:** Li, Qiao, Zhao, Zhao, Tang, Zhang, Yang (Huawei)
**Jahr:** 2024
**Quelle:** arXiv:2409.13262v1

### 1. Zentrale Learnings

PY-GEC demonstriert, dass Pinyin als phonetische Repraesentationsform die LLM-basierte Fehlerkorrektur fuer chinesisches ASR signifikant verbessert. Multitask-Training mit Pinyin-Text-Konversionsaufgaben fuehrt zu einer relativen CER-Reduktion von 8.3% und einer relativen Entity-Recall-Verbesserung von 3.9%. Die Aufmerksamkeitsanalyse zeigt, dass das Modell nach Multitask-Training den hoechsten Aufmerksamkeitswert auf Pinyin-Features legt. Die Kosinus-Aehnlichkeit zwischen Text- und Pinyin-Feature-Raeumen steigt von 0.26 (ohne Finetuning) auf 0.82 (mit Multitask-Training).

**Belegende Zitate:**
- "We propose Pinyin-enhanced GEC (PY-GEC), which leverages Pinyin--the phonetic representation of Mandarin Chinese--as supplementary information to improve Chinese ASR error correction."
- "multitask training enhances overall performance and contributes to a relative CER reduction of 8.3% and a relative entity recall improvement of 3.9% on average compared to direct correction."
- "Multitask training enhances the importance of Pinyin features, as indicated by the highest attention score, demonstrating that the LLM better comprehends Pinyin features."
- "Initially, without fine-tuning, the LLaMA-3-8B-Chinese shows poor alignment with a low cosine similarity of 0.26. Fine-tuning with PY-GEC or multitask training can both significantly boost the alignment with cosine similarity improved to 0.74 and 0.82 respectively."
- "Due to Chinese homophones, most errors in Chinese ASR are substitutions."
- "This further emphasizes the importance of promoting phonetic representation understanding within large language models for better correction performance."

### 2. Verwendbarkeit fuer unsere Arbeit

- **RQ1 (Pinyin-Konversion):** PY-GEC zeigt direkt, dass LLMs Pinyin verstehen und nutzen koennen. Die Erkenntnis, dass Multitask-Training die Text-Pinyin-Alignment verbessert, ist fuer unsere off-the-shelf-Evaluation relevant -- es zeigt, was bei finegetunten Modellen moeglich ist.
- **RQ4 (Tonale Fehlermuster):** Die Erkenntnis, dass Homophone die Hauptfehlerquelle in chinesischem ASR sind, ist direkt relevant fuer unsere Analyse tonaler Verwechslungsmuster.
- **RQ5 (Phonetische Aehnlichkeit):** PY-GEC bestaetigt, dass "ASR errors, unlike typographical and grammatical errors, often involve misrecognizing one word as another due to similar pronunciation" -- dies begruendet unsere Untersuchung der Korrelation zwischen Transkriptionsfehlern und phonetischer Aehnlichkeit.

**Belegendes Zitat:** "ASR errors, unlike typographical and grammatical errors, often involve misrecognizing one word as another due to similar pronunciation. Consequently, Chinese ASR error correction poses a challenge because there is no direct connection between the pronunciation and the written form of Chinese characters."

### 3. Forschungsluecken (aus dem Paper)

PY-GEC arbeitet mit Text-basierter Fehlerkorrektur (Post-Processing), nicht mit direkter Audio-zu-Pinyin-Transkription. Es wird nur CER auf Zeichenebene evaluiert, ohne separate tonale Fehleranalyse. Die Autoren selbst identifizieren die Erweiterung auf multimodale LLMs als zukuenftige Arbeit.

**Belegendes Zitat:** "For future research, we aim to extend our experiments to larger-scale LLMs and multi-modal LLMs."

### 4. Adressierung durch meine Arbeit

- **Gap 1 (Phonem-/Tongranularitaet):** PY-GEC evaluiert nur aggregierte CER. Unsere Arbeit fuegt die fehlende Aufschluesselung nach Initialen, Finalen und Toenen hinzu.
- **Gap 2 (Toene als separate Achse):** Obwohl PY-GEC Pinyin nutzt (das Toninformation enthaelt), wird die Tongenauigkeit nicht separat evaluiert. Unsere Arbeit behandelt Toene explizit als eigenstaendige Evaluationsachse.
- **Gap 3 (Head-to-Head-Vergleich):** PY-GEC zeigt die Nuetzlichkeit von Pinyin fuer ein einzelnes Modell (LLaMA-3-8B-Chinese). Unsere Arbeit vergleicht mehrere multimodale LLMs direkt auf dieser Aufgabe.

---

## Paper 6: FireRedASR2S -- A State-of-the-Art Industrial-Grade All-in-One Automatic Speech Recognition System
**Autoren:** Xu, Jia, Huang, Chen, Li, Liu, Xie, Tang, Hu (Xiaohongshu)
**Jahr:** 2026
**Quelle:** arXiv:2603.10420v1

### 1. Zentrale Learnings

FireRedASR2S ist ein industrietaugliches All-in-One ASR-System, das ASR, VAD, Spracherkennung (LID) und Interpunktionsvorhersage integriert. Es bietet zwei ASR-Varianten: eine LLM-basierte (8B+ Parameter) und eine AED-basierte (1B+ Parameter). Das System erreicht 2.89% durchschnittliche CER auf 4 oeffentlichen Mandarin-Benchmarks und 11.55% auf 19 chinesischen Dialekt-Benchmarks. Es unterstuetzt Mandarin, chinesische Dialekte, Englisch, Code-Switching und Gesangstranskription. Die Evaluation erfolgt ausschliesslich ueber CER, ohne phonetische oder tonale Granularitaet.

**Belegende Zitate:**
- "FireRedASR2-LLM achieves 2.89% average CER on 4 public Mandarin benchmarks and 11.55% on 19 public Chinese dialects and accents benchmarks, outperforming competitive baselines including Doubao-ASR, Qwen3-ASR, and Fun-ASR."
- "FireRedASR2 builds upon our previous FireRedASR models with minimal architectural changes. Compared to FireRedASR, FireRedASR2 improves recognition accuracy and expands coverage to a broader range of Chinese dialects, primarily by scaling supervised training data to approximately 200k hours with broader domain, language, and dialect diversity."
- "We use Character Error Rate (CER, %) for Chinese."
- "Future work will focus on further improving performance and expanding support for more languages."

### 2. Verwendbarkeit fuer unsere Arbeit

- **RQ1 (Pinyin-Konversion):** FireRedASR2S nutzt die Encoder-Adapter-LLM-Architektur, die zeigt, wie LLMs fuer ASR eingesetzt werden koennen. Es transkribiert jedoch nur in chinesische Zeichen, nicht in Pinyin mit Tonnummern.
- **RQ2 (Modellvergleich):** Die CER-Ergebnisse (2.89% Mandarin) bieten eine starke Referenz fuer die aktuelle State-of-the-Art ASR-Leistung. Unser Vergleich multimodaler LLMs auf phonetischer Ebene kann gegen diese Baselines kontextualisiert werden.
- **RQ3 (Fehlerprofile):** FireRedASR2S evaluiert nur CER -- keine Aufschluesselung nach Initialen, Finalen oder Toenen. Dies bestaetigt erneut die Luecke feingranularer phonetischer Evaluation.

**Belegendes Zitat:** "FireRedASR2-LLM achieves the best overall accuracy across all aggregated metrics, reaching 2.89% average CER on Mandarin (Avg-Mandarin-4), 11.55% on Chinese dialect (Avg-Dialect-19), and 9.67% on Avg-All-24."

### 3. Forschungsluecken (aus dem Paper)

Das Paper evaluiert nur auf CER-Ebene und bietet keine phonetische Fehleranalyse. Trotz Unterstuetzung von Mandarin und 19 Dialekten gibt es keine Analyse tonaler Fehler oder Verwechslungsmuster. Die zukuenftige Arbeit fokussiert auf Leistungsverbesserung und Spracherweiterung, nicht auf tiefere linguistische Evaluation.

**Belegendes Zitat:** "Future work will focus on further improving performance and expanding support for more languages."

### 4. Adressierung durch meine Arbeit

- **Gap 1 (Phonem-/Tongranularitaet):** FireRedASR2S misst nur aggregierte CER. Unsere Arbeit liefert die fehlende feingranulare Evaluation auf Phonem- und Tonebene fuer Mandarin.
- **Gap 2 (Toene als separate Achse):** Das Paper behandelt die vier Mandarin-Toene nicht als separate Evaluationsdimension. Unsere Arbeit fuehrt genau diese Analyse durch, mit separater Ton Error Rate (TER).
- **Gap 3 (Head-to-Head-Vergleich):** FireRedASR2S vergleicht sich mit Doubao-ASR, Qwen3-ASR und Fun-ASR auf CER-Ebene. Unsere Arbeit fuehrt den fehlenden Vergleich multimodaler LLMs auf phonetischer Transkriptionsgenauigkeit durch.


# Batch 2: Papers 7-12 (ASR DL Survey, Pitch-Aware RNN-T, Tone-syllable Synchrony, Sparks of LAMs, ST SFM+LLM, LLMs Meet Speech)


## Paper 1: Deep Learning Approaches for Automatic Speech Recognition: A Survey
**Autoren:** Ahlawat, H., Aggarwal, N., Gupta, D.
**Quelle:** International Journal of Cognitive Computing in Engineering 6 (2025), 201-237

### 1. Zentrale Learnings

**Zusammenfassung:** Dieses Survey bietet einen umfassenden Ueberblick ueber DNN-basierte ASR-Methoden von 2010 bis 2024. Es deckt die Evolution von HMMs/GMMs ueber DNNs, CNNs, RNNs hin zu Transformern und Conformern ab, analysiert oeffentlich zugaengliche Datensaetze (inkl. Mandarin-Datensaetze AISHELL-1/2), definiert zentrale Evaluationsmetriken (WER, CER, PER) und beschreibt die Architekturen moderner Modelle wie Whisper, wav2vec 2.0, HuBERT und SeamlessM4T.

**Belegende Zitate:**

- "This paper provides a thorough review of the numerous studies conducted since 2010, as well as an extensive [...] datasets. In this paper, we have also analyzed the various models on publicly accessible speech datasets to understand model performance across diverse datasets for practical deployment." (S. 201)

- "The Phone error rate (PER) (Morris et al., 2004) is computed by dividing the total number of phonemes by the number of phoneme errors (inserted (I), deleted (D), and modified phonemes (M))." (S. 214)

- "Aishell (Bu, Du, Na, Wu, & Zheng, 2017) is one of the largest datasets used for speech recognition research and system development in Mandarin. The dataset consists of 170 h Mandarin speech data which includes audio from 400 distinct speakers of various genders and ages." (S. 214)

- "High-resource languages like English, Mandarin, and Spanish benefit from abundant labeled data, resulting in a lower WER. In contrast, low-resource languages, particularly many African and indigenous languages, experience higher WER due to limited data. Languages with complex morphology, such as Finnish and Turkish, and those with diverse phonetic sounds, like Vietnamese, pose additional challenges." (S. 225)

- "Whisper is a general-purpose model designed for speech recognition in noisy or low-resource settings, and is capable of performing multiple speech-related tasks and uses weak supervision and a minimalist approach to data pre-processing." (Table 8)

### 2. Verwendbarkeit fuer unsere Arbeit

| Forschungsfrage | Relevanz | Erklaerung |
|---|---|---|
| RQ1 (Text-only Baseline) | Gering | Survey fokussiert auf Audio-basierte ASR, nicht auf Text-zu-Pinyin-Konversion |
| RQ2 (Native-Speaker Transkription) | Mittel | Definiert PER/CER/WER-Metriken, die wir verwenden; beschreibt Whisper-Architektur als unsere Baseline |
| RQ3 (Non-Native/L2 Speech) | Gering | Erwaehnt Akzent-Variabilitaet als Herausforderung, aber keine L2-spezifische Evaluation |
| RQ4 (Vergleich mit dedizierten Systemen) | Hoch | Umfassender Ueberblick ueber Whisper, wav2vec 2.0, HuBERT, SeamlessM4T -- liefert Hintergrund fuer unseren Modellvergleich |
| RQ5 (Fehlerstruktur) | Gering | Definiert PER-Metrik formell, aber keine Analyse tonaler Fehlerstrukturen |

### 3. Forschungsluecken (mit woertlichen Zitaten)

- **Luecke A:** Evaluation beschraenkt sich auf WER/CER auf Satzebene; keine phonem- oder tonspezifische Granularitaet.
  > "Some popular datasets, such as LibriSpeech (WER 1.4), TIMIT (PE 8.3), Switchboard (PE 4.3), and TED-LIUM (WER 5.3), are related to the generic domain. However, further research attention is required for ASR systems in specific domains, particularly in improving speech recognition in healthcare, education, and corporate sectors, especially in the context of low resource languages." (S. 229)

- **Luecke B:** Fokus liegt auf low-resource Sprachen; tonale Sprachen werden nicht als eigene Herausforderungskategorie behandelt.
  > "considerable research is still needed before speech recognition becomes widely accessible and consistently reliable for all users. Achieving this objective poses challenges but promises more natural, accessible, and seamless interactions with technology." (S. 229)

- **Luecke C:** Akzent- und Dialektvariabilitaet als offenes Problem, aber keine systematische L2-Evaluation.
  > "Although most models are trained on standard language datasets, a little change in language accent might impact model accuracy. Another aspect to consider in order to develop more accent-robust ASR systems." (S. 230)

### 4. Adressierung durch meine Arbeit

| Luecke | Adressierung |
|---|---|
| Luecke 1 (Keine Phonem/Ton-Granularitaet bei Frontier-LLMs) | Unser Design evaluiert explizit auf Phonem- und Tonebene (PER, TER), nicht nur WER/CER -- genau die Granularitaet, die dieses Survey nicht abdeckt |
| Luecke 2 (Toene selten als separate Evaluationsachse) | Wir fuehren TER als dedizierte Metrik fuer Tonerkennungsgenauigkeit ein |
| Luecke 3 (Wenige Studien zu L2/Non-Native Speech) | Unser Stretch-Goal (RQ3) adressiert L2-Sprecher, ein Bereich den das Survey als Herausforderung identifiziert aber nicht untersucht |
| Luecke 4 (Kein systematischer Head-to-Head-Vergleich) | Wir vergleichen 5-8 Frontier-Modelle + Whisper unter identischen Bedingungen -- das Survey zeigt, dass solche Vergleiche in der Literatur fehlen |

---

## Paper 2: Pitch-Aware RNN-T for Mandarin Chinese Mispronunciation Detection and Diagnosis
**Autoren:** Wang, X., Shi, M., Wang, Y.
**Quelle:** arXiv:2406.04595v1, Juni 2024

### 1. Zentrale Learnings

**Zusammenfassung:** Das Paper praesentiert ein stateless RNN-T-Modell fuer Mandarin-MDD (Mispronunciation Detection and Diagnosis), das HuBERT-Features mit Pitch-Embeddings durch einen Pitch Fusion Block kombiniert. Das Modell verwendet das Initial-Final-Tone-System mit 5 Toenen und wird ausschliesslich auf nativen Sprecherdaten (AISHELL-1) trainiert, aber auf L2-Daten (LATIC-Datensatz: Russisch, Koreanisch, Franzoesisch, Arabisch als L1) evaluiert.

**Belegende Zitate:**

- "Mispronunciation Detection and Diagnosis (MDD) systems, leveraging Automatic Speech Recognition (ASR), face two main challenges in Mandarin Chinese: 1) The two-stage models create an information gap between the phoneme or tone classification stage and the MDD stage. 2) The scarcity of Mandarin MDD datasets limits model training." (Abstract)

- "Pronunciations are represented using the initial-final-tone system, where syllables are broken down into their initial consonant sounds, final vowel sounds, and tones. This phoneme set includes five tones: Tone 1 (high), Tone 2 (rising), Tone 3 (falling then rising), Tone 4 (high then falling), and Tone 5 (neutral or toneless)." (S. 2)

- "Our model, trained solely on native speaker data, shows a 3% improvement in Phone Error Rate and a 7% increase in False Acceptance Rate over the state-of-the-art baseline in non-native scenarios." (Abstract)

- "The vocabulary size in our work is 215, including 214 tonal phonemes tokens and a blank token." (S. 3)

- "Following previous works [32, 33], we employ several evaluation metrics to assess the performance of the MDD model, including False Rejection Rate (FRR; FR/(FR + TA)), False Acceptance Rate (FAR; FA/(FA + TR)), Recall (RE; TR/(FA + TR)), Precision (PR; TR/(FR + TR)), and F1-score (2*(RE * PR)/(RE + PR))." (S. 3)

- "We anticipate that the findings presented herein will serve as a catalyst for future research in the areas of tonal language Automatic Speech Recognition and Mispronunciation Detection and Diagnosis." (S. 4)

### 2. Verwendbarkeit fuer unsere Arbeit

| Forschungsfrage | Relevanz | Erklaerung |
|---|---|---|
| RQ1 (Text-only Baseline) | Gering | Modell arbeitet mit Audio-Input, nicht Text-zu-Pinyin |
| RQ2 (Native-Speaker Transkription) | Hoch | Zeigt phonem- und tonebene-Evaluation auf nativen AISHELL-1-Daten; PER als Kernmetrik |
| RQ3 (Non-Native/L2 Speech) | Sehr hoch | Evaluation auf L2-Sprechern (LATIC-Datensatz) mit 4 verschiedenen L1-Hintergruenden -- direkter Bezug zu unserem Stretch-Goal |
| RQ4 (Vergleich mit dedizierten Systemen) | Hoch | Stellt ein spezialisiertes MDD-System dar, gegen das Frontier-LLMs verglichen werden koennen |
| RQ5 (Fehlerstruktur) | Sehr hoch | Verwendet FAR/FRR/F1 auf Phonem-Ebene; 214 tonale Phonem-Tokens ermoeglichen feinkoernige Fehleranalyse |

### 3. Forschungsluecken (mit woertlichen Zitaten)

- **Luecke A:** Datenmangel fuer L2-Mandarin bleibt kritisch.
  > "Data sparsity, highlighting the scarcity of annotated non-native speech data, is a critical issue in Mandarin Chinese MDD." (S. 1)

- **Luecke B:** Kein Vergleich mit multimodalen Foundation Models oder LLMs.
  > "We anticipate that the findings presented herein will serve as a catalyst for future research in the areas of tonal language Automatic Speech Recognition and Mispronunciation Detection and Diagnosis." (S. 4)

- **Luecke C:** Pitch-Information wurde in frueheren SSL-basierten MDD-Systemen nicht explizit extrahiert.
  > "However, this method did not explicitly extract pitch information from speech, which has been shown to enhance the performance of MDD models in tonal languages." (S. 1)

- **Luecke D:** Modell wird nur auf einem einzigen L2-Datensatz (LATIC) evaluiert; breitere L2-Evaluation fehlt.
  > "To our knowledge, the only publicly available L2 Mandarin Chinese dataset for training is the relatively small LATIC dataset." (S. 1)

### 4. Adressierung durch meine Arbeit

| Luecke | Adressierung |
|---|---|
| Luecke 1 (Keine Phonem/Ton-Granularitaet bei Frontier-LLMs) | Wir uebertragen die phonem-/tonebene-Evaluationsmethodik dieses Papers auf Frontier-LLMs -- ein bisher unerforschtes Gebiet |
| Luecke 2 (Toene selten als separate Evaluationsachse) | Das Paper zeigt mit seinen 5 Ton-Kategorien und 214 tonalen Phonemen, warum Ton-Evaluation zentral ist; wir nutzen aehnliche Granularitaet mit Pinyin-Ton-Nummern |
| Luecke 3 (Wenige Studien zu L2/Non-Native Speech) | Das Paper demonstriert L2-Evaluation auf LATIC; unser Stretch-Goal (RQ3) erweitert dies auf multimodale LLMs |
| Luecke 4 (Kein systematischer Head-to-Head-Vergleich) | Das Paper vergleicht nur wav2vec2.0 vs. HuBERT mit RNN-T; wir vergleichen 5-8 Frontier-Modelle systematisch |

---

## Paper 3: Tone-syllable synchrony in Mandarin: New evidence and implications
**Autoren:** Kang, W., Xu, Y.
**Quelle:** Speech Communication 163 (2024), 103121

### 1. Zentrale Learnings

**Zusammenfassung:** Dieses phonetische Grundlagenwork beweist durch eine minimal contrast Methodik und statistische Analyse (GAMMs + Bayes-Faktoren), dass Ton und Vokal in Mandarin-Silben vollstaendig synchron einsetzen (CVT co-onset). Die Ergebnisse zeigen, dass der Ton nicht -- wie frueher angenommen -- antizipatorisch vor der Silbengrenze beginnt, sondern innerhalb der artikulatorischen Silbe startet. Dies hat tiefgreifende Implikationen fuer das Verstaendnis der Mandarin-Silbenstruktur.

**Belegende Zitate:**

- "The results indicate that tone and vowel onsets are fully synchronized. There is therefore evidence for strict alignment of consonant, vowel and tone as hypothesized in the synchronization model of the syllable." (Abstract)

- "Also, with the newly established tone onset, the previously reported 'anticipatory raising' effect of tone now appears to occur within rather than before the articulatory syllable." (Abstract)

- "The Bayes factor was 252.84 in the [normalized time analysis]" (S. 9)

- "The Bayes factor was 229.79, which again well exceeds the threshold of 3, indicating strong support for tone-vowel synchrony." (S. 10)

- "there is now quantitative evidence for full CVT co-onset as proposed in the synchronization model of the syllable" (S. 10)

- "the common onset of vowel and tone determined in the present study has shifted the CVT synchronized syllable onset leftward from the conventional acoustic onset by over 125 ms in real time, or 43.6 % in normalized time." (S. 10)

### 2. Verwendbarkeit fuer unsere Arbeit

| Forschungsfrage | Relevanz | Erklaerung |
|---|---|---|
| RQ1 (Text-only Baseline) | Gering | Rein phonetische Studie ohne ASR-Bezug |
| RQ2 (Native-Speaker Transkription) | Mittel | Liefert theoretische Begruendung, warum Tonerkennungsfehler in ASR fundamental problematisch sind: Ton ist integraler Bestandteil der Silbe |
| RQ3 (Non-Native/L2 Speech) | Gering | Studie verwendet nur native Mandarin-Sprecher |
| RQ4 (Vergleich mit dedizierten Systemen) | Gering | Kein Bezug zu ASR-Systemen |
| RQ5 (Fehlerstruktur) | Hoch | Begruendet phonetisch, warum Ton nicht unabhaengig vom Segment betrachtet werden kann; CVT-Synchronie bedeutet, dass Tonfehler gleichzeitig Segmentfehler implizieren koennen |

### 3. Forschungsluecken (mit woertlichen Zitaten)

- **Luecke A:** Die Studie untersucht nur die Produktion; Perzeption und maschinelle Erkennung werden nicht adressiert.
  > "What remains less clear is the laryngeal dimension of the syllable, for which evidence of tone synchrony with the consonant-vowel syllable has been circumstantial." (Abstract)

- **Luecke B:** Nur native Sprecher; L2-Sprecher koennten andere Synchronisierungsmuster zeigen.
  > (Implizit: Die Studie erwaehnt keine L2-Sprecher-Daten)

- **Luecke C:** Keine Verbindung zur maschinellen Tonerkennung oder ASR.
  > "Implications of these findings will be discussed." (Abstract) -- jedoch werden ausschliesslich phonetisch-linguistische Implikationen diskutiert, keine technologischen.

### 4. Adressierung durch meine Arbeit

| Luecke | Adressierung |
|---|---|
| Luecke 1 (Keine Phonem/Ton-Granularitaet bei Frontier-LLMs) | Die CVT-Synchronie-Erkenntnisse stuetzen unsere Entscheidung, Ton als separate Evaluationsachse zu verwenden: Wenn Ton integral mit dem Segment synchronisiert ist, muss er auch separat evaluiert werden |
| Luecke 2 (Toene selten als separate Evaluationsachse) | Das Paper liefert die phonetisch-linguistische Begruendung, warum TER als Metrik unverzichtbar ist |
| Luecke 3 (Wenige Studien zu L2/Non-Native Speech) | Die Studie adressiert L2-Sprecher nicht; unser RQ3 koennte zeigen, ob LLMs bei nicht-synchronisierten Ton-Mustern von L2-Sprechern scheitern |
| Luecke 4 (Kein systematischer Head-to-Head-Vergleich) | Nicht direkt adressiert; Paper liefert aber die theoretische Grundlage fuer unsere Evaluationsmethodik |

---

## Paper 4: Sparks of Large Audio Models: A Survey and Outlook
**Autoren:** Latif, S. et al.
**Quelle:** arXiv:2308.12792v3, 2023

### 1. Zentrale Learnings

**Zusammenfassung:** Dieses umfassende Survey erfasst die Landschaft der Large Audio Models (LAMs), einschliesslich SpeechGPT, AudioPaLM, AudioLM, SeamlessM4T und Whisper. Es deckt ASR, TTS, Speech Translation, Musikgenerierung und Dialogsysteme ab. Die Autoren identifizieren offene Herausforderungen wie Tokenisierung, Berechnungskosten, begrenzte Kontextlaenge, Verstaendnis paralinguistischer Information, Prompt-Sensitivitaet, Halluzinationen und ethische Fragen.

**Belegende Zitate:**

- "This paper offers the first comprehensive survey on Large Audio Models, capturing the nuanced interplay of various LLMs within the audio sector." (S. 22, Conclusion)

- "Multilingual speech tokenisation poses additional complexities as the same statement might demand a varying number of tokens in different languages." (S. 20)

- "While LLMs excel in speech generation, their capacity to comprehend and generate various emotions remains largely untapped and understudied." (S. 21)

- "Despite the progress made, to our knowledge, the literature has yet to explore the design and testing of specialised prompts tailored specifically for speech-based scenarios. Addressing this gap presents an intriguing avenue for research, one that could pave the way for enhanced interactions with Large Audio Models." (S. 21)

- "the generated speech might not consistently align with intended accents or dialects for marginalised groups." (S. 22)

### 2. Verwendbarkeit fuer unsere Arbeit

| Forschungsfrage | Relevanz | Erklaerung |
|---|---|---|
| RQ1 (Text-only Baseline) | Gering | Survey fokussiert auf Audio-Modelle |
| RQ2 (Native-Speaker Transkription) | Mittel | Gibt Ueberblick ueber ASR-Faehigkeiten von LAMs; zeigt WER-Vergleiche (z.B. FLEURS), aber keine phonem-/tonebene Evaluation |
| RQ3 (Non-Native/L2 Speech) | Gering | Erwaehnt Akzente/Dialekte als ethische Herausforderung, aber keine L2-Evaluation |
| RQ4 (Vergleich mit dedizierten Systemen) | Hoch | Listet und beschreibt die Modelle (Whisper, SeamlessM4T, AudioPaLM), die wir vergleichen wollen |
| RQ5 (Fehlerstruktur) | Gering | Keine Analyse tonaler oder phonemischer Fehlerstrukturen |

### 3. Forschungsluecken (mit woertlichen Zitaten)

- **Luecke A:** Tokenisierung fuer tonale Sprachen ist ein ungeloestes Problem.
  > "The continuous nature of audio signals, speech variability, and background noise compound the challenge of tokenisation in Large Audio Models. [...] Multilingual speech tokenisation poses additional complexities as the same statement might demand a varying number of tokens in different languages." (S. 20)

- **Luecke B:** Paralinguistische Information (Emotion, Prosodie) wird von LAMs kaum verstanden.
  > "While LLMs excel in speech generation, their capacity to comprehend and generate various emotions remains largely untapped and understudied." (S. 21)

- **Luecke C:** Sprachspezifische Prompts fuer Audio-Modelle sind unerforscht.
  > "Despite the progress made, to our knowledge, the literature has yet to explore the design and testing of specialised prompts tailored specifically for speech-based scenarios." (S. 21)

- **Luecke D:** Halluzinationen in Audio-Modellen sind kaum untersucht.
  > "It is notable, however, that discussions around the hallucination challenge within Large Audio Models remain somewhat limited." (S. 21)

### 4. Adressierung durch meine Arbeit

| Luecke | Adressierung |
|---|---|
| Luecke 1 (Keine Phonem/Ton-Granularitaet bei Frontier-LLMs) | Dieses Survey zeigt, dass die gesamte LAM-Landschaft auf WER-Niveau evaluiert; wir gehen mit PER/TER darueber hinaus |
| Luecke 2 (Toene selten als separate Evaluationsachse) | Das Survey erwaehnt Toene nur im Kontext von TTS-Prosodie, nie als Evaluationskriterium -- genau unsere Luecke |
| Luecke 3 (Wenige Studien zu L2/Non-Native Speech) | Survey adressiert L2-Sprecher nicht; unser RQ3 fuellt diese Luecke |
| Luecke 4 (Kein systematischer Head-to-Head-Vergleich) | Survey beschreibt viele Modelle, aber vergleicht sie nicht systematisch auf derselben Aufgabe -- unser Design tut genau das |

---

## Paper 5: Speech Translation with Foundation Models and Large Language Models: A Survey
**Autoren:** Gaido, M. et al.
**Quelle:** arXiv:2402.12025v3, 2024

### 1. Zentrale Learnings

**Zusammenfassung:** Dieses Survey analysiert 9 Arbeiten zur Kombination von Speech Foundation Models (SFMs) mit Large Language Models (LLMs) fuer Speech-to-Text-Translation. Es identifiziert 5 architektonische Bausteine (SFM, Length Adapter, Modality Adapter, Prompt-Speech Mixer, LLM) und zeigt, dass fehlende standardisierte Trainings- und Evaluationsbedingungen faire Vergleiche verhindern. Die Autoren fordern standardisierte Benchmarks, semantische Metriken jenseits von BLEU und feinkoernige Evaluationen.

**Belegende Zitate:**

- "the lack of common experimental settings prevents the fair and direct comparison of different works" (implizit in Analyse, S. 7)

- "Therefore, we advocate for future research to adhere to established data-setting standards, paving the way for cumulative progress and shared understanding in the field." (S. 8)

- "the emergence of the SFM+LLM solution calls for thorough and fine-grained evaluations to investigate its peculiarities compared to other, more traditional methods." (S. 8)

- "the speech source contains a wide range of information that can be exploited depending on the paradigm used (e.g., prosody is not handled by cascade systems -- Zhou et al. 2024). As such, the ability of SFM+LLM models to leverage this information has to be investigated." (S. 8)

- "the well-known tendency of n-gram-based metrics to penalize translations generated by LLMs that are, in general, less literal" (S. 8)

### 2. Verwendbarkeit fuer unsere Arbeit

| Forschungsfrage | Relevanz | Erklaerung |
|---|---|---|
| RQ1 (Text-only Baseline) | Gering | Fokus auf Speech-to-Text Translation, nicht Pinyin-Konversion |
| RQ2 (Native-Speaker Transkription) | Mittel | Architektonischer Ueberblick ueber SFM+LLM-Integration ist relevant fuer das Verstaendnis unserer Modelle |
| RQ3 (Non-Native/L2 Speech) | Gering | Kein L2-Fokus |
| RQ4 (Vergleich mit dedizierten Systemen) | Sehr hoch | Die Forderung nach standardisierten Vergleichen und feinkoerniger Evaluation stuetzt direkt unsere Methodik |
| RQ5 (Fehlerstruktur) | Mittel | Forderung nach feinkoerniger Evaluation jenseits von BLEU ist analog zu unserer Forderung nach Evaluation jenseits von WER/CER |

### 3. Forschungsluecken (mit woertlichen Zitaten)

- **Luecke A:** Fehlende standardisierte Trainings- und Evaluationssettings.
  > "Therefore, we advocate for future research to adhere to established data-setting standards, paving the way for cumulative progress and shared understanding in the field." (S. 8)

- **Luecke B:** Feinkoernige Evaluation fehlt; Abhaengigkeit von BLEU ist problematisch.
  > "the emergence of the SFM+LLM solution calls for thorough and fine-grained evaluations to investigate its peculiarities compared to other, more traditional methods." (S. 8)

- **Luecke C:** Prosodie-Nutzung durch SFM+LLM-Modelle ist unerforscht.
  > "the speech source contains a wide range of information that can be exploited depending on the paradigm used (e.g., prosody is not handled by cascade systems). As such, the ability of SFM+LLM models to leverage this information has to be investigated." (S. 8)

- **Luecke D:** In-Context Learning fuer Speech Translation ist nicht gesichert.
  > "investigating whether and to what extent the integration of SFMs with LLMs transfers the ICL ability of the latter to the ST task is an important and interesting avenue for future studies." (S. 9)

### 4. Adressierung durch meine Arbeit

| Luecke | Adressierung |
|---|---|
| Luecke 1 (Keine Phonem/Ton-Granularitaet bei Frontier-LLMs) | Unsere feinkoernige PER/TER-Evaluation adressiert exakt die vom Paper geforderte "thorough and fine-grained evaluation" |
| Luecke 2 (Toene selten als separate Evaluationsachse) | Das Paper zeigt, dass Prosodie-Information (inkl. Ton) in der SFM+LLM-Evaluation fehlt; wir evaluieren Toene explizit |
| Luecke 3 (Wenige Studien zu L2/Non-Native Speech) | Nicht direkt vom Paper adressiert |
| Luecke 4 (Kein systematischer Head-to-Head-Vergleich) | Das Paper fordert explizit standardisierte Vergleiche -- unser Design mit identischen Bedingungen fuer 5-8 Modelle + Whisper setzt diese Forderung um |

---

## Paper 6: When LLMs Meet Speech: A Survey on Integration of Speech with Large Language Models
**Autoren:** Yang, Y. et al.
**Quelle:** Findings of ACL 2025

### 1. Zentrale Learnings

**Zusammenfassung:** Dieses ACL 2025 Findings Paper bietet eine systematische Taxonomie der Integration von LLMs mit Speech-Modalitaet. Es identifiziert drei Integrationsansaetze (text-basiert, latent-representation-basiert, audio-token-basiert) und vergleicht sie quantitativ ueber ASR, S2TT, S2ST und TTS hinweg. Die Autoren identifizieren Mehrsprachigkeit, fairen Vergleich und Echtzeit-Verarbeitung als zentrale offene Herausforderungen und betonen den English-zentrierten Bias der meisten Speech-LLM-Modelle.

**Belegende Zitate:**

- "Most existing speech-LLM models are designed to handle only English, which limits their range of applicability." (S. 17)

- "Only a small fraction of research efforts explicitly consider the multilingualism of pre-trained LLMs." (S. 17)

- "Beyond these integration-approach-specific challenges, there is a notable gap in comparing the different integration approaches under a unified setting. Most existing works focus on one approach, making it difficult to assess their relative merits consistently." (S. 17)

- "A fair comparison among these integration methods could clarify how different factors affect performance. Developing standardized benchmarks, protocols, and reporting practices for speech-LLM research would help future work isolate the core differences between these approaches." (S. 17)

- "However, numerous challenges remain in this evolving field, presenting significant opportunities for future research." (S. 20)

### 2. Verwendbarkeit fuer unsere Arbeit

| Forschungsfrage | Relevanz | Erklaerung |
|---|---|---|
| RQ1 (Text-only Baseline) | Mittel | Text-basierter Integrationsansatz (Kaskade: ASR -> LLM) ist relevant fuer unser Text-only-Baseline-Design |
| RQ2 (Native-Speaker Transkription) | Hoch | Quantitativer Vergleich von Speech-LLM-Ansaetzen ueber ASR-Aufgaben; zeigt aktuelle Leistungsgrenzen |
| RQ3 (Non-Native/L2 Speech) | Gering | Kein L2-Fokus im Survey |
| RQ4 (Vergleich mit dedizierten Systemen) | Sehr hoch | Fordert explizit faire Vergleiche unter einheitlichen Bedingungen -- genau unser Ansatz |
| RQ5 (Fehlerstruktur) | Gering | Keine feinkoernige phonem-/tonebene Fehleranalyse |

### 3. Forschungsluecken (mit woertlichen Zitaten)

- **Luecke A:** English-zentrierter Bias der meisten Speech-LLM-Modelle.
  > "Most existing speech-LLM models are designed to handle only English, which limits their range of applicability." (S. 17)

- **Luecke B:** Kein fairer Vergleich zwischen Integrationsansaetzen.
  > "there is a notable gap in comparing the different integration approaches under a unified setting. Most existing works focus on one approach, making it difficult to assess their relative merits consistently." (S. 17)

- **Luecke C:** Standardisierte Benchmarks und Protokolle fehlen.
  > "A fair comparison among these integration methods could clarify how different factors affect performance. Developing standardized benchmarks, protocols, and reporting practices for speech-LLM research would help future work isolate the core differences between these approaches." (S. 17)

- **Luecke D:** Mehrsprachige Faehigkeiten sind unterentwickelt.
  > "Only a small fraction of research efforts explicitly consider the multilingualism of pre-trained LLMs." (S. 17)

### 4. Adressierung durch meine Arbeit

| Luecke | Adressierung |
|---|---|
| Luecke 1 (Keine Phonem/Ton-Granularitaet bei Frontier-LLMs) | Das Paper zeigt, dass Speech-LLM-Forschung auf Satzebene (WER) evaluiert; wir gehen mit Phonem/Ton-Granularitaet weit darueber hinaus |
| Luecke 2 (Toene selten als separate Evaluationsachse) | Das Paper erwaehnt Toene nicht als Evaluationsdimension; unser TER adressiert diese blinde Stelle |
| Luecke 3 (Wenige Studien zu L2/Non-Native Speech) | English-zentrierter Bias betrifft nicht nur Sprachen, sondern auch Sprechertypen; unser L2-Fokus (RQ3) erweitert dies |
| Luecke 4 (Kein systematischer Head-to-Head-Vergleich) | Das Paper fordert "fair comparison [...] under a unified setting" -- genau das Design unserer Arbeit mit identischem Corpus (Tone Perfect) und off-the-shelf-Modellen |

---

## Zusammenfassende Einordnung aller 6 Papers

Die analysierten Papers bilden zusammen ein kohaerentes Bild der Forschungslandschaft, das die Relevanz unserer Masterarbeit deutlich unterstreicht:

1. **Paper 1 (Ahlawat et al.)** liefert den ASR-Hintergrund und die Metrik-Definitionen (PER, CER, WER), zeigt aber, dass die gesamte ASR-Literatur Toene nicht als separate Evaluationsachse behandelt.

2. **Paper 2 (Wang et al.)** ist das am staerksten verwandte Paper: Es zeigt phonem- und tonebene-MDD fuer Mandarin, aber nur mit spezialisierten Modellen (RNN-T + HuBERT) -- nicht mit Frontier-LLMs.

3. **Paper 3 (Kang & Xu)** liefert die phonetisch-linguistische Begruendung, warum Tonerkennung fundamental ist: CVT-Synchronie beweist, dass Ton kein "Add-on" ist, sondern integraler Bestandteil der Mandarin-Silbe.

4. **Paper 4 (Latif et al.)** kartiert die LAM-Landschaft und zeigt, dass tonale/phonemische Evaluation in keinem der beschriebenen Modelle systematisch durchgefuehrt wird.

5. **Paper 5 (Gaido et al.)** fordert explizit standardisierte, feinkoernige Evaluation fuer SFM+LLM-Modelle -- eine Forderung, die wir mit unserem Design umsetzen.

6. **Paper 6 (Yang et al.)** bestaetigt den English-zentrierten Bias und das Fehlen fairer Vergleiche unter einheitlichen Bedingungen -- beides zentrale Motivationen unserer Arbeit.

Insgesamt adressiert unsere Masterarbeit alle vier identifizierten Forschungsluecken systematisch: Wir evaluieren Frontier-LLMs auf Phonem- und Tonebene (Luecke 1), fuehren TER als dedizierte Ton-Metrik ein (Luecke 2), untersuchen optional L2-Sprecher (Luecke 3) und vergleichen 5-8 Modelle unter identischen Bedingungen (Luecke 4).


# Batch 3: Papers 13-18 (Recent Advances SLMs, Survey Speech LLMs, MMAU, Dynamic-SUPERB, ASR-EC Bench, PERL)


> **Thesis-Kontext:** "Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech" (Stefan Dosch, IU M.Sc. Data Science, Betreuer: Tim Schlippe)

---

## 1. Recent Advances in Speech Language Models: A Survey (Cui et al., 2025)

**Vollstaendiger Titel:** Recent Advances in Speech Language Models: A Survey
**Autoren:** Wenqian Cui, Dianzhi Yu, Xiaoqi Jiao, Ziqiao Meng, Guangyan Zhang, Qichao Wang, Yiwen Guo, Irwin King
**Quelle:** arXiv:2410.03751v4, August 2025

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Das Paper liefert den ersten umfassenden Ueberblick ueber Speech Language Models (SpeechLMs), die End-to-End-Sprachverarbeitung ohne den Umweg ueber Text ermoeglichen.
  - Die naive "ASR + LLM + TTS"-Pipeline wird als problematisch identifiziert wegen drei Kernproblemen: Informationsverlust (paralinguistische Merkmale wie Pitch, Timbre, Tonalitaet gehen verloren), signifikante Latenz und kumulative Fehler.
  - SpeechLMs nutzen Speech Tokenizer (semantische, akustische oder gemischte Tokenizer), Language Models und Vocoder als Drei-Komponenten-Architektur.
  - Das Paper unterscheidet zwischen semantischen Tokenizern (z.B. HuBERT, wav2vec 2.0) und akustischen Tokenizern (z.B. EnCodec) und betont deren unterschiedliche Staerken.
  - Text-Speech-Alignment verbessert semantische Modellierung, kann aber paralinguistische Merkmale wie Ton und Emotion beeintraechtigen.

* **Belegende Zitate:**
  - "Speech signals not only contain semantic information (i.e., the meaning of the speech) but also paralinguistic information (e.g., pitch, timbre, tonality, etc.). Putting a text-only LLM in the middle will cause the complete loss of paralinguistic information in the input speech" (Section I, Introduction)
  - "the meaning of speech can vary depending on tone" (Section I, Introduction)
  - "text primarily conveys semantic information, which can improve a SpeechLM's semantic modeling capabilities but may compromise its ability to capture paralinguistic features, such as tone and emotion, during alignment" (Section IV-B1, Text-Speech Alignment)
  - "While some studies have compared various component choices—primarily focusing on speech tokenizers—the comparisons tend to be limited in scope and depth. Consequently, there remains a significant gap in understanding the advantages and disadvantages of different component selections." (Section VII-A, Challenges)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Direkt relevant fuer die theoretische Einordnung der Thesis: Das Paper erklaert, warum End-to-End-Modelle paralinguistische Informationen wie Toene besser bewahren koennten als Pipeline-Ansaetze (relevant fuer RQ4, Vergleich mit Whisper).
  - Liefert die taxonomische Grundlage, um die Architekturunterschiede der getesteten Modelle (GPT-4o, Gemini etc.) einzuordnen und zu erklaeren, warum manche Modelle Toene besser erkennen als andere.
  - Die Erkenntnis, dass Text-Speech-Alignment Ton-Information kompromittieren kann, ist eine direkte theoretische Erklaerung fuer potentielle Schwaechen bei Ton-Transkription (relevant fuer RQ2, RQ5).

* **Stuetzendes Zitat:**
  - "text primarily conveys semantic information, which can improve a SpeechLM's semantic modeling capabilities but may compromise its ability to capture paralinguistic features, such as tone and emotion, during alignment" (Section IV-B1)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:**
  - Es fehlt ein systematischer Vergleich der verschiedenen Architekturkomponenten (Tokenizer, LM, Vocoder). Die Evaluierung beschraenkt sich auf High-Level-Metriken; eine Granularitaet auf Phonem- oder Ton-Ebene wird nicht adressiert. Die Faehigkeit von SpeechLMs, spezifische tonale und phonetische Merkmale zu erfassen, bleibt unerforscht.

* **Belegende Zitate:**
  - "there remains a significant gap in understanding the advantages and disadvantages of different component selections. Therefore, studies aimed at comprehensively comparing these choices are essential." (Section VII-A)
  - "the research in this area is still under explored. In this section, we survey challenges, unsolved questions, and possible directions for future research in the study of SpeechLMs." (Section VII, Challenges and Future Directions)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:**
  - Die Thesis adressiert Luecke 1 und Luecke 4 direkt: Sie fuehrt einen systematischen Head-to-Head-Vergleich multimodaler Frontier-Modelle auf Phonem- und Ton-Granularitaet durch -- genau die Evaluationstiefe, die dieses Survey als fehlend identifiziert. Die explizite Messung von TER (Tone Error Rate) prueft, ob die theoretisch vorhergesagte Kompromittierung paralinguistischer Features bei Text-Speech-Alignment empirisch nachweisbar ist.

---

## 2. A Survey on Speech Large Language Models for Understanding (Peng et al., 2025)

**Vollstaendiger Titel:** A Survey on Speech Large Language Models for Understanding
**Autoren:** Jing Peng, Yucheng Wang, Bohan Li, Yiwei Guo, Hankun Wang, YanGui Fang, Yu Xi, Haoyu Li, Xu Li, Ke Zhang, Shuai Wang, Kai Yu
**Quelle:** arXiv:2410.18908v6, November 2025

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Das Paper definiert Speech Understanding erstmals systematisch als multimodalen kognitiven Prozess mit drei Dimensionen: informationell (linguistisch, paralinguistisch, nicht-linguistisch), funktional (Perception, Shallow Cognition, Deep Cognition) und formatbezogen (Input-/Output-Struktur).
  - Zwei zentrale Herausforderungen aktueller Speech LLMs werden identifiziert: (1) Instruction Sensitivity -- Modelle reagieren stark auf Formulierungsaenderungen in Prompts; (2) Degradation der semantischen Reasoning-Faehigkeit -- die Integration der Sprachmodalitaet schwaecht die urspruenglichen Textverstaendnis-Faehigkeiten.
  - Speech LLMs haben Schwierigkeiten bei der zuverlaessigen Erfassung feingranularer akustischer Informationen und bei Deep Cognition Tasks.
  - ASR wird als "Perception Task" eingeordnet, waehrend die Thesis-Aufgabe (phonemische/tonale Transkription) an der Grenze zwischen Perception und Shallow Cognition liegt.

* **Belegende Zitate:**
  - "Speech LLMs often struggle to maintain robustness in the face of nuanced or ambiguous instructions. This sensitivity to instruction variability affects their ability to consistently generate accurate and contextually appropriate output." (Section IX, Future Exploration)
  - "as Speech LLMs are applied to increasingly complex tasks, their reasoning capabilities tend to degrade, particularly in understanding subtle, multi-step contexts" (Section IX, Future Exploration)
  - "Speech LLMs still have a long way to go in achieving true multitask generalizability, where a model can handle tasks it hasn't been explicitly trained on. At present, models often fail to perform certain tasks unless they have been specifically trained for them." (Section IX, Future Exploration)
  - "they often struggle to reliably capture and reason over acoustic information, leading to restricted performance in tasks that require fine-grained auditory understanding" (Section VIII, Challenges)
  - "Linguistic Information: Focuses on transcribing and understanding lexical and syntactic content from speech. Key tasks include Automatic Speech Recognition (ASR) and Spoken Language Translation (SLT), where challenges arise from homophones, dialectal variations, and transcription errors." (Section II-A)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Die Drei-Dimensionen-Taxonomie ist direkt nuetzlich fuer die theoretische Einordnung der Thesis: Mandarin-Ton-Transkription faellt in die informationelle Dimension (paralinguistisch: Prosodie/Toene), die funktionale Dimension (Perception) und die Format-Dimension (Structured Output: Pinyin-Transkription).
  - Die identifizierten Herausforderungen (Instruction Sensitivity, Reasoning Degradation) liefern einen Erklaerungsrahmen fuer RQ5 (Fehlerstruktur): Variationen in Prompt-Formulierungen koennten die Ton-Erkennungsleistung beeinflussen.
  - Die Erkenntnis, dass Speech LLMs bei "fine-grained auditory understanding" Schwaechen zeigen, stuetzt direkt die Forschungsluecke der Thesis (Luecke 1: Phonem-/Ton-Granularitaet unerforscht).
  - Relevant fuer RQ4: Das Survey zeigt, dass Whisper/ASR-basierte Pipelines bei semantischen Aufgaben weiterhin stark sind, aber bei paralinguistischer Information (zu der Toene gehoeren) Schwaechen haben.

* **Stuetzendes Zitat:**
  - "they often struggle to reliably capture and reason over acoustic information, leading to restricted performance in tasks that require fine-grained auditory understanding" (Section VIII, Challenges)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:**
  - Die feingranulare akustische Informationsextraktion aus Sprache bleibt unzureichend. Obwohl Speech LLMs Sprache verarbeiten koennen, ist ihre Faehigkeit, das volle Spektrum akustischer Merkmale wie Intonation und Sprecher-Emotion zuverlaessig zu erfassen, begrenzt. Die Anwendung auf tonale Sprachen und die explizite Evaluation auf Ton-Ebene werden nicht behandelt.

* **Belegende Zitate:**
  - "Although current models can process speech to some extent, their ability to capture the full range of acoustic cues, such as speaker emotion, intonation, and environmental noise, remains limited. Exploring methods to improve the extraction of acoustic information and incorporating these features more effectively into the reasoning process will be key to enabling more nuanced and context-aware speech understanding." (Section IX, Future Exploration)
  - "integrating the speech modality leads to less satisfactory end-to-end transcription results" (Section VIII-B)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:**
  - Die Thesis adressiert Luecke 1 und Luecke 2 direkt: Sie evaluiert multimodale LLMs auf genau der feingranularen akustischen Ebene (Phoneme, Toene), die das Survey als untererforscht identifiziert. Die Verwendung von Mandarin-Toenen als Evaluationsziel prueft die Faehigkeit der Modelle, paralinguistische/prosodische Information zu extrahieren -- eine explizite Testung der im Survey genannten Schwaeche. Zudem wird Instruction Sensitivity indirekt adressiert, da die Thesis mit standardisierten Prompts arbeitet.

---

## 3. MMAU: A Massive Multi-Task Audio Understanding and Reasoning Benchmark (Sakshi et al., 2024)

**Vollstaendiger Titel:** MMAU: A Massive Multi-Task Audio Understanding and Reasoning Benchmark
**Autoren:** S Sakshi, Utkarsh Tyagi, Sonal Kumar, Ashish Seth, Ramaneswaran Selvakumar, Oriol Nieto, Ramani Duraiswami, Sreyan Ghosh, Dinesh Manocha
**Quelle:** arXiv:2410.19168v1, Oktober 2024

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - MMAU ist ein Benchmark mit 10.000 Multiple-Choice-Fragen ueber drei Audio-Domaenen (Speech, Sound, Music), der 27 verschiedene Skills mit Informationsextraktion und Reasoning testet.
  - Selbst die besten Modelle erreichen nur ~53% Accuracy (Gemini Pro v1.5: 52.97%), waehrend Menschen 82% erzielen -- eine massive Luecke.
  - Kaskadierte Ansaetze (Audio-Captioning + LLM) uebertreffen direkte Audio-LLMs und erreichen bis zu 59% mit GPT-4o.
  - Modelle performen am besten bei Sound und am schlechtesten bei Speech-Reasoning, was zeigt, dass Sprachverstaendnis ueber reinen Inhalt hinaus eine Herausforderung bleibt.
  - Einige Modelle (MuLLaMa, SALMONN) nutzen den Audio-Input kaum -- ihre Performance aendert sich wenig, wenn Audio durch Rauschen ersetzt wird.
  - MMAU testet u.a. "Phonemic Stress Pattern Analysis" und "Emotional Tone Interpretation" als spezifische Skills.

* **Belegende Zitate:**
  - "even the most advanced Gemini Pro v1.5 achieves only 52.97% accuracy, and the state-of-the-art open-source Qwen2-Audio achieves only 52.50%, highlighting considerable room for improvement" (Abstract)
  - "Models perform best on sound and worst on speech. With average scores of 18%, 30%, 23% across speech, sound, and music, models perform best on sound-related tasks and struggle the most with music." (Section 5.1)
  - "reasoning over spoken language—especially perception beyond mere content—remains a challenge" (Section 5.1)
  - "Captioning audios first and then prompting LLMs yields the best results. Enhancing the quality of the captions further improves overall performance." (Section 5.1)
  - "the performance of MuLLaMa and SALMONN remains largely unaffected, indicating that these models may not always rely on the audio input to generate responses" (Section 5.2)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - MMAU bestaetigt empirisch, dass aktuelle Modelle bei Speech-Tasks besonders schlecht abschneiden und bei "perception beyond mere content" scheitern (relevant fuer alle RQs, insb. RQ2).
  - Der Befund, dass kaskadierte Ansaetze (Caption + LLM) besser abschneiden als direkte Audio-LLMs, ist relevant fuer RQ4 (Vergleich mit Whisper): Die Thesis kann pruefen, ob Whisper+LLM auch bei Ton-Transkription besser performt als End-to-End-Modelle.
  - Die Erkenntnis, dass manche Modelle Audio-Input kaum nutzen, liefert eine wichtige methodische Warnung fuer die Thesis-Evaluation.
  - Die Skills "Phonemic Stress Pattern Analysis" und "Emotional Tone Interpretation" sind thematisch verwandt mit der Thesis, werden aber nicht auf Mandarin-Toene angewendet.

* **Stuetzendes Zitat:**
  - "reasoning over spoken language—especially perception beyond mere content—remains a challenge" (Section 5.1)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:**
  - MMAU evaluiert nur Multiple-Choice-Aufgaben und keine offene Generierung (wie Pinyin-Transkription). Der Benchmark enthaelt keine sprachspezifischen Tasks fuer tonale Sprachen. Phonemische Analyse beschraenkt sich auf Stress-Muster, nicht auf Ton-Identifikation. Keine Evaluation auf Non-Native/L2-Sprache.

* **Belegende Zitate:**
  - "MMAU focuses on multiple-choice tasks and does not evaluate open-ended generation, which allows models to reason more freely and exhibit errors such as language hallucinations. Including open-ended tasks will help us better understand these kinds of errors." (Section 6, Conclusion, Limitations and Future Work)
  - "As part of future work, we aim to address in future iterations of MMAU: [...] Lastly, we plan to broaden the range of tasks and skills covered by MMAU to enhance its challenge and relevance to future models." (Section 6)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:**
  - Die Thesis adressiert Luecke 1 und Luecke 2: Sie evaluiert Open-Ended-Generierung (Pinyin-Transkription statt Multiple-Choice), was MMAU als eigene Limitation nennt. Zudem testet sie spezifisch auf Mandarin-Toene -- eine phonetische Dimension, die in MMAU nicht abgedeckt ist. Die Thesis geht auch ueber MMAU hinaus durch die L2/Non-Native-Evaluation (Luecke 3) und den systematischen Head-to-Head-Vergleich auf Ton-Ebene (Luecke 4).

---

## 4. Dynamic-SUPERB Phase-2 (Huang et al., 2024)

**Vollstaendiger Titel:** Dynamic-SUPERB Phase-2: A Collaboratively Expanding Benchmark for Measuring the Capabilities of Spoken Language Models with 180 Tasks
**Autoren:** Chien-yu Huang, Wei-Chih Chen, Shu-wen Yang et al.
**Quelle:** arXiv:2411.05361v1, November 2024

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Dynamic-SUPERB Phase-2 ist der groesste Benchmark fuer universelle Sprachmodelle mit 180 Tasks ueber Speech, Music und Audio, mit einer detaillierten Taxonomie.
  - Die Taxonomie umfasst einen expliziten Bereich "Phonetics, Phonology, and Prosody" mit Tasks wie Phoneme Recognition, Pronunciation Evaluation und Third Tone Sandhi Recognition (Mandarin).
  - Kein Modell dominiert ueber alle Tasks: SALMONN-13B ist gut bei English ASR (WER 2.8), WavLLM bei Emotion Recognition (79.1% Acc), aber alle versagen bei Query-by-Example und Speaker Diarization.
  - Whisper-LLaMA (kaskadierter Ansatz) bleibt in Speech Recognition und Spoken Language Understanding unbesiegt.
  - Modelle zeigen sehr schlechte Performance bei Phoneme Recognition: Nur SALMONN erreicht akzeptable PER-Werte (~25), alle anderen liegen bei ~100 PER oder hoeher.

* **Belegende Zitate:**
  - "Phonetics, Phonology, and Prosody focuses on the sound structure of speech, including phoneme recognition, pronunciation evaluation, and prosodic features like stress and accent classification." (Section 3.4.1)
  - "In phoneme recognition (PR), the SALMONN models were the only ones to achieve a relatively lower phoneme error rate (PER) compared to the others." (Section 5.2)
  - "No single model excels across all tasks." (Section 5.2)
  - "in the phonetics and prosody domain, WavLLM and LTU-AS exhibit poor scores due to their erroneous outputs in phone/phoneme segment counting tasks, predicting thousands of segments in utterances lasting only seconds" (Section 5.1)
  - "using ASR such as Whisper remains a strong baseline for language understanding, as text more explicitly represents semantic information than speech" (Section 5.1)
  - "The recent models show good performance on specific tasks but poor generalization across common tasks like those in SUPERB, highlighting the need for further research on universal models." (Section 6, Conclusions)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Hoechst relevant fuer RQ2 und RQ4: Dynamic-SUPERB enthaelt explizite Phoneme-Recognition-Tasks und zeigt, dass universelle Modelle dort dramatisch versagen (PER ~100). Dies stuetzt die Hypothese der Thesis, dass Phonem-/Ton-Granularitaet eine ungeloeste Herausforderung ist.
  - Der Task "Third Tone Sandhi Recognition" auf dem NCCU Corpus of Spoken Taiwan Mandarin ist direkt thematisch verwandt mit der Thesis (Mandarin-Toene).
  - Die Erkenntnis, dass Whisper-basierte Kaskaden bei Speech Recognition weiterhin dominieren, ist direkt relevant fuer RQ4.
  - Die Taxonomie kann als Referenzrahmen fuer die Einordnung der Thesis-Tasks verwendet werden.

* **Stuetzendes Zitat:**
  - "In phoneme recognition (PR), the SALMONN models were the only ones to achieve a relatively lower phoneme error rate (PER) compared to the others." (Section 5.2)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:**
  - Trotz 180 Tasks und einer Phonetik-Kategorie bleibt die Evaluation auf Ton-Ebene fuer tonale Sprachen minimal (nur Third Tone Sandhi als Klassifikationstask). Es gibt keine separate Ton-Error-Rate-Metrik. Die meisten Frontier-Modelle (GPT-4o, Gemini) werden nicht evaluiert. Die Evaluation beschraenkt sich auf Instruction-basierte Open-Source-Modelle.

* **Belegende Zitate:**
  - "Although Dynamic-SUPERB Phase-2 is the largest and most comprehensive benchmark, we acknowledge its limitations. It lacks comprehensive speech-generation tasks, as Phase-2 focused on understanding tasks due to the few universal generation models." (Section 6, Limitations)
  - "Despite our efforts to develop the task taxonomy scientifically, new domains may emerge as the benchmark grows, and tasks can be categorized in various ways." (Section 6, Limitations)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:**
  - Die Thesis adressiert Luecke 1, 2 und 4: Sie geht ueber Dynamic-SUPERB hinaus, indem sie (a) Frontier-Modelle wie GPT-4o und Gemini evaluiert (die in Dynamic-SUPERB nicht getestet werden), (b) eine dedizierte Ton-Error-Rate als separate Evaluationsachse einfuehrt (Luecke 2), und (c) die Evaluation auf Mandarin-spezifische Phonem- und Ton-Granularitaet fokussiert, statt nur einen einzelnen Tone-Sandhi-Klassifikationstask zu verwenden.

---

## 5. ASR-EC Benchmark: Evaluating Large Language Models on Chinese ASR Error Correction (Wei et al., 2024)

**Vollstaendiger Titel:** ASR-EC Benchmark: Evaluating Large Language Models on Chinese ASR Error Correction
**Autoren:** Victor Junqiu Wei, Weicheng Wang, Di Jiang, Yuanfeng Song, Lu Wang
**Quelle:** arXiv:2412.03075v1, Dezember 2024

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Das Paper erstellt den ersten chinesischen ASR-Error-Correction-Benchmark (ASR-EC) basierend auf vier Korpora (THCHS-30, AISHELL-1, AISHELL-2, WeNetSpeech) mit zwei ASR-Pipelines (Kaldi-K1 DNN-HMM, Kaldi-K2 Zipformer-Transducer).
  - Drei Paradigmen fuer LLM-basierte Fehlerkorrektur werden systematisch verglichen: Prompting (ineffektiv, verschlechtert CER), Finetuning (moderat effektiv mit LoRA), und Multimodal Augmentation (am effektivsten, nutzt Audio + Text gemeinsam).
  - Prompting fuehrt zu Overcorrection -- LLMs korrigieren auch korrekte Saetze, was die CER verschlechtert.
  - Multimodales Finetuning von Qwen-Audio mit dem ASR-EC-Datensatz erreicht die besten Ergebnisse (CER von 3.24 auf Mixed Utterances vs. 12.42 Baseline).
  - Manche Fehlertypen (Eigennamen, Pronomen) koennen auch mit multimodalen Ansaetzen nicht korrigiert werden, da Kontext und Vorwissen fehlen.

* **Belegende Zitate:**
  - "Prompting is not effective for ASR error correction. Finetuning is effective only for a portion of LLMs. Multi-modal augmentation is the most effective method for error correction and achieves state-of-the-art performance." (Abstract)
  - "Under the zero-shot and one-shot settings, LLMs tend to correct every sentence, regardless of whether the sentence has errors or not, and thus, the CER actually increased after correction by LLMs." (Section 7.2)
  - "LLMs still face challenges in correcting errors requiring deep contextual understanding" (Section 8, Conclusion)
  - "some errors in the hypothesis texts outputted by the ASR system can not be corrected by LLMs due to the lack of context and prior knowledge" (Section 7.4)
  - "To the best of our knowledge, there are no existing benchmarks for Chinese ASR error correction, even though Chinese is one of the most popular languages and has a large number of users in the world." (Section 2)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Direkt relevant fuer RQ1 (Text-only Baseline) und RQ4 (Vergleich mit dedizierten Systemen): Das Paper zeigt, dass Zero-Shot-Prompting fuer Chinese ASR Error Correction versagt -- ein Warnsignal fuer die Thesis, dass auch Ton-Transkription per Prompting allein schwierig sein koennte.
  - Die Erkenntnis, dass multimodale Ansaetze (Audio + Text) deutlich besser funktionieren als rein textbasierte, stuetzt die Motivation der Thesis, multimodale Modelle (nicht nur Text-LLMs) zu evaluieren.
  - Die verwendeten Korpora (AISHELL-1, AISHELL-2) ueberschneiden sich mit dem Mandarin-Sprachraum der Thesis und liefern Vergleichswerte fuer CER.
  - Die Fehleranalyse (Substitution, Deletion, Insertion) ist methodisch verwandt mit der Thesis-Metrik (PER, TER).

* **Stuetzendes Zitat:**
  - "Multi-modal augmentation stands out as the most effective approach, significantly enhancing error correction by jointly analyzing the audio and its corresponding transcript, thereby achieving state-of-the-art performance in correcting ASR errors." (Section 1, Introduction)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:**
  - Der Benchmark fokussiert ausschliesslich auf Character-Level-Fehlerkorrektur (CER) und evaluiert nicht auf Phonem- oder Ton-Ebene. Die Fehleranalyse differenziert nicht zwischen phonetisch motivierten Fehlern (z.B. Tonverwechslungen) und semantischen Fehlern. Frontier-Modelle wie GPT-4o/Gemini werden nicht als multimodale Audio-Modelle getestet, sondern nur als Text-LLMs.

* **Belegende Zitate:**
  - "LLMs still face challenges in correcting errors requiring deep contextual understanding" (Section 8, Conclusion)
  - "some errors in the hypothesis texts outputted by the ASR system can not be corrected by LLMs due to the lack of context and prior knowledge. Therefore, there is a minimum threshold CER that LLMs' error correction cannot surpass." (Section 7.4)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:**
  - Die Thesis adressiert Luecke 1 und Luecke 2: Waehrend ASR-EC nur CER auf Character-Ebene misst, fuehrt die Thesis separate Metriken fuer Phoneme (PER) und Toene (TER) ein. Die Thesis analysiert zudem explizit, welche Tonverwechslungen am haeufigsten auftreten (RQ5) -- eine phonetisch motivierte Fehleranalyse, die ASR-EC nicht bietet. Ausserdem werden in der Thesis Frontier-Modelle direkt als Audio-zu-Pinyin-Transkribenten getestet, nicht nur als Text-Error-Correctors.

---

## 6. PERL: Pinyin Enhanced Rephrasing Language Model for Chinese ASR N-Best Error Correction (Liang & Zhang, 2025)

**Vollstaendiger Titel:** PERL: Pinyin Enhanced Rephrasing Language Model for Chinese ASR N-Best Error Correction
**Autoren:** Junhong Liang, Bojun Zhang
**Quelle:** arXiv:2412.03230v2, September 2025

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - PERL ist eine Pipeline, die semantische (BERT) und phonetische (Pinyin-Encoder) Embeddings gemeinsam modelliert, um N-Best-ASR-Hypothesen fuer Mandarin-Chinesisch zu korrigieren.
  - Kernkomponenten: (1) vortrainierter Pinyin-Encoder (GRU + Transformer), der Zeichen auf phonetische Embeddings abbildet, (2) Length Predictor fuer die korrekte Ausgabelaenge, (3) Pinyin-Enhanced Rephrasing Model, das semantische und phonetische Features ueber gelerntes Gating fusioniert.
  - PERL erreicht 29.11% CER-Reduktion auf Aishell-1 und ~70% auf domainspezifischen Datensaetzen (DoAD: Recht, Medizin, Verwaltung).
  - Mandarin-ASR-Fehler folgen phonetischen Mustern, da viele Zeichen aehnliche Aussprachen (Pinyin) teilen.
  - LLMs (GPT-4o, DeepSeek, Qwen2.5) performen schlechter als PERL auf DoAD, da generative Modelle bei laengenkonstrained Aufgaben Schwaechen haben.
  - Ablationsstudie zeigt: Entfernung des Pinyin-Encoders oder des Length-Predictors verschlechtert die Performance signifikant.

* **Belegende Zitate:**
  - "In Mandarin Chinese, ASR errors often follow phonetic patterns, as many characters share similar pronunciations. This is closely related to the phonetic system of Chinese, Pinyin, a Romanized representation of character pronunciations." (Section 1, Introduction)
  - "Existing Chinese ASR correction methods have not effectively utilized Pinyin information, a unique feature of the Chinese language." (Abstract)
  - "PERL could calculate the correct length and incorporate Pinyin knowledge to recover the wrong token 松 (song, pine) into 宗 (zong, sect) since they share similar Pinyin." (Section 4.2, Case Study)
  - "LLMs perform worse than PERL on DoAD. We attribute this to the inherent limitations of generative models when handling tasks with length constraints, where noisy inputs often mislead them into producing erroneous outputs." (Section 4.1)
  - "removing the Pinyin encoder or relying solely on the N-best results significantly degrades performance" (Section 4.2, Ablation)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Hoechst relevant fuer RQ1 (Text-only Baseline: Character zu Pinyin) und RQ5 (Fehlerstruktur): PERL demonstriert, dass Pinyin-Information zentral fuer die Korrektur phonetischer Fehler im Mandarin ist. Die Thesis kann PERL als Referenzpunkt fuer die Frage nutzen, ob multimodale LLMs implizit aehnliche phonetische Muster lernen.
  - Die Erkenntnis, dass ASR-Fehler im Mandarin phonetischen Mustern folgen (gleicher Pinyin, verschiedene Zeichen), ist direkt relevant fuer RQ5 (welche Phonem-/Tonkontraste werden verwechselt).
  - PERL zeigt, dass explizite Pinyin-Embeddings LLMs ueberlegen sind -- dies liefert einen Kontrastpunkt fuer die Thesis-Frage, ob Frontier-Modelle phonetisches Wissen implizit durch Training erworben haben.
  - Die Verwendung von Aishell-1 und Whisper als ASR-Backend ist direkt vergleichbar mit dem Thesis-Setup.

* **Stuetzendes Zitat:**
  - "In Mandarin Chinese, ASR errors often follow phonetic patterns, as many characters share similar pronunciations. This is closely related to the phonetic system of Chinese, Pinyin, a Romanized representation of character pronunciations." (Section 1, Introduction)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:**
  - PERL fokussiert auf Error Correction (Post-Processing) und evaluiert nicht die direkte Transkriptionsfaehigkeit multimodaler Modelle. Toene werden als Teil des Pinyin implizit behandelt, aber nicht als separate Evaluationsachse gemessen. Die Methode setzt Finetuning voraus und evaluiert nicht die Off-the-Shelf-Faehigkeiten von Frontier-Modellen. Non-Native/L2-Sprache wird nicht betrachtet.

* **Belegende Zitate:**
  - "balancing phonetic and semantic information under strict length constraints remains an open challenge" (Section 1, Introduction)
  - "PERL integrates a Pinyin module to reduce phonetic errors and a length predictor to address sequence inconsistencies in N-best Chinese ASR error correction." (Section 5, Discussion & Conclusion)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:**
  - Die Thesis adressiert alle vier Luecken: (1) Sie evaluiert direkte Transkription statt Post-Correction (Luecke 1), (2) sie fuehrt Toene als explizite, separate Evaluationsachse ein (Luecke 2), (3) sie testet Off-the-Shelf-Modelle ohne Finetuning (Luecke 4), und (4) sie untersucht L2/Non-Native-Sprache (Luecke 3). Waehrend PERL zeigt, dass Pinyin-Wissen fuer ASR-Correction kritisch ist, testet die Thesis, ob Frontier-Modelle dieses Wissen ohne explizite Pinyin-Embeddings leisten koennen.


# Batch 4: Papers 19-24 (VoxEval, FireRedASR, Landscape SLMs, Kimi-Audio, Holistic Eval LALMs, ZIPA)


---

## Paper 19: VoxEval: Benchmarking the Knowledge Understanding Capabilities of End-to-End Spoken Language Models
**Autoren:** Wenqian Cui, Xiaoqi Jiao, Ziqiao Meng, Irwin King
**Jahr:** 2025
**Quelle:** arXiv:2501.04962v4

### 1. Zentrale Learnings

**Learning 1: End-to-End SLMs versagen bei Wissensverständnis und liegen meist unter der Zufallsbaseline**
- Belegende Zitate:
  - "Results reveal that current SLMs perform poorly, with most failing to surpass random guessing. Since VoxEval questions have four answer choices, random guessing yields an expected score of 25%. However, only GLM-4-Voice exceeds this baseline, indicating that SLMs often struggle to follow instructions and select the correct answer." (Section 4.2)
  - "VoxEval presents significant challenges to current SLMs, revealing their sensitivity to varying audio conditions and highlighting the need to enhance reasoning capabilities in future development." (Abstract)
- Relevanz: Dieses Learning quantifiziert die fundamentalen Grenzen aktueller SLMs bei Wissensaufgaben. Wenn SLMs bereits bei allgemeinem Faktenwissen scheitern, ist zu erwarten, dass spezialisierte phonetische Aufgaben wie Mandarin-Tontranskription ebenfalls erhebliche Herausforderungen darstellen werden. Dies kontextualisiert die off-the-shelf Evaluierung in der Masterarbeit (RQ1).

**Learning 2: SLMs sind hochsensitiv gegenüber variierenden Audio-Eingabebedingungen, besonders Tonhöhenverschiebungen**
- Belegende Zitate:
  - "SLMs are susceptible to different input audio conditions. Although SLM performances do not vary significantly across different speaker voices, variations in speaking styles and audio quality can cause up to a 6% performance drop." (Section 4.2, RQ3)
  - "Different input audio conditions have different impacts on SLMs. For instance, environment acoustics and linguistic variations have minimal impact on SLMs' performance, whereas pitch shifts pose the greatest challenge for most SLMs." (Section 4.2, RQ3)
- Relevanz: Pitch-Sensitivität ist direkt relevant für die Mandarin-Tonerkennung (RQ2, RQ3), da Mandarin-Töne fundamentale Pitch-Konturen sind. Wenn Pitch-Shifts bereits bei englischsprachigen Wissens-QA die grösste Herausforderung darstellen, ist eine systematische Untersuchung der Tonerkennungsfähigkeiten besonders wichtig.

**Learning 3: Chain-of-Thought (CoT) Prompting reduziert die Leistung von SLMs, anders als bei Text-LLMs**
- Belegende Zitate:
  - "Using CoT reduces the performance of SLMs. We observe this trend consistently across all tested models. This suggests that CoT does not enhance SLMs in the same way it improves TLMs, emphasizing the need for specialized techniques to improve SLM reasoning." (Section 4.2, RQ4)
  - "Existing SLMs perform similarly across different levels of math questions. [...] This counterintuitive fact suggests that most SLMs are unlikely to possess basic math reasoning abilities." (Section 4.2, RQ4)
- Relevanz: Dies informiert die Prompt-Strategie der Masterarbeit. Wenn CoT bei SLMs kontraproduktiv ist, muss die Evaluierung verschiedener Prompting-Strategien für die phonetische Transkription sorgfältig gestaltet werden (RQ1, RQ4).

### 2. Verwendbarkeit für unsere Arbeit

**Direkt verwendbare Elemente:**
- Die 26 Input-Audio-Bedingungen (6 Sprecher, linguistische Variationen, Pitch-Shifts, Speed-Changes, Noise, Environment-Acoustics) bieten ein Framework für die Robustheitsevaluierung der Mandarin-Tontranskription (RQ3)
- GLM-4-Voice als bester SLM (37-55% Accuracy) ist ein relevantes Vergleichsmodell für die off-the-shelf Evaluierung
- Die Erkenntnis, dass end-to-end Speech-to-Speech Evaluierung signifikant schwieriger ist als Speech-to-Text, kontextualisiert die Wahl zwischen verschiedenen Evaluierungsmodalitäten

**Methodische Übertragbarkeit:**
- VoxEval nutzt Whisper-large-v3 als ASR-Zwischenschritt zur Evaluierung gesprochener Antworten -- ähnlich könnte die Masterarbeit Whisper als Baseline für die CER-Evaluierung verwenden
- Die systematische Auswertung über verschiedene Input-Bedingungen (Sprecher, Stil, Qualität) könnte auf das Tone-Perfect-Korpus übertragen werden

### 3. Forschungslücken (identifiziert im Paper)

- **Lücke A:** "existing question-answering (QA) benchmarks fall short in evaluating SLMs' knowledge understanding due to their inability to support end-to-end speech evaluation and account for varied input audio conditions" (Abstract) -- VoxEval adressiert QA, aber keine phonetische/tonale Transkription
- **Lücke B:** "the benchmark's reliance on synthetic speech generation through TTS systems may not fully capture the complexity and variability of natural human speech, including emotional expressions, regional dialects, and spontaneous speech patterns that are difficult to replicate artificially" (Section 6, Limitations)
- **Lücke C:** Keine Evaluierung tonaler Sprachen oder phonetischer Merkmale -- VoxEval ist ausschliesslich auf Englisch und inhaltliches Wissensverständnis fokussiert

### 4. Adressierung durch meine Arbeit

| Forschungslücke | Bezug zu meiner Arbeit |
|---|---|
| Lücke A: Keine phonetische Evaluierung von SLMs | Meine Arbeit evaluiert erstmals systematisch die phonetische und tonale Transkriptionsfähigkeit multimodaler Foundation Models für Mandarin (RQ1, RQ2), was über reine Wissens-QA hinausgeht |
| Lücke B: Synthetische vs. natürliche Sprache | Meine Arbeit nutzt das Tone-Perfect-Korpus mit natürlichen Mandarin-Aufnahmen statt TTS-generierter Sprache, was die ökologische Validität erhöht |
| Lücke C: Keine tonalen Sprachen evaluiert | Meine Arbeit fokussiert explizit auf Mandarin als tonale Sprache mit Pinyin+Tonnummern als Transkriptionsformat und misst TER, PER und CER (RQ2, RQ3) |
| Pitch-Sensitivität als grösste Herausforderung | Meine Arbeit untersucht systematisch, wie gut Modelle Mandarin-Töne (= Pitch-Konturen) erkennen (RQ2), was direkt an VoxEvals Befund zur Pitch-Sensitivität anknüpft (Lücke 2) |

---

## Paper 20: FireRedASR: Open-Source Industrial-Grade Mandarin Speech Recognition Models
**Autoren:** Kai-Tuo Xu, Feng-Long Xie, Xu Tang, Yao Hu (Xiaohongshu Inc.)
**Jahr:** 2025
**Quelle:** arXiv:2501.14350v1

### 1. Zentrale Learnings

**Learning 1: Professionell transkribierte Daten sind entscheidend für hohe ASR-Genauigkeit bei Mandarin**
- Belegende Zitate:
  - "Unlike weakly-labeled datasets used in Whisper, the majority of our data was manually transcribed by professional annotators, ensuring high transcription accuracy and reliability." (Section 2.1)
  - "Our empirical studies demonstrate that one thousand hours of high-quality, human-labeled data yields better results than ten thousand hours of weakly-labeled data (e.g., from video captions, OCR results, or ensemble ASR outputs), explaining our advantage over Whisper-like models." (Section 4, Discussion)
- Relevanz: Dies erklärt, warum Whisper bei Mandarin (9.86% CER) deutlich schlechter abschneidet als spezialisierte Modelle, und ist direkt relevant für die Interpretation der Whisper-Baseline in der Masterarbeit (RQ1).

**Learning 2: FireRedASR-LLM erreicht mit 3.05% CER den State-of-the-Art auf Mandarin-Benchmarks und übertrifft Whisper um ein Vielfaches**
- Belegende Zitate:
  - "FireRedASR-LLM (8.3B parameters) achieves an average Character Error Rate (CER) of 3.05%, surpassing the latest SOTA of 3.33% with an 8.4% relative CER reduction (CERR)." (Abstract)
  - "FireRedASR-AED achieves a 29%-68% CERR with fewer parameters than Whisper-Large-v3, SenseVoice-L, and Qwen-Audio." (Section 3.1)
- Relevanz: Diese CER-Werte setzen den Massstab für Mandarin-ASR und kontextualisieren die CER-Ergebnisse der Masterarbeit. Das enorme Gefälle zwischen spezialisierten Mandarin-Modellen (3.05% CER) und multilingualen Modellen wie Whisper (9.86% CER) zeigt die Herausforderung für off-the-shelf Frontier-Modelle bei Mandarin (RQ1).

**Learning 3: Die Encoder-Adapter-LLM-Architektur ermöglicht die Integration von Sprachverarbeitungsfähigkeiten in vortrainierte Text-LLMs**
- Belegende Zitate:
  - "FireRedASR-LLM is also an end-to-end ASR model but designed to integrate robust speech processing capabilities of FireRedASR-AED with the superior language capabilities of LLM. It comprises three core components: a Conformer-based audio Encoder, a lightweight audio-text alignment Adapter and a pre-trained text-based LLM, forming what we term the Encoder-Adapter-LLM architecture." (Section 2.2)
  - "The LLM component of FireRedASR-LLM is initialized with pre-trained weights from Qwen2-7B-Instruct, a notable open-source LLM." (Section 2.2)
- Relevanz: Diese Architektur (Encoder-Adapter-LLM) ist repräsentativ für die in der Masterarbeit evaluierten multimodalen Foundation Models wie Qwen-Audio, die ähnliche Architekturen verwenden (RQ1, Lücke 1).

### 2. Verwendbarkeit für unsere Arbeit

**Direkt verwendbare Elemente:**
- Whisper-Large-v3 Ergebnis auf Mandarin (9.86% Average CER über vier Benchmarks) als Referenzwert für die Whisper-Baseline in der Masterarbeit
- Die CER-Metrik und die Mandarin-Benchmarks (AISHELL-1, AISHELL-2, WenetSpeech) als Vergleichsrahmen
- Die Erkenntnis, dass Datenqualität > Datenquantität für Mandarin-ASR gilt, kontextualisiert die Limitationen von off-the-shelf Modellen

**Methodische Übertragbarkeit:**
- Die Verwendung von CER für Chinesisch und WER für Englisch als Standard-Metrik-Trennung
- Die Evaluierung über mehrere diverse Testsets (Standard-Benchmarks + Multi-Source + Singing) als methodisches Vorbild für eine umfassende Evaluierung

### 3. Forschungslücken (identifiziert im Paper)

- **Lücke A:** FireRedASR evaluiert ausschliesslich Character-Level Accuracy (CER), aber keine phonetische oder tonale Transkription -- Pinyin-Transkription, Ton-Accuracy oder phonemische Analyse fehlen vollständig
- **Lücke B:** "Future work will focus on further improving performance and expanding support for more languages and varied tasks." (Section 5) -- Aktuell ist das Modell auf Mandarin-Zeichenerkennung fokussiert ohne phonetische Dekomposition
- **Lücke C:** Keine Evaluierung von Frontier-LLMs (GPT-4o, Gemini etc.) in einer off-the-shelf Konfiguration -- nur spezialisierte ASR-Modelle werden verglichen

### 4. Adressierung durch meine Arbeit

| Forschungslücke | Bezug zu meiner Arbeit |
|---|---|
| Lücke A: Keine phonetische/tonale Evaluierung | Meine Arbeit geht über CER hinaus und evaluiert PER, TER sowie Pinyin-mit-Tonnummern-Genauigkeit, was eine tiefere Analyse der Sprachverarbeitungsfähigkeiten ermöglicht (RQ2, RQ3) |
| Lücke B: Keine phonetische Dekomposition | Meine Arbeit dekomponiert die Transkription in Initialen, Finalen und Töne, um zu verstehen, wo genau Modelle versagen (RQ2, Lücke 2) |
| Lücke C: Keine off-the-shelf Frontier-LLM-Evaluierung | Meine Arbeit evaluiert ~5-8 Frontier-Modelle (GPT-4o, Gemini, Qwen-Audio etc.) + Whisper ohne Finetuning, was die Fähigkeiten allgemeiner multimodaler Modelle für Mandarin offenlegt (RQ1, Lücke 1) |
| FireRedASR als Mandarin-SOTA-Referenz | Die 3.05% CER von FireRedASR-LLM dient als obere Referenz für spezialisierte Systeme, gegen die die off-the-shelf Frontier-Modelle kontextualisiert werden können (Lücke 3) |

---

## Paper 21: On The Landscape of Spoken Language Models: A Comprehensive Survey
**Autoren:** Siddhant Arora, Kai-Wei Chang, Chung-Ming Chien, Yifan Peng, Haibin Wu et al.
**Jahr:** 2025
**Quelle:** arXiv:2504.08528v1

### 1. Zentrale Learnings

**Learning 1: SLMs lassen sich in drei Kategorien einteilen: Pure Speech LMs, Speech+Text LMs und Speech-aware Text LMs -- mit unterschiedlichen Stärken und Limitationen**
- Belegende Zitate:
  - "Several common classes of models have emerged, all referred to as SLMs: (1) pure SLMs: models of the distribution of speech, p(speech), typically trained on unlabeled tokenized speech data only with a next-token prediction objective [...]; (2) speech+text SLMs: models of the joint distribution of speech and the corresponding text p(text, speech) [...]; and (3) speech-aware text LMs: models that combine text LLMs with pre-trained speech encoders to retain the instruction-following (IF) characteristics of the text LLMs and reason about the input speech or audio recordings" (Section 1)
  - "Models in category (3) typically start with fine-tuned LLMs, but involve some additional post-training to both align the speech and text representations and enable the model to perform new speech-specific tasks." (Section 1)
- Relevanz: Diese Taxonomie ist direkt anwendbar auf die in der Masterarbeit evaluierten Modelle: Qwen-Audio ist ein Speech-aware Text LM, GLM-4-Voice ein Speech+Text LM, und Whisper ein task-spezifisches Modell. Die Kategorisierung hilft, die unterschiedlichen Ergebnisse bei der Tontranskription zu erklären (RQ1).

**Learning 2: Die optimale Sprachrepräsentation in SLMs ist ungeklärt -- diskrete vs. kontinuierliche Repräsentationen haben unterschiedliche Vor- und Nachteile**
- Belegende Zitate:
  - "The optimal representation of speech within SLMs remains unclear. Speech representations in SLMs include both discrete and continuous varieties, derived from a wide range of encoders. This design choice can also influence other architectural choices in an SLM, for example depending on the information rate of the encoder and whether it encodes more phonetic or other types of information." (Section 8, Challenges)
  - "Another open question is determining the best method to combine speech and text, which applies to all aspects of SLM modeling and training." (Section 8, Challenges)
- Relevanz: Die Frage, ob phonetische oder akustische Token verwendet werden, ist direkt relevant für die Tonerkennung. Modelle mit phonetischen Tokens (z.B. HuBERT) könnten anders bei Mandarin-Tönen performen als solche mit akustischen Codec-Tokens (RQ1, Lücke 1).

**Learning 3: SLM-Evaluation ist fragmentiert und deckt nicht die volle Bandbreite gesprochener Sprachaufgaben ab**
- Belegende Zitate:
  - "Thus far, different SLMs have typically been evaluated on different datasets and tasks. [...] Existing benchmarks also do not cover the full range of spoken language tasks." (Section 8, Challenges - Evaluation)
  - "the range of spoken language tasks is arguably much larger than that of text tasks, since speech tasks include the vast majority of text tasks [...] and in addition include a variety of speech-specific tasks related to speaker properties, accents, or prosody-specific content." (Section 8, Challenges - Evaluation)
- Relevanz: Die Feststellung, dass prosodie-spezifische Aufgaben in der SLM-Evaluation fehlen, ist direkt relevant, da Mandarin-Töne prosodische Merkmale sind. Die Masterarbeit adressiert diese Lücke durch die systematische Evaluierung tonaler Transkription (RQ2, Lücke 2).

### 2. Verwendbarkeit für unsere Arbeit

**Direkt verwendbare Elemente:**
- Die umfassende Taxonomie der SLM-Architekturen (Table 3) als Referenz für die Einordnung der in der Masterarbeit getesteten Modelle
- Die Beschreibung der Encoder-Adapter-LLM-Architektur und der verschiedenen Speech-Encoder (Whisper, HuBERT, etc.) als theoretischer Rahmen
- Die Liste der Speech-aware Text LMs (SALMONN, Qwen-Audio etc.) und Speech+Text LMs (GLM-4-Voice, Moshi) zur Modellauswahl

**Methodische Übertragbarkeit:**
- Der Überblick über Evaluierungsmethoden (likelihood-based, generative metrics, trustworthiness) hilft bei der Einordnung der eigenen Metrik-Wahl (PER, TER, CER)
- Die Diskussion über den Tradeoff zwischen vortrainierten Textfähigkeiten und neuen Sprachaufgaben kontextualisiert die off-the-shelf Evaluierung

### 3. Forschungslücken (identifiziert im Paper)

- **Lücke A:** "SLM research has, thus far, understandably focused on high-resource languages and settings. As SLMs become more performant and enter commercial products in daily use, it will be critical to make them accessible to as broad a range of users as possible, including a variety of languages, dialects, and speech-related medical conditions." (Section 8, Improving inclusiveness)
- **Lücke B:** "there is a lack of standardized benchmarking for speech generation tasks" (Section 8, Evaluation) -- und ebenso fehlt Benchmarking für phonetische/tonale Transkriptionsaufgaben
- **Lücke C:** "There is a lack of public high-quality training data, particularly for instruction tuning and chat-based training." (Section 8, Training)

### 4. Adressierung durch meine Arbeit

| Forschungslücke | Bezug zu meiner Arbeit |
|---|---|
| Lücke A: Fokus auf High-Resource Sprachen | Meine Arbeit evaluiert Mandarin, eine tonale Sprache, die trotz hoher Sprecherzahl spezifische Herausforderungen für SLMs darstellt, die in westlich-zentrierten Evaluierungen übersehen werden (RQ1, RQ2, Lücke 4) |
| Lücke B: Fehlende Benchmarks für phonetische Aufgaben | Meine Arbeit liefert erstmals eine systematische Benchmark-Evaluierung für phonetische und tonale Transkription von Mandarin durch Frontier-Modelle (RQ1-RQ5, Lücke 2) |
| Lücke C: Fragmentierte Evaluation | Meine Arbeit evaluiert konsistent ~5-8 Modelle auf dem gleichen Korpus (Tone Perfect) mit den gleichen Metriken (PER, TER, CER), was eine direkte Vergleichbarkeit ermöglicht (RQ1) |
| Prosodie-spezifische Aufgaben fehlen | Mandarin-Töne sind prosodische Merkmale; meine Arbeit testet, ob SLMs diese erkennen können, was eine der im Survey identifizierten Evaluierungslücken schliesst (Lücke 2) |

---

## Paper 22: Kimi-Audio Technical Report
**Autoren:** Kimi Team (Moonshot AI)
**Jahr:** 2025
**Quelle:** arXiv:2504.18425v1

### 1. Zentrale Learnings

**Learning 1: Aktuelle Audio-Vortrainierung fokussiert auf Transkription ("was gesagt wird") und vernachlässigt Paralanguage-Information wie Ton, Emotion und Stil**
- Belegende Zitate:
  - "Current pre-training paradigms for audio foundation models typically leverage audio-text pre-training to bridge the gap between text and audio, where the text is obtained from audio (speech) by ASR transcription. However, text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone), acoustic scene, and non-linguistic sounds." (Section 8, Challenges - From Audio Transcription to Audio Description)
  - "Incorporating both transcriptive and descriptive text of the audio enables models to better understand and generate not only spoken language but also complex acoustic environments, paving the way for more nuanced, multimodal audio processing systems" (Section 8)
- Relevanz: Die Vernachlässigung von "tone" als Paralanguage-Information in der Vortrainierung ist hochrelevant für RQ2 und Lücke 2: Wenn Modelle nicht auf Toninformation trainiert werden, ist ihre Fähigkeit zur Mandarin-Tonerkennung inherent limitiert.

**Learning 2: Kimi-Audio erreicht SOTA bei Mandarin-ASR mit einem hybriden Ansatz aus diskreten semantischen Tokens und kontinuierlichen akustischen Features**
- Belegende Zitate:
  - "We leverage a 12.5Hz audio tokenizer, design a novel LLM-based architecture with continuous features as input and discrete tokens as output" (Abstract)
  - "we concatenate the semantic audio token with continuous acoustic vectors in the input to enhance perception capability, and concatenate with discrete text tokens in the output to enhance the generation capability" (Section 2.1)
  - Kimi-Audio erreicht 0.60 WER auf AISHELL-1 und 2.56 auf AISHELL-2 (Table 4)
- Relevanz: Der hybride Tokenisierungsansatz (semantisch + akustisch) ist architektonisch relevant für das Verständnis, warum bestimmte Modelle besser bei der Tonerkennung abschneiden könnten (RQ1, Lücke 1).

**Learning 3: Semantische Audio-Tokens basieren auf ASR-Hilfsverlusten und erfassen daher keine akustischen Details wie Töne**
- Belegende Zitate:
  - "Semantic tokens are typically obtained by ASR-based auxiliary loss, which focuses on transcription-oriented information and fails to capture rich acoustic details crucial for understanding and generation. Acoustic tokens are typically learned by audio reconstruction loss, which focuses on description-oriented acoustic details and fails to capture abstractive semantic information" (Section 8, Better Audio Representations)
  - "A valuable research direction is to develop representations that integrate both transcription-oriented semantic information and description-oriented acoustic features, encompassing nuances like speaker identity, emotion, and environmental sounds while maintaining high-level abstractive information" (Section 8)
- Relevanz: Dies erklärt einen fundamentalen Mechanismus, warum aktuelle Audio-LLMs bei der Tonerkennung versagen könnten: Ihre semantischen Tokens sind transkriptionsorientiert und kodieren Tonhöhenkonturen nicht explizit (RQ2, Lücke 2).

### 2. Verwendbarkeit für unsere Arbeit

**Direkt verwendbare Elemente:**
- Kimi-Audio als potenzielles Evaluierungsmodell für die Masterarbeit (open-source, SOTA auf Mandarin-Benchmarks)
- Die AISHELL-1 und AISHELL-2 Ergebnisse als Kontextualisierung der Mandarin-ASR-Leistung
- Die Erkenntnis über die Limitierung semantischer Tokens als theoretische Erklärung für Tonerkennungsfehler

**Methodische Übertragbarkeit:**
- Kimi-Audios standardisiertes Evaluierungs-Toolkit (WER-Berechnung, GPT-4o-mini als Judge) als methodisches Vorbild
- Der Ansatz, ein einheitliches Evaluierungs-Toolkit zu entwickeln, inspiriert die konsistente Metrik-Anwendung in der Masterarbeit

### 3. Forschungslücken (identifiziert im Paper)

- **Lücke A:** "text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)" (Section 8) -- Toninformation wird in der Vortrainierung vernachlässigt
- **Lücke B:** "Semantic tokens are typically obtained by ASR-based auxiliary loss, which focuses on transcription-oriented information and fails to capture rich acoustic details" (Section 8) -- Token-Repräsentationen erfassen keine akustischen Details
- **Lücke C:** "the audio models behave like a sophisticated distillation of existing ASR and TTS systems. As a result, they can hardly achieve performance far beyond the ceiling of ASR/TTS" (Section 8) -- Audio-Modelle sind durch ASR/TTS-Systeme begrenzt

### 4. Adressierung durch meine Arbeit

| Forschungslücke | Bezug zu meiner Arbeit |
|---|---|
| Lücke A: Paralanguage (Ton) wird vernachlässigt | Meine Arbeit evaluiert explizit die Ton-Erkennungsfähigkeit (TER) und testet, ob Modelle den für Mandarin essentiellen Ton korrekt transkribieren können (RQ2, Lücke 2) |
| Lücke B: Semantische Tokens vernachlässigen Akustik | Meine Arbeit quantifiziert empirisch, inwieweit diese Limitation die Mandarin-Tontranskription beeinflusst, indem sie Modelle mit verschiedenen Tokenisierungsansätzen vergleicht (RQ1, RQ2) |
| Lücke C: Audio-Modelle durch ASR/TTS begrenzt | Meine Arbeit testet, ob aktuelle Frontier-Modelle über die reine Zeichenerkennung (CER) hinaus auch phonetische Details (PER, TER) korrekt erfassen, was die Grenze der ASR-Distillation aufzeigt (RQ3) |
| Kimi-Audio als SOTA-Referenz | Kimi-Audios 0.60 WER auf AISHELL-1 zeigt die obere Leistungsgrenze, gegen die off-the-shelf Modelle bei Mandarin kontextualisiert werden (Lücke 3) |

---

## Paper 23: Towards Holistic Evaluation of Large Audio-Language Models: A Comprehensive Survey
**Autoren:** Chih-Kai Yang, Neo S. Ho, Hung-yi Lee
**Jahr:** 2025
**Quelle:** arXiv:2505.15957v4

### 1. Zentrale Learnings

**Learning 1: Die Evaluierungslandschaft für LALMs ist fragmentiert und es fehlt eine systematische Taxonomie**
- Belegende Zitate:
  - "While numerous benchmarks have emerged to assess LALMs' performance, they remain fragmented and lack a structured taxonomy." (Abstract)
  - "Existing surveys focus primarily on model architectures and training methodologies, with less emphasis on the equally important role of evaluation in assessing LALMs' capabilities." (Section 1)
- Relevanz: Die Fragmentierung der Evaluierung betrifft auch die phonetische/tonale Dimension, die in keinem der bestehenden Benchmarks systematisch abgedeckt wird. Die Masterarbeit schliesst diese Lücke für Mandarin-Tontranskription (RQ1-RQ5, Lücke 2).

**Learning 2: LALMs zeigen signifikante Instabilität bei Content-based Reasoning über verschiedene Sprechstile hinweg, auch mit Chain-of-Thought**
- Belegende Zitate:
  - "These benchmarks reveal gaps in current LALMs' content-based reasoning abilities, even with chain-of-thought. Moreover, model performance varies significantly across speaking styles, indicating instability in their reasoning." (Section 4.3.1)
  - "Different LALMs excel in different domains, each with their own strengths, but their performance often noticeably declines outside their own specialized areas" (Section 4.2)
- Relevanz: Diese Instabilität über Sprechstile hinweg ist direkt relevant für die Evaluierung auf dem Tone-Perfect-Korpus, das verschiedene Sprecher umfasst. Die Masterarbeit kann überprüfen, ob ähnliche Instabilitäten bei der Tontranskription auftreten (RQ3).

**Learning 3: Low-Resource-Sprachen, Code-Switching und sprachliche/kulturelle Vielfalt sind in LALM-Evaluierungen unterrepräsentiert**
- Belegende Zitate:
  - "While current benchmarks cover major languages like English and Mandarin, many overlook crucial aspects such as low-resource languages and code-switching. Although these have been explored in traditional speech technologies, they remain underexamined in LALMs." (Section 7.2)
  - "This limited coverage fails to capture the full linguistic diversity of human communication, as different languages possess unique characteristics" (Section 7.2)
  - "future evaluations should carefully consider linguistic, cultural, and communicative diversity" (Section 7.2)
- Relevanz: Obwohl Mandarin als "major language" erwähnt wird, fehlt die Evaluierung tonaler Merkmale, die ein einzigartiges linguistisches Charakteristikum darstellen. Die Masterarbeit adressiert genau diese Dimension der linguistischen Vielfalt (RQ2, Lücke 4).

### 2. Verwendbarkeit für unsere Arbeit

**Direkt verwendbare Elemente:**
- Die vierteilige Taxonomie (General Auditory Awareness/Processing, Knowledge/Reasoning, Dialogue-oriented Ability, Fairness/Safety/Trustworthiness) als Einordnungsrahmen -- die Masterarbeit fällt unter "General Auditory Processing" (Phonemerkennung) und "Linguistic Knowledge"
- Die Übersicht über relevante Benchmarks (VoxEval, VoiceBench, MMAU, Dynamic-SUPERB) als Related-Work-Referenzen
- Die Diskussion über LLM-as-a-judge Evaluierungsmethoden als mögliche Ergänzung zur regelbasierten Metrik-Evaluierung

**Methodische Übertragbarkeit:**
- Die Identifizierung von Datencontamination als Evaluierungsproblem mahnt zur Vorsicht bei der Interpretation von Ergebnissen auf bekannten Benchmarks
- Die Diskussion über inkonsistente Metriken (z.B. verschiedene WER-Berechnungen) unterstreicht die Wichtigkeit standardisierter Evaluierung

### 3. Forschungslücken (identifiziert im Paper)

- **Lücke A:** "many overlook crucial aspects such as low-resource languages and code-switching. Although these have been explored in traditional speech technologies, they remain underexamined in LALMs." (Section 7.2)
- **Lücke B:** "Auditory cues such as tone, emotion, and voice quality can also influence user experience and raise concerns if uncontrolled." (Section 7.3) -- Ton als auditorisches Merkmal wird als Sicherheitsaspekt erwähnt, aber phonetische Ton-Evaluierung fehlt
- **Lücke C:** "Benchmark results reveal significant gaps in LALMs compared to their LLM backbones in instruction following, indicating catastrophic forgetting when adapting LLMs to auditory modalities." (Section 5.2)

### 4. Adressierung durch meine Arbeit

| Forschungslücke | Bezug zu meiner Arbeit |
|---|---|
| Lücke A: Sprachliche Vielfalt unterrepräsentiert | Meine Arbeit evaluiert ein einzigartiges linguistisches Merkmal (Mandarin-Töne), das in keinem bestehenden LALM-Benchmark systematisch getestet wird (RQ2, Lücke 4) |
| Lücke B: Tonale Merkmale nicht systematisch evaluiert | Meine Arbeit liefert die erste systematische Evaluierung der Tonerkennungsfähigkeit (TER) von LALMs für Mandarin (RQ2, RQ3, Lücke 2) |
| Lücke C: Catastrophic Forgetting bei Modalitätsadaption | Meine off-the-shelf Evaluierung (ohne Finetuning) misst implizit, wie gut LALMs ihre Sprachverarbeitungsfähigkeiten nach dem Audio-Training beibehalten haben (RQ1, Lücke 1) |
| Fragmentierte Evaluierungslandschaft | Meine Arbeit trägt zur Konsolidierung bei, indem sie eine standardisierte Evaluierung (gleiche Metriken, gleiches Korpus) über mehrere Frontier-Modelle durchführt (RQ1) |

---

## Paper 24: ZIPA: A family of efficient models for multilingual phone recognition
**Autoren:** Jian Zhu, Farhan Samir, Eleanor Chodroff, David R. Mortensen
**Jahr:** 2025
**Quelle:** arXiv:2505.23170v1

### 1. Zentrale Learnings

**Learning 1: Phonerkennungsmodelle glätten soziophoneetische Variation und geben Wörterbuchaussprachen statt tatsächlicher Aussprache wieder**
- Belegende Zitate:
  - "Phone recognition models tend to smooth out the phonetic variation during inference. In Table 3, there is a systematic gap between the performance of L2-Standard and the L2-Perceived test sets." (Section 7, Analysis)
  - "Given the exact same L2 speech, ZIPA predictions tend to better match the standard dictionary pronunciation than the actual pronunciation." (Section 7)
  - "This is likely an artifact of data curation, as all of the training data were generated from pronunciation dictionaries and G2P models. Yet this finding also implies that the phone recognition models are still matching the frequent phone patterns in the dataset, rather than transcribing phones as they are actually produced." (Section 7)
- Relevanz: Dieses Glättungsphänomen ist hochrelevant für die Mandarin-Tonerkennung (RQ2): Wenn Modelle systematisch Wörterbuchaussprachen bevorzugen, könnten sie bei natürlicher Sprachvariation (z.B. Tone-Sandhi, kontextuelle Tonvariation) versagen. Dies betrifft direkt Lücke 2 und die Evaluierung auf dem Tone-Perfect-Korpus.

**Learning 2: Vokalsubstitutionen sind der häufigste Fehlertyp, und Vokale sind sprachübergreifend schwieriger zu erkennen als Konsonanten**
- Belegende Zitate:
  - "Compared to consonants, vowel realizations tend to be more gradient in their acoustics, resulting in higher acoustic overlap between otherwise contrastive vowel categories and therefore more ambiguous." (Section 7)
  - "For substitution, the top errors are the substitution of vowels that are close in the vowel space." (Section 7)
  - Die häufigsten Substitutionsfehler: "a->A, i->e, E->e, o->u, e->i, u->o" (Table 5)
- Relevanz: Mandarin hat ein komplexes Finalsystem mit vielen Vokalen. Die Erkenntnis, dass Vokale schwieriger zu erkennen sind, ist direkt relevant für die Phonem-Error-Rate (PER) bei Mandarin-Finalen (RQ2, RQ3).

**Learning 3: ZIPA-Modelle mit nur 64M Parametern übertreffen 300M-Baseline-Modelle bei multilingualer Phonerkennung, Mandarin inklusive**
- Belegende Zitate:
  - "even the 64M ZIPA models outperform previous phone recognition models with 300M parameters, while being more computationally efficient." (Section 1)
  - "Our study shows that careful curation of data, including increasing data quantity and carefully normalizing the IPA labels, as well as a good choice of backbone model can yield effective improvement." (Section 6)
  - Mandarin (cmn) erreicht 0.38-0.54 PFER auf Seen Languages (Table 2)
- Relevanz: ZIPA zeigt, dass effiziente Modelle bei multilingualer Phonerkennung (inklusive Mandarin) kompetitiv sein können. Die Mandarin-PFER-Werte bieten einen Vergleichspunkt für die PER-Ergebnisse der Masterarbeit (RQ1, RQ3).

### 2. Verwendbarkeit für unsere Arbeit

**Direkt verwendbare Elemente:**
- Die Phonetic Feature Error Rate (PFER) als alternative/ergänzende Metrik zur PER, die artikulatorische Ähnlichkeit berücksichtigt
- Die Erkenntnis über die Glättung soziophoneetischer Variation als theoretische Erklärung für Tonerkennungsfehler
- Mandarin (cmn) PFER-Werte (0.38-0.54) als Baseline für phonetische Genauigkeit
- Die Verwendung von Aishell-1 als Mandarin-Evaluierungsdatensatz

**Methodische Übertragbarkeit:**
- Die Trennung in Seen/Unseen Languages und die Evaluierung soziophoneetischer Variation als methodisches Vorbild
- Die Fehleranalyse nach Deletions, Insertions und Substitutions ist direkt auf die Mandarin-Phonem-Analyse übertragbar
- Die Verwendung von PanPhon für die PFER-Berechnung als Tool-Referenz

### 3. Forschungslücken (identifiziert im Paper)

- **Lücke A:** "the data quality is limited as dictionary pronunciations might not reflect the actual pronunciation in spontaneous speech. This also results in the ZIPA models to smooth out variation in some L2 speech." (Limitations)
- **Lücke B:** "simply scaling up the G2P for transcribed speech data alone might not be able to solve phone recognition, as models can simply memorize the standard pronunciation." (Section 8, Conclusions)
- **Lücke C:** Das Paper nutzt numerische Tonrepräsentationen für Mandarin statt IPA-Tonbuchstaben: "it uses a numerical representation of Mandarin tones rather than the standard IPA tone notations, also known as Chao tone letters" (Section 3.2) -- die tonale Dimension wird nicht separat evaluiert
- **Lücke D:** "The number of languages studied in our paper is still limited. The distribution of languages is highly skewed in our dataset, which still biases our models towards high-resource languages." (Limitations)

### 4. Adressierung durch meine Arbeit

| Forschungslücke | Bezug zu meiner Arbeit |
|---|---|
| Lücke A: Wörterbuch- vs. tatsächliche Aussprache | Meine Arbeit verwendet das Tone-Perfect-Korpus mit natürlichen Aufnahmen, was die Diskrepanz zwischen Wörterbuch- und tatsächlicher Aussprache einschliesst (RQ2) |
| Lücke B: Memorierung von Standardaussprachen | Meine Arbeit testet, ob Frontier-Modelle tatsächliche phonetische/tonale Merkmale erkennen oder nur frequente Muster reproduzieren, insbesondere bei Mandarin-Tönen (RQ2, Lücke 2) |
| Lücke C: Tonale Dimension nicht separat evaluiert | Meine Arbeit evaluiert die Tone Error Rate (TER) als eigenständige Metrik, was die tonale Dimension explizit von der segmentalen Phonemerkennung trennt (RQ2, RQ3) |
| Lücke D: Sprachskewing in Trainingsdaten | Meine Arbeit quantifiziert, wie gut off-the-shelf Modelle bei Mandarin-spezifischen Aufgaben performen, unabhängig von den Trainingsdatenverteilungen der Modelle (RQ1, Lücke 4) |
| Vokalsubstitutionen als häufigste Fehler | Meine Arbeit kann überprüfen, ob ähnliche Muster (Vokalverwechslungen in Finalen) auch bei der Mandarin-Transkription durch Frontier-Modelle auftreten (RQ3) |

---

## Zusammenfassung der Batch-4-Analyse

### Übergreifende Muster

1. **Pitch/Ton-Sensitivität als zentrale Herausforderung:** VoxEval zeigt, dass Pitch-Shifts die grösste Herausforderung für SLMs darstellen (Paper 19). Kimi-Audio identifiziert "tone" explizit als vernachlässigte Paralanguage-Information (Paper 22). ZIPA zeigt, dass Modelle soziophoneetische Variation glätten (Paper 24). Zusammen etablieren diese Befunde eine starke Motivation für die systematische Evaluierung der Mandarin-Tonerkennung.

2. **CER ist unzureichend als alleinige Metrik:** FireRedASR (Paper 20) erreicht 3.05% CER, aber evaluiert keine phonetische Dekomposition. Die Masterarbeit geht mit PER, TER und Pinyin-Transkription über reine CER hinaus.

3. **Off-the-shelf Evaluierung ist unterrepräsentiert:** Keines der Papers evaluiert Frontier-LLMs (GPT-4o, Gemini etc.) systematisch für Mandarin-phonetische Aufgaben. Die Masterarbeit schliesst diese Lücke (Lücke 1).

4. **Fragmentierte Evaluierungslandschaft:** Sowohl der Landscape-Survey (Paper 21) als auch die Holistic Evaluation (Paper 23) identifizieren fragmentierte Evaluation als zentrales Problem. Die Masterarbeit bietet eine konsistente, vergleichbare Evaluierung.

### Mapping auf Forschungslücken der Masterarbeit

| Lücke | Relevante Papers in Batch 4 |
|---|---|
| Lücke 1: Off-the-shelf Evaluierung multimodaler Modelle fehlt | Papers 19, 20, 21, 22, 23 |
| Lücke 2: Tonale Transkriptionsfähigkeit nicht systematisch getestet | Papers 19, 22, 23, 24 |
| Lücke 3: Fehlender Vergleich Frontier-Modelle vs. spezialisierte Systeme | Papers 20, 22 |
| Lücke 4: Mandarin-spezifische Evaluierung unterrepräsentiert | Papers 21, 23, 24 |


# Batch 5: Papers 25-30 (WildSpeech-Bench, ContextASR-Bench, Step-Audio 2, TELEVAL, VocalBench-zh, Qwen3-ASR)


Kontext der Masterarbeit:
**"Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech"** von Stefan Dosch (IU M.Sc. Data Science, Betreuer: Tim Schlippe).

- **RQ1:** Text-only Baseline (Character -> Pinyin, a) ohne Töne, b) mit Tönen)
- **RQ2:** Native-Speaker Sprachtranskription auf Wort-/Phonem-/Tonebene
- **RQ3 (Stretch):** Non-Native/L2-Sprecher
- **RQ4:** Vergleich mit dedizierten Systemen (Whisper etc.)
- **RQ5:** Fehlerstruktur -- welche Töne/Phonemkontraste werden am häufigsten verwechselt
- **Lücke 1:** Frontier multimodale LLMs nur auf Satz-/Wortebene evaluiert (WER/CER); Phonem-/Tongranularität für Mandarin unerforscht
- **Lücke 2:** Töne selten als explizite, separate Evaluationsachse
- **Lücke 3:** Sehr wenige Studien zu L2/Non-Native-Sprechern
- **Lücke 4:** Kein systematischer Head-to-Head-Vergleich multimodaler Modelle auf dieser Granularität
- **Design:** Pinyin mit Tonnummern (ma1); ~5-8 Frontier-Modelle + Whisper; Metriken: PER, TER, CER, F1, FAR/FRR; Korpus: Tone Perfect; off-the-shelf, kein Finetuning

---

## Paper 25: WildSpeech-Bench: Benchmarking Audio LLMs in Natural Speech Conversation
**Quelle:** Zhang et al. (2025), arXiv:2506.21875v1

### 1. Zentrale Learnings

WildSpeech-Bench ist ein Benchmark für End-to-End Speech LLMs, der auf 1.100 realen Sprachkonversationsszenarien basiert. Der Benchmark evaluiert Audio-LLMs in fünf Kategorien: Information Inquiry, Solution Request, Opinion Exchange, Text Creation sowie Paralinguistic-Featured (PF) Queries. Die PF-Kategorie umfasst Pause, Stress, Tone, Stuttering und Near-Homophones. Die Evaluation ist audio-to-audio mit query-aware Bewertung und GPT-4o-mini als Scorer.

Die zentrale Erkenntnis ist, dass selbst die besten Modelle bei realen Sprachszenarien erheblich Schwächen zeigen, insbesondere bei paralinguistischen Phänomenen.

**Belegende Zitate:**

> "with a total score of 10 points, even the best model, GPT-4o-Audio, has an average score of only 6.29 points. This indicates that there is still significant room for improvement in current end-to-end speech large models for real-world applications." (Zeile 657-659)

> "GPT-4o-Audio outperforms other models in all categories. Notably, GPT-4o-Audio demonstrates not only superior performance in general conversational tasks, but also maintains a significant lead in paralinguistic-featured queries, further highlighting its comprehensive capabilities." (Zeile 650-652)

> "Among all open-source models evaluated, Qwen-2.5-omni achieves the highest overall performance" (Zeile 652-653)

> "while Qwen-2.5-omni excels in general dialogue, its performance on paralinguistic-featured queries lags behind that of GLM-4-voice and MiniCPM, indicating that there is still room for improvement in its speech understanding abilities." (Zeile 654-656)

### 2. Verwendbarkeit für unsere Arbeit

**RQ2/RQ4:** Der Benchmark liefert eine Rangliste der Modellperformance (GPT-4o-Audio > Qwen-2.5-Omni > MiniCPM > GLM-4-Voice), die als Referenz für die Modellauswahl dient. Allerdings evaluiert WildSpeech-Bench nur auf Antwortqualität (1-10 Score), nicht auf Transkriptionsgenauigkeit oder phonetische Granularität.

**RQ5:** Die Ergebnisse in der PF-Subkategorie "Tone" (wo GPT-4o 5.85 erzielt, Qwen nur 4.13) zeigen, dass selbst leading Modelle bei Tonunterscheidung schwach abschneiden -- dies betrifft allerdings englische Intonation/Prosodie, nicht Mandarin-lexikalische Töne.

**Lücke 1 & 2:** WildSpeech-Bench bestätigt die Notwendigkeit unserer Arbeit: Die Evaluation ist rein auf Gesprächsqualität ausgerichtet, nicht auf phonetische oder tonale Transkriptionsgenauigkeit.

> "Existing evaluation methods often adapt text-based benchmarks, overlooking speech's unique characteristics and challenges, including prosody, homophones, stuttering, and differing user expectations." (Zeile 12-14)

### 3. Forschungslücken

**Lücke A (Sprache):** Der Benchmark ist ausschließlich auf Englisch ausgerichtet und deckt keine Mandarin-spezifischen Herausforderungen ab.

**Lücke B (Single-Turn):** Nur Single-Turn-Dialoge werden evaluiert.

**Lücke C (Keine Transkriptionsmetriken):** Keine phonetische oder tonale Evaluationsachse; Bewertung basiert auf subjektiver Antwortqualität.

> "our current benchmark focuses solely on single-turn dialogue evaluation, failing to comprehensively capture the intricacies of multi-turn speech interactions." (Zeile 1506-1507)

> "although we have made extensive efforts to simulate real-world speech scenarios by carefully designing the query distribution and incorporating various types of noise, the lack of actual in-the-wild speech data for comparison may still lead to discrepancies between our simulated data and real-world situations." (Zeile 1502-1504)

### 4. Adressierung durch meine Arbeit

- **Zu Lücke A:** Unsere Arbeit evaluiert explizit Mandarin-Sprache und adressiert damit die fehlende Abdeckung von WildSpeech-Bench (**Lücke 1, 2**).
- **Zu Lücke C:** Während WildSpeech-Bench nur Response-Qualität misst, evaluieren wir mit PER, TER, CER, F1 auf phonetischer und tonaler Granularität (**Lücke 1, 2, 4**).
- Die von WildSpeech-Bench getesteten Modelle (GPT-4o, Qwen-2.5-Omni) sind auch Teil unseres Modellsets, was einen ergänzenden Vergleich ermöglicht.

---

## Paper 26: ContextASR-Bench: A Massive Contextual Speech Recognition Benchmark
**Quelle:** Wang et al. (2025), arXiv:2507.05727v2

### 1. Zentrale Learnings

ContextASR-Bench ist ein groß angelegter kontextueller ASR-Benchmark mit über 40.000 Datenpunkten und mehr als 300.000 Named Entities in über 10 Domänen, sowohl auf Englisch als auch Mandarin. Der Benchmark führt zwei neue Metriken ein -- NE-WER (Named Entity Word Error Rate) und NE-FNR (Named Entity False Negative Rate) -- die gezielt die Erkennung von Fachbegriffen und Named Entities bewerten. Die Evaluation erfolgt in drei Modi: Contextless, Coarse-grained Context und Fine-grained Context.

Die zentrale Erkenntnis ist, dass LALMs (Large Audio Language Models) konventionellen ASR-Modellen dank des Weltwissens der zugrundeliegenden LLMs deutlich überlegen sind, aber dennoch erhebliche Schwächen in der kontextuellen Spracherkennung aufweisen.

**Belegende Zitate:**

> "LALMs outperform conventional ASR models by a large margin thanks to the strong world knowledge and context modeling of LLMs" (Zeile 25-27)

> "ASR systems without LLMs generally have NE-FNR rates exceeding 50% on both ContextASR-Speech set and ContextASR-Dialogue set." (Zeile 758-759)

> "WER treats all words equally, conflicting with human evaluation priorities that emphasize critical content, such as named entities, technical terms, over functional words, such as tone words or pronouns." (Zeile 495-498)

> "current LALMs still struggle in contextual ASR, indicating ample room for improvement." (Zeile 825-826)

### 2. Verwendbarkeit für unsere Arbeit

**RQ4:** ContextASR-Bench enthält sowohl English- als auch Mandarin-Testsets und liefert direkte Vergleichswerte für LALMs vs. konventionelle ASR-Modelle. Die Ergebnistabelle (Table 2) zeigt u.a. Qwen2.5-Omni-7B mit 1.95% WER auf ContextASR-Speech-ZH (Contextless), was als Referenzwert für die Leistungsfähigkeit multimodaler Modelle bei Mandarin-ASR dient.

**RQ5:** Die Erkenntnis, dass WER allein nicht ausreicht, unterstützt unser Argument für differenziertere Metriken (PER, TER).

**Lücke 1 & 2:** Obwohl Mandarin enthalten ist, evaluiert ContextASR-Bench nur auf WER/CER/NE-WER-Ebene. Phonem- und tonale Granularität fehlen vollständig.

> "Even FireredASR-AED-L, the current SOTA ASR model for Mandarin, shows an NE-FNR exceeding 40% on ContextASR-Bench." (Zeile 761-762)

### 3. Forschungslücken

**Lücke A (Keine Phonem-/Ton-Evaluation):** Trotz Mandarin-Abdeckung wird ausschließlich auf Wort-/Entity-Ebene evaluiert; Pinyin-Töne oder phonetische Segmente werden nicht betrachtet.

**Lücke B (Synthetische Daten):** Alle Audiodaten sind TTS-synthetisiert, was die Übertragbarkeit auf reale Sprecherdaten einschränkt.

**Lücke C (Kein L2-Sprecher):** Nicht-muttersprachliche Mandarin-Sprecher sind nicht berücksichtigt.

> "current LALMs still struggle in contextual ASR, indicating ample room for improvement. We open-source ContextASR-Bench in the hope of stimulating further research on how LALMs can better leverage additional context information and accelerating progress toward truly universal ASR systems across all domains." (Zeile 825-827)

### 4. Adressierung durch meine Arbeit

- **Zu Lücke A:** Unsere Arbeit evaluiert explizit auf Phonem- und Tonebene mit PER und TER, was ContextASR-Bench nicht abdeckt (**Lücke 1, 2**).
- **Zu Lücke C:** Mit RQ3 adressieren wir L2-Sprecher, eine Population, die in ContextASR-Bench fehlt (**Lücke 3**).
- **Zu Lücke B:** Wir verwenden das Tone-Perfect-Korpus mit realen Aufnahmen, nicht TTS-synthetisierte Daten.
- Die NE-WER/NE-FNR-Metriken von ContextASR-Bench bestätigen konzeptuell unseren Ansatz, über Standard-WER hinauszugehen, da auch wir argumentieren, dass granularere Metriken nötig sind.

---

## Paper 27: Step-Audio 2 Technical Report
**Quelle:** StepFun Audio Team (2025), arXiv:2507.16632v3

### 1. Zentrale Learnings

Step-Audio 2 ist ein End-to-End Large Audio Language Model für Audio-Verständnis und Sprachkonversation. Das Modell integriert einen latenten Audio-Encoder, Reinforcement Learning und die Generierung diskreter Audio-Tokens in die Sprachmodellierung. Es wurde auf 8 Millionen Stunden Sprachdaten und 680 Milliarden Text-Tokens trainiert. Step-Audio 2 erzielt SOTA-Ergebnisse in ASR (3.14% WER Englisch, 3.08% CER Chinesisch), Audio Understanding und Speech-to-Speech Konversation.

Besonders relevant ist der StepEval-Audio-Paralinguistic Benchmark mit 550 Samples über 11 Dimensionen (Gender, Age, Timbre, Emotion, Pitch, Rhythm, Speed, Style, Vocal, Scenario, Event), bei dem Step-Audio 2 mit 83.09 durchschnittlicher Accuracy deutlich vor allen Konkurrenten liegt.

**Belegende Zitate:**

> "Step-Audio 2 achieves state-of-the-art performance on various audio understanding and conversational benchmarks compared to other open-source and commercial solutions." (Zeile 18-19)

> "Step-Audio 2 outperforms existing open-source and commercial ASR models in both general English and Chinese recognition, achieving an average word error rate (WER) of 3.14% on English and an average character error rate (CER) of 3.08% on Chinese test sets." (Zeile 594-597)

> "The experimental results highlight the comprehensive capabilities of Step-Audio 2 in understanding various paralinguistic information, achieving an average accuracy of 83.09, which is a significant improvement over other baseline models." (Zeile 835-836)

> "Trained on millions of hours of speech and audio data, Step-Audio 2 delivers intelligence and expressiveness across diverse conversational scenarios." (Zeile 17-18)

### 2. Verwendbarkeit für unsere Arbeit

**RQ2/RQ4:** Step-Audio 2 erreicht 3.08% CER auf Chinesisch (Durchschnitt über 6 Testsets), was einen starken Referenzwert für Mandarin-ASR darstellt. Das Modell unterstützt auch chinesische Dialekte (Sichuan, Shanghai, Anhui, Guangdong, Guangxi, Shanxi), was seine robuste Chinesisch-Verarbeitung zeigt.

**RQ5:** Der StepEval-Audio-Paralinguistic Benchmark evaluiert paralinguistische Dimensionen (einschließlich Pitch, Speed, Rhythm), die mit tonaler Wahrnehmung verwandt sind -- aber keine lexikalischen Mandarin-Töne als solche.

**Lücke 1 & 2:** Trotz exzellenter CER-Werte und paralinguistischer Evaluation wird nirgends Pinyin-Transkription, Phonem-Segmentierung oder tonale Genauigkeit gemessen.

> "Step-Audio 2 leverages a latent audio encoder and reinforcement learning to enhance its speech and audio comprehension capabilities." (Zeile 1327-1329)

### 3. Forschungslücken

**Lücke A (Keine Ton-Evaluation):** Obwohl das Modell CER für Chinesisch berichtet und paralinguistische Information versteht, gibt es keine Evaluation der Mandarin-Tonerkennung auf Phonemebene.

**Lücke B (Kein Technical Report zu Limitationen):** Der Technical Report enthält keine explizite Limitations-Sektion; die Conclusion fokussiert auf Stärken.

**Lücke C (Kein L2-Benchmark):** Nicht-muttersprachliche Sprecher werden nicht evaluiert.

> "Step-Audio 2 is also capable of utilizing external tools including web search and audio search for multi-modal RAG. Trained on 8 million hours of speeches and audios, Step-Audio 2 demonstrates state-of-the-art performance across various tasks, including ASR, audio understanding, speech translation, and general speech conversation, outperforming both open-source and commercial solutions." (Zeile 1337-1340)

### 4. Adressierung durch meine Arbeit

- **Zu Lücke A:** Unsere Arbeit evaluiert explizit die tonale Genauigkeit (TER) von Modellen wie Step-Audio 2, die im Technical Report nur CER berichten (**Lücke 1, 2**).
- **Zu Lücke C:** Mit RQ3 decken wir L2-Sprecher ab (**Lücke 3**).
- Step-Audio 2 ist ein potenzieller Kandidat für unser Modellset (RQ4), da es SOTA-Chinesisch-ASR bietet und uns ermöglicht, die Diskrepanz zwischen CER- und TER-Performance zu untersuchen.
- Unsere Arbeit liefert den fehlenden Head-to-Head-Vergleich auf Phonem-/Tongranularität (**Lücke 4**).

---

## Paper 28: TELEVAL: A Dynamic Benchmark for Spoken Language Models in Chinese Interactive Scenarios
**Quelle:** Li et al. (2025), arXiv:2507.18061v3

### 1. Zentrale Learnings

TELEVAL ist ein dynamischer, nutzerorientierter Benchmark für Spoken Language Models (SLMs) in realistischen chinesischen Interaktionsszenarien mit über 40.000 Samples. Der Benchmark evaluiert zwei Kernaspekte: Reliable Content Fulfillment (semantische Korrektheit) und Interactional Appropriateness (pragmatische und paralinguistische Angemessenheit). TELEVAL umfasst Dialektverständnis (Kantonesisch, Henan, Nordost-Mandarin, Shanghainesisch, Sichuanesisch), empathische Reaktionen, altersgerechte Antworten und NSV-Bewusstsein (Non-Speech Vocalizations).

Die zentrale Erkenntnis ist das "Caption Trap"-Phänomen: Modelle erkennen paralinguistische Signale korrekt (z.B. Husten), generieren aber keine angemessenen Reaktionen (z.B. "Geht es dir gut?"), sondern beschreiben die Signale nur deskriptiv.

**Belegende Zitate:**

> "despite strong performance on semantic and knowledge-oriented tasks, current SLMs still struggle to produce natural and interactionally appropriate responses" (Zeile 28-31)

> "models like Kimi-Audio accurately recognize paralinguistic cues (e.g., coughing) but fail to generate informed responses (e.g., 'Are you okay?')." (Zeile 2062-2063)

> "This likely stems from training regimes treating paralinguistic data as explicit classification targets rather than implicit signals for behavioral adaptation" (Zeile 2063-2065)

> "Basic Knowledge scores range 9.88-52.93%, Dialect Comprehension 4.98-45.45%" (Table 2, Zeile 1383-1410)

### 2. Verwendbarkeit für unsere Arbeit

**RQ2/RQ4:** TELEVAL liefert umfangreiche Vergleichsdaten für chinesische SLMs (GPT-4o-Audio, Qwen2.5-Omni, Kimi-Audio, StepAudio2-mini, Qwen3-Omni, Mimo-Audio-Instruct etc.). Die Ergebnisse zeigen, dass selbst die besten Modelle (GPT-4o: 52.93% Basic Knowledge, StepAudio2-mini: 45.45% Dialect Comprehension) erhebliche Schwächen haben.

**RQ5:** Das "Caption Trap"-Phänomen ist methodisch relevant: Es zeigt, dass Modelle paralinguistische Information erkennen können, aber nicht angemessen darauf reagieren. Dies könnte sich analog bei tonaler Information zeigen -- Modelle könnten Töne "hören" aber nicht korrekt in Pinyin übersetzen.

**Lücke 1:** TELEVAL evaluiert pragmatische Interaktionsfähigkeit, nicht phonetische Transkriptionsgenauigkeit.

> "Although they can detect paralinguistic cues, they often fail to incorporate them into natural response strategies, exhibiting a 'task-driven' rather than 'interaction-aware' bias." (Zeile 2105-2106)

### 3. Forschungslücken

**Lücke A (Keine ASR-/Transkriptionsevaluation):** TELEVAL fokussiert auf Interaktionsqualität, nicht auf Transkriptionsgenauigkeit; Phonem- oder Tonebene werden nicht evaluiert.

**Lücke B (Synthetische Daten teilweise):** Teile der Audiodaten sind synthetisiert.

**Lücke C (Kein L2-Sprecher):** Dialekte werden getestet, aber keine fremdsprachigen Mandarin-Sprecher.

> "although we utilize real human speech for tasks involving paralinguistic information, the synthesized audio used in other tasks may not fully capture the complexity and variability of natural human speech." (Zeile 2114-2117)

> "natural interaction is typically multi-turn. In our multi-turn evaluations, we only assess whether the model can effectively use historical context when generating responses, and do not measure the interactional appropriateness of multi-turn replies." (Zeile 2122-2126)

### 4. Adressierung durch meine Arbeit

- **Zu Lücke A:** Unsere Arbeit ergänzt TELEVAL, indem wir die gleichen Modelle (GPT-4o, Qwen2.5-Omni, Kimi-Audio etc.) auf einer völlig anderen Dimension evaluieren: phonetische und tonale Transkriptionsgenauigkeit (**Lücke 1, 2**).
- **Zu Lücke C:** Mit RQ3 (L2-Sprecher) gehen wir über TELEVALs Dialekt-Tests hinaus zu fremdsprachigen Lernern (**Lücke 3**).
- Das "Caption Trap"-Phänomen von TELEVAL motiviert unsere Hypothese, dass Modelle möglicherweise tonale Information wahrnehmen, aber nicht korrekt in Pinyin mit Tonnummern umsetzen können.

---

## Paper 29: VocalBench-zh: Decomposing and Benchmarking the Speech Conversational Abilities in Mandarin Context
**Quelle:** Liu et al. (2025), arXiv:2511.08230v1

### 1. Zentrale Learnings

VocalBench-zh ist ein umfassender Mandarin Speech-to-Speech Benchmark mit 10 Subsets und über 11.000 Instanzen, der 14 mainstream Modelle evaluiert. Der Benchmark deckt Wissen, Reasoning, Kreativität, Single-/Multi-Round Dialoge, Instruction Following, Emotional Empathy, Safety Alignment, Code-Switching und Robustheit ab. Alle Evaluationen sind strikt Speech-Instruction-basiert (keine Multiple-Choice oder Audio-Understanding-Formate).

Die Ergebnisse zeigen, dass das LLM-Backbone der Hauptfaktor für semantische Qualität ist, Scaling Laws deutlich erkennbar sind, und SpeechLLMs bei chinesischen Schriftzeichen-Strukturen und klassischer Literatur versagen. Qwen3-Omni (30B MoE) führt mit 78.439 Overall Score.

**Belegende Zitate:**

> "LLM backbone is the main factor affecting overall performance, especially semantic quality." (Observation 1, Zeile 474)

> "The existence of the scaling law is evident." (Observation 2, Zeile 862)

> "SpeechLLMs currently lack awareness of Chinese character components and structure." (Observation 5, Zeile 907)

> "Existing models exhibit limited effectiveness in understanding and generating ancient literary works." (Observation 6, Zeile 920-921)

> "SpeechLLMs lack control over the paralinguistic control of their speech responses." (Observation 10, Zeile 971-972)

### 2. Verwendbarkeit für unsere Arbeit

**RQ4:** VocalBench-zh liefert die umfassendste Rangliste von Mandarin-fähigen SpeechLLMs (Table 2): Qwen3-Omni (78.4) > MiMo-Audio-Instruct (76.2) > Kimi-Audio (69.5) > VocalNet2-8B (69.3) > Qwen2.5-Omni (67.9). Diese Rangliste dient als Referenz für die Auswahl unseres Modellsets.

**RQ5:** Observation 5 (fehlende Schriftzeichenstruktur-Awareness) und Observation 10 (fehlende paralinguistische Kontrolle) deuten darauf hin, dass SpeechLLMs auch bei phonetischen/tonalen Aspekten Schwächen haben dürften, da diese eng mit Mandarin-Schriftzeichenstruktur zusammenhängen.

**Lücke 1 & 2:** Trotz des Mandarin-Fokus evaluiert VocalBench-zh keine phonetische oder tonale Transkriptionsgenauigkeit. PER wird nur als Alignment-Metrik zwischen Text- und Sprachausgabe verwendet, nicht als ASR-Evaluationsmetrik.

> "the current version relies exclusively on synthetic data and does not include human-recorded speech." (Zeile 1041-1042)

### 3. Forschungslücken

**Lücke A (Keine phonetische/tonale Evaluation):** Trotz umfassender Mandarin-Evaluation fehlt jede Dimension für Pinyin-Transkription oder Tonerkennung.

**Lücke B (Nur synthetische Daten):** Keine menschlichen Aufnahmen enthalten.

**Lücke C (Kein L2-/Dialekt-Evaluation):** Dialektbewertung fehlt in der Erstversion.

> "real human recordings are essential for evaluating nuanced aspects such as subtle pronunciation variations, prosody, and natural dialogue dynamics." (Zeile 1043-1044)

> "this performance advantage is almost entirely confined to semantic quality, with negligible improvements observed in acoustic fidelity or emotional empathy." (Zeile 856-858)

### 4. Adressierung durch meine Arbeit

- **Zu Lücke A:** Unsere Arbeit evaluiert exakt die Dimension, die VocalBench-zh fehlt: phonetische und tonale Transkriptionsgenauigkeit bei Mandarin (**Lücke 1, 2**).
- **Zu Lücke B:** Wir verwenden das Tone-Perfect-Korpus mit realen menschlichen Aufnahmen (**Lücke 1**).
- **Zu Lücke C:** RQ3 adressiert L2-Sprecher (**Lücke 3**).
- Die Modellrangliste von VocalBench-zh erlaubt uns, zu untersuchen, ob die semantische Überlegenheit von Qwen3-Omni und MiMo-Audio auch auf phonetischer/tonaler Ebene gilt (**Lücke 4**).

---

## Paper 30: Qwen3-ASR Technical Report
**Quelle:** Qwen Team (2026), arXiv:2601.21337v1

### 1. Zentrale Learnings

Qwen3-ASR ist eine Familie von ASR-Modellen (1.7B und 0.6B Parameter), die 52 Sprachen und Dialekte unterstützen (30 Sprachen, 22 chinesische Dialekte). Die Modelle nutzen ein 4-stufiges Training: AuT Pretraining (40M Stunden pseudo-gelabelte Daten), Omni Pretraining (3T Tokens), ASR Supervised Finetuning und ASR Reinforcement Learning (GSPO). Qwen3-ASR-1.7B erreicht SOTA-Performance unter Open-Source-ASR-Modellen und ist kompetitiv mit proprietären APIs.

Besonders bemerkenswert ist Qwen3-ForcedAligner-0.6B, der erste LLM-basierte nicht-autoregressive Forced Aligner für 11 Sprachen, der Wort-/Zeichen-Level Timestamp-Vorhersagen liefert.

**Belegende Zitate:**

> "Qwen3-ASR-1.7B achieves state-of-the-art performance among open-sourced ASR models and is competitive with the strongest proprietary APIs" (Zeile 19-20)

> "Qwen3-ASR-0.6B can achieve an average time-to-first-token as low as 92ms and transcribe 2,000 seconds speech in 1 second" (Zeile 21-23)

> "RL plays an essential role for models' noise robustness, transcribing stability and ability to analyze difficult cases." (Zeile 200-201)

> "Qwen3-ASR-1.7B and Qwen3-ASR-0.6B finely support 30 languages, 22 Chinese dialects ASR" (Zeile 67)

### 2. Verwendbarkeit für unsere Arbeit

**RQ4:** Qwen3-ASR ist ein primärer Vergleichskandidat für unsere Arbeit als dediziertes ASR-System. Die berichteten Werte (Table 3: 4.97/5.88 CER auf WenetSpeech net/meeting für Qwen3-ASR-1.7B; 1.63/3.38 WER auf LibriSpeech clean/other) bilden starke Baselines. Der Head-to-Head-Vergleich zwischen Qwen3-ASR (dediziertes ASR) und multimodalen LLMs (GPT-4o, Qwen2.5-Omni etc.) auf Phonem-/Tonebene ist ein Kernbeitrag unserer RQ4.

**RQ2/RQ5:** Die 22 chinesischen Dialekte und die robuste Chinesisch-Performance machen Qwen3-ASR zu einem idealen Benchmark-System. Die Frage ist, ob die exzellente CER-Performance auch auf Tonebene gilt.

**Lücke 1 & 2:** Trotz umfassender Mandarin- und Dialekt-Unterstützung berichtet Qwen3-ASR ausschließlich WER/CER. Pinyin-Transkription oder Tonerkennung werden nicht als separate Evaluationsachse behandelt.

> "We conduct comprehensive internal evaluation besides the open-sourced benchmarks as ASR models might differ little on open-sourced benchmark scores but exhibit significant quality differences in real-world scenarios." (Zeile 17-18)

### 3. Forschungslücken

**Lücke A (Keine Ton-Evaluation):** Obwohl Qwen3-ASR Mandarin und 22 Dialekte unterstützt, gibt es keine Evaluation der tonalen Genauigkeit (TER) oder Pinyin-Transkription.

**Lücke B (Keine L2-Evaluation):** Nicht-muttersprachliche Sprecher werden nicht getestet, obwohl "elders and kids speech" im internen Benchmark enthalten ist.

**Lücke C (Nur Mandarin-Zeichen-Output):** Die Modelle geben Mandarin-Zeichen aus, keine Pinyin-Transkription; die Fähigkeit zur direkten Pinyin+Ton-Ausgabe ist unbekannt.

> "Qwen3-ASR delivers consistently strong performance across all subsets, and scaling from 0.6B to 1.7B yields stable gains." (Zeile 913-914)

> "the model learns to be an ASR-only model that does not follow natural-language instructions in the prompt, in order to mitigate instruction injection and instruction-following failures." (Zeile 186-187)

### 4. Adressierung durch meine Arbeit

- **Zu Lücke A:** Unsere Arbeit evaluiert Qwen3-ASR (bzw. Qwen-basierte Modelle) explizit auf Tonebene (TER) und Phonemebene (PER), was im Technical Report komplett fehlt (**Lücke 1, 2**).
- **Zu Lücke B:** Mit RQ3 testen wir L2-Sprecher, die in Qwen3-ASRs Evaluation nicht vorkommen (**Lücke 3**).
- **Zu Lücke C:** Unsere Arbeit untersucht, ob Modelle wie Qwen3-ASR (oder das zugrundeliegende Qwen3-Omni) direkt Pinyin mit Tonnummern (z.B. "ma1") generieren können, eine Fähigkeit die im Technical Report nicht dokumentiert ist.
- Der systematische Vergleich von Qwen3-ASR als dediziertem System gegen Frontier-multimodale LLMs auf Phonem-/Tongranularität ist ein Kernbeitrag unserer Arbeit (**Lücke 4**).


# Batch 6: Papers 31-36 (Survey LALMs Trust, Zipformer Mandarin, SCCM, L2 Tones Review, ML Tone Recognition, ASR Tonal Languages)


**Thesis-Kontext:** "Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech" (Stefan Dosch, IU M.Sc. Data Science)

---

## Paper 1: A Survey of Large Audio Language Models: Generalization, Trustworthiness, and Outlook

**Autoren:** Kaiwen Luo, Zhenhong Zhou, Leo Wang, Liang Lin et al. (2026), arXiv:2605.20266v1

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Umfassender Survey zu Large Audio Language Models (LALMs), der sowohl architektonische Grundlagen als auch Trustworthiness-Aspekte systematisch untersucht
  - LALMs durchlaufen eine Evolution von kaskadierten Systemen hin zu unified End-to-End-Frameworks; zentrale Modelle umfassen GPT-4o, Gemini-Reihe, Qwen-Audio, Whisper, SALMONN etc.
  - Architektur besteht aus drei Kernkomponenten: Acoustic Encoder, Alignment Projector, LLM Backbone
  - Sechs Trustworthiness-Dimensionen identifiziert: Hallucination, Robustness, Safety, Privacy, Fairness, Authentication
  - Schwerwiegendes Problem der "Modality Neglect": LALMs nutzen akustische Information oft nicht ausreichend und fallen auf textuelle Shortcuts zurueck
  - Emergente Reasoning-Mechanismen wie Audio-Chain-of-Thought (Audio-CoT) ermittelt

* **Belegende Zitate:**
  - "Systematic studies demonstrate that models over-rely on lexical cues rather than acoustic emotion signals, and that replacing audio inputs with silence or noise causes negligible performance changes on certain benchmarks" (Section 3.1, Hallucination and Faithfulness)
  - "The foundational capabilities established by Large Language Models (LLMs) have paved the way for Multimodal Large Language Models (MLLMs), within which Large Audio Language Models (LALMs) are essential for realizing universal auditory intelligence." (Abstract)
  - "LALMs are highly sensitive not only to semantic deception but also to non-semantic acoustic cues, where subtle shifts in tone can trigger safety violations" (Section 4.2.2)
  - "The evolution of LALMs from specialized speech recognition to complex paralinguistic reasoning necessitates a robust framework for assessing their trustworthiness in high-stakes domains." (Section 3)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Direkt relevant fuer die Positionierung der Thesis: Zeigt den aktuellen Stand multimodaler Audio-LLMs auf, deren Faehigkeiten und bekannte Schwaechen (RQ4: Vergleich mit dedizierten Systemen)
  - Das Problem der "Modality Neglect" ist hochrelevant fuer RQ2/RQ5: Wenn LALMs dazu neigen, akustische Signale zu ignorieren und sich auf textuelle Cues zu verlassen, koennte dies erklaeren, warum Tonunterscheidung in Mandarin problematisch ist
  - Die Taxonomie der Trustworthiness (besonders Hallucination und Robustness) liefert Rahmen fuer die Fehleranalyse (RQ5)
  - Chronologische Roadmap aller relevanten Audio-LLMs (GPT-4o, Gemini, Qwen-Audio etc.) als Referenz fuer Modellauswahl

* **Stuetzendes Zitat:**
  - "models over-rely on lexical cues rather than acoustic emotion signals, and that replacing audio inputs with silence or noise causes negligible performance changes on certain benchmarks" (Section 3.1) -- Dies stuetzt die Hypothese, dass multimodale LLMs bei feingranulaerer phonetischer Analyse (Tonerkennung) Schwaechen zeigen koennten.

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:** Der Survey deckt umfassend Architektur und Sicherheit ab, evaluiert aber LALMs nicht auf phonem-/tonspezifischer Granularitaet. Die Bewertung erfolgt primaer auf Satz-/Aufgabenebene (ASR WER, Emotion Recognition), nicht auf der Ebene einzelner Phoneme oder Toene. Zudem fehlt ein systematischer Head-to-Head-Vergleich multimodaler Modelle fuer tonal-spezifische Transkription.

* **Belegende Zitate:**
  - "Existing research predominantly details architectural innovations or specific concerns, yet there remains a significant lack of work dedicated to a systematic taxonomy of the safety implications for these systems." (Section 1, Introduction)
  - "the research landscape remains fragmented and lacks a unified roadmap" (Section 1)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:** Die Masterarbeit adressiert direkt Luecke 1 und Luecke 4: Waehrend dieser Survey LALMs auf Makroebene bewertet, fuehrt die Thesis eine Evaluation auf Phonem-/Ton-Granularitaet durch (PER, TER) und erstellt einen systematischen Head-to-Head-Vergleich von ~5-8 Frontier-Modellen. Die von dem Survey dokumentierte "Modality Neglect" kann durch unsere Ergebnisse empirisch fuer den spezifischen Fall der Mandarin-Tonerkennung geprueft werden.

---

## Paper 2: A Study on Phonemes Recognition Method for Mandarin Pronunciation Based on Improved Zipformer-RNN-T(Pruned) Modeling

**Autoren:** Zhaohui Du, Xiaofeng Zhao, Lin Li, Baohua Yu, Lijiang Miao (2025), PLoS ONE 20(5): e0324048

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Schlaegt ein verbessertes Zipformer-RNN-T(Pruned)-Modell fuer Mandarin-Phonem-Erkennung vor
  - Erstellt zwei neue Mandarin-Phonem-Datensaetze: AISHELL1-PHONEME und ST-CMDS-PHONEME durch Konvertierung von Zeichen-Labels in Phonem-Labels
  - Erreicht exzellente WER-Werte: 1.92% (Dev) / 2.12% (Test) auf AISHELL1-PHONEME und 4.28% (Dev) / 4.51% (Test) auf ST-CMDS-PHONEME
  - Das Modell hat nur 61.1M Parameter -- deutlich leichter als Vergleichsmodelle
  - Detaillierte Analyse von 66 Mandarin-Phonemen mit Konfusionsmatrix: haeufigste Verwechslungen bei /B/ vs. /P/, /IN/ vs. /ING/, /Z/ vs. /ZH/, /S/ vs. /SH/, /C/ vs. /CH/
  - Drei Kernverbesserungen: Deep Zipformer Block, GELU-basiertes Pred Network, hybride Pruned RNN-T/CTC Loss

* **Belegende Zitate:**
  - "tones play a crucial role in the semantic structure of Chinese, with the same syllable having completely different meanings depending on the tone. For example, 'ma' represents 'mother' 'hemp' 'horse', and 'scold' in the first to fourth tones, respectively." (Section 1, Introduction)
  - "the method performs excellently in the phoneme recognition task, achieving a Word Error Rate (WER) of 1.92% (Dev) and 2.12% (Test) on the AISHELL1-PHONEME dataset, and 4.28% (Dev) and 4.51% (Test) on the ST-CMDS-PHONEME dataset" (Abstract)
  - "Mandarin pronunciation is subdivided into 66 distinct phonemes. [...] the proposed model Zipformer-RNN-T(Pruned) demonstrates good phoneme recognition accuracy on both the AISHELL1-PHONEME and ST-CMDS-PHONEME datasets, covering all phoneme categories." (Section 4.3.1)
  - "the bilabial bursts /B/ and /P/ are often confused with each other due to subtle differences in amplitude and spectrum in the speech signal; [...] for front and back nasals, the difference between /IN/ and /ING/ is mainly in the presence or absence and duration of the nasal rhyme-final" (Section 4.6)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Direkt relevant als Vergleichssystem fuer RQ4: Stellt einen State-of-the-Art-Benchmark fuer dedizierte Mandarin-Phonem-Erkennung dar, gegen den die LLM-Ergebnisse gemessen werden koennen
  - Die Konfusionsmatrix-Analyse (Section 4.6) liefert wichtigen Kontext fuer RQ5 (Fehlerstruktur): zeigt, welche Phonemkontraste auch fuer spezialisierte Systeme schwierig sind
  - Die Methodik der Phonem-Label-Konvertierung (Zeichen -> Phoneme via phrase-level Mapping) ist methodisch relevant fuer unser Evaluations-Pipeline
  - WER-Metriken koennen als Referenzwerte fuer die Einordnung der LLM-Leistung dienen

* **Stuetzendes Zitat:**
  - "Chinese phoneme recognition tasks face two main challenges: first, the high requirement for modeling accuracy due to subtle phonetic differences such as tones; second, the strict limitations on inference speed and response delay in application scenarios." (Section 1)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:** Das Paper behandelt ausschliesslich dedizierte, trainierte Modelle fuer Phonem-Erkennung. Es gibt keinen Vergleich mit multimodalen LLMs oder Foundation Models. Zudem fehlen L2/Non-Native-Sprecher-Daten, und die Ton-Ebene wird nicht als separate Evaluationsachse behandelt -- Toene sind in den Phonem-Labels enthalten, werden aber nicht isoliert evaluiert.

* **Belegende Zitate:**
  - "there is a relative scarcity of Chinese reading evaluation datasets, which limits the effectiveness of model training and evaluation" (Section 5, Conclusion)
  - "future research will focus on exploring data augmentation techniques and self-supervised learning methods. By incorporating more diverse training data and unsupervised learning strategies, the goal is to address the problem of data scarcity" (Section 5, Conclusion)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:** Die Masterarbeit adressiert Luecke 1 und Luecke 2: Waehrend dieses Paper Phoneme als Ganzes evaluiert, separiert die Thesis Toene als explizite Evaluationsachse (TER neben PER und CER). Zudem stellt die Thesis den fehlenden Vergleich zwischen dedizierten Systemen und multimodalen LLMs her (Luecke 4). Die Analyse von L2-Sprechern (Luecke 3) geht ueber den Scope dieses Papers hinaus.

---

## Paper 3: A Syllable-Character Collaborative Model for Enhanced Pinyin and Chinese Recognition (SCCM)

**Autoren:** Zeyuan Chen, Cheng Zhong, Danyang Chen (2025), PLoS One 20(7): e0325045

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Schlaegt das Syllable-Character Collaborative Model (SCCM) vor, das Pinyin, Initiale/Finale und chinesische Zeichen gleichzeitig dekodiert
  - Architektur: Shared Conformer-Encoder + drei Decoder (Pinyin via CTC, Initiale/Finale via CTC, Zeichen via Transformer)
  - Pinyin-Ensemble (PE) Modul nutzt Ensemble Learning (10 Basisklassifikatoren) zur Verbesserung der Pinyin-Vorhersage
  - Erreicht CER von 6.4% (Text) und 3.1% (Pinyin) auf AISHELL-1 -- 45.7% relative CER-Reduktion gegenueber der Baseline
  - Ablationsstudie zeigt: Pinyin-Decoder allein reduziert CER signifikant; Kombination mit Initialen/Finalen und PE-Modul bringt weitere 28% Reduktion der Pinyin-Fehlerrate
  - Pycorrector als Nachverarbeitungsschritt korrigiert Homophon-Fehler

* **Belegende Zitate:**
  - "In Chinese speech recognition, end-to-end speech recognition models usually use Chinese characters as direct output and perform poorly compared with other language models. The main reason for this phenomenon is that the relationship between Chinese text and pronunciation is more complex." (Abstract)
  - "our approach not only reduces pinyin and character error rates compared to a prior end-to-end method using pinyin as auxiliary information, but also achieves a 45.7% relative reduction in Character Error Rate (CER) over the AISHELL-1 baseline." (Abstract)
  - "by integrating both pinyin and initials/finals information through our Pinyin-Ensemble (PE) module, we achieve a 28.0% reduction in pinyin error rate (from 3.54% to 2.55%) and a 14.0% reduction in text CER (from 7.83% to 6.73%)." (Experiment section)
  - "Pinyin embedding vectors share acoustic representations through the multi-task learning framework, which has been proven to reduce the impact of noise in speech and narrowing the search space." (Methodology)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Hoch relevant fuer RQ1 (Text-only Baseline: Character -> Pinyin): Das SCCM demonstriert, wie Pinyin als Zwischenschicht die Genauigkeit verbessert und bestaetigt, dass der Pfad Sprache -> Pinyin -> Zeichen effektiver ist als direktes Sprache -> Zeichen
  - Die Pinyin-CER von 3.1% ist ein wichtiger Benchmark fuer RQ4, da sie zeigt, was ein spezialisiertes System bei der Pinyin-Erkennung erreichen kann
  - Die Unterscheidung zwischen Pinyin, Initialen/Finalen als separate Modellierungseinheiten ist konzeptionell relevant fuer das Evaluationsdesign der Thesis
  - Die Multi-Task-Architektur illustriert den Vorteil von phonetischen Zwischenrepraesentationen

* **Stuetzendes Zitat:**
  - "It has been demonstrated that using pinyin as modeling units achieves the highest accuracy when decoded with CTC" (Section: Pinyin decoder module)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:** Das Paper evaluiert Pinyin ohne explizite Tonmarkierungen -- es wird nicht unterschieden, ob ma1, ma2, ma3, ma4 korrekt erkannt werden. Die Toninformation geht in der Pinyin-Darstellung verloren oder wird nicht separat evaluiert. Zudem gibt es keinen Vergleich mit multimodalen LLMs, und es werden nur Native-Speaker-Daten (AISHELL-1) verwendet.

* **Belegende Zitate:**
  - "The model fully utilizes the units of Chinese speech and simultaneously performs decoding tasks for initials and finals, pinyin and Chinese characters." (Conclusion) -- Hier fehlt die explizite Nennung von Toenen als eigene Evaluationseinheit.

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:** Die Masterarbeit schliesst direkt Luecke 2: Toene werden als separate, explizite Evaluationsachse mit eigenem TER-Metrik behandelt. Waehrend SCCM Pinyin als Ganzes evaluiert (z.B. "zhong" ohne Tonnummer), verwendet die Thesis Pinyin mit Tonnummern (z.B. "zhong1") und bewertet Ton-Korrektheit isoliert. Zudem adressiert die Thesis Luecke 3 (L2-Sprecher) und Luecke 4 (LLM-Vergleich).

---

## Paper 4: Perception-Production of Second-Language Mandarin Tones Based on Interpretable Computational Methods: A Review

**Autoren:** Yujiao Huang, Zhaohong Xu, Xianming Bei, Huakun Huang (2025/2026), Mathematics 14, 145

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Systematischer Review ueber L2-Mandarin-Tonerwerb an der Schnittstelle von Verhaltensforschung, Neurophysiologie und Computational Modeling
  - Organisiert in vier Straenge (Strands): (A) konventionelle Evaluationen, (B) physiologische Instrumentierung (EEG, Eye-Tracking), (C) rein akustische Audio-Analyse, (D) relationsbasiertes, erklaerbares Modelling (nascent/vorgeschlagen)
  - Zentrale Erkenntnis: Toene sind dynamische F0-Trajektorien; Steigung (slope), Wendepunkt-Timing (turning-point) und lokaler F0-Bereich (range) sind diagnostischer als absolute Tonhoehe
  - T2 (steigend) vs. T3 (fallend-steigend/tief) ist der persistenteste und schwierigste Kontrast fuer L2-Lerner
  - L1-Hintergrund (tonal vs. non-tonal) beeinflusst Cue-Weighting-Strategien signifikant
  - Schlaegt Graph Neural Network (GNN) + XAI-Framework vor fuer parametrisch ausgerichtetes, paedagogisch nutzbares Feedback

* **Belegende Zitate:**
  - "evidence converges on tones as time-evolving F0 trajectories, so movement, turning-point timing, and local F0 range are more diagnostic than height alone, and the contrast between Tone 2 (rising) and Tone 3 (dipping/low) remains the persistent difficulty" (Abstract)
  - "learners with tonal vs. non-tonal language backgrounds weight these cues differently" (Abstract)
  - "L2 productions judged 'categorically correct' often diverge from native targets in slope and turning-point timing; parameterized analysis therefore reveals residual gaps that label-only scoring misses" (Section 3.1)
  - "Persistent T2-T3 difficulty is robust across proficiency levels" (Section 3.1)
  - "the same learner will repeat the same items in different tasks (ID/AX vs. read-aloud), several learners will pronounce the same item, and perception-production pairs are naturally aligned by item and session. In other words, the data are relational." (Section 2.4)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Zentral fuer RQ5 (Fehlerstruktur): Die robuste Evidenz, dass T2 vs. T3 der schwierigste Kontrast ist, liefert Hypothesen, die in der Thesis ueberprueft werden koennen -- zeigen LLMs die gleichen Konfusionsmuster?
  - Direkt relevant fuer RQ3 (L2/Non-Native): Umfassende Evidenz zu L2-Tonschwierigkeiten, insbesondere die Modulation durch L1-Hintergrund, informiert das Evaluationsdesign
  - Die vorgeschlagenen Metriken (slope, turning-point timing, F0 range) koennen als Referenz fuer die Thesis-Fehleranalyse dienen
  - Die Evaluation-Metriken (per-tone F1, Confusion Matrix, RMSE for slope, MAE for TP) sind direkt uebertragbar auf das Thesis-Design

* **Stuetzendes Zitat:**
  - "the contrast between Tone 2 (rising) and Tone 3 (dipping/low) remains the persistent difficulty; learners with tonal vs. non-tonal language backgrounds weight these cues differently" (Abstract)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:** Es gibt keine Studie im Review, die multimodale LLMs fuer L2-Tonevaluation einsetzt. Strand D (relationsbasiertes, erklaerbares Modelling) wird als "nascent" und als "evidence gap" beschrieben -- es existieren nur zwei Studien. Zudem fehlt ein classroom-ready, item-matched P-P-Framework mit Relation-Aware-Explainability komplett.

* **Belegende Zitate:**
  - "none of the included studies meets our full criteria for a classroom-ready, item-matched perception-production (P <-> P) framework; therefore, we treat strand D as an evidence gap and as the design target of Section 4." (Section 2)
  - "Most audio-only tone recognizers still make an i.i.d. assumption: every token is fed to an encoder, a tone label is returned, and the system moves on. This is convenient but it does not match how L2 tone corpora are collected." (Section 2.4)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:** Die Masterarbeit adressiert Luecke 1 und Luecke 4 aus einer anderen Perspektive: Waehrend dieses Review dedizierte ML-Pipelines untersucht und deren Grenzen aufzeigt, evaluiert die Thesis multimodale Foundation Models (GPT-4o, Gemini etc.) auf der gleichen Granularitaetsebene (Phonem/Ton). Die T2/T3-Konfusion kann als Benchmark-Hypothese fuer RQ5 dienen. Die Thesis adressiert auch Luecke 3 (L2-Sprecher-Evaluation) direkt mit dem Tone Perfect Korpus.

---

## Paper 5: Machine Learning for Mandarin Tone Recognition: A Systematic Review with Applications to Neurotypical and Clinical Populations

**Autoren:** Aaron Zou, Xinran Han, Yena Koo, Xu Yan, Yang Zhang (2025, Preprint), doi:10.20944/preprints202510.2478.v1

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Systematischer Review von 61 Studien zu ML-basierter Mandarin-Tonerkennung nach PRISMA-Richtlinien
  - Deep Learning uebertrifft traditionelle Modelle signifikant (mittlere Genauigkeit 88.8% vs. 83.1%)
  - CNNs erreichen bis zu 99.16% Genauigkeit auf isolierten Silben (ToneNet), waehrend BiLSTM/Attention-Modelle in kontinuierlicher Sprache dominieren (7.03% TER)
  - Tone 3 ist konsistent die fehleranfaelligste Kategorie; T2/T3-Verwechslung ist der persistenteste Fehler
  - Kritische Luecken: fehlende diverse Datensaetze, schwache Prosodie-/Dialekt-Modellierung, unzureichende Validierungsstrenge
  - Praktische Empfehlungen fuer verschiedene Einsatzszenarien: CNN fuer isolierte Silben, BiLSTM+Attention fuer kontinuierliche Sprache, Random Forest fuer mobile/leichtgewichtige Anwendungen

* **Belegende Zitate:**
  - "Deep learning models outperform traditional approaches in Mandarin tone classification (mean accuracy 88.8% vs. 83.1%). Convolutional Neural Networks (CNNs) achieve up to 99.16% accuracy for isolated syllables, while Bidirectional Long Short-Term Memory (BiLSTM or BLSTM) and attention-based models improve continuous speech recognition by capturing temporal dependencies with 7.03% error rate." (Abstract)
  - "Performance is affected by Tone 3 variability, neutral tones, and challenging conditions like background noise and disordered speech." (Abstract)
  - "Confusion matrices consistently reveal Tone 3 as the most error-prone category." (Section: Model Validation and Performance Metrics)
  - "L2 corpora like iCALL (142 hours, 305 learners) exist but receive less attention than native-speaker datasets" (Section: Training and Testing Datasets)
  - "replacing hard targets with soft-label tone probabilities (e.g., [0.2, 0.6, 0.1, 0.1] for a Tone 2-Tone 3 blend) reduced the Equal Error Rate (EER) from 5.77% to 5.13% in mispronunciation detection" (Section: Discussion)
  - "Tonal errors are among the most frequent and socially impactful barriers to fluency; unlike segmental mistakes, tonal mispronunciations often render speech not merely accented but unintelligible" (Introduction)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Zentral fuer RQ4 (Vergleich mit dedizierten Systemen): Liefert umfassende Performance-Benchmarks fuer traditionelle und Deep-Learning-Modelle der Tonerkennung
  - Direkt relevant fuer RQ5 (Fehlerstruktur): Robuste Evidenz, dass T3 die fehleranfaelligste Kategorie ist und T2/T3 der schwierigste Kontrast -- diese Hypothese kann in der Thesis fuer LLMs getestet werden
  - Die Metrik TER (Tone Error Rate) wird als Standard fuer kontinuierliche Sprache vorgeschlagen -- identisch mit unserem Evaluationsdesign
  - Die Empfehlungen zu FAR/FRR fuer Mispronunciation Detection sind relevant fuer das Metrikdesign der Thesis
  - Evidenz zu L2-Lerner-Herausforderungen (soft-label approaches, Tone 3 dominante Fehler) informiert RQ3

* **Stuetzendes Zitat:**
  - "For continuous speech, the Tone Error Rate (TER) analogous to Word Error Rate is preferred. TER computes the Levenshtein distance between predicted and reference tone sequences, accounting for substitutions, insertions, and deletions" (Section: Model Validation)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:** Der gesamte Review behandelt ausschliesslich dedizierte ML-Modelle (HMMs, SVMs, CNNs, BiLSTMs, Transformers) fuer Tonerkennung. Multimodale Foundation Models (GPT-4o, Gemini etc.) werden nicht einmal erwaehnt. Es fehlt zudem: (1) Evaluation auf L2/klinischen Populationen mit robusten Datensaetzen, (2) Standardisierung von Datensaetzen und Metriken, (3) Anwendung auf emotionale/dialektale Sprachvariationen.

* **Belegende Zitate:**
  - "While deep learning models represents the state of the art, several gaps limit practical deployment, including the lack of diverse datasets, weak prosody and dialect modeling, and insufficient validation rigor." (Abstract/Conclusion)
  - "Real-world deployment remains limited by a lack of diverse, representative datasets, weak modeling of prosody and dialect variation, and inconsistent validation practices." (Conclusion)
  - "Few datasets include speech from L2 learners or clinical populations, or account for common variables like tone sandhi, emotional prosody, or dialectal variation." (Introduction)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:** Die Masterarbeit fuellt eine fundamentale Luecke dieses Reviews: Multimodale Foundation Models werden erstmals systematisch auf Mandarin-Tonerkennung evaluiert (Luecke 4). Die Thesis nutzt die gleichen Metriken (TER, Confusion Matrices, F1 per tone) und kann ihre Ergebnisse direkt gegen die hier berichteten Benchmarks einordnen. Die Evaluation auf dem Tone Perfect Korpus mit isolierten Silben schliesst an die staerksten Ergebnisse dedizierter Systeme an (99.16% bei ToneNet) und ermoeglicht einen fairen Vergleich (Luecke 1).

---

## Paper 6: Automatic Speech Recognition System for Tonal Languages: State-of-the-Art Survey

**Autoren:** Jaspreet Kaur, Amitoj Singh, Virender Kadyan (2021), Archives of Computational Methods in Engineering 28:1039-1068

### 1. Zentrale Learnings

* **Zusammenfassung:**
  - Umfassender Survey ueber ASR-Systeme fuer tonale Sprachen weltweit: Asien (Chinesisch, Mandarin, Thai, Vietnamesisch, Mizo, Bodo), Indo-Europaeisch (Punjabi, Litauisch, Schwedisch, Kroatisch), Afrikanisch (Yoruba, Hausa)
  - Mandarin hat 5 Toene (4 lexikalische + Neutralton); Thai hat 5 Toene; Vietnamesisch hat 6 Toene
  - F0 ist der primaere Cue fuer Ton; ergaenzende Features: Delta-F0, F0-Intercept, segmentweise F0-Mittelwerte
  - Historische Entwicklung: von HMM-GMM (1990er) ueber DNN-HMM zu End-to-End-Modellen (CTC, RNN-T, Attention)
  - Fuer Mandarin: Kontextabhaengige Tonmodelle (CD) uebertreffen kontextunabhaengige (CI); Ton-Kern-Ansatz verbessert Erkennung
  - Viel Arbeit fuer asiatische Tonsprachen, sehr wenig fuer Indo-Europaeische und Afrikanische

* **Belegende Zitate:**
  - "ASR of tonal language needs the detection of tone, consonants and vowels of syllable." (Section 3.2)
  - "The language is said to be a tonal language, if the meaning of the word is changed with the pitch of word. The vowels in the tonal language must be recognized first. The Fundamental Frequency (f0) act as a cue for tone and phoneme acts as a cue for first, second formants (f1, f2)." (Section 3.2)
  - "It is observed that the lot of work have been done for the Asian continent tonal languages i.e. Chinese, Thai, Vietnamese, Mandarin but little work been reported for the Mizo, Bodo, Indo-European tonal languages like Punjabi, Latvian, Lithuanian as well for the African continental tonal languages i.e. Hausa and Yourba." (Abstract)
  - "The new tone recognition method was developed by studying the physical parameter that is the Time Periods of the Successive Human Vocal Cords Vibrations (F0) measured in Hertz and relative heights between neighboring tones. The full syllable method obtained 75.3% tone recognition accuracy whereas the proposed method accounted 85.5% tone recognition accuracy." (Section 5.2, re: Zhang and Hirose)

### 2. Verwendbarkeit fuer unsere Arbeit

* **Konkreter Nutzen:**
  - Liefert historischen Kontext fuer RQ4: Dokumentiert die Evolution von ASR-Systemen fuer Mandarin von fruehem HMM-basiertem Tonmodelling bis zu End-to-End-Ansaetzen
  - Die phonologischen Tabellen (Table 2) zu tonalen Sprachen sind nuetzlich fuer die Einleitung der Thesis: Mandarin hat 5 Toene, 7 Vokale, 22 Konsonanten
  - Historische CER-Werte fuer Mandarin ASR (z.B. 9.1% CER fuer DARPA GALE 2007) dienen als Vergleichspunkte
  - Die Beschreibung der F0-basierten Features und Tonmodellierung ist relevant fuer das Verstaendnis der Grundlagen der Tonerkennung

* **Stuetzendes Zitat:**
  - "ASR of tonal language needs the detection of tone, consonants and vowels of syllable. Tonal features are given below: f0 and its f0 difference (Delta-f0); Subsection of f0 and f0 intercept; Subsection mean of f0 and mean of Delta-f0" (Section 3.2)

### 3. Forschungsluecken (Research Gaps)

* **Identifizierte Luecke:** Der Survey ist auf traditionelle und fruehe Deep-Learning-ASR-Systeme (vor 2020) beschraenkt. Es fehlen voellig: (1) Multimodale Foundation Models und LLMs, (2) Self-Supervised Learning Ansaetze (wav2vec 2.0 etc.), (3) Evaluation auf Phonem-/Ton-Granularitaet jenseits von CER/WER, (4) L2/Non-Native-Sprecher-Szenarien. Der Survey ist zudem datiert (publiziert 2021, Literatur groesstenteils vor 2019).

* **Belegende Zitate:**
  - "It is observed that the lot of work have been done for the Asian continent tonal languages i.e. Chinese, Thai, Vietnamese, Mandarin but little work been reported for the Mizo, Bodo, Indo-European tonal languages" (Abstract)
  - "Many issues and challenges related with tonal languages are discussed." (Abstract)

### 4. Adressierung durch meine Arbeit

* **Meine Abdeckung:** Die Masterarbeit aktualisiert und erweitert diesen aelteren Survey grundlegend: Sie evaluiert die neueste Generation multimodaler Foundation Models (GPT-4o, Gemini, Qwen etc.) auf Mandarin-Tonerkennung -- eine Modellkategorie, die 2020 noch nicht existierte (Luecke 4). Die Thesis fuehrt zudem die fehlende feingranulaere Evaluation (PER, TER separat) ein (Luecke 1 und 2) und beruecksichtigt L2-Sprecher (Luecke 3), was in diesem Survey gaenzlich fehlt. Der historische Kontext dieses Papers ist wertvoll fuer die Einordnung des technologischen Fortschritts.


# Batch 7: Papers 37-40 (ResNet Siamese Tones, MSPB, L2 Mandarin Rating, PYG-ASR)


---

## Paper 1

### Evaluating Mandarin Tone Pronunciation Accuracy for Second Language Learners Using a ResNet-Based Siamese Network (Bu et al., 2025, Scientific Reports)

**1. Zentrale Learnings**

* **Zusammenfassung:**
  - Vorgestellt wird eine Methode zur automatischen Bewertung der Mandarin-Tonaussprache fuer L2-Lerner basierend auf einem Siamese Network (SN) mit integrierten Bilderkennungsmodellen (ResNet-18, VGG-16, AlexNet).
  - Tonmerkmale werden als 40D-Vektor (1D-Feature) und als 40x50 Binaer-Pixelbild (2D-Feature) extrahiert, basierend auf der Normalisierung der F0-Kontur mittels des Fuenf-Stufen-Tonskala-Modells (Chao).
  - Ein dediziertes Korpus fuer Ton-Forschung wurde konstruiert, bestehend aus standard- und nicht-standard-akzentuiertem Mandarin (u.a. THCHS-30, AIShell-1/2/3, plus Eigenaufnahmen von tibetischen L1-Sprechern).
  - ResNet-18 mit 2D-Features erzielte die besten Ergebnisse: MSE 2.295 / RMSE 1.515 im subjektiven Vergleich mit Experten, MSE 0.189 / RMSE 0.435 im objektiven Test.
  - Tone3 wird von allen Modellen am besten unterschieden, was auf markante Erkennungsmerkmale hindeutet.
  - Der Ansatz operiert auf Silben-Ebene, nicht auf Satzebene, und bewertet Tonunterschiede als Diskrepanz-Score (0-5 Skala).

* **Belegende Zitate:**
  - "Evaluating tone pronunciation is essential for helping second-language (L2) learners master the intricate nuances of Mandarin tones." (Abstract)
  - "We identified two key features from the normalized pitch contour: a 40D vector (1D feature) and a 40 x 50 binary pixel image (2D feature), effectively capturing each syllable's tonal characteristics." (Abstract)
  - "In subjective evaluations, our model achieved a Mean Squared Error (MSE) of 2.295 and a Root Mean Squared Error (RMSE) of 1.515 compared to expert assessments. We recorded an MSE of 0.189 and an RMSE of 0.435 in objective evaluations." (Abstract)
  - "ResNet-18 emerges as the top performer, demonstrating a robust ability to understand and differentiate these features, regardless of whether 1D or 2D features are utilized." (Discussion, Models performance)
  - "Compared to the other tones, Tone3 is more effectively distinguished and recognized by different models. This suggests that Tone3 is easier for the models to learn and has distinctive recognition features, which remain consistent whether using 1D or 2D features." (Discussion, Performance of different tones during evaluation)

**2. Verwendbarkeit fuer unsere Arbeit**

* **Konkreter Nutzen:**
  - Direkt relevant als Vergleichssystem fuer die Ton-Evaluationsachse unserer Thesis (Luecke 2, RQ5). Bu et al. repraesentieren einen dedizierten, spezialisierten Ansatz zur Tonbewertung, gegen den die off-the-shelf multimodalen LLMs benchmarked werden koennen (RQ4).
  - Die Erkenntnis, dass Tone3 von trainierten Modellen am leichtesten erkannt wird, liefert eine Hypothese fuer unsere Fehlerstrukturanalyse (RQ5): Wenn LLMs ebenfalls Tone3 seltener verwechseln, spricht das fuer universelle akustische Marker; andernfalls fuer modellspezifische Schwaechen.
  - Das Fuenf-Stufen-Modell und die Silben-Ebene als Evaluationseinheit stuetzen unser Pinyin-basiertes Design auf Phonem-/Ton-Granularitaet (Luecke 1).
  - Nutzung von THCHS-30 und AIShell als Korpora zeigt Benchmarking-Standards, die wir mit Tone Perfect vergleichen koennen.

* **Stuetzendes Zitat:**
  - "Unlike non-tonal languages such as English, German, Uyghur, and Kazakh, Mandarin is a tonal language that features four distinct tones: High-Level (Tone1), Low-Rising (Tone2), Falling-Rising (Tone3), and High-Falling (Tone4). These tones are crucial for differentiating word meanings and grammatical structures, making mastery of Mandarin tones a significant hurdle in L2 acquisition." (Introduction)
  - "We do not specifically distinguish between citation (underlying) forms and sandhi (surface) realizations. For example, both sandhi-modified Tone3 (which acoustically approximates Tone2 while remaining phonemically distinct from Tone2) and canonical Tone3 are systematically categorized under the T3 tonal class." (Experiments, Datasets)

**3. Forschungsluecken (Research Gaps)**

* **Identifizierte Luecke:**
  - Die Methode stuetzt sich ausschliesslich auf berechnete Tonmerkmale (F0-basierte 1D/2D-Features), die die Komplexitaet der Toncharakteristika moeglicherweise uebervereinfachen. Es fehlt eine End-to-End-Evaluation direkt aus Roh-Audiodaten.
  - Kein Vergleich mit modernen LLM-basierten oder Transformer-basierten Sprachmodellen.
  - Die Evaluation erfolgt nur fuer L2-Sprecher mit tibetischem L1-Hintergrund -- keine breite Diversitaet von L1-Hintergruenden.

* **Belegende Zitate:**
  - "However, the method still relies on the computed tone features, which may oversimplify the complexity of tone characteristics, presenting a limitation of the current approach. Future research will aim to extract raw, deep tone features from speech data using neural networks and effectively integrate these features into the existing framework." (Conclusion)
  - "most studies depend on publicly available corpora, which can vary significantly, and there is currently no dedicated corpus specifically designed for tone research." (Introduction)

**4. Adressierung durch meine Arbeit**

* **Meine Abdeckung:**
  - Unsere Thesis adressiert die Luecke des fehlenden Vergleichs mit modernen LLM-basierten Modellen (Luecke 4): Wir testen multimodale Frontier-Modelle (GPT-4o, Gemini etc.) direkt auf Ton-Granularitaet und vergleichen sie mit dedizierten Systemen wie Whisper. Bu et al.s spezialisierter Ansatz dient als Kontrast zu unserem off-the-shelf-Paradigma.
  - Die separate Ton-Evaluationsachse (TER) in unserer Arbeit adressiert Luecke 2, waehrend Bu et al. Toene nur als Diskrepanz-Score behandeln, nicht als separates Transkriptionselement.
  - Unsere Thesis nutzt Tone Perfect mit diverseren L1-Hintergruenden (RQ3, Luecke 3), waehrend Bu et al. nur tibetische L1-Sprecher einbeziehen.

---

## Paper 2

### Can AI Understand Mandarin Speech Prosody? A Framework and Benchmark Showcase (Wang et al., Interspeech 2025 -- MSPB)

**1. Zentrale Learnings**

* **Zusammenfassung:**
  - Einfuehrung des Mandarin Speech Prosody Benchmark (MSPB) mit 8 prosodischen Tasks (Intonation, prosodische Disambiguierung, prosodische Fokusmarkierung, Fokus-Operator, Skalar, Ironie, emotionale Prosodie mit/ohne Kontext).
  - 6 Speech LLMs evaluiert: GPT-4o, Gemini-1.5-Pro, Gemini-2-Flash, Qwen2-Audio-7B-Instruct, GLM-4-Voice, MiniCPM-o 2.6.
  - GPT-4o erzielte die hoechste Durchschnittsgenauigkeit (59.70%), gefolgt von Gemini-2-Flash (59.28%) und Gemini-1.5-Pro (56.05%).
  - Signifikante Performanzluecke zwischen Speech LLMs und menschlichen Teilnehmern (81.01%-95.67% Genauigkeit bei Menschen vs. bestenfalls ~60% bei Modellen).
  - Modelle performen besser bei Tasks mit kontextuellen Hinweisen (Ironie, Fokus-Operator), scheitern aber bei rein prosodisch-abhaengigen Tasks (Fokusmarkierung, Skalar-Bedeutung).
  - Menschliche Teilnehmer erzielten nahezu perfekte Ergebnisse bei emotionaler Prosodie mit Kontext (95.67%) und Intonation (95.29%).

* **Belegende Zitate:**
  - "Although some models perform well with context-rich cues (e.g., irony), they generally struggle with subtle prosodic variations (e.g., focus marking) and underperform humans." (Abstract)
  - "GPT-4o achieved the highest average accuracy(59.70%), followed by Gemini-2-flash (59.28%) and Gemini-1.5-pro (56.05%)." (Results, 3.2.2)
  - "Human testee significantly outperformed models, especially when prosodic cues were the sole cues available, underscoring the considerable challenge these tasks pose for Speech LLMs." (Section 3.3)
  - "the better performance on context-rich tasks compared to prosody-dependent tasks underscores a critical limitation of current Speech LLMs: they rely more on contextual cues than on subtle speech prosody variations." (Results, 3.2.2)
  - "Mandarin Chinese presents a particularly complex interplay between tone and intonation, a characteristic that distinguishes it from many Western languages." (Introduction)

**2. Verwendbarkeit fuer unsere Arbeit**

* **Konkreter Nutzen:**
  - Hoechst relevant als direkte Evidenz fuer die Schwaechen multimodaler LLMs bei prosodischer Verarbeitung von Mandarin (Luecke 1). Die Ergebnisse zeigen, dass genau die Modelle, die wir testen wollen (GPT-4o, Gemini-Reihe), Schwierigkeiten mit subtilen prosodischen Variationen haben.
  - MSPB evaluiert Prosodie auf Satz-/Pragmatik-Ebene, waehrend unsere Thesis auf Phonem-/Ton-Granularitaet fokussiert -- die Arbeiten sind komplementaer (Luecke 1).
  - Die Modellauswahl (GPT-4o, Gemini, Qwen2-Audio) ueberlappt stark mit unserer Modellpalette und liefert Vergleichswerte fuer verwandte Aufgaben.
  - Die Erkenntnis der Kontextabhaengigkeit statt reiner prosodischer Verarbeitung stuetzt unsere Hypothese, dass LLMs moeglicherweise Toene nicht direkt aus dem Audiosignal erkennen, sondern sich auf lexikalisches Wissen stuetzen (RQ1, RQ2).

* **Stuetzendes Zitat:**
  - "Crucially, they often neglect the pragmatic implications conveyed by prosodic cues, which are paramount for accurately discerning speaker intent, especially in interactive dialogues. Furthermore, many current benchmarks overlook language-specific prosodic profiles." (Introduction)
  - "This highlights a critical need for future research to better integrate fine-grained prosodic information into Speech LLMs." (Conclusion)

**3. Forschungsluecken (Research Gaps)**

* **Identifizierte Luecke:**
  - MSPB evaluiert Prosodie auf pragmatischer Ebene (Ironie, Fokus, Emotion), aber NICHT auf der Ebene lexikalischer Toene als phonetische Transkription. Es gibt keine Phonem-/Ton-Transkriptionsaufgabe.
  - Nur Zero-Shot Single-Round Evaluation in Mandarin; keine Multi-Turn-Szenarien oder multilinguale Erweiterung.
  - Kein L2/Non-Native-Sprecher-Test -- nur Native-Aufnahmen einer einzelnen Sprecherin.

* **Belegende Zitate:**
  - "It is noteworthy that this study focused on a zero-shot, single-round approach in Mandarin, and future work should explore multi-turn conversational settings and expand to a multilingual benchmark." (Conclusion)
  - "Despite these limitations, our findings provide crucial insights into the current state of Speech LLMs and underscore the importance of prosodic understanding for achieving truly natural and effective human-machine interaction." (Conclusion)

**4. Adressierung durch meine Arbeit**

* **Meine Abdeckung:**
  - Unsere Thesis schliesst die Luecke der fehlenden Phonem-/Ton-Granularitaet (Luecke 1): Waehrend MSPB Prosodie auf Satzebene evaluiert, testen wir dieselben Modelle (GPT-4o, Gemini) auf der Ebene einzelner Tontranskriptionen (PER, TER).
  - Wir adressieren die fehlende L2-Dimension (Luecke 3): MSPB testet nur Native-Sprechersprache, waehrend wir auch Non-Native-Aufnahmen aus Tone Perfect einbeziehen.
  - Die Tonebene als separate Evaluationsachse (Luecke 2) wird in MSPB nicht abgedeckt -- genau das ist unser Kernbeitrag.
  - Unser Head-to-Head-Vergleich (Luecke 4) geht ueber MSPB hinaus, indem wir dedizierte Systeme (Whisper) einbeziehen.

---

## Paper 3

### The Assessment of Automated Rating of L2 Mandarin Prosody in Lexical Tone Recognition and Pauses (Wu & Shen, Speech Prosody 2024)

**1. Zentrale Learnings**

* **Zusammenfassung:**
  - Evaluation zweier kommerzieller Automated-Rating-Engines (A1: chinesisches Unternehmen; A2: internationales Unternehmen) fuer L2-Mandarin-Leseaufgaben, mit Fokus auf Tonerkennung und Pausen.
  - Die Diagnostic Accuracy (DA) fuer Tonerkennung erreichte im Durchschnitt ca. 80% ueber alle vier Toene; FAR und FRR wurden ebenfalls berichtet.
  - Die Korrelation zwischen Automated Engines und menschlichen Ratern erreichte maximal 0.60 (H-A1 overall), deutlich unter den 0.69-0.81 Korrelationen, die bei massgefertigten Testsystemen berichtet werden.
  - Grosse Diskrepanzen bei Fluency-Scores: A1 bewertet Fluency streng (Mean 53.1), A2 extrem mild (Mean 97.9), menschliche Rater bei 83.8.
  - Die False Rejection Rate (FRR) uebersteigt durchgehend die False Acceptance Rate (FAR) -- die Engines sind zu streng bei der Identifikation von Fehlern.
  - 12 L2-Lerner mit diversen L1-Hintergruenden (Indonesisch, Englisch, Japanisch, Nepali).

* **Belegende Zitate:**
  - "The study found that the overall accuracy of tonal diagnosis reached 80%, as determined through the calculation of false rejection and false acceptance rates." (Abstract)
  - "the False Rejection Rate (FRR) still exceeds the False Acceptance Rate (FAR) of a minimum 10%. This implies that a higher percentage of correctly pronounced items are falsely rejected compared to the mispronounced items being falsely accepted." (Results, 3.2)
  - "the highest correlation, at 0.60, is observed between the scores of the human raters and A1, which is slightly below the lowest correlation of 0.69 reported in prior studies." (Results, 3.1)
  - "readily available automated tools may not offer significant advantages for language instructors focusing on accuracy and fluency assessment." (Discussion)
  - "Research indicates that the majority of pronunciation errors made by L2 Mandarin learners pertain to tones rather than vowels or consonants." (Introduction)

**2. Verwendbarkeit fuer unsere Arbeit**

* **Konkreter Nutzen:**
  - Direkt relevant fuer RQ4 (Vergleich mit dedizierten Systemen): Die Studie zeigt Baseline-Performance kommerzieller Automated-Rating-Engines bei L2-Tonerkennung mit FAR/FRR/DA-Metriken -- exakt die Metriken, die wir auch verwenden (FAR, FRR).
  - Die Erkenntnis, dass generische Engines schlechter korrelieren als massgefertigte Systeme, stuetzt unsere Hypothese, dass off-the-shelf multimodale LLMs ebenfalls Schwaechen bei Tongenauigkeit haben koennten.
  - Die L2-Lerner-Daten und die Beobachtung, dass Toene die Hauptfehlerquelle bei L2-Lernern sind (nicht Konsonanten/Vokale), stuetzt die Relevanz unserer RQ3 und RQ5.
  - Die tonspezifischen FAR/FRR-Werte (Tone 1: DA 75%, Tone 2: DA 85%, Tone 3: DA 84%, Tone 4: DA 73%) liefern direkte Vergleichswerte fuer unsere Fehlerstrukturanalyse.

* **Stuetzendes Zitat:**
  - "The average DA across the four tones (Tones 1 to 4) has reached 82.45% for the development of automated engines." (Section 2.2)
  - "For detailed tone recognition and error diagnosis in Mandarin, three metrics were proposed before: False Rejection Rate (FRR), representing the percentage of correctly pronounced units falsely rejected; False Acceptance Rate (FAR), denoting the percentage of mispronounced units falsely accepted; and Diagnostic Accuracy (DA), indicating the percentage of phones correctly recognized relative to human raters." (Section 2.2)

**3. Forschungsluecken (Research Gaps)**

* **Identifizierte Luecke:**
  - Keine Untersuchung der Beziehung zwischen Sprechgeschwindigkeit und Fluency-Bewertung.
  - Keine Visualisierung von Pitchkurven und Intonationsmustern als Feedback-Mechanismus.
  - Nur 12 Lerner mit ungleichmaessiger L1-Verteilung; keine systematische Analyse des L1-Einflusses.
  - Nur Lese-Aufgaben (read-aloud), keine freien Antworten.
  - Kein Vergleich mit modernen multimodalen LLMs oder Whisper.

* **Belegende Zitate:**
  - "It is important to note that this study did not explore the relationship between speech rate in the fluency descriptor of automated models and the fluency scores assigned by human raters." (Discussion)
  - "To enhance alignment in fluency and prosody assessments, future research should develop and analyze more additional metrics in automated rating. Ideally, these metrics can also be sensible with both human raters and learners, incorporating elements such as visualization of pitch curves and intonation patterns." (Discussion)
  - "Furthermore, additional audio recordings with more evenly distributed L1 background will be collected to study whether this influences the accuracy of scoring." (Discussion)

**4. Adressierung durch meine Arbeit**

* **Meine Abdeckung:**
  - Unsere Thesis adressiert die Luecke des fehlenden Vergleichs mit multimodalen LLMs (Luecke 4): Wir vergleichen Frontier-Modelle und Whisper direkt mit den Leistungsniveaus, die Wu & Shen fuer kommerzielle Engines berichten.
  - Die separate Ton-Evaluation (Luecke 2) ist unser Kerndesign: Waehrend Wu & Shen DA/FAR/FRR pro Ton berichten, erweitern wir dies auf eine vollstaendige Transkriptionsaufgabe mit PER und TER.
  - Wir nutzen das Tone-Perfect-Korpus mit kontrollierten Aufnahmen und haben eine systematischere L1-Diversitaet (Luecke 3), obwohl unser Hauptfokus auf Native-Sprechern liegt mit L2 als Stretch Goal.
  - Die von Wu & Shen verwendeten Metriken (FAR, FRR) sind identisch mit denen in unserem Design -- direkte Vergleichbarkeit.

---

## Paper 4

### Pinyin-Guided Chinese Speech Recognition with Large Language Model (Zhengjie & Cheng, Interspeech 2025 -- PYG-ASR)

**1. Zentrale Learnings**

* **Zusammenfassung:**
  - PYG-ASR ist ein neuartiges chinesisches ASR-Framework, das Pinyin-Vorhersage explizit in LLM-basierte Spracherkennung integriert, um phonetische Komplexitaet im Chinesischen besser abzubilden.
  - Zwei Outputstrategien: PYG-ASR1 (sequentielle Ausgabe: erst gesamte Pinyin-Sequenz, dann Zeichensequenz) und PYG-ASR2 (iterative Ausgabe: Pinyin-Zeichen-Paare).
  - Pinyin-Guided Error Correction (PYGEC) nutzt Chain-of-Thought-Prompting mit einem Text-LLM (DeepSeek-v3) zur Fehlerkorrektur.
  - 25% CER-Reduktion auf AISHELL-1 Test-Set (von 4.0% auf 3.0% CER); Pinyin Error Rate von 1.9%.
  - Kontextuelles Biasing via Pinyin-Matching (PYGEC-CB) reduziert B-CER um 49.2% relativ.
  - Architektur: HuBERT Speech-Encoder + Qwen2-7B LLM, trainiert mit LoRA.

* **Belegende Zitate:**
  - "This paper proposes Pinyin-Guided ASR (PYG-ASR), which innovatively modifies the LLM-ASR to simultaneously map acoustic features to both Pinyin and text tokens, enhancing linguistic representation." (Abstract)
  - "Experiments show that PYG-ASR reduces CER by 25% on the AISHELL-1 test set." (Abstract)
  - "In Chinese, for instance, a small number of characters originate as pictographs and ideographs, but the vast majority are what are called phono-semantic compounds, which involve an element of pronunciation in their meaning. This indicates that there is no direct correspondence between the pronunciation and the written form of Chinese characters." (Introduction)
  - "The presence of tonal variations and a high number of homophones makes it essential to explicitly model pronunciation features, a capability inherently absent in most LLM-ASR models." (Introduction)
  - "The PYG-ASR1 model achieved a Pinyin error rate of 1.6%/1.9%, and PYG-ASR2 gets 1.7%1.9%." (Results, 4.1)
  - "Directly mapping acoustic features to Chinese characters without the aid of Pinyin is like trying to write Chinese characters without the guidance of Pinyin. While it's not impossible, it can be more challenging and prone to transcription errors." (Introduction)

**2. Verwendbarkeit fuer unsere Arbeit**

* **Konkreter Nutzen:**
  - Hoechst relevant fuer RQ1 (Pinyin als Transkriptionsformat) und RQ4 (Vergleich mit dedizierten Systemen): PYG-ASR demonstriert, dass explizite Pinyin-Modellierung die ASR-Genauigkeit signifikant verbessert -- unsere Thesis evaluiert, ob off-the-shelf multimodale LLMs ohne solche explizite Modellierung ueberhaupt zuverlaessig Pinyin generieren koennen.
  - Die Pinyin Error Rate von 1.9% liefert einen harten Benchmark fuer die Pinyin-Transkriptionsgenauigkeit, gegen den wir unsere Modelle messen koennen.
  - Die Erkenntnis, dass LLM-ASR-Modelle ohne Pinyin-Guidance Homophon-Fehler und Tonverwechslungen produzieren, stuetzt direkt unsere RQ5 (Fehlerstruktur).
  - Die Chain-of-Thought-Fehlerkorrektur zeigt, dass Text-LLMs effektiv zur Nachbearbeitung von Pinyin-Transkriptionen eingesetzt werden koennen -- relevant fuer unsere RQ1 (Text-only Pinyin-Konversion).

* **Stuetzendes Zitat:**
  - "However, this approach may introduce biases in languages with rich phonetic complexity, such as Chinese." (Introduction)
  - "Despite the guidance of Pinyin, cases still persist where the pronunciation is accurately recognized, yet the corresponding Chinese characters are incorrectly generated." (Section 2.4)

**3. Forschungsluecken (Research Gaps)**

* **Identifizierte Luecke:**
  - Nur auf AISHELL-1 evaluiert -- keine breitere Generalisierung auf andere Korpora, Sprecherstile oder L2-Sprecher.
  - Keine Evaluation auf Ton-Granularitaet: Pinyin Error Rate wird global berichtet, aber nicht nach Toenen aufgeschluesselt.
  - Kein Vergleich mit multimodalen Frontier-Modellen (GPT-4o, Gemini) in einer End-to-End-Transkriptionsaufgabe.
  - Nur Native-Speaker-Daten; keine Non-Native/L2-Evaluation.

* **Belegende Zitate:**
  - "For future works, we intend to conduct larger-scale experiments and explore a more profound integration of Pinyin information into LLM-ASR models." (Conclusion)

**4. Adressierung durch meine Arbeit**

* **Meine Abdeckung:**
  - Unsere Thesis adressiert die fehlende Ton-Granularitaet (Luecke 1 und Luecke 2): Waehrend PYG-ASR die Pinyin Error Rate global berichtet, schluesseln wir nach PER (Phoneme) und TER (Toene) separat auf.
  - Wir testen genau die multimodalen Frontier-Modelle (Luecke 4), die bei PYG-ASR fehlen: GPT-4o, Gemini, Claude etc. werden auf derselben Aufgabe (Audio-zu-Pinyin) evaluiert, aber off-the-shelf ohne Finetuning.
  - Unsere Thesis schliesst die L2-Luecke (Luecke 3): PYG-ASR testet nur Native-Sprecher auf AISHELL-1, waehrend wir L2-Sprecher als Stretch Goal einbeziehen.
  - PYG-ASRs Pinyin Error Rate (1.9%) dient als quantitativer Benchmark fuer unseren Head-to-Head-Vergleich (RQ4).


---

# Abschliessende Zusammenfassungstabelle

| # | Paper Titel | Learnings (Zusammenfassung) | Learnings (Exakte Zitate) | Verwendbarkeit fuer Thesis | Forschungsluecken (Zusammenfassung) | Luecken (Exakte Zitate) | Adressierte Luecken |
|---|---|---|---|---|---|---|---|
| 1 | Seed-ASR: Understanding Diverse Speech and Contexts with LLM-based Speech Recognition (Seed Team/ByteDance, 2024) | Seed-ASR ist ein LLM-basiertes ASR-Modell mit Audio-Encoder (~2B Parameter) und MoE-LLM, das 2.98% CER auf Mandarin-Benchmarks erreicht. Evaluation erfolgt ausschliesslich auf CER/WER ohne phonetische oder tonale Granularitaet. | "Seed-ASR is developed based on the framework of audio conditioned LLM (AcLLM), leveraging the capabilities of LLMs by inputting continuous speech representations together with contextual information into the LLM." / "Word error rate (WER) is often considered a core metric for evaluating the performance of ASR models, but certain parts of content (e.g. keyword) in a sentence plays a more crucial role in the understanding of the whole sentence." | Liefert CER-Baselines (2.98%) fuer Mandarin-ASR zur Kontextualisierung unserer Ergebnisse (RQ2); bestaetigt durch reine CER-Evaluation die Luecke feingranularer phonetischer Metriken (RQ3). | Evaluiert nur auf CER/WER; keine Aufschluesselung nach Initialen, Finalen oder Toenen; tonale Dimension wird trotz Mandarin-Unterstuetzung nicht separat untersucht. | "In future, we will focus on extending Seed-ASR's ability to deal with multiple tasks within a single model, further enhance the long-form ability and increase the number of supported languages." | L1, L2, L4 |
| 2 | Comparing efficacy of IPA vs Pinyin romanisation transcriptions for complex tonal languages -- A case study in Baima (Chirkova et al., 2025) | Untersucht automatische Tontranskription der Tonsprache Baima und vergleicht IPA, Pinyin und Simple Romanisation mit Wav2Vec2, MMS und Whisper. Fuehrt die Metriken "tonal CER" und "tonal WER" ein; Toene sind der schwierigste Part der Transkription. | "tones remain the most difficult part of the transcription" / "The tonal CER is the percentage of characters in this transcription that are wrong. The tonal WER is the percentage of words that have a tonal error in them." | Direkte methodische Vorlage: Vergleich von IPA vs. Pinyin als Transkriptionsformat zeigt Einfluss auf Tongenauigkeit (RQ1); separate tonale CER/WER-Berechnung ist Vorbild fuer unsere Evaluationsstrategie (RQ3, RQ4). | Untersucht nur traditionelle ASR-Modelle, nicht multimodale LLMs; arbeitet mit extrem ressourcenarmer Sprache (Baima, 186 Min.), nicht Mandarin. | "In this paper we therefore focus on transcription systems and how they might impact the automatic transcription of complex tones, testing different base models as well as the usefulness of adding an LM to the decoding process." | L1, L2, L4 |
| 3 | AI Index Report 2026, Chapter 2 -- Technical Performance (Stanford HAI, 2026) | Dokumentiert die Konvergenz der Frontier-Modelle (Top-4 innerhalb 25 Elo-Punkten) und Benchmark-Saettigung als zentrales Problem. Die Luecke zwischen US- und chinesischen Modellen hat sich fast vollstaendig geschlossen. | "the gap between top models is shrinking. This narrowing extends geographically, as the distance between top U.S. and Chinese models has closed almost completely." / "Benchmark saturation, where models reach scores so high that a test can no longer distinguish between them, remains a concern." | Die Modellkonvergenz auf generischen Benchmarks unterstreicht den Bedarf an differenzierenden, domaenenspezifischen Tests wie tonale Transkription (RQ2); unsere feingranulare Evaluation bietet die Art von Metrik, die der Bericht als noetig identifiziert (RQ3). | Keine ASR-spezifischen oder phonetischen Benchmarks; Audio/Sprache wird nicht als separate Evaluationskategorie gefuehrt. | "Several challenges highlighted in previous editions of this report persist. Benchmark saturation, where models reach scores so high that a test can no longer distinguish between them, remains a concern." | L1, L4 |
| 4 | AudioBench: A Universal Benchmark for Audio Large Language Models (Wang et al., 2025) | Erster umfassender Benchmark fuer AudioLLMs mit 8 Tasks und 26 Datensaetzen; kein Modell dominiert ueber alle Aufgaben. ASR wird nur ueber WER evaluiert, ausschliesslich fuer Englisch, ohne phonetische oder tonale Dimension. | "We introduce AudioBench, a universal benchmark designed to evaluate Audio Large Language Models (AudioLLMs). It encompasses 8 distinct tasks and 26 datasets" / "the current AudioBench exclusively includes English datasets. However, multilingual capabilities and code-switching are crucial for comprehensive speech understanding and generation." | Zeigt, dass kein AudioLLM ueber alle Aufgaben dominiert, was aufgabenspezifischen Vergleich motiviert (RQ2); bestaetigt fehlende multilinguale und tonale Evaluationsdimension (RQ3). | Nur englische Datensaetze; ASR nur ueber WER gemessen; multilinguale Faehigkeiten und tonale Sprachen als explizite Luecken benannt. | "First, the current AudioBench exclusively includes English datasets. However, multilingual capabilities and code-switching are crucial for comprehensive speech understanding and generation." | L1, L2, L4 |
| 5 | Large Language Model Should Understand Pinyin for Chinese ASR Error Correction (Li et al./Huawei, 2024) | PY-GEC zeigt, dass Pinyin als phonetische Repraesentation die LLM-basierte ASR-Fehlerkorrektur signifikant verbessert (8.3% relative CER-Reduktion). Multitask-Training steigert die Text-Pinyin-Alignment-Kosinus-Aehnlichkeit von 0.26 auf 0.82. | "multitask training enhances overall performance and contributes to a relative CER reduction of 8.3% and a relative entity recall improvement of 3.9%" / "ASR errors, unlike typographical and grammatical errors, often involve misrecognizing one word as another due to similar pronunciation." | Demonstriert direkt, dass LLMs Pinyin verstehen koennen (RQ1); Homophone als Hauptfehlerquelle relevant fuer tonale Verwechslungsmuster (RQ4, RQ5). | Text-basierte Fehlerkorrektur, nicht direkte Audio-zu-Pinyin-Transkription; nur CER auf Zeichenebene ohne separate tonale Fehleranalyse. | "For future research, we aim to extend our experiments to larger-scale LLMs and multi-modal LLMs." | L1, L2, L4 |
| 6 | FireRedASR2S: A State-of-the-Art Industrial-Grade All-in-One ASR System (Xu et al./Xiaohongshu, 2026) | Industrietaugliches ASR-System mit LLM- und AED-Variante; erreicht 2.89% CER auf Mandarin-Benchmarks und 11.55% auf 19 Dialekt-Benchmarks. Evaluation erfolgt ausschliesslich ueber CER. | "FireRedASR2-LLM achieves 2.89% average CER on 4 public Mandarin benchmarks and 11.55% on 19 public Chinese dialects and accents benchmarks" / "We use Character Error Rate (CER, %) for Chinese." | Starke Mandarin-CER-Referenz (2.89%) zur Kontextualisierung unserer Ergebnisse (RQ2); bestaetigt die Luecke feingranularer phonetischer Evaluation (RQ3). | Nur CER-Evaluation; keine phonetische Fehleranalyse; keine Analyse tonaler Fehler trotz Mandarin- und Dialekt-Unterstuetzung. | "Future work will focus on further improving performance and expanding support for more languages." | L1, L2, L4 |
| 7 | Deep Learning Approaches for Automatic Speech Recognition: A Survey (Ahlawat et al., 2025) | Umfassender Ueberblick ueber DNN-basierte ASR-Methoden (2010-2024), deckt Evolution von HMMs zu Transformern ab und definiert zentrale Metriken (WER, CER, PER). Beschreibt Mandarin-Datensaetze (AISHELL-1/2) und moderne Modelle wie Whisper. | "The Phone error rate (PER) is computed by dividing the total number of phonemes by the number of phoneme errors (inserted, deleted, and modified phonemes)." / "High-resource languages like English, Mandarin, and Spanish benefit from abundant labeled data, resulting in a lower WER." | Definiert PER/CER/WER-Metriken formal (RQ2); liefert Hintergrund zu Whisper, wav2vec 2.0, HuBERT als Vergleichssysteme (RQ4). | Evaluation beschraenkt auf WER/CER auf Satzebene; keine phonem- oder tonspezifische Granularitaet; tonale Sprachen nicht als eigene Herausforderungskategorie. | "considerable research is still needed before speech recognition becomes widely accessible and consistently reliable for all users." | L1, L2, L3, L4 |
| 8 | Pitch-Aware RNN-T for Mandarin Chinese Mispronunciation Detection and Diagnosis (Wang et al., 2024) | Praesentiert ein RNN-T-Modell fuer Mandarin-MDD mit HuBERT-Features und Pitch-Embeddings; verwendet das Initial-Final-Tone-System mit 5 Toenen und 214 tonalen Phonem-Tokens. Trainiert auf AISHELL-1 (Native), evaluiert auf LATIC (L2). | "Mispronunciation Detection and Diagnosis (MDD) systems face two main challenges in Mandarin Chinese: 1) The two-stage models create an information gap 2) The scarcity of Mandarin MDD datasets limits model training." / "Our model, trained solely on native speaker data, shows a 3% improvement in Phone Error Rate and a 7% increase in False Acceptance Rate over the state-of-the-art baseline in non-native scenarios." | Zeigt phonem- und tonebene-Evaluation auf nativen und L2-Daten -- direkt relevant fuer RQ2, RQ3, RQ5; spezialisiertes MDD-System als Benchmark fuer Frontier-LLMs (RQ4). | Kein Vergleich mit multimodalen Foundation Models; nur ein L2-Datensatz (LATIC); Datenmangel fuer L2-Mandarin bleibt kritisch. | "We anticipate that the findings presented herein will serve as a catalyst for future research in the areas of tonal language Automatic Speech Recognition and Mispronunciation Detection and Diagnosis." | L1, L2, L3, L4 |
| 9 | Tone-syllable synchrony in Mandarin: New evidence and implications (Kang & Xu, 2024) | Beweist durch minimal-contrast-Methodik und statistische Analyse (GAMMs, Bayes-Faktoren >200), dass Ton und Vokal in Mandarin-Silben vollstaendig synchron einsetzen (CVT co-onset). Der Ton beginnt innerhalb der artikulatorischen Silbe, nicht antizipatorisch davor. | "The results indicate that tone and vowel onsets are fully synchronized. There is therefore evidence for strict alignment of consonant, vowel and tone as hypothesized in the synchronization model of the syllable." / "there is now quantitative evidence for full CVT co-onset as proposed in the synchronization model of the syllable" | Liefert phonetisch-linguistische Begruendung, warum Ton integral mit dem Segment ist und separat evaluiert werden muss (RQ5); theoretische Grundlage fuer TER als Metrik. | Nur Produktionsstudie, keine Perzeption oder maschinelle Erkennung; nur Native-Sprecher; keine Verbindung zu ASR. | "What remains less clear is the laryngeal dimension of the syllable, for which evidence of tone synchrony with the consonant-vowel syllable has been circumstantial." | L1, L2 |
| 10 | Sparks of Large Audio Models: A Survey and Outlook (Latif et al., 2023) | Umfassendes Survey der Large Audio Models (SpeechGPT, AudioPaLM, Whisper etc.); identifiziert Herausforderungen bei Tokenisierung, Prompt-Sensitivitaet, Halluzinationen und multilingualer Verarbeitung. | "This paper offers the first comprehensive survey on Large Audio Models, capturing the nuanced interplay of various LLMs within the audio sector." / "Multilingual speech tokenisation poses additional complexities as the same statement might demand a varying number of tokens in different languages." | Ueberblick ueber ASR-Faehigkeiten von LAMs und Modellbeschreibungen (Whisper, SeamlessM4T) fuer RQ4; zeigt fehlende tonale/phonemische Evaluation in der gesamten LAM-Landschaft. | Tokenisierung fuer tonale Sprachen ungeloest; sprachspezifische Prompts fuer Audio-Modelle unerforscht; Halluzinationen in Audio-Modellen kaum untersucht. | "Despite the progress made, to our knowledge, the literature has yet to explore the design and testing of specialised prompts tailored specifically for speech-based scenarios." | L1, L2, L3, L4 |
| 11 | Speech Translation with Foundation Models and Large Language Models: A Survey (Gaido et al., 2024) | Analysiert 9 Arbeiten zur SFM+LLM-Integration fuer Speech-to-Text-Translation; identifiziert 5 architektonische Bausteine. Fordert standardisierte Benchmarks, feinkoernige Evaluation jenseits von BLEU und Nutzung prosodischer Information. | "the emergence of the SFM+LLM solution calls for thorough and fine-grained evaluations to investigate its peculiarities compared to other, more traditional methods." / "the speech source contains a wide range of information that can be exploited depending on the paradigm used (e.g., prosody is not handled by cascade systems)." | Fordert explizit standardisierte, feinkoernige Evaluation -- stuetzt direkt unsere Methodik (RQ4); Prosodie-Nutzung durch SFM+LLM unerforscht, relevant fuer Tonerkennung (RQ2). | Fehlende standardisierte Trainings-/Evaluationssettings; feinkoernige Evaluation fehlt; Prosodie-Nutzung durch SFM+LLM unerforscht. | "Therefore, we advocate for future research to adhere to established data-setting standards, paving the way for cumulative progress and shared understanding in the field." | L1, L2, L4 |
| 12 | When LLMs Meet Speech: A Survey on Integration of Speech with Large Language Models (Yang et al., ACL 2025) | Systematische Taxonomie der LLM-Speech-Integration (text-basiert, latent-basiert, audio-token-basiert); identifiziert English-zentrierten Bias, fehlendem fairem Vergleich und fehlende Mehrsprachigkeit als zentrale Probleme. | "Most existing speech-LLM models are designed to handle only English, which limits their range of applicability." / "there is a notable gap in comparing the different integration approaches under a unified setting." | Fordert faire Vergleiche unter einheitlichen Bedingungen -- genau unser Design (RQ4); English-zentrierter Bias motiviert Mandarin-Evaluation (RQ1, RQ2). | English-zentrierter Bias; kein fairer Vergleich zwischen Integrationsansaetzen; standardisierte Benchmarks und Protokolle fehlen. | "A fair comparison among these integration methods could clarify how different factors affect performance. Developing standardized benchmarks, protocols, and reporting practices for speech-LLM research would help future work." | L1, L2, L4 |
| 13 | Recent Advances in Speech Language Models: A Survey (Cui et al., 2025) | Ueberblick ueber SpeechLMs mit End-to-End-Sprachverarbeitung; identifiziert Informationsverlust paralinguistischer Merkmale (Pitch, Tonalitaet) in ASR+LLM+TTS-Pipelines. Text-Speech-Alignment kann Ton-Information kompromittieren. | "Speech signals not only contain semantic information but also paralinguistic information (e.g., pitch, timbre, tonality, etc.). Putting a text-only LLM in the middle will cause the complete loss of paralinguistic information" / "text primarily conveys semantic information, which can improve a SpeechLM's semantic modeling capabilities but may compromise its ability to capture paralinguistic features, such as tone and emotion" | Erklaert, warum End-to-End-Modelle Toene besser bewahren koennten als Pipeline-Ansaetze (RQ4); Text-Speech-Alignment als Erklaerung fuer Tonschwaechen (RQ2, RQ5). | Kein systematischer Vergleich der Architekturkomponenten; keine Evaluation auf Phonem-/Ton-Granularitaet; Faehigkeit von SpeechLMs bei tonalen Merkmalen unerforscht. | "there remains a significant gap in understanding the advantages and disadvantages of different component selections. Therefore, studies aimed at comprehensively comparing these choices are essential." | L1, L4 |
| 14 | A Survey on Speech Large Language Models for Understanding (Peng et al., 2025) | Definiert Speech Understanding als multimodalen Prozess mit drei Dimensionen (informationell, funktional, formatbezogen). Speech LLMs kaempfen mit feingranularer akustischer Information und zeigen Instruction Sensitivity und Reasoning Degradation. | "Speech LLMs often struggle to maintain robustness in the face of nuanced or ambiguous instructions." / "they often struggle to reliably capture and reason over acoustic information, leading to restricted performance in tasks that require fine-grained auditory understanding" | Drei-Dimensionen-Taxonomie ordnet Thesis-Task ein (paralinguistisch/Perception); Schwaeche bei fine-grained auditory understanding stuetzt direkt unsere Forschungsluecke (RQ2, RQ5). | Feingranulare akustische Informationsextraktion bleibt unzureichend; Anwendung auf tonale Sprachen und explizite Ton-Evaluation nicht behandelt. | "Although current models can process speech to some extent, their ability to capture the full range of acoustic cues, such as speaker emotion, intonation, and environmental noise, remains limited." | L1, L2 |
| 15 | MMAU: A Massive Multi-Task Audio Understanding and Reasoning Benchmark (Sakshi et al., 2024) | Benchmark mit 10.000 MC-Fragen; beste Modelle erreichen nur ~53% (vs. 82% Mensch). Kaskadierte Ansaetze (Caption+LLM) uebertreffen direkte Audio-LLMs. Modelle performen am schlechtesten bei Speech-Reasoning. | "even the most advanced Gemini Pro v1.5 achieves only 52.97% accuracy" / "reasoning over spoken language -- especially perception beyond mere content -- remains a challenge" | Bestaetigt empirisch, dass Modelle bei Speech-Tasks schlecht abschneiden (RQ2); kaskadierte Ansaetze besser als direkte Audio-LLMs -- relevant fuer Whisper-Vergleich (RQ4). | Nur Multiple-Choice, keine offene Generierung; keine Tasks fuer tonale Sprachen; Phonemanalyse nur auf Stress-Muster beschraenkt. | "MMAU focuses on multiple-choice tasks and does not evaluate open-ended generation, which allows models to reason more freely and exhibit errors such as language hallucinations." | L1, L2, L4 |
| 16 | Dynamic-SUPERB Phase-2: A Collaboratively Expanding Benchmark with 180 Tasks (Huang et al., 2024) | Groesster Benchmark fuer universelle Sprachmodelle mit 180 Tasks inkl. Phonetik-Kategorie und Third Tone Sandhi Recognition. Modelle zeigen katastrophale Performance bei Phoneme Recognition (PER ~100), nur SALMONN erreicht ~25 PER. | "In phoneme recognition (PR), the SALMONN models were the only ones to achieve a relatively lower phoneme error rate (PER) compared to the others." / "using ASR such as Whisper remains a strong baseline for language understanding, as text more explicitly represents semantic information than speech" | Zeigt dramatisches Versagen universeller Modelle bei Phonem-Erkennung (RQ2); Whisper-basierte Kaskaden dominieren weiterhin (RQ4); Third Tone Sandhi Task ist thematisch verwandt. | Ton-Evaluation fuer tonale Sprachen minimal (nur Tone Sandhi als Klassifikation); keine separate TER-Metrik; Frontier-Modelle (GPT-4o, Gemini) nicht evaluiert. | "Although Dynamic-SUPERB Phase-2 is the largest and most comprehensive benchmark, we acknowledge its limitations. It lacks comprehensive speech-generation tasks." | L1, L2, L4 |
| 17 | ASR-EC Benchmark: Evaluating Large Language Models on Chinese ASR Error Correction (Wei et al., 2024) | Erster chinesischer ASR-Error-Correction-Benchmark; vergleicht Prompting (ineffektiv, Overcorrection), Finetuning (moderat) und multimodale Augmentation (am besten). Zero-Shot-Prompting verschlechtert CER. | "Prompting is not effective for ASR error correction. Finetuning is effective only for a portion of LLMs. Multi-modal augmentation is the most effective method." / "Under the zero-shot and one-shot settings, LLMs tend to correct every sentence, regardless of whether the sentence has errors or not." | Warnsignal: Zero-Shot-Prompting versagt bei Chinese ASR Error Correction (RQ1); multimodale Ansaetze deutlich besser als textbasierte -- stuetzt Motivation fuer multimodale Modelle (RQ4). | Nur Character-Level-Fehlerkorrektur (CER); keine phonem- oder tonspezifische Fehleranalyse; Frontier-Modelle nicht als multimodale Audio-Modelle getestet. | "LLMs still face challenges in correcting errors requiring deep contextual understanding" | L1, L2 |
| 18 | PERL: Pinyin Enhanced Rephrasing Language Model for Chinese ASR N-Best Error Correction (Liang & Zhang, 2025) | PERL fusioniert semantische (BERT) und phonetische (Pinyin-Encoder) Embeddings; erreicht 29.11% CER-Reduktion auf Aishell-1. LLMs (GPT-4o, DeepSeek) performen schlechter als PERL, da generative Modelle bei laengenkonstrained Aufgaben Schwaechen haben. | "In Mandarin Chinese, ASR errors often follow phonetic patterns, as many characters share similar pronunciations." / "LLMs perform worse than PERL on DoAD. We attribute this to the inherent limitations of generative models when handling tasks with length constraints." | Zeigt, dass Pinyin-Information zentral fuer Mandarin-ASR-Korrektur ist (RQ1, RQ5); explizite Pinyin-Embeddings ueberlegen LLMs -- Kontrastpunkt fuer off-the-shelf-Evaluation (RQ4). | Fokus auf Error Correction (Post-Processing), nicht direkte Transkription; Toene nicht als separate Evaluationsachse; kein L2-Sprecher-Test. | "balancing phonetic and semantic information under strict length constraints remains an open challenge" | L1, L2, L3, L4 |
| 19 | VoxEval: Benchmarking the Knowledge Understanding Capabilities of End-to-End Spoken Language Models (Cui et al., 2025) | End-to-End SLMs versagen bei Wissensverstaendnis (meist unter Zufallsbaseline 25%); Pitch-Shifts sind die groesste Herausforderung. Chain-of-Thought Prompting reduziert SLM-Leistung, anders als bei Text-LLMs. | "Results reveal that current SLMs perform poorly, with most failing to surpass random guessing." / "Different input audio conditions have different impacts on SLMs. [...] pitch shifts pose the greatest challenge for most SLMs." | Pitch-Sensitivitaet direkt relevant fuer Mandarin-Tonerkennung, da Toene Pitch-Konturen sind (RQ2, RQ3); CoT-Befund informiert Prompt-Strategie (RQ1). | Keine Evaluierung tonaler Sprachen oder phonetischer Merkmale; nur Englisch und inhaltliches Wissensverstaendnis; synthetische TTS-Sprache statt natuerlicher Aufnahmen. | "existing question-answering (QA) benchmarks fall short in evaluating SLMs' knowledge understanding due to their inability to support end-to-end speech evaluation and account for varied input audio conditions" | L1, L2, L4 |
| 20 | FireRedASR: Open-Source Industrial-Grade Mandarin Speech Recognition Models (Xu et al./Xiaohongshu, 2025) | FireRedASR-LLM erreicht 3.05% CER (SOTA) auf Mandarin; professionell transkribierte Daten sind entscheidend (1000h hochwertig > 10000h schwach-gelabelt). Whisper-Large-v3 erreicht nur 9.86% CER auf Mandarin. | "FireRedASR-LLM (8.3B parameters) achieves an average Character Error Rate (CER) of 3.05%, surpassing the latest SOTA of 3.33%." / "one thousand hours of high-quality, human-labeled data yields better results than ten thousand hours of weakly-labeled data." | Mandarin-SOTA-CER (3.05%) als obere Referenz; Whisper-CER (9.86%) als Baseline fuer unseren Vergleich (RQ4); erklaert Whisper-Schwaeche bei Mandarin (RQ1). | Nur CER-Evaluation, keine phonetische/tonale Transkription; keine off-the-shelf Frontier-LLM-Evaluierung; keine Pinyin-Dekomposition. | "Future work will focus on further improving performance and expanding support for more languages and varied tasks." | L1, L2, L4 |
| 21 | On The Landscape of Spoken Language Models: A Comprehensive Survey (Arora et al., 2025) | Taxonomie von SLMs: Pure Speech LMs, Speech+Text LMs, Speech-aware Text LMs; optimale Sprachrepraesentation (diskret vs. kontinuierlich) ist ungeklaert. SLM-Evaluation ist fragmentiert und deckt nicht Prosodie-spezifische Aufgaben ab. | "The optimal representation of speech within SLMs remains unclear." / "Existing benchmarks also do not cover the full range of spoken language tasks. [...] speech tasks include [...] a variety of speech-specific tasks related to speaker properties, accents, or prosody-specific content." | Taxonomie direkt anwendbar auf evaluierte Modelle (RQ1); prosodie-spezifische Aufgaben fehlen in SLM-Evaluation -- Mandarin-Toene sind prosodische Merkmale (RQ2). | Fokus auf High-Resource-Sprachen; fehlende Benchmarks fuer phonetische/tonale Aufgaben; fragmentierte Evaluation. | "SLM research has, thus far, understandably focused on high-resource languages and settings. [...] it will be critical to make them accessible to as broad a range of users as possible." | L1, L2, L4 |
| 22 | Kimi-Audio Technical Report (Kimi Team/Moonshot AI, 2025) | Aktuelle Audio-Vortrainierung fokussiert auf "was gesagt wird" und vernachlaessigt Paralanguage-Information wie Ton. Semantische Audio-Tokens basieren auf ASR-Hilfsverlusten und erfassen keine akustischen Details wie Toene. | "Current pre-training paradigms [...] text transcription focuses on the content of spoken words (what is said), neglecting [...] paralanguage information (e.g., emotion, style, timbre, tone)" / "Semantic tokens are typically obtained by ASR-based auxiliary loss, which focuses on transcription-oriented information and fails to capture rich acoustic details" | Erklaert fundamentalen Mechanismus, warum Audio-LLMs bei Tonerkennung versagen koennten: semantische Tokens kodieren Tonhoehenkonturen nicht (RQ2); AISHELL-Ergebnisse als Kontextualisierung (RQ4). | Toninformation in Vortrainierung vernachlaessigt; Token-Repraesentationen erfassen keine akustischen Details; Audio-Modelle durch ASR/TTS-Systeme begrenzt. | "text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)" | L1, L2, L4 |
| 23 | Towards Holistic Evaluation of Large Audio-Language Models: A Comprehensive Survey (Yang et al., 2025) | Evaluierungslandschaft fuer LALMs ist fragmentiert; LALMs zeigen Instabilitaet bei Content-based Reasoning ueber Sprechstile. Low-Resource-Sprachen und sprachliche Vielfalt sind unterrepraesentiert. | "While numerous benchmarks have emerged to assess LALMs' performance, they remain fragmented and lack a structured taxonomy." / "many overlook crucial aspects such as low-resource languages and code-switching. Although these have been explored in traditional speech technologies, they remain underexamined in LALMs." | Fragmentierte Evaluierung betrifft auch phonetische/tonale Dimension (RQ1-RQ5); Instabilitaet ueber Sprechstile relevant fuer Tone-Perfect-Evaluation (RQ3). | Tonale Merkmale nicht systematisch evaluiert; sprachliche/kulturelle Vielfalt unterrepraesentiert; Catastrophic Forgetting bei Modalitaetsadaption. | "Auditory cues such as tone, emotion, and voice quality can also influence user experience and raise concerns if uncontrolled." | L1, L2, L4 |
| 24 | ZIPA: A family of efficient models for multilingual phone recognition (Zhu et al., 2025) | Phonerkennungsmodelle glaetten soziophoneetische Variation und geben Woerterbuchaussprachen statt tatsaechlicher Aussprache wieder. Vokalsubstitutionen sind der haeufigste Fehlertyp; 64M-Parameter-Modelle uebertreffen 300M-Baselines. | "Phone recognition models tend to smooth out the phonetic variation during inference. [...] ZIPA predictions tend to better match the standard dictionary pronunciation than the actual pronunciation." / "Compared to consonants, vowel realizations tend to be more gradient in their acoustics, resulting in higher acoustic overlap." | Glaettungsphaenomen relevant fuer Mandarin-Tonerkennung bei natuerlicher Variation (RQ2); Vokalverwechslungen relevant fuer Mandarin-Finalen (RQ3, RQ5). | Tonale Dimension nicht separat evaluiert (numerische Tonrepraesentation statt IPA-Tonbuchstaben); Sprachskewing in Trainingsdaten; Woerterbuch- vs. tatsaechliche Aussprache. | "simply scaling up the G2P for transcribed speech data alone might not be able to solve phone recognition, as models can simply memorize the standard pronunciation." | L1, L2, L4 |
| 25 | WildSpeech-Bench: Benchmarking Audio LLMs in Natural Speech Conversation (Zhang et al., 2025) | Benchmark fuer End-to-End Speech LLMs mit 1.100 realen Szenarien inkl. Paralinguistic-Featured Queries (Pause, Stress, Tone). GPT-4o-Audio fuehrt (6.29/10), aber selbst beste Modelle zeigen erhebliche Schwaechen bei paralinguistischen Phaenomenen. | "even the best model, GPT-4o-Audio, has an average score of only 6.29 points. This indicates that there is still significant room for improvement." / "Existing evaluation methods often adapt text-based benchmarks, overlooking speech's unique characteristics and challenges, including prosody, homophones, stuttering." | Modellrangliste (GPT-4o > Qwen-2.5-Omni) als Referenz (RQ4); PF-Subkategorie "Tone" zeigt Schwaechen, allerdings bei englischer Intonation, nicht Mandarin-Toenen (RQ2). | Nur Englisch; keine Transkriptionsmetriken; keine phonetische oder tonale Evaluationsachse; Bewertung basiert auf subjektiver Antwortqualitaet. | "our current benchmark focuses solely on single-turn dialogue evaluation, failing to comprehensively capture the intricacies of multi-turn speech interactions." | L1, L2, L4 |
| 26 | ContextASR-Bench: A Massive Contextual Speech Recognition Benchmark (Wang et al., 2025) | Kontextueller ASR-Benchmark mit >40.000 Datenpunkten (Englisch+Mandarin); fuehrt NE-WER und NE-FNR ein. LALMs uebertreffen konventionelle ASR dank LLM-Weltwissen, haben aber noch erhebliche Schwaechen. | "LALMs outperform conventional ASR models by a large margin thanks to the strong world knowledge and context modeling of LLMs" / "WER treats all words equally, conflicting with human evaluation priorities that emphasize critical content, such as named entities, technical terms." | Mandarin-Testsets und LALM-Vergleichswerte (z.B. Qwen2.5-Omni 1.95% WER) als Referenz (RQ4); Argument fuer differenziertere Metriken stuetzt PER/TER-Ansatz (RQ2). | Trotz Mandarin-Abdeckung nur Wort-/Entity-Level-Evaluation; keine Phonem-/Ton-Evaluation; synthetische TTS-Daten; kein L2-Sprecher. | "current LALMs still struggle in contextual ASR, indicating ample room for improvement." | L1, L2, L3 |
| 27 | Step-Audio 2 Technical Report (StepFun Audio Team, 2025) | End-to-End Large Audio Language Model mit SOTA bei ASR (3.08% CER Chinesisch) und paralinguistischem Verstaendnis (83.09% Accuracy ueber 11 Dimensionen inkl. Pitch). Trainiert auf 8 Mio. Stunden Sprachdaten. | "Step-Audio 2 achieves state-of-the-art performance on various audio understanding and conversational benchmarks." / "Step-Audio 2 outperforms existing open-source and commercial ASR models [...] achieving an average character error rate (CER) of 3.08% on Chinese test sets." | Starker CER-Referenzwert (3.08%) fuer Chinesisch (RQ4); paralinguistischer Benchmark (inkl. Pitch) verwandt mit Tonwahrnehmung, aber keine lexikalischen Mandarin-Toene (RQ2). | Keine Ton-Evaluation auf Phonemebene trotz exzellenter CER und paralinguistischer Evaluation; kein L2-Benchmark; keine explizite Limitations-Sektion. | "Step-Audio 2 demonstrates state-of-the-art performance across various tasks, including ASR, audio understanding, speech translation, and general speech conversation, outperforming both open-source and commercial solutions." | L1, L2, L3, L4 |
| 28 | TELEVAL: A Dynamic Benchmark for Spoken Language Models in Chinese Interactive Scenarios (Li et al., 2025) | Dynamischer Benchmark fuer SLMs in chinesischen Interaktionsszenarien mit >40.000 Samples; evaluiert Content Fulfillment und Interactional Appropriateness. Identifiziert "Caption Trap": Modelle erkennen paralinguistische Signale, generieren aber keine angemessenen Reaktionen. | "despite strong performance on semantic and knowledge-oriented tasks, current SLMs still struggle to produce natural and interactionally appropriate responses" / "models like Kimi-Audio accurately recognize paralinguistic cues (e.g., coughing) but fail to generate informed responses." | "Caption Trap" motiviert Hypothese, dass Modelle Toene wahrnehmen, aber nicht korrekt in Pinyin umsetzen (RQ2, RQ5); umfangreiche Modellvergleichsdaten (RQ4). | Keine ASR-/Transkriptionsevaluation; Phonem-/Tonebene nicht evaluiert; kein L2-Sprecher (nur Dialekte). | "This likely stems from training regimes treating paralinguistic data as explicit classification targets rather than implicit signals for behavioral adaptation" | L1, L2, L3 |
| 29 | VocalBench-zh: Decomposing and Benchmarking Speech Conversational Abilities in Mandarin Context (Liu et al., 2025) | Umfassender Mandarin Speech-to-Speech Benchmark mit 14 Modellen; LLM-Backbone ist Hauptfaktor fuer semantische Qualitaet. SpeechLLMs fehlt Awareness fuer chinesische Schriftzeichenstruktur und paralinguistische Kontrolle. | "LLM backbone is the main factor affecting overall performance, especially semantic quality." / "SpeechLLMs lack control over the paralinguistic control of their speech responses." | Modellrangliste (Qwen3-Omni > MiMo > Kimi-Audio) als Referenz fuer Modellauswahl (RQ4); fehlende paralinguistische Kontrolle deutet auf Tonschwaechen (RQ5). | Trotz Mandarin-Fokus keine phonetische/tonale Transkriptionsgenauigkeit; nur synthetische Daten; kein L2-/Dialekt-Evaluation. | "real human recordings are essential for evaluating nuanced aspects such as subtle pronunciation variations, prosody, and natural dialogue dynamics." | L1, L2, L3, L4 |
| 30 | Qwen3-ASR Technical Report (Qwen Team, 2026) | ASR-Modellfamilie (1.7B, 0.6B) fuer 52 Sprachen/Dialekte mit 4-stufigem Training (AuT Pretraining, Omni Pretraining, SFT, RL). SOTA unter Open-Source-ASR; erster LLM-basierter Forced Aligner. | "Qwen3-ASR-1.7B achieves state-of-the-art performance among open-sourced ASR models and is competitive with the strongest proprietary APIs" / "RL plays an essential role for models' noise robustness, transcribing stability and ability to analyze difficult cases." | Primaerer Vergleichskandidat als dediziertes ASR-System (RQ4); CER-Werte als starke Baselines; Frage ob exzellente CER auch auf Tonebene gilt (RQ2). | Trotz Mandarin-/Dialekt-Unterstuetzung nur WER/CER; keine TER oder Pinyin-Transkription; keine L2-Evaluation; nur Mandarin-Zeichen-Output. | "the model learns to be an ASR-only model that does not follow natural-language instructions in the prompt, in order to mitigate instruction injection and instruction-following failures." | L1, L2, L3, L4 |
| 31 | A Survey of Large Audio Language Models: Generalization, Trustworthiness, and Outlook (Luo et al., 2026) | Umfassender LALM-Survey zu Architektur und Trustworthiness; identifiziert "Modality Neglect" -- LALMs nutzen akustische Information oft nicht und fallen auf textuelle Shortcuts zurueck. Subtile Ton-Shifts koennen Safety-Violations ausloesen. | "models over-rely on lexical cues rather than acoustic emotion signals, and that replacing audio inputs with silence or noise causes negligible performance changes on certain benchmarks" / "LALMs are highly sensitive not only to semantic deception but also to non-semantic acoustic cues, where subtle shifts in tone can trigger safety violations" | "Modality Neglect" erklaert potentielle Schwaechen bei Tonunterscheidung (RQ2, RQ5); chronologische Roadmap aller Audio-LLMs als Referenz fuer Modellauswahl (RQ4). | LALMs nicht auf phonem-/tonspezifischer Granularitaet evaluiert; kein Head-to-Head-Vergleich fuer tonal-spezifische Transkription; fragmentierte Forschungslandschaft. | "Existing research predominantly details architectural innovations or specific concerns, yet there remains a significant lack of work dedicated to a systematic taxonomy of the safety implications." | L1, L4 |
| 32 | A Study on Phonemes Recognition Method for Mandarin Pronunciation Based on Improved Zipformer-RNN-T (Du et al., 2025) | Verbessertes Zipformer-RNN-T-Modell fuer Mandarin-Phonem-Erkennung; erstellt AISHELL1-PHONEME-Datensatz mit 66 Phonemen. Erreicht 2.12% WER (Test); Konfusionsmatrix zeigt haeufigste Verwechslungen bei /B/-/P/, /IN/-/ING/, /Z/-/ZH/. | "tones play a crucial role in the semantic structure of Chinese, with the same syllable having completely different meanings depending on the tone." / "the method performs excellently in the phoneme recognition task, achieving a Word Error Rate (WER) of 1.92% (Dev) and 2.12% (Test) on the AISHELL1-PHONEME dataset" | SOTA-Benchmark fuer dedizierte Mandarin-Phonem-Erkennung (RQ4); Konfusionsmatrix-Analyse liefert Kontext fuer Fehlerstruktur (RQ5). | Kein Vergleich mit multimodalen LLMs; keine L2-Sprecher; Ton-Ebene nicht als separate Evaluationsachse -- Toene sind in Phonem-Labels enthalten, aber nicht isoliert evaluiert. | "there is a relative scarcity of Chinese reading evaluation datasets, which limits the effectiveness of model training and evaluation" | L1, L2, L4 |
| 33 | A Syllable-Character Collaborative Model for Enhanced Pinyin and Chinese Recognition -- SCCM (Chen et al., 2025) | SCCM dekodiert Pinyin, Initiale/Finale und chinesische Zeichen gleichzeitig; erreicht 3.1% Pinyin-CER und 6.4% Text-CER auf AISHELL-1 (45.7% relative CER-Reduktion). Pinyin-Ensemble-Modul verbessert Pinyin-Vorhersage signifikant. | "In Chinese speech recognition, end-to-end speech recognition models usually use Chinese characters as direct output and perform poorly compared with other language models." / "by integrating both pinyin and initials/finals information through our Pinyin-Ensemble (PE) module, we achieve a 28.0% reduction in pinyin error rate." | Pinyin-CER (3.1%) als Benchmark fuer Pinyin-Erkennung (RQ4); Multi-Task-Ansatz mit Pinyin als Zwischenschicht zeigt Wert phonetischer Repraesentationen (RQ1). | Evaluiert Pinyin ohne explizite Tonmarkierungen; kein Vergleich mit multimodalen LLMs; nur Native-Speaker (AISHELL-1). | "The model fully utilizes the units of Chinese speech and simultaneously performs decoding tasks for initials and finals, pinyin and Chinese characters." | L1, L2, L3, L4 |
| 34 | Perception-Production of Second-Language Mandarin Tones Based on Interpretable Computational Methods: A Review (Huang et al., 2025/2026) | Systematischer Review ueber L2-Mandarin-Tonerwerb; T2 (steigend) vs. T3 (fallend-steigend) ist der persistenteste Kontrast. L1-Hintergrund beeinflusst Cue-Weighting signifikant. Schlaegt GNN+XAI-Framework fuer paedagogisches Feedback vor. | "evidence converges on tones as time-evolving F0 trajectories, so movement, turning-point timing, and local F0 range are more diagnostic than height alone, and the contrast between Tone 2 and Tone 3 remains the persistent difficulty" / "learners with tonal vs. non-tonal language backgrounds weight these cues differently" | T2/T3-Konfusion als Benchmark-Hypothese fuer LLM-Fehlerstruktur (RQ5); umfassende L2-Evidenz informiert Evaluationsdesign (RQ3). | Keine Studie im Review nutzt multimodale LLMs fuer L2-Tonevaluation; Strand D (erklaerbares Modelling) wird als "evidence gap" beschrieben. | "none of the included studies meets our full criteria for a classroom-ready, item-matched perception-production framework; therefore, we treat strand D as an evidence gap." | L1, L3, L4 |
| 35 | Machine Learning for Mandarin Tone Recognition: A Systematic Review (Zou et al., 2025) | Systematischer Review von 61 Studien; Deep Learning uebertrifft traditionelle Modelle (88.8% vs. 83.1%). CNNs erreichen bis 99.16% auf isolierten Silben; Tone 3 ist konsistent fehleranfaelligste Kategorie. TER als Standard fuer kontinuierliche Sprache vorgeschlagen. | "Deep learning models outperform traditional approaches in Mandarin tone classification (mean accuracy 88.8% vs. 83.1%). Convolutional Neural Networks (CNNs) achieve up to 99.16% accuracy for isolated syllables" / "Confusion matrices consistently reveal Tone 3 as the most error-prone category." | Umfassende Performance-Benchmarks fuer dedizierte Tonerkennung (RQ4); T3 als fehleranfaelligste Kategorie testbar mit LLMs (RQ5); TER-Metrik identisch mit unserem Design (RQ2). | Multimodale Foundation Models werden nicht einmal erwaehnt; fehlende diverse Datensaetze; schwache Prosodie-/Dialekt-Modellierung; unzureichende Validierungsstrenge. | "While deep learning models represents the state of the art, several gaps limit practical deployment, including the lack of diverse datasets, weak prosody and dialect modeling, and insufficient validation rigor." | L1, L2, L3, L4 |
| 36 | Automatic Speech Recognition System for Tonal Languages: State-of-the-Art Survey (Kaur et al., 2021) | Survey ueber ASR fuer tonale Sprachen weltweit (Mandarin, Thai, Vietnamesisch u.a.); F0 ist primaerer Ton-Cue. Historische Entwicklung von HMM-GMM zu End-to-End; viel Arbeit fuer asiatische Tonsprachen, wenig fuer Indo-Europaeische/Afrikanische. | "ASR of tonal language needs the detection of tone, consonants and vowels of syllable." / "The language is said to be a tonal language, if the meaning of the word is changed with the pitch of word." | Historischer Kontext fuer Mandarin-Tonerkennung (RQ4); phonologische Tabellen zu tonalen Sprachen nuetzlich fuer Einleitung; CER-Vergleichswerte. | Datiert (vor 2020); keine multimodalen Foundation Models oder LLMs; keine Self-Supervised-Learning-Ansaetze; keine L2-Szenarien; keine feingranulaere Evaluation jenseits CER/WER. | "It is observed that the lot of work have been done for the Asian continent tonal languages but little work been reported for the Mizo, Bodo, Indo-European tonal languages." | L1, L2, L3, L4 |
| 37 | Evaluating Mandarin Tone Pronunciation Accuracy for L2 Learners Using a ResNet-Based Siamese Network (Bu et al., 2025) | Siamese Network mit ResNet-18 fuer L2-Ton-Bewertung; extrahiert F0-basierte 1D/2D-Features und bewertet auf Silbenebene mit Diskrepanz-Score (0-5). Tone3 wird von allen Modellen am besten unterschieden. Nur tibetische L1-Sprecher. | "Evaluating tone pronunciation is essential for helping second-language (L2) learners master the intricate nuances of Mandarin tones." / "Compared to the other tones, Tone3 is more effectively distinguished and recognized by different models. This suggests that Tone3 is easier for the models to learn and has distinctive recognition features." | Dediziertes Ton-Bewertungssystem als Vergleich (RQ4); Tone3-Befund liefert Hypothese fuer Fehlerstrukturanalyse (RQ5); Silben-Ebene stuetzt Pinyin-basiertes Design (RQ2). | Kein Vergleich mit LLM-basierten Modellen; nur F0-basierte Features (moeglicherweise uebervereinfacht); nur tibetische L1-Sprecher ohne breite L1-Diversitaet. | "the method still relies on the computed tone features, which may oversimplify the complexity of tone characteristics. Future research will aim to extract raw, deep tone features from speech data using neural networks." | L2, L3, L4 |
| 38 | Can AI Understand Mandarin Speech Prosody? A Framework and Benchmark Showcase -- MSPB (Wang et al., Interspeech 2025) | Mandarin Speech Prosody Benchmark mit 8 Tasks; evaluiert GPT-4o, Gemini, Qwen2-Audio u.a. GPT-4o erreicht nur 59.70% (vs. 81-96% Mensch). Modelle scheitern bei rein prosodisch-abhaengigen Tasks, performen besser bei kontextreichen Tasks. | "GPT-4o achieved the highest average accuracy (59.70%), followed by Gemini-2-flash (59.28%) and Gemini-1.5-pro (56.05%)." / "they rely more on contextual cues than on subtle speech prosody variations." | Direkte Evidenz fuer Schwaechen der gleichen Modelle (GPT-4o, Gemini) bei Prosodie (RQ2); Kontextabhaengigkeit statt prosodischer Verarbeitung stuetzt Hypothese zu Ton-Erkennung (RQ1, RQ5). | Evaluiert Prosodie auf pragmatischer Ebene, NICHT lexikalische Toene als Transkription; nur Native-Aufnahmen einer Sprecherin; kein L2-Test. | "This highlights a critical need for future research to better integrate fine-grained prosodic information into Speech LLMs." | L1, L2, L3, L4 |
| 39 | The Assessment of Automated Rating of L2 Mandarin Prosody in Lexical Tone Recognition and Pauses (Wu & Shen, Speech Prosody 2024) | Evaluation zweier kommerzieller Automated-Rating-Engines fuer L2-Mandarin-Tonerkennung; Diagnostic Accuracy ca. 80% ueber alle Toene. FRR uebersteigt durchgehend FAR (Engines zu streng). Korrelation mit menschlichen Ratern maximal 0.60. | "The study found that the overall accuracy of tonal diagnosis reached 80%, as determined through the calculation of false rejection and false acceptance rates." / "Research indicates that the majority of pronunciation errors made by L2 Mandarin learners pertain to tones rather than vowels or consonants." | FAR/FRR/DA-Baselines fuer L2-Tonerkennung als direkte Vergleichswerte (RQ4); Toene als Hauptfehlerquelle bei L2-Lernern stuetzt RQ3 und RQ5. | Kein Vergleich mit multimodalen LLMs oder Whisper; nur 12 Lerner mit ungleichmaessiger L1-Verteilung; nur Lese-Aufgaben. | "readily available automated tools may not offer significant advantages for language instructors focusing on accuracy and fluency assessment." | L2, L3, L4 |
| 40 | Pinyin-Guided Chinese Speech Recognition with Large Language Model -- PYG-ASR (Zhengjie & Cheng, Interspeech 2025) | PYG-ASR integriert explizite Pinyin-Vorhersage in LLM-basierte ASR; erreicht 25% CER-Reduktion (3.0% CER) und 1.9% Pinyin Error Rate auf AISHELL-1. Chain-of-Thought-Fehlerkorrektur mit DeepSeek-v3 reduziert Biasing-CER um 49.2%. | "This paper proposes Pinyin-Guided ASR (PYG-ASR), which innovatively modifies the LLM-ASR to simultaneously map acoustic features to both Pinyin and text tokens." / "The presence of tonal variations and a high number of homophones makes it essential to explicitly model pronunciation features, a capability inherently absent in most LLM-ASR models." | Pinyin Error Rate (1.9%) als harter Benchmark (RQ4); zeigt, dass LLM-ASR ohne Pinyin-Guidance Tonverwechslungen produziert (RQ5); CoT-Korrektur relevant fuer Text-only-Baseline (RQ1). | Nur AISHELL-1 evaluiert; Pinyin Error Rate nicht nach Toenen aufgeschluesselt; kein Vergleich mit multimodalen Frontier-Modellen; nur Native-Speaker. | "For future works, we intend to conduct larger-scale experiments and explore a more profound integration of Pinyin information into LLM-ASR models." | L1, L2, L3, L4 |

---

**Legende:**
- **L1** = Luecke 1: Phonem-/Ton-Granularitaet bei LLMs unerforscht
- **L2** = Luecke 2: Toene als separate Evaluationsachse
- **L3** = Luecke 3: L2-Sprecher
- **L4** = Luecke 4: Systematischer Head-to-Head multimodaler Modelle
