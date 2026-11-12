# Core Information: Literaturanalyse aller Paper

> Automatisch erstellt am 2026-06-22. Fuer jedes Paper werden zentrale Learnings, konkrete Verwendbarkeit, Forschungsluecken und deren Relevanz fuer die eigene Arbeit dokumentiert.

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
- **Luecke 1:** Frontier multimodale LLMs werden nur auf Satz-/Wortebene (WER/CER) bewertet; Phonem-/Ton-Granularitaet fuer Mandarin ist weitgehend unerforscht
- **Luecke 2:** Toene werden selten als explizite, separate Evaluationsachse behandelt
- **Luecke 3:** Sehr wenige Studien decken L2/Non-Native-Sprechersprache ab (Stretch-Goal)
- **Luecke 4:** Kein systematischer Head-to-Head-Vergleich aktueller multimodaler Modelle untereinander und gegen dedizierte Systeme auf dieser Granularitaet

**Design:** Pinyin mit Tonnummern (z.B. `ma1`) als Zielformat; ~5-8 Frontier-Modelle + Whisper als Baseline; Metriken: PER, TER, CER, F1, FAR/FRR; Korpus: Tone Perfect; Off-the-Shelf-Evaluation ohne Finetuning.

---

## 1. Seed-ASR (2407.04675v2)

**Titel:** Seed-ASR: Understanding Diverse Speech and Contexts with LLM-based Speech Recognition  
**Autoren:** Seed Team, ByteDance (2024)

### Zentrale Learnings
- LLM-basierte ASR-Modelle uebertreffen klassische End-to-End-Modelle deutlich: "Seed-ASR demonstrates significant improvement over end-to-end models on comprehensive evaluation sets, including multiple domains, accents/dialects and languages" (Abstract)
- Skalierung des Audio-Encoders zeigt nahezu lineares Scaling Law (75M bis 5B Parameter)
- Kontextinformation (Dialoghistorie, Meetingteilnehmer) verbessert Keyword-Erkennung um >15% Recall
- Reinforcement Learning mit gewichtetem WER als Reward konsolidiert Leistung
- Mehrstufiges Training (SSL -> SFT -> Context SFT -> RL) mit jeweils unterschiedlicher Funktion
- Seed-ASR erreicht 10%-40% WER/CER-Reduktion gegenueber grossen ASR-Modellen auf oeffentlichen Testsets

### Konkret verwendbar fuer unsere Arbeit
- Seed-ASR demonstriert, dass LLM-basierte ASR Mandarin auf Satzebene exzellent beherrscht — die These prueft, ob das auch auf Phonem-/Ton-Granularitaet gilt (RQ1-RQ2)
- Scaling-Law-Befunde liefern Kontext fuer die Modellgroessen-Diskussion im Related-Work-Kapitel (§2.4.1)
- LUISE-SSL-Pretraining als Referenz fuer die Diskussion, wie Audio-Encoder phonetische Information kodieren
- CER-Benchmarkwerte als Vergleichsrahmen fuer RQ4

### Forschungsluecken
- "In future, we will focus on extending Seed-ASR's ability to deal with multiple tasks within a single model" (Conclusion)
- Langform-Erkennung auf max. 5 Minuten beschraenkt
- Derzeit nur 8+ Sprachen; Erweiterung auf 40+ geplant
- Keine systematische Untersuchung von Low-Resource-Sprachen/Dialekten ausserhalb Chinesisch

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Seed-ASR evaluiert nur auf WER/CER (Satz-/Wortebene) — unsere Arbeit prueft Phonem-/Ton-Granularitaet
- **Luecke 2:** Keine separate Ton-Evaluation — unsere Arbeit fuehrt explizite TER als Metrik ein
- **Luecke 4:** Seed-ASR ist ein Einzelsystem — unsere Arbeit vergleicht systematisch mehrere multimodale Modelle

---

## 2. Baima ASR (2025.computel-main.20)

**Titel:** Comparing efficacy of IPA vs Pinyin romanisation transcriptions for complex tonal languages: A case study in Baima  
**Autoren:** Chirkova, Coto-Solano, Griffiths, Meelen (2025)

### Zentrale Learnings
- Fuer extrem low-resource tonale Sprachen (186 Min.) liefert ASR brauchbare Ergebnisse: IPA + Wav2Vec2 + KenLM erreicht CER=17%, WER=37%
- Die Wahl des Transkriptionssystems beeinflusst Tonausgaben, selbst bei identischen Tonmarkierungen: "the way vowels and consonants are transcribed can affect tonal character outputs" (Abstract)
- Mehr Sprachen im Basismodell helfen nicht notwendigerweise: Wav2Vec2 (53 Sprachen) uebertrifft MMS (1162 Sprachen)
- Ein Language Model (KenLM) verbessert Tonerkennung deutlich: tonal CER sinkt um 8.8 Punkte
- Toene sind der schwierigste Teil der Transkription: "complex tones remain the most difficult part of the phonology to transcribe" (Conclusion)

### Konkret verwendbar fuer unsere Arbeit
- **Direkt relevant fuer Evaluationsmethodik:** Tonal CER als separate Metrik — entspricht dem TER-Ansatz unserer Arbeit (RQ2c, RQ5)
- Erkenntnis, dass Transkriptionssystem-Wahl die Tonergebnisse beeinflusst — stuetzt unsere Entscheidung, Pinyin+Tonnummern als fixes Format vorzugeben (Nordstern §5)
- Befund "Toene sind schwierigster Teil" als empirische Motivation fuer den Fokus auf Ton-Evaluation
- KenLM-Verbesserung zeigt, dass Sprachmodell-Wissen Tonerkennung hilft — relevant fuer Diskussion, ob LLMs dies implizit leisten

### Forschungsluecken
- Nur 3 Sprecher und 186 Minuten Daten
- Geliehene Toene (55, 35) werden extrem schlecht erkannt (bis 100% Fehlerrate)
- Keine multilinguale Cross-linguale Transfer-Learning-Analyse
- Tone Sandhi in polysyllabischen Woertern "remains largely understudied"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Studie verwendet dedizierte ASR (Wav2Vec2), nicht multimodale LLMs — unsere Arbeit testet Frontier-LLMs auf Ton-Granularitaet
- **Luecke 4:** Kein Vergleich mit multimodalen Foundation Models — unsere Arbeit liefert diesen Vergleich

---

## 3. AI Index Report 2026 (ai_index_report_2026_chapter_2_technical)

**Titel:** AI Index Report 2026, Chapter 2: Technical Performance  
**Autoren:** AI Index Team, Stanford University (2026)

### Zentrale Learnings
- AI-Faehigkeiten ueberholen Benchmarks: "Frontier models gained 30 percentage points in a single year on Humanity's Last Exam"
- Modellleistung konvergiert an der Spitze: Top-4-Modelle liegen <25 Punkte auseinander auf Arena Leaderboard
- US-China-Luecke praktisch geschlossen: DeepSeek-R1 erreichte nahezu Paritaet
- Benchmarks haben Qualitaetsprobleme: "error rates up to 42% on widely used evaluations"
- "Jagged Intelligence": Modelle gewinnen Gold bei IMO, lesen analoge Uhren nur zu 50.1% korrekt

### Konkret verwendbar fuer unsere Arbeit
- Benchmark-Konvergenz auf Text-Tasks als Argument, warum domain-spezifische Evaluation (Mandarin-Toene) notwendig ist — Modelle sind generell stark, aber "jagged intelligence" koennte auch bei Toenen auftreten (Motivation Kapitel 1)
- Benchmark-Qualitaetsprobleme als Motivation fuer sorgfaeltige Evaluationsmethodik mit klaren Metriken (PER, TER, CER)
- Konvergenz der Top-Modelle stuetzt die Erwartung, dass ein Head-to-Head-Vergleich (RQ4) differenzierte Ergebnisse liefert

### Forschungsluecken
- "Benchmarks for multiagent coordination, human-AI interaction, tool-using agents remain underdeveloped"
- Fehlende standardisierte Benchmarks fuer Speech/Audio im Vergleich zu Text/Vision
- "The field should adopt centaur evaluations" (Human-AI-Kollaborationsbenchmarks fehlen)

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Der Report konstatiert fehlende Speech/Audio-Benchmarks — unsere Arbeit liefert einen spezifischen Benchmark fuer Mandarin-Ton-Transkription durch multimodale LLMs
- **Luecke 4:** "Jagged intelligence" motiviert systematischen Vergleich — unsere Arbeit prueft, ob die Konvergenz auch auf Phonem-/Ton-Ebene gilt

---

## 4. AudioBench (2406.16020v5)

**Titel:** AudioBench: A Universal Benchmark for Audio Large Language Models  
**Autoren:** Wang et al., A*STAR Singapore (2025)

### Zentrale Learnings
- Kein einzelnes Modell dominiert: "no single model excels consistently across all tasks" (Abstract)
- AudioLLMs kaempfen mit langen Audioaufnahmen (>10 Minuten)
- Kaskaden-Modelle (Whisper+Llama3) uebertreffen End-to-End AudioLLMs bei sprachintensiven Aufgaben
- AudioLLMs sind empfindlich gegenueber Prompt-Variationen
- Llama-3-70B zeigt starke Korrelation mit GPT-4 als Judge (>0.85)

### Konkret verwendbar fuer unsere Arbeit
- Prompt-Sensitivitaet direkt relevant: unsere Arbeit muss Pinyin+Ton-Format im Prompt fixieren, um Variabilitaet zu kontrollieren (Nordstern §5 "target-leakage / auto-correction problem")
- Befund "Kaskade oft besser als E2E" motiviert den Whisper-Baseline-Vergleich (RQ4)
- Robustheitstests mit multiplen Prompts als methodische Anregung fuer unsere Prompt-Strategie
- Model-as-Judge-Methodik als moegliche Ergaenzung zur automatischen Metrik-Berechnung

### Forschungsluecken
- "The current AudioBench exclusively includes English datasets" (Limitations)
- "Multilingual Capabilities, Code-Switching, and Dialects" fehlen komplett
- "Long Audio Processing and Understanding" bleibt offen
- "Multi-round Query Handling" limitiert bei Open-Source-Modellen

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** AudioBench ist rein englischsprachig und evaluiert nicht auf Phonem-/Ton-Ebene — unsere Arbeit fuellt diese Luecke fuer Mandarin
- **Luecke 2:** Keine tonale Evaluation — unsere Arbeit fuehrt TER als explizite Achse ein

---

## 5. PY-GEC (2409.13262v1)

**Titel:** Large Language Model Should Understand Pinyin for Chinese ASR Error Correction  
**Autoren:** Li et al., Huawei (2024)

### Zentrale Learnings
- Pinyin als phonetische Repraesentierung verbessert chinesische ASR-Fehlerkorrektur konsistent
- Multitask-Training (Korrektur + Pinyin-Text-Konversion) bringt 8.3% relative CER-Reduktion und 3.9% Entity-Recall-Verbesserung
- Nur synthetische Fehler fuer Training noetig -- kein Zugang zu ASR-Modellen oder Sprachdaten erforderlich
- Multitask-Training richtet Text- und Pinyin-Feature-Raeume aus: Cosine Similarity steigt von 0.26 auf 0.82
- Pinyin-Rerank als effektive Ensemble-Methode

### Konkret verwendbar fuer unsere Arbeit
- **Zentral fuer RQ1:** Zeigt, dass LLMs Pinyin-Verstaendnis durch Training erwerben koennen — unsere Arbeit prueft, ob multimodale LLMs dieses Wissen off-the-shelf besitzen
- Cosine-Similarity-Analyse (Text vs. Pinyin Feature-Raum) als moegliche Interpretationsmethode fuer RQ5
- Bestaetigt Pinyin als geeignete Zwischenrepraesentation — stuetzt unsere Wahl von Pinyin+Tonnummern als Zielformat
- Future Work explizit: "extend experiments to multi-modal LLMs" — genau das tut unsere Arbeit

### Forschungsluecken
- "For future research, we aim to extend our experiments to larger-scale LLMs and multi-modal LLMs" (Conclusion)
- Nur LLaMA-3-8B-Chinese getestet; Generalisierung unklar
- Nur 1-best Hypothesis; N-best koennte Verbesserungen bringen
- Keine Untersuchung fuer andere tonale Sprachen

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1 + 4:** PY-GEC fordert explizit Extension auf multimodale LLMs — unsere Arbeit liefert genau diesen systematischen Vergleich mehrerer multimodaler Modelle auf Pinyin-/Ton-Ebene
- **Luecke 2:** Pinyin-Toene nicht separat evaluiert — unsere Arbeit isoliert Ton-Accuracy als eigene Metrik

---

## 6. FireRedASR2S (2603.10420v1)

**Titel:** FireRedASR2S: A State-of-the-Art Industrial-Grade All-in-One ASR System  
**Autoren:** Xu et al., Xiaohongshu (2026)

### Zentrale Learnings
- Modulares integriertes System (VAD + LID + ASR + Punc) erreicht SOTA
- Datenskalierung ist Haupttreiber: Training stieg von 70k auf 200k Stunden
- FireRedASR2-LLM: 2.89% CER auf 4 Mandarin-Benchmarks, 11.55% auf 19 Dialekt-Benchmarks
- Hierarchische LID (Sprache -> Dialekt) effektiver als flache Klassifikation: 97.18% Accuracy auf FLEURS
- Ultra-leichtgewichtiger VAD (0.6M Parameter) mit 97.57% F1

### Konkret verwendbar fuer unsere Arbeit
- CER-Benchmarkwerte (2.89%) als Referenz fuer die Leistungsfaehigkeit dedizierter Mandarin-ASR-Systeme im Vergleich zu multimodalen LLMs (RQ4)
- Open-Source-Modell als potenzielle zusaetzliche Baseline neben Whisper
- Demonstriert, dass spezialisierte Systeme auf Zeichenebene nahezu perfekt sind — die Frage ist, ob das auch fuer Phonem/Ton gilt

### Forschungsluecken
- "Future work will focus on further improving performance and expanding support for more languages" (Conclusion)
- Keine Evaluation der End-to-End-Pipeline-Fehlerfortpflanzung
- Kein Streaming-ASR evaluiert
- Code-Switching nicht systematisch evaluiert

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** FireRedASR evaluiert nur auf CER (Zeichenebene) — unsere Arbeit prueft Phonem-/Ton-Granularitaet
- **Luecke 2:** Keine Ton-Evaluation — unsere Arbeit fuehrt TER ein
- **Luecke 4:** Kein Vergleich mit multimodalen LLMs — unsere Arbeit positioniert solche Systeme als Baseline

---

## 7. ASR Deep Learning Survey (1-s2.0-S2666307424000573-main)

**Titel:** Automatic Speech Recognition: A survey of deep learning techniques and approaches  
**Autoren:** Ahlawat, Aggarwal, Gupta (2025)

### Zentrale Learnings
- Uebergang von HMM/GMM zu DNN hat WER von 20-30% auf unter 5% reduziert
- Self-Supervised Learning (wav2vec 2.0, HuBERT) transformativ fuer Low-Resource-Sprachen
- End-to-End-Modelle vereinfachen ASR-Pipeline erheblich
- Multilingual-E2E-Modelle uebertreffen monolinguale Systeme

### Konkret verwendbar fuer unsere Arbeit
- Systematische Uebersicht der ASR-Architekturentwicklung als Grundlage fuer Related Work §2.2 (Pre-Transformer und Transformer-Methoden)
- Vergleich von Evaluationsmetriken (WER, CER, PER) — hilft bei der Begruendung unserer Metrikwahl (PER + TER + CER)
- SSL-Modelle (wav2vec 2.0, HuBERT) als Kontext fuer die Diskussion, wie Audio-Encoder in multimodalen LLMs phonetische Repraesentationen lernen

### Forschungsluecken
- Grosse Diskrepanz zwischen High- und Low-Resource-Sprachen (WER 1.4% Englisch vs. 20.6% Odiya)
- Akzent-Robustheit bleibt problematisch
- Domain-spezifisches ASR (Healthcare, Education) unterentwickelt

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Survey behandelt ASR nur auf WER/CER-Ebene — unsere Arbeit geht auf Phonem-/Ton-Granularitaet
- **Luecke 2:** Toene werden im Survey nicht als Evaluationsdimension erwaehnt — unsere Arbeit macht dies explizit

---

## 8. Pitch-Aware RNN-T fuer Mandarin MDD (2406.04595v1)

**Titel:** Pitch-Aware RNN-T for Mandarin Chinese Mispronunciation Detection and Diagnosis  
**Autoren:** Wang, Shi, Wang, NUS (2024)

### Zentrale Learnings
- Stateless RNN-T mit HuBERT + Pitch Fusion Block: 3% PER-Verbesserung, 7% FAR-Steigerung
- Pitch Fusion Block kombiniert Multi-Head Self-Attention (global) mit residualen Convolution Blocks (lokal) fuer F0
- Training ausschliesslich auf Native-Speaker-Daten (AISHELL-1), Evaluation auf Non-Native (LATIC)
- Mel-skalierte F0 mit 40ms Hop-Size liefert beste Ergebnisse
- Niedrigster Diagnostic Error Rate (DER) im Vergleich

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant als dedizierte Baseline fuer RQ4:** Pitch-Aware RNN-T ist ein spezialisiertes MDD-System fuer Mandarin-Toene
- Metriken PER, FRR, FAR, DER direkt uebertragbar auf unsere Evaluation (RQ5 Fehlerstrukturanalyse)
- Ansatz "Training auf Native, Evaluation auf L2" entspricht genau unserem Phase-1 → Phase-2-Design
- F0-Extraktionsmethode (DIO/WORLD) als Referenz, falls wir Pitch-Features als Erklaerungsvariable heranziehen
- LATIC-Dataset als potenzielle L2-Datenquelle fuer RQ3 (Stretch)

### Forschungsluecken
- Nur 4 Non-Native-Sprecher im LATIC-Dataset evaluiert
- Keine Evaluation suprasegmentaler Features jenseits von Ton
- Nur Mandarin getestet; Transferierbarkeit auf andere tonale Sprachen nicht untersucht
- Kein Vergleich mit SLM-basierten Ansaetzen

### Welche Luecken adressiert meine Arbeit?
- **Luecke 3:** Nur 4 L2-Sprecher — unsere Arbeit zielt auf breitere L2-Evaluation (Stretch-Goal RQ3)
- **Luecke 4:** Kein Vergleich mit multimodalen LLMs — unsere Arbeit stellt diesen Vergleich her (RQ4: multimodale LLMs vs. dedizierte Systeme wie Pitch-Aware RNN-T)

---

## 9. Tone-syllable synchrony (1-s2.0-S016763932400092X-main)

**Titel:** Tone-syllable synchrony in Mandarin: New evidence and implications  
**Autoren:** Kang, Xu, UCL (2024)

### Zentrale Learnings
- Erstmals quantitativer Nachweis fuer vollstaendige Synchronisation von Ton- und Vokal-Onset: "tone and vowel onsets are fully synchronized" mittels GAMMs und Bayes-Faktor (>200)
- Silbengrenze liegt ca. 125ms frueher als konventionelle akustische Grenze (43.6% normalisierte Zeit)
- "The previously reported 'anticipatory raising' effect of tone now appears to occur within rather than before the articulatory syllable"
- Evidenz fuer vollstaendige CVT (Konsonant-Vokal-Ton) Co-Onset

### Konkret verwendbar fuer unsere Arbeit
- Fundamentales phonetisches Hintergrundwissen fuer Related Work §2.2 und §2.4.2: erklaert, warum Toene nicht einfach als separates Feature extrahiert werden koennen
- Implikation fuer unsere Evaluation: da Ton und Vokal synchron beginnen, muss ein Modell den gesamten Silbenanfang korrekt verarbeiten, um den Ton zu identifizieren
- Stuetzt die Entscheidung, auf Silbenebene (Tone Perfect Korpus) zu evaluieren

### Forschungsluecken
- Basiert auf nur einer Sprecherin -- Generalisierung noetig
- "Even articulatory divergence is just the result of muscle activities" -- praezisere Messungen noetig
- Revision des Target Approximation Models moeglicherweise erforderlich
- Keine L2-Sprecher untersucht

### Welche Luecken adressiert meine Arbeit?
- **Luecke 3:** Keine L2-Sprecher untersucht — unsere Arbeit prueft (im Stretch-Goal) L2-Sprechersprache
- Indirekt relevant: Die Studie liefert phonetische Grundlagen, die erklaeren, warum Ton-Evaluation (Luecke 2) methodisch anspruchsvoll ist

---

## 10. Sparks of Large Audio Models (2308.12792v3)

**Titel:** Sparks of Large Audio Models: A Survey and Outlook  
**Autoren:** Latif et al. (2023)

### Zentrale Learnings
- Erster umfassender Survey ueber Large Audio Models in Speech, Music und Environmental Sounds
- Tokenisation ist kritisches Problem: "tokenisation is a potential limitation in their performance"
- Paralinguistische Information (Emotionen, Prosodie) untererforscht: "largely untapped and understudied"
- Multi-Domain-Datenbalance ungeloest

### Konkret verwendbar fuer unsere Arbeit
- Befund "paralinguistische Information untererforscht" schliesst Toene ein — direkte Motivation fuer unsere Forschungsfrage
- Tokenisierungsproblem relevant: wie Audio-Tokens phonetische/tonale Information kodieren, beeinflusst, ob LLMs Toene "hoeren" koennen
- Taxonomie der Large Audio Models als Rahmen fuer Related Work §2.3-2.4

### Forschungsluecken
- "Understanding paralinguistic information" -- Emotionen und Prosodie "largely untapped and understudied"
- "Tokenisation for speech/audio data processing requires further attention"
- Halluzinationen auch bei Large Audio Models
- Computational Cost enorm (AudioPaLM 530B: ~$530M)

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1 + 2:** Prosodie/Toene als "untapped" identifiziert — unsere Arbeit untersucht explizit, ob multimodale LLMs tonale Information verarbeiten koennen
- **Luecke 4:** Survey fordert systematische Evaluation — unsere Arbeit liefert einen Head-to-Head-Vergleich fuer den Spezialfall Mandarin-Toene

---

## 11. Speech Translation SFM+LLM (2402.12025v3)

**Titel:** Speech Translation with Speech Foundation Models and Large Language Models: What is There and What is Missing?  
**Autoren:** Gaido et al., FBK (2024)

### Zentrale Learnings
- 5 gemeinsame Bausteine: SFM, Length Adapter, Modality Adapter, Prompt-Speech Mixer, LLM
- "There is no consensus on the best SFM to choose" und kein fairer Vergleich unter kontrollierten Bedingungen
- "The majority of the SFMs used are not publicly available"
- Length Adapter essentiell wegen unterschiedlicher Sequenzlaengen Audio/Text

### Konkret verwendbar fuer unsere Arbeit
- 5-Bausteine-Framework als analytisches Schema, um die Architektur der multimodalen LLMs in unserer Studie zu kategorisieren (Related Work §2.3)
- Befund "kein Konsens ueber beste SFM" stuetzt die Notwendigkeit unseres systematischen Vergleichs (RQ4)
- Length Adapter als potenzielle Erklaerung, warum manche Modelle Kurzaeusserungen (Tone Perfect: isolierte Silben) besser verarbeiten als andere

### Forschungsluecken
- Fehlende standardisierte Trainingseinstellungen
- In-Context Learning: "The only attempt in ST has not been similarly successful"
- Inferenz-Effizienz: SFM+LLM vs. traditionelle Modelle (100-300M Parameter)
- Nutzung prosodischer Information wenig untersucht

### Welche Luecken adressiert meine Arbeit?
- **Luecke 2:** Prosodische Information (inkl. Toene) "wenig untersucht" — unsere Arbeit prueft explizit tonale Verarbeitung
- **Luecke 4:** "Kein fairer Vergleich unter kontrollierten Bedingungen" — unsere Arbeit liefert einen kontrollierten Vergleich fuer den Mandarin-Ton-Task

---

## 12. When LLMs Meet Speech (2025.findings-acl.1041)

**Titel:** When Large Language Models Meet Speech: A Survey on Integration Approaches  
**Autoren:** Yang et al., Kyoto University (2025)

### Zentrale Learnings
- Drei Integrationsansaetze: (a) Text-basiert (ASR+LLM+TTS), (b) Latent-Representation-basiert, (c) Audio-Token-basiert
- Text-basiert: "inevitably introduces a layer of abstraction and potential information loss such as prosody and emotion"
- Latent-Representation: "The primary challenge lies in aligning these acoustic representations with the textual embedding space"
- Audio-Token: Semantische Tokens fangen Phonetik, akustische Tokens bieten Signaltreue

### Konkret verwendbar fuer unsere Arbeit
- **Zentral fuer Related Work §2.3:** Drei-Wege-Taxonomie erklaert, warum Text-basierte Pipelines Toninformation verlieren — direkte theoretische Motivation fuer RQ1 (Text-only) vs. RQ2 (Audio)
- "Prosody loss" bei Text-basiertem Ansatz stuetzt unsere Hypothese, dass reine ASR+LLM-Pipelines Toene schlechter erfassen als nativ multimodale Modelle
- Kategorisierung hilft, die getesteten Modelle (GPT-4o, Gemini etc.) nach Integrationstyp einzuordnen

### Forschungsluecken
- "There is a notable gap in comparing the different integration approaches under a unified setting"
- "Most existing speech-LLM models are designed to handle only English"
- "Speech-based applications more often require real-time processing"
- "The alignment of text and speech data remains underexplored"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** "Most models handle only English" — unsere Arbeit evaluiert auf Mandarin mit Phonem-/Ton-Granularitaet
- **Luecke 4:** "Gap in comparing approaches under unified setting" — unsere Arbeit vergleicht verschiedene multimodale Modelle auf demselben Mandarin-Ton-Task

---

## 13. Recent Advances in Speech LMs (2410.03751v4)

**Titel:** Recent Advances in Speech Language Models: A Survey  
**Autoren:** Cui et al. (2025)

### Zentrale Learnings
- Drei Probleme des ASR+LLM+TTS-Frameworks: (1) Information Loss paralinguistischer Information, (2) Signifikante Latenz, (3) Kumulative Fehler
- SpeechLMs koennen "speaker-specific information and emotional nuances" erfassen
- Separate Optimierung der Komponenten "may hinder the model's overall potential"
- Sicherheitsrisiken: semantisch gefaehrliche Inhalte, akustisch unangemessene Inhalte, Privatsphaere

### Konkret verwendbar fuer unsere Arbeit
- "Information Loss paralinguistischer Information" in kaskadierten Pipelines — erklaert theoretisch, warum Whisper (ASR) + nachgelagerter LLM Toninformation verlieren koennte (relevant fuer RQ4-Diskussion)
- Taxonomie der SpeechLM-Komponenten (Tokenizer, LM, Vocoder) als Strukturierung fuer §2.3
- End-to-End vs. Kaskade-Argumentation direkt relevant fuer die Interpretation unserer Ergebnisse

### Forschungsluecken
- "There remains a significant gap in understanding the advantages and disadvantages of different component selections"
- End-to-End-Training mit Gradientenfluss von Vocoder zu Tokenizer wenig erforscht
- Echtzeit-Sprachgenerierung "remains underexplored"
- Sicherheitsfragen "not thoroughly investigated"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** "Gap in understanding component advantages" — unsere Arbeit zeigt empirisch, ob nativ multimodale Modelle tonale Information besser verarbeiten als kaskadierte Systeme
- **Luecke 2:** Paralinguistische Info (Toene) als verlustbehaftete Dimension identifiziert — unsere Arbeit quantifiziert diesen Verlust durch TER

---

## 14. Survey Speech LLMs Understanding (2410.18908v6)

**Titel:** A Survey on Speech Large Language Models for Understanding  
**Autoren:** Peng et al., SJTU/ETH (2025)

### Zentrale Learnings
- Erste Taxonomie: Informational (linguistisch/paralinguistisch/nicht-linguistisch), Functional (Perception/Shallow/Deep Cognition), Format
- Zwei zentrale Herausforderungen: **Instruction Sensitivity** und **Degradation in Semantic Reasoning**
- Instruction Sensitivity: WER springt von 4.61% auf 48.48% bei Prompt-Aenderungen
- "Speech LLMs' original textual reasoning capacity may be weakened due to modality alignment constraints"
- Dreistufige Architektur: Feature Extraction -> Information Fusion -> LLM Inference

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant fuer Methodik:** Instruction Sensitivity (WER 4.61% → 48.48%) zeigt, dass Prompt-Design kritisch ist — stuetzt unsere Entscheidung, Pinyin+Tonnummern-Format fest vorzugeben und Prompt-Varianten zu kontrollieren
- IFR-Metrik (Instruction Following Rate) als moegliche Zusatzmetrik: misst, ob das Modell ueberhaupt im gewuenschten Pinyin-Format antwortet
- Dreidimensionale Taxonomie fuer die Einordnung unserer Aufgabe: linguistisch (Phonem/Ton) × Perception × structured output

### Forschungsluecken
- "Speech LLMs still have a long way to go in achieving true multitask generalizability"
- "Preference alignment based on reinforcement learning has received limited attention"
- "Managing speaker turns in multi-party conversations remain open problems"
- "Temporal alignment of speech segments has been relatively underexplored"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Survey zeigt, dass Speech LLMs auf ASR-Tasks (WER) evaluiert werden — unsere Arbeit geht auf Phonem-/Ton-Granularitaet
- **Luecke 2:** "Temporal alignment underexplored" — Toene erfordern temporale Praezision; unsere Arbeit prueft dies implizit durch Ton-Accuracy

---

## 15. MMAU (2410.19168v1)

**Titel:** MMAU: A Massive Multi-Task Audio Understanding and Reasoning Benchmark  
**Autoren:** Sakshi et al., U Maryland/Adobe (2024)

### Zentrale Learnings
- Erster umfassender Benchmark fuer multimodales Audio-Reasoning: 10.000 QA-Paare ueber Speech, Sound, Music
- "Even the most advanced Gemini Pro v1.5 achieves only 52.97% accuracy" -- massive Defizite bei Audio-Reasoning
- 27 distinct Skills, darunter "multi-speaker role mapping, emotional shift detection, temporal acoustic event analysis"

### Konkret verwendbar fuer unsere Arbeit
- Befund "bestes Modell nur 53%" zeigt, dass LALMs bei komplexen Audio-Tasks kaempfen — motiviert die Frage, ob das auch fuer Ton-Transkription gilt
- Skill-Taxonomie als Referenz: unsere Aufgabe waere ein neuer Skill "tonal transcription" der in keinem existierenden Benchmark vorkommt
- Getestete Modelle (Gemini Pro, GPT-4o etc.) ueberschneiden sich mit unserer Modellauswahl

### Forschungsluecken
- Kein spezifisches prosodisches oder tonales Reasoning getestet
- LALMs haben sich "in isolation" mit Fokus auf einzelne Domaenen entwickelt

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1 + 2:** MMAU testet kein tonales Reasoning — unsere Arbeit fuellt genau diese Luecke mit Phonem-/Ton-Evaluation
- **Luecke 4:** Mehrere der in MMAU getesteten Modelle werden in unserer Arbeit auf Ton-Granularitaet verglichen

---

## 16. Dynamic-SUPERB Phase-2 (2411.05361v1)

**Titel:** Dynamic-SUPERB Phase-2: A Collaboratively Expanding Benchmark with 180 Tasks  
**Autoren:** Huang et al., NTU/CMU/UT Austin (2024)

### Zentrale Learnings
- Groesstes Benchmark fuer instruktionsbasierte SLMs: 180 Tasks
- "No model outperforms Whisper-LLaMA in speech recognition and spoken language understanding"
- Bei Speaker/Paralinguistik uebertreffen alle Modelle die Whisper-LLaMA-Baseline (ASR verwirft kritische Information)
- "None of the models performed well universally"

### Konkret verwendbar fuer unsere Arbeit
- Befund "ASR verwirft paralinguistische Information" direkt relevant: Whisper als ASR-Baseline koennte Toninformation verlieren — stuetzt die Hypothese, dass nativ multimodale Modelle hier Vorteile haben (RQ4)
- "Kein universelles Modell" motiviert unseren Head-to-Head-Vergleich
- Task-Taxonomie als Referenz — "Mandarin tone transcription" fehlt als Task-Kategorie

### Forschungsluecken
- "It lacks comprehensive speech-generation tasks"
- Mandarin und andere Sprachen unterrepraesentiert
- Ca. 7% der Audiodateien laenger als 30s -- Whisper-Limitation

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Mandarin unterrepraesentiert, keine Phonem-/Ton-Tasks — unsere Arbeit liefert genau diese Evaluation
- **Luecke 2:** Toene als Task-Dimension fehlt komplett
- **Luecke 4:** Kein universelles Modell → unsere Arbeit vergleicht systematisch auf einem neuen Task

---

## 17. ASR-EC Benchmark (2412.03075v1)

**Titel:** ASR-EC Benchmark: Evaluating LLMs on Chinese ASR Error Correction  
**Autoren:** Wei et al., HKUST/WeBank (2024)

### Zentrale Learnings
- Drei Paradigmen: Prompting (ineffektiv), Finetuning (teilweise effektiv), Multi-modal Augmentation (am effektivsten)
- "Prompting is not effective for ASR error correction" -- Zero-Shot fuehrt zu Over-Correction
- "Multi-modal augmentation is the most effective method" -- gemeinsame Audio+Text-Nutzung
- Mindest-CER-Schwelle: Namensfehler und Pronomen koennen LLMs nicht korrigieren

### Konkret verwendbar fuer unsere Arbeit
- Befund "Multimodale Augmentation am effektivsten" stuetzt unseren Ansatz, nativ multimodale Modelle (Audio-Input) zu testen statt Text-only-Pipelines
- "Prompting ineffektiv fuer Fehlerkorrektur" warnt vor Over-Correction — relevant fuer unsere "target-leakage"-Problematik (Nordstern §5): LLMs koennten L2-Fehler auto-korrigieren statt sie zu transkribieren
- Fehlertyp-Klassifikation (Substitution, Deletion, Insertion) als Rahmen fuer RQ5 (Fehlerstrukturanalyse)

### Forschungsluecken
- "LLMs still face challenges in correcting errors requiring deep contextual understanding"
- Keine tonspezifischen Fehler untersucht (obwohl Mandarin)
- Kein Vergleich mit neueren SLM-Modellen

### Welche Luecken adressiert meine Arbeit?
- **Luecke 2:** Keine tonspezifischen Fehler untersucht — unsere Arbeit analysiert explizit Ton-Verwechslungsmuster (RQ5)
- **Luecke 4:** Kein Vergleich mit multimodalen LLMs — unsere Arbeit testet diese direkt

---

## 18. PERL (2412.03230v2)

**Titel:** PERL: Pinyin Enhanced Rephrasing Language Model for Chinese ASR N-Best Error Correction  
**Autoren:** Liang, Zhang (MBZUAI/CAS, 2025)

### Zentrale Learnings
- Pinyin als unternutztes Feature: "Existing Chinese ASR correction methods have not effectively utilized Pinyin information"
- 29.11% CER-Reduktion auf Aishell-1, ~70% auf DoAD (domain-spezifisch)
- N-Best optimal bei n=5; n=6 verschlechtert CER trotz hoeherem WCC
- Laengenvorhersage kritisch: Entfernung des Length Predictors fuehrt zu signifikanter Verschlechterung
- PERL (3.09ms) vs. LLMs (hunderte ms) -- extrem niedrige Latenz

### Konkret verwendbar fuer unsere Arbeit
- Bestaetigt Pinyin als unternutzte aber wertvolle Repraesentierung — stuetzt unsere Wahl von Pinyin+Tonnummern als Zielformat
- Phonetische Embedding-Methodik (Pinyin Encoder) als Kontext fuer die Diskussion, wie multimodale LLMs Pinyin intern repraesentieren koennten
- Latenz-Vergleich (PERL 3ms vs. LLMs hunderte ms) als Diskussionspunkt fuer die praktische Anwendbarkeit in einem Vokabeltrainer

### Forschungsluecken
- Nur Chinesisch evaluiert; Uebertragbarkeit unklar
- DoAD basiert auf synthetischer Sprache (TTS + Rauschen)
- Keine Integration in End-to-End-Systeme
- Keine Evaluation auf realen Sprachdaten

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** PERL ist ein dediziertes System, kein multimodaler LLM — unsere Arbeit testet, ob multimodale LLMs Pinyin-Wissen off-the-shelf besitzen
- **Luecke 2:** Toene in Pinyin nicht separat evaluiert — unsere Arbeit isoliert Ton-Accuracy

---

## 19. VoxEval (2501.04962v4)

**Titel:** VoxEval: Benchmarking Knowledge Understanding of End-to-End Spoken Language Models  
**Autoren:** Cui et al., CUHK (2025)

### Zentrale Learnings
- Erster End-to-End (Speech-in, Speech-out) Benchmark: 13.938 QA-Paare, 56 Faecher, 26 Audio-Bedingungen
- Aktuelle SLMs: "low accuracies often below random guessing, sensitivity to audio conditions"
- Mathematisches Reasoning in gesprochener Form besonders herausfordernd

### Konkret verwendbar fuer unsere Arbeit
- Methodik zur Evaluation unter diversen Audio-Bedingungen (Noise, Sprechstile) — relevant falls wir verschiedene Aufnahmequalitaeten testen
- Befund "unter Zufallsniveau" zeigt, dass SLMs bei bestimmten Tasks fundamental kaempfen — motiviert die Frage, ob das auch fuer Mandarin-Ton-Transkription gilt
- Erkenntnis, dass E2E-Evaluation andere Ergebnisse als Speech-to-Text liefert — stuetzt unseren Ansatz, Modelle direkt (off-the-shelf) zu testen

### Forschungsluecken
- Nur Englisch; keine tonalen Sprachen
- Keine Aussprachebewertung oder MDD
- "Other critical aspects such as fairness, toxicity, hallucination" nicht abgedeckt

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Nur Englisch, keine tonalen Sprachen — unsere Arbeit evaluiert explizit Mandarin mit Ton-Granularitaet
- **Luecke 2:** Keine Aussprachebewertung — unsere Arbeit macht Phonem-/Ton-Transkription zum zentralen Task

---

## 20. FireRedASR (2501.14350v1)

**Titel:** FireRedASR: Open-Source Industrial-Grade Mandarin Speech Recognition  
**Autoren:** Xu et al., Xiaohongshu (2025)

### Zentrale Learnings
- FireRedASR-LLM (8.3B): SOTA CER 3.05% auf Mandarin; FireRedASR-AED (1.1B): 3.18% CER
- "One thousand hours of high-quality, human-labeled data yields better results than ten thousand hours of weakly-labeled data"
- Progressive Regularization Training: erst ohne, dann graduell staerkere Regularisierung
- Gesangstexterkennung: 50-67% CERR gegenueber Industriestandard

### Konkret verwendbar fuer unsere Arbeit
- **Potenzielle Baseline fuer RQ4:** Open-Source (Apache 2.0), SOTA fuer Mandarin ASR — kann als dediziertes System neben Whisper verglichen werden
- CER-Benchmarkwerte (3.05%) als obere Referenz fuer Zeichenebene — unsere Arbeit prueft, ob diese Genauigkeit auch auf Phonem-/Ton-Ebene gilt
- "Datenqualitaet > Quantitaet" als Designprinzip relevant fuer die Diskussion unserer Korpuswahl (Tone Perfect: klein aber hochqualitativ)

### Forschungsluecken
- Primaer Mandarin-fokussiert
- Kein kontextuelles/domain-spezifisches ASR
- Kein Streaming/Real-Time ASR evaluiert

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** FireRedASR evaluiert auf CER (Zeichenebene) — unsere Arbeit geht auf Phonem-/Ton-Granularitaet
- **Luecke 2:** Keine Ton-Evaluation — unsere Arbeit fuehrt TER ein
- **Luecke 3:** Keine L2-Evaluation — unsere Arbeit adressiert dies im Stretch-Goal
- **Luecke 4:** Kein Vergleich mit multimodalen LLMs — unsere Arbeit positioniert FireRedASR als potenzielle Baseline

---

## 21. Landscape of SLMs (2504.08528v1)

**Titel:** On The Landscape of Spoken Language Models: A Comprehensive Survey  
**Autoren:** Arora et al., CMU/NTU/TTIC (2025)

### Zentrale Learnings
- Drei SLM-Kategorien: Pure Speech LMs, Speech+Text LMs, Speech-aware Text LMs
- "One may wonder whether a better path is to combine ASR, text LLMs, and TTS in series. This is indeed a strong baseline approach"
- Modality Adapter spielt Schluesselrolle bei Alignment-Qualitaet (Linear, CNN, CTC, Q-Former)
- "End-to-end SLM approach can avoid compounding of errors"

### Konkret verwendbar fuer unsere Arbeit
- Drei-Kategorien-Taxonomie fuer Einordnung der getesteten Modelle in Related Work §2.3
- Architekturformulierung (Encoder → Adapter → Sequence Model → Decoder) als analytisches Schema
- Befund "Kaskade ist starke Baseline" bestaetigt die Relevanz unseres Whisper-Baseline-Vergleichs (RQ4)
- Modell-Tabelle (50+ SLMs) als Referenz fuer die Modellauswahl

### Forschungsluecken
- "The optimal representation of speech within SLMs remains unclear"
- "There is a lack of public high-quality training data"
- "Very few SLMs are fully open-source"
- "Scaling studies for SLMs are needed"
- "Improving inclusiveness and safety" fuer unterrepraesentierte Sprachen/Dialekte

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** "Optimale Repraesentation unklar" — unsere Arbeit prueft empirisch, ob aktuelle Repraesentationen fuer Phonem-/Ton-Granularitaet ausreichen
- **Luecke 4:** Survey listet 50+ SLMs ohne direkten Vergleich auf Ton-Tasks — unsere Arbeit liefert diesen Vergleich

---

## 22. Kimi-Audio (2504.18425v1)

**Titel:** Kimi-Audio Technical Report  
**Autoren:** Kimi Team, Moonshot AI (2025)

### Zentrale Learnings
- Open-Source Audio Foundation Model fuer Understanding, Generation und Conversation
- Hybride Tokenisierung: diskrete semantische Tokens (12.5Hz) + kontinuierliche akustische Features
- >13 Millionen Stunden Audio-Pretraining-Daten
- Chunk-wise Streaming Detokenizer basierend auf Flow Matching
- "From Audio Transcription to Audio Description" -- Pretraining vernachlaessigt paralinguistische Information

### Konkret verwendbar fuer unsere Arbeit
- **Potenzielles Testmodell:** Open-Source Audio Foundation Model, das in unsere Evaluation aufgenommen werden koennte
- Befund "Pretraining vernachlaessigt paralinguistische Information" (inkl. Toene) direkt relevant — erklaert potenzielle Schwaechen bei Ton-Transkription
- Hybride Tokenisierung (semantisch + akustisch) als Erklaerungsansatz: semantische Tokens koennten Toninformation verlieren, akustische Features koennten sie bewahren

### Forschungsluecken
- "Text transcription neglects important information such as paralanguage (emotion, style, timbre, tone)"
- "Current audio foundation models rely heavily on ASR and TTS -- can hardly achieve performance far beyond the ceiling of ASR/TTS"
- Kein Fokus auf Mandarin-Ton-Bewertung oder L2-Pronunciation Assessment

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1 + 2:** Kimi-Audio selbst konstatiert, dass Toene bei Transkription vernachlaessigt werden — unsere Arbeit prueft genau diese Faehigkeit systematisch
- **Luecke 3:** Kein L2-Pronunciation Assessment — unser Stretch-Goal adressiert dies
- **Luecke 4:** Kimi-Audio als eines der zu vergleichenden Modelle in unserem Head-to-Head

---

## 23. Holistic Evaluation LALMs (2505.15957v4)

**Titel:** Towards Holistic Evaluation of Large Audio-Language Models: A Comprehensive Survey  
**Autoren:** Yang, Ho, Lee, NTU (2026)

### Zentrale Learnings
- Vierdimensionale Taxonomie: (1) General Auditory Awareness, (2) Knowledge and Reasoning, (3) Dialogue, (4) Fairness/Safety/Trustworthiness
- "Different LALMs excel in different domains, each with their own strengths"
- LLM-as-a-judge als "emerging trend" fuer skalierbare Evaluation

### Konkret verwendbar fuer unsere Arbeit
- Vierdimensionale Taxonomie als Rahmen: unsere Aufgabe faellt unter "General Auditory Awareness" → "Speech" → sublexical/tonal — zeigt die Nische unserer Arbeit im Gesamtbild
- "Verschiedene LALMs in verschiedenen Domaenen stark" stuetzt unseren Multi-Modell-Vergleich (RQ4)
- Ueberblick existierender Benchmarks zeigt, dass keiner Mandarin-Ton-Granularitaet abdeckt — bestaetigt unsere Forschungsluecke

### Forschungsluecken
- Datenkontamination: "Models may have seen these datasets during training"
- "Many overlook crucial aspects such as low-resource languages and code-switching"
- "Safety should cover auditory comfort, not just content harmlessness"
- Personalisierung: "LALMs must become familiar with users' voice characteristics"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Survey zeigt, dass Low-Resource-Sprachen und sublexikalische Granularitaet uebersehen werden — unsere Arbeit liefert Mandarin-Phonem-/Ton-Evaluation
- **Luecke 4:** Survey fordert systematische Evaluation — unsere Arbeit liefert einen kontrollierten Vergleich

---

## 24. ZIPA (2505.23170v1)

**Titel:** ZIPA: A family of efficient models for multilingual phone recognition  
**Autoren:** Zhu et al., UBC/CMU (2025)

### Zentrale Learnings
- ZIPA (Zipformer-basiert): 64M Modelle uebertreffen 300M-Parameter-Modelle bei Phone Recognition
- IPAPack++: 17.132 Stunden in 88 Sprachen mit normalisierten IPA-Transkriptionen
- "Phone recognition models tend to smooth out phonetic variation" -- Standard-Aussprache statt soziophonischer Variation
- "Smaller models generalize better to unseen languages but perform worse on seen languages"
- "Vowels are more difficult to predict crosslinguistically"

### Konkret verwendbar fuer unsere Arbeit
- **Potenzielle Baseline fuer RQ4:** ZIPA als dedizierter Phone-Recognizer koennte neben Whisper als spezialisierte Baseline dienen
- PFER (Phonetic Feature Error Rate) als alternative Metrik — koennte PER ergaenzen
- Befund "Modelle glaetten phonetische Variation" relevant fuer L2-Evaluation (RQ3): wenn Phone-Recognizer L2-Variation glaetten, koennten LLMs das auch tun (auto-correction problem)
- IPAPack++ als Trainingsressource, falls wir IPA-basierte Vergleiche ergaenzen

### Forschungsluecken
- Sprachenverteilung stark verzerrt (Bias zu High-Resource-Sprachen)
- G2P-Trainingsdaten bilden soziophonische Variation nicht ab
- "Simply scaling up G2P might not solve phone recognition"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** ZIPA evaluiert Phone Recognition ohne Ton-Dimension — unsere Arbeit fuegt Ton-Granularitaet hinzu
- **Luecke 2:** Toene nicht als separate Achse — unsere Arbeit isoliert TER
- **Luecke 4:** Kein Vergleich mit multimodalen LLMs — unsere Arbeit vergleicht LLMs mit dedizierten Phone-Recognizern

---

## 25. WildSpeech-Bench (2506.21875v1)

**Titel:** WildSpeech-Bench: Benchmarking Audio LLMs in Natural Speech Conversation  
**Autoren:** Zhang et al., Tencent WeChat AI (2025)

### Zentrale Learnings
- Drei Probleme bestehender Benchmarks: "biased query source", "absence of acoustic features", "untargeted evaluation"
- Open-Source-Modelle haben deutliche Luecken zu GPT-4o
- Query-aware Evaluation mit individuellen Checklisten verbessert Genauigkeit signifikant
- Paralinguistische Features (Pausen, Betonung, Stottern, Near-Homophones) entscheidend

### Konkret verwendbar fuer unsere Arbeit
- Query-aware Evaluierungsmethode als methodische Inspiration: fuer jede Silbe individuelle Checkliste (korrektes Phonem? korrekter Ton?)
- Befund "acoustic features werden in Benchmarks ignoriert" stuetzt unsere Motivation — Toene sind akustische Features
- Near-Homophones als Parallel zu Ton-Minimalpaaren (z.B. ma1 vs. ma2)

### Forschungsluecken
- "Our current benchmark focuses solely on single-turn dialogue evaluation"
- "The lack of actual in-the-wild speech data"
- Nur Englisch; multilinguale Abdeckung fehlt

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Nur Englisch, keine Phonem-/Ton-Evaluation — unsere Arbeit evaluiert Mandarin auf sublexikalischer Ebene
- **Luecke 2:** Akustische Features (inkl. Toene) werden ignoriert — unsere Arbeit macht sie zum zentralen Evaluationskriterium

---

## 26. ContextASR-Bench (2507.05727v2)

**Titel:** ContextASR-Bench: A Massive Contextual Speech Recognition Benchmark  
**Autoren:** Wang et al., Alibaba (2025)

### Zentrale Learnings
- 40.000+ Eintraege, 300.000+ Named Entities, 10+ Domaenen (EN+ZH)
- LALMs uebertreffen konventionelle ASR bei Named Entities dank LLM-Weltwissen
- Drei Evaluierungsmodi: Contextless, Coarse-grained Context, Fine-grained Context
- Neue Metriken NE-WER und NE-FNR fuer Named-Entity-Genauigkeit
- Bei manchen LALMs schwere Halluzinationen bei Fine-grained Context

### Konkret verwendbar fuer unsere Arbeit
- Halluzinationsproblem relevant: LALMs koennten bei Ton-Transkription halluzinieren (z.B. Pinyin eines anderen Zeichens ausgeben) — muss in Fehleranalyse (RQ5) beruecksichtigt werden
- "LALMs besser bei Named Entities dank Weltwissen" — analoges Argument: LLMs koennten Mandarin-Toene besser transkribieren, wenn sie Pinyin-Wissen aus Texttraining haben (relevant fuer RQ1)
- Evaluierungsmodi (contextless vs. context) als methodische Anregung: wir koennten testen, ob Kontextinformation (z.B. "der Sprecher sagt ein Wort mit Ton 3") die Ton-Accuracy beeinflusst

### Forschungsluecken
- "Current LALMs still struggle in contextual ASR"
- Basiert auf TTS, keine realen Aufnahmen
- Halluzinationsproblem ungeloest

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** ContextASR-Bench evaluiert auf Wort-/Entity-Ebene — unsere Arbeit geht auf Phonem-/Ton-Granularitaet
- **Luecke 2:** Keine Ton-Evaluation — unsere Arbeit fuehrt diese ein

---

## 27. Step-Audio 2 (2507.16632v3)

**Titel:** Step-Audio 2 Technical Report  
**Autoren:** StepFun Audio Team (2025)

### Zentrale Learnings
- End-to-End multimodales LLM mit Latent Audio Encoder + Reasoning-centric RL
- Integriert diskrete Audio-Token-Generierung in Language Modeling
- RAG und externe Tools (Web Search, Audio Search) zur Halluzinations-Reduktion
- SOTA: MMAU 78.0, AISHELL 97.9, LibriSpeech 98.5
- Herausragend bei Paralinguistic Understanding: 83.09% (vs. GPT-4o: 43.45%)

### Konkret verwendbar fuer unsere Arbeit
- **Potenzielles Testmodell:** Step-Audio 2 mit starker paralinguistischer Performance — koennte bei Ton-Erkennung besonders gut abschneiden
- Paralinguistic Understanding 83% vs. GPT-4o 43% — zeigt, dass nicht alle multimodalen LLMs gleich sind; motiviert unseren Head-to-Head (RQ4)
- AISHELL-Performance (97.9%) als Vergleichswert fuer Mandarin-CER

### Forschungsluecken
- Kein Fokus auf Tonbewertung/L2-Pronunciation Assessment
- Keine Evaluation auf tonspezifischen Benchmarks

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Trotz starker Paralinguistik keine Phonem-/Ton-Evaluation — unsere Arbeit prueft genau das
- **Luecke 2:** Keine Ton-spezifische Evaluation — unsere Arbeit fuehrt TER ein
- **Luecke 3:** Keine L2-Evaluation — unser Stretch-Goal
- **Luecke 4:** Step-Audio 2 als eines der zu vergleichenden Modelle in unserem Head-to-Head

---

## 28. TELEVAL (2507.18061v3)

**Titel:** TELEVAL: A Dynamic Benchmark for SLMs in Chinese Interactive Scenarios  
**Autoren:** Li et al., TeleAI (2026)

### Zentrale Learnings
- Dynamischer nutzer-zentrierter Benchmark: >40.000 Evaluationssamples
- Zwei Dimensionen: Reliable Content Fulfillment + Interactional Appropriateness
- "Despite strong performance on knowledge tasks, current SLMs exhibit a clear deficit in social-pragmatic competence"
- Modelle erkennen paralinguistische Cues, handeln aber nicht responsiv

### Konkret verwendbar fuer unsere Arbeit
- Befund "Modelle erkennen paralinguistische Cues, handeln aber nicht responsiv" relevant: LLMs koennten Toene wahrnehmen, aber nicht korrekt in Pinyin ausgeben (RQ5)
- Kritik an bestehenden Benchmarks (Modality Leakage, unrealistische Formate) als methodische Warnung fuer unser Evaluationsdesign
- Chinese-spezifischer Benchmark als thematischer Kontext fuer Related Work §2.4

### Forschungsluecken
- Synthetische Audio-Daten, keine natuerliche Sprache
- Multi-Turn-Interaktional-Angemessenheit nicht bewertet
- "LLM-Judge-Korrelation" nicht explizit analysiert

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** TELEVAL evaluiert auf Aufgabenebene, nicht Phonem-/Ton-Ebene — unsere Arbeit geht auf sublexikalische Granularitaet
- **Luecke 2:** Keine Ton-Evaluation trotz Chinese-Fokus — unsere Arbeit fuellt diese Luecke

---

## 29. VocalBench-zh (2511.08230v1)

**Titel:** VocalBench-zh: Decomposing and Benchmarking Speech Conversational Abilities in Mandarin  
**Autoren:** Liu et al., SJTU/Ant Group (2025)

### Zentrale Learnings
- Erstes umfassendes Mandarin-spezifisches S2S-Benchmark: 10 Subsets, 11.115+ Instanzen, 14 Modelle evaluiert
- "LLM backbone is the main factor affecting overall performance, especially semantic quality"
- "SpeechLLMs currently lack awareness of Chinese character components and structure" (Radikale, Strichfolge)
- "Robustness is an inherent advantage of end-to-end solutions"
- "Language alignment in multilingual input processing remains a largely overlooked challenge" -- >1/3 korrekte Antworten verloren bei Code-Switching

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant:** Befund "SpeechLLMs fehlt Bewusstsein fuer chinesische Zeichenstruktur" impliziert potenzielle Schwaechen bei Pinyin-Generierung — direkt relevant fuer RQ1 und RQ2
- 14 evaluierte Modelle als Referenz fuer unsere Modellauswahl
- "LLM-Backbone entscheidend" informiert unsere Diskussion: die Text-LLM-Qualitaet koennte die Pinyin-Ausgabe staerker beeinflussen als die Audio-Verarbeitung
- Benchmark-Taxonomie (Semantic, Acoustic, Chat, Robust) als Vorbild fuer unsere Evaluationsstruktur

### Forschungsluecken
- "Relies exclusively on synthetic data" -- keine menschlichen Aufnahmen
- Dialekt-Evaluation fehlt
- Paralinguistische Kontrolle und emotionale Generierung bleiben offen

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** VocalBench-zh evaluiert auf Satzebene (semantisch), nicht Phonem-/Ton-Ebene — unsere Arbeit fuegt sublexikalische Granularitaet hinzu
- **Luecke 2:** Keine isolierte Ton-Evaluation — unsere Arbeit fuehrt TER als explizite Achse ein
- **Luecke 3:** Nur synthetische Daten — unsere Arbeit verwendet echte Aufnahmen (Tone Perfect) und potentiell L2-Daten

---

## 30. Qwen3-ASR (2601.21337v1)

**Titel:** Qwen3-ASR Technical Report  
**Autoren:** Qwen Team (2026)

### Zentrale Learnings
- Zwei All-in-One ASR-Modelle (1.7B, 0.6B) + NAR Forced Alignment Modell
- Unterstuetzung fuer 52 Sprachen/Dialekte (30 Sprachen + 22 chinesische Dialekte)
- "RL plays an essential role for noise robustness, transcribing stability"
- ForcedAligner: "first LLM-based speech forced aligner with accurate timestamps at flexible granularities"
- SOTA unter Open-Source ASR, kompetitiv mit proprietaeren APIs

### Konkret verwendbar fuer unsere Arbeit
- **Starke Baseline-Kandidat fuer RQ4:** Open-Source, SOTA fuer Mandarin, Apache 2.0 — neben Whisper als dediziertes System
- ForcedAligner koennte fuer Ton-Alignment genutzt werden: Timestamps auf Silbenebene helfen bei der Zuordnung von Toenen zu Silben
- 22 chinesische Dialekte als Kontext fuer die Diskussion, ob Dialektvariabilitaet die Ton-Accuracy beeinflusst
- Qwen3-ASR ist auch ein LLM-basiertes System — interessanter Zwischenfall zwischen "dediziert" und "multimodal allgemein"

### Forschungsluecken
- Kein Fokus auf Tone Assessment/Pronunciation Evaluation
- Keine Evaluation fuer L2-Learner
- Forced Alignment auf Wort-Ebene, nicht Ton-Level

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Qwen3-ASR evaluiert auf CER/WER — unsere Arbeit prueft Phonem-/Ton-Granularitaet
- **Luecke 2:** Kein Tone Assessment — unsere Arbeit fuehrt TER ein
- **Luecke 3:** Keine L2-Evaluation — unser Stretch-Goal
- **Luecke 4:** Qwen3-ASR als Baseline neben Whisper im systematischen Vergleich

---

## 31. Survey LALMs Trustworthiness (2605.20266v1)

**Titel:** A Survey of Large Audio Language Models: Generalization, Trustworthiness, and Outlook  
**Autoren:** Luo et al. (2026)

### Zentrale Learnings
- Umfassendster Survey mit Fokus auf Trustworthiness (sechs Saeulen: Hallucination, Robustness, Safety, Privacy, Fairness, Authentication)
- Modality Neglect Problem: "models over-rely on lexical cues rather than acoustic emotion signals"
- Cross-Modal Alignment Luecke: "Most LALMs inherit safety alignment from text-only RLHF"
- "The escalation of LALMs' capabilities has significantly outpaced systemic frameworks to ensure their trustworthiness"

### Konkret verwendbar fuer unsere Arbeit
- **"Modality Neglect"** direkt relevant: wenn Modelle lexikalische statt akustischer Cues nutzen, koennten sie bei Ton-Transkription das "richtige" Pinyin aus Textwissen generieren statt den tatsaechlich gehoerten Ton zu transkribieren — genau unser "target-leakage/linguistic trap" (Nordstern §5)
- Umfassende LALM-Architektur-Tabelle (2022-2026) als Referenz fuer Modellauswahl und Related Work §2.3
- Halluzinationsproblematik als Erklaerungsrahmen fuer potenzielle Fehler bei Ton-Transkription

### Forschungsluecken
- "Community lacks a comprehensive Safety Leaderboard"
- "Audio-aware alignment" fehlt -- RLHF mit multimodalen Praeferenzsignalen noetig
- Kein Fokus auf tonale Sprachen oder L2-Szenarien

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Survey identifiziert "Modality Neglect" — unsere Arbeit testet empirisch, ob multimodale LLMs tatsaechlich akustische Ton-Cues nutzen oder auf lexikalisches Wissen zurueckfallen
- **Luecke 2:** Keine tonale Evaluation — unsere Arbeit fuehrt TER als explizite Metrik ein
- **Luecke 3:** Kein L2-Fokus — unser Stretch-Goal

---

## 32. Zipformer Mandarin Phonemes (journal.pone.0324048)

**Titel:** A study on phonemes recognition method for Mandarin pronunciation based on improved Zipformer-RNN-T(Pruned)  
**Autoren:** Du et al., Shihezi University (2025)

### Zentrale Learnings
- WER 1.92%/2.12% auf AISHELL1-PHONEME mit nur 61.1M Parametern
- 97% der 66 Phoneme mit >95% Accuracy
- Typische Verwechslungen: /B/ vs. /P/, /IN/ vs. /ING/, /Z/ vs. /ZH/, /S/ vs. /SH/
- Greedy Search: bestes WER/RTF-Verhaeltnis (RTF 0.002)
- AISHELL1-PHONEME Dataset mit phrasenlevelbasierter Zuordnung fuer polyphone Zeichen

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant fuer RQ5:** Verwechslungsmatrix (/B/ vs. /P/, /IN/ vs. /ING/, /Z/ vs. /ZH/, /S/ vs. /SH/) als Referenz — wir koennen pruefen, ob multimodale LLMs dieselben Phonem-Verwechslungen zeigen
- AISHELL1-PHONEME Dataset-Konstruktionsmethodik als Vorbild fuer die Erstellung unseres Phonem-Level-Ground-Truth
- Leichtgewichtige Architektur (61.1M) als potenzielle Baseline fuer RQ4
- 97% der Phoneme >95% Accuracy als obere Referenz fuer Phonem-Erkennung durch dedizierte Systeme

### Forschungsluecken
- "Scarcity of Chinese reading evaluation datasets"
- Keine Evaluation auf L2-Sprecher
- Keine Toninformation (F0/Pitch) integriert
- Kein Self-Supervised Pre-training (HuBERT, wav2vec2.0)

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Nur Phonem-Erkennung ohne Toene — unsere Arbeit fuegt die Ton-Dimension hinzu
- **Luecke 2:** "Keine Toninformation integriert" — unsere Arbeit macht Toene zur zentralen Evaluationsachse
- **Luecke 3:** Keine L2-Evaluation — unser Stretch-Goal
- **Luecke 4:** Kein Vergleich mit multimodalen LLMs — unsere Arbeit stellt diesen her

---

## 33. SCCM (journal.pone.0325045)

**Titel:** A syllable-character collaborative model for enhanced Pinyin and Chinese recognition  
**Autoren:** Chen et al., Guangxi University (2025)

### Zentrale Learnings
- Multi-Task-Architektur: Shared Encoder (Conformer) + 3 Decoder (Pinyin-CTC, Initials/Finals-CTC, Character-Transformer)
- Pinyin-Ensemble (PE) Modul: 10 Klassifikatoren kombinieren Outputs aller Decoder
- 45.7% relative CER-Reduktion auf AISHELL-1 Baseline
- "The relationship between Chinese text and pronunciation is more complex" als bei phonetischen Schriftsystemen

### Konkret verwendbar fuer unsere Arbeit
- Dreistufige Dekodierung (Pinyin → Initials/Finals → Characters) zeigt, dass Pinyin als Zwischenstufe wertvoll ist — stuetzt unsere Wahl von Pinyin als Zielformat
- Befund "Beziehung Text-Aussprache komplex" erklaert, warum RQ1 (Text-only Baseline) nicht-trivial ist: polyphone Zeichen machen Character→Pinyin-Konversion schwierig
- Pinyin-Ensemble-Konzept als Diskussionspunkt: koennte ein Ensemble mehrerer LLMs die Ton-Accuracy verbessern?

### Forschungsluecken
- Nur AISHELL-1 evaluiert
- Keine Beruecksichtigung von Toenen
- Kein LLM-Einsatz
- Kein Streaming/Real-Time

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Kein LLM-Einsatz — unsere Arbeit testet multimodale LLMs
- **Luecke 2:** "Keine Beruecksichtigung von Toenen" — unsere Arbeit macht Toene zur zentralen Evaluationsachse
- **Luecke 4:** Kein Vergleich multimodaler Modelle — unsere Arbeit liefert diesen

---

## 34. Perception-Production L2 Mandarin Tones (mathematics-14-00145)

**Titel:** Perception-Production of Second-Language Mandarin Tones Based on Interpretable Computational Methods: A Review  
**Autoren:** Huang et al. (2026)

### Zentrale Learnings
- Systematischer Review (54 Studien, 2015-2025) mit PRISMA-Methodologie
- "The contrast between Tone 2 (rising) and Tone 3 (dipping/low) remains the persistent difficulty"
- Vier Straenge: (A) konventionelle Evaluationen, (B) physiologische Instrumente (EEG), (C) Audio-only Speech Analysis (CNN/RNN/CTC, SSL), (D) interpretable Modelle (GNNs + XAI)
- Slope, Turning-Point-Timing und lokaler F0-Range diagnostisch wichtiger als absolute Tonhoehe
- "0% of studies had adequate measurement reliability/QA reporting" -- massive methodologische Luecken

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant fuer RQ5 und Related Work §2.5:** Tone 2/Tone 3-Verwechslung als primaere Schwierigkeit — wir koennen pruefen, ob multimodale LLMs dasselbe Muster zeigen
- 4-Strand-Systematik als Strukturierung fuer §2.2 (traditionelle Methoden) und §2.4.2 (Tonerkennung)
- Diagnostische Parameter (Slope, Turning-Point-Timing, F0-Range) als Erklaerung, warum bestimmte Toene schwieriger sind
- Explainability-Methoden (SHAP, Integrated Gradients) als potenzielle Erweiterung fuer die Fehleranalyse (RQ5)
- Methodologische Luecken ("0% adequate QA") als Motivation fuer sorgfaeltiges Evaluationsdesign

### Forschungsluecken
- "Classroom evidence remains limited relative to laboratory studies"
- "Many studies measure perception and production separately"
- "85.2% of studies had unclear transparency/reproducibility"
- "0% had adequate measurement reliability reporting"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Review deckt traditionelle und spezialisierte ML-Methoden ab, aber keine multimodalen LLMs — unsere Arbeit fuellt diese Luecke
- **Luecke 3:** "Classroom evidence limited" — unsere Arbeit testet den L2-Fall (Stretch-Goal RQ3) mit Fokus auf praktische Anwendbarkeit
- **Luecke 4:** Kein Vergleich multimodaler Foundation Models — unsere Arbeit liefert den ersten systematischen Head-to-Head

---

## 35. ML Mandarin Tone Recognition Review (preprints202510.2478.v1)

**Titel:** Machine Learning for Mandarin Tone Recognition: A Systematic Review  
**Autoren:** Zou et al. (2025)

### Zentrale Learnings
- 61 Studien analysiert; Deep Learning uebertrifft traditionelle Ansaetze (88.8% vs. 83.1%)
- CNNs bis 99.16% fuer isolierte Silben; kontinuierliche Sprache liegt >15% dahinter
- "Performance is affected by Tone 3 variability, neutral tones, and challenging conditions"
- Goodness of Tone (GOT) Framework fuer diagnostisches CALL-Feedback
- "Intelligent feature selection can reduce input to 12-15 critical features while maintaining >97.5% accuracy"

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant fuer Related Work §2.4.2 und §2.5:** Umfassendste Taxonomie der ML-Ansaetze fuer Mandarin-Tonerkennung
- GOT (Goodness of Tone) Framework als Referenz fuer diagnostisches Feedback — relevant fuer die Diskussion, ob LLM-basierte Transkription GOT-artige Informationen liefern kann
- 99.16% fuer isolierte Silben als obere Referenz — unser Tone-Perfect-Korpus besteht aus isolierten Silben; wir koennen vergleichen, ob LLMs an diese Genauigkeit herankommen
- Tone 3-Variabilitaet und neutrale Toene als erwartete Fehlerquellen fuer RQ5
- 6 praktische Empfehlungen (Table 3) als Designleitfaden
- Soft-Label-Ansatz fuer L2-Bewertung als Methodik fuer RQ3

### Forschungsluecken
- "Lack of diverse datasets, weak prosody and dialect modeling, insufficient validation rigor"
- "Most models trained on young adult male speakers"
- "Speech corpora for clinical populations remain scarce"
- "Existing methods are highly Mandarin-specific"

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Review deckt nur dedizierte ML-Modelle ab — unsere Arbeit testet erstmals multimodale LLMs auf Ton-Granularitaet
- **Luecke 4:** Kein Vergleich von multimodalen Foundation Models — unsere Arbeit liefert genau diesen Head-to-Head und vergleicht mit den im Review beschriebenen dedizierten Systemen

---

## 36. ASR Tonal Languages Survey (s11831-020-09414-4)

**Titel:** Automatic Speech Recognition System for Tonal Languages: State-of-the-Art Survey  
**Autoren:** Kaur, Singh, Kadyan (2021)

### Zentrale Learnings
- Umfassender Survey ueber ASR fuer tonale Sprachen weltweit (Asien, Indo-Europa, Afrika)
- F0 als primaerer Cue; MFCC und HMM am haeufigsten verwendet
- "Lot of work for Asian tonal languages (Chinese, Thai, Vietnamese) but little for Mizo, Bodo, Indo-European tonal languages"

### Konkret verwendbar fuer unsere Arbeit
- Historischer Kontext fuer Related Work §2.2.1 (Pre-Transformer-Methoden): zeigt den Entwicklungsstand vor LLMs
- F0 als primaerer Cue bestaetigt: erklaert, warum Tonerkennung akustische Analyse erfordert und nicht rein textbasiert geloest werden kann (Motivation fuer RQ1 vs. RQ2 Vergleich)
- Klassifikation tonaler Sprachen als Hintergrund fuer §2.2

### Forschungsluecken
- "Lack of benchmark speech corpus for majority languages"
- "ASR for tonal languages of American and Austral-Asia not studied"
- Keine LLM/Deep-Learning-Beruecksichtigung (pre-Transformer-Aera)

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Survey ist pre-Transformer und pre-LLM — unsere Arbeit bringt die Ton-Evaluation in die Aera multimodaler Foundation Models
- **Luecke 2:** Toene als separate Achse nicht systematisch evaluiert — unsere Arbeit fuehrt TER ein
- **Luecke 4:** Kein Vergleich moderner Modelle — unsere Arbeit liefert den ersten multimodalen LLM-Vergleich

---

## 37. ResNet Siamese Mandarin Tones (s41598-025-08544-8)

**Titel:** Evaluating Mandarin tone pronunciation accuracy using a ResNet-based Siamese network  
**Autoren:** Bu et al. (2025)

### Zentrale Learnings
- Siamese Network mit ResNet-18 bewertet Ton-Diskrepanzen zwischen L2-Lernern und Standard-Mandarin
- Zwei neuartige Features: 40D-Vektor (1D) und 40x50 Pixelbild (2D) basierend auf Fuenf-Stufen-Tonskala
- 2D Features + ResNet-18 zeigen beste Konsistenz und Stabilitaet
- Ton 3 wird am besten unterschieden ("easier for models to learn")
- Spezialisierter Korpus mit 40 tibetischen L2-Sprechern

### Konkret verwendbar fuer unsere Arbeit
- **Relevant fuer RQ3 und RQ5:** Five-Degree Tone Scale Normalisierung (T-value) als Feature fuer die Interpretation von Ton-Fehlern
- Kontrastive Paar-Diskrepanz-Methode als alternatives Evaluationsparadigma — statt "ist der Ton korrekt?" die Frage "wie weit weicht er ab?"
- Ton 3 "am besten unterschieden" widerspricht dem T2/T3-Verwechslungsbefund aus Paper 34 — interessanter Diskussionspunkt fuer RQ5
- L2-Korpus (tibetische Sprecher) als Referenz fuer die Diskussion der L1-Transfereffekte

### Forschungsluecken
- "Method relies on computed tone features, which may oversimplify tone characteristics"
- Keine End-to-End-Verarbeitung; erfordert Silben-Segmentierung und F0-Extraktion
- Beschraenkt auf Silbenebene; keine Satz-/Diskursebene
- Korpus klein und auf tibetische L2-Sprecher beschraenkt

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Nur dediziertes System (ResNet Siamese) — unsere Arbeit testet multimodale LLMs off-the-shelf
- **Luecke 3:** Nur tibetische L2-Sprecher — unsere Arbeit zielt auf breitere L2-Evaluation (Stretch)
- **Luecke 4:** Kein Vergleich mit multimodalen Foundation Models — unsere Arbeit liefert diesen

---

## 38. Mandarin Speech Prosody Benchmark (wang25v_interspeech)

**Titel:** Can AI Understand Mandarin Speech Prosody? A Framework and Benchmark Showcase  
**Autoren:** Wang et al., Microsoft Research Asia (2025)

### Zentrale Learnings
- MSPB: 8 prosodische Tasks (Intonation, Disambiguation, Focus, Irony, Emotion etc.)
- Human Accuracy 81-96%, Modelle stark schwankend
- "Current Speech LLMs still require substantial improvement to fully capture nuanced subtleties of human speech prosody"
- Kritisch: "They rely more on contextual cues than on subtle speech prosody variations"

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant:** Befund "Modelle verlassen sich auf Kontext statt Prosodie" ist zentral fuer unsere Thesis — wenn LLMs bei Prosodie auf Kontext zurueckfallen, tun sie das wahrscheinlich auch bei Toenen (target-leakage/linguistic trap, Nordstern §5)
- MSPB als Vorbild fuer unser Benchmark-Design: wir ergaenzen die fehlende Ton-Dimension (T1-T4)
- Getestete Modelle (GPT-4o, Gemini, Qwen2-Audio, GLM-4-Voice) ueberschneiden sich mit unserer Modellauswahl — Performance-Referenzwerte verfuegbar
- 8-Task-Taxonomie als Kontext fuer §2.5 (Evaluation of pronunciation and tone recognition)

### Forschungsluecken
- "Future work should explore multi-turn conversational settings"
- Keine Tone-spezifische Evaluation (T1-T4) -- nur suprasegmentale Prosodie
- Nur Mandarin; multilingualer Benchmark fehlt

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1 + 2:** MSPB evaluiert suprasegmentale Prosodie, aber nicht Toene (T1-T4) — unsere Arbeit fuellt genau diese Luecke mit expliziter Ton-Evaluation auf Silbenebene
- **Luecke 4:** MSPB testet 4 Modelle auf Prosodie — unsere Arbeit testet ~5-8 Modelle spezifisch auf Toene

---

## 39. L2 Mandarin Prosody Rating (wu24_speechprosody)

**Titel:** The assessment of automated rating of L2 Mandarin prosody in lexical tone recognition and pauses  
**Autoren:** Wu, Shen, XJTLU (2024)

### Zentrale Learnings
- Evaluation zweier kommerzieller APIs: "The overall accuracy of tonal diagnosis reached 80%"
- Fluency-Korrelation mit menschlichen Bewertern nur 0.6 (vs. 0.69-0.81 bei standardisierten Tests)
- "Readily available automated tools may not offer significant advantages for language instructors"
- Tone 3 hat hoechste FRR (30%); Tone 2 hoechste DA (85%)
- Nur 12 Teilnehmer (hauptsaechlich indonesisch)

### Konkret verwendbar fuer unsere Arbeit
- **Hochrelevant fuer RQ3, RQ4, RQ5:** Direkte Vergleichsstudie — "80% Ton-Diagnose-Accuracy" als Benchmark fuer kommerzielle Systeme, gegen die unsere LLM-Ergebnisse positioniert werden koennen
- FRR/FAR/DA-Framework direkt uebertragbar als Evaluationsmetriken (Nordstern §5)
- Tone 3 als schwierigster Ton (30% FRR) — erwartetes Muster fuer unsere Fehleranalyse (RQ5)
- Befund "kommerzielle Tools bieten keinen signifikanten Vorteil" motiviert die Frage: sind multimodale LLMs besser? (RQ4)
- Methodologische Schwaechen (12 Teilnehmer, nur Leseuebungen) zeigen, wo unsere Studie robuster sein kann

### Forschungsluecken
- "Future research should develop more additional metrics in automated rating"
- "Visualization of pitch curves and intonation patterns" empfohlen
- Nur Leseuebungen; keine freien Sprachaeusserungen
- Kleine Stichprobe (12 Teilnehmer, 1.388 Silben)

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** Nur kommerzielle APIs getestet, keine multimodalen LLMs — unsere Arbeit testet Frontier-Foundation-Models
- **Luecke 3:** Kleine L2-Stichprobe — unser Stretch-Goal zielt auf breitere L2-Evaluation
- **Luecke 4:** Nur 2 kommerzielle APIs — unsere Arbeit vergleicht systematisch ~5-8 multimodale Modelle + dedizierte Baselines

---

## 40. PYG-ASR (zhengjie25_interspeech)

**Titel:** Pinyin-Guided Chinese Speech Recognition with Large Language Model  
**Autoren:** Zhengjie, Cheng, CAS (2025)

### Zentrale Learnings
- "LLM-ASR's implicit alignment often fails to capture phonetic relationships in Chinese"
- Zwei Strategien: PYG-ASR1 (sequenziell: erst Pinyin, dann Text) und PYG-ASR2 (iterativ)
- PYGEC: Chain-of-Thought Error Correction mit Text-LLM ohne Finetuning
- PYGEC-CB (Contextual Biasing): 49.2% CER-Reduktion fuer Bias-Phrasen
- Gesamt: 25% CER-Reduktion auf AISHELL-1

### Konkret verwendbar fuer unsere Arbeit
- **Zentral fuer RQ1 und RQ2:** Befund "implizites Alignment erfasst phonetische Beziehungen nicht" direkt relevant — wenn dedizierte LLM-ASR Pinyin schlecht generiert, koennten multimodale LLMs dasselbe Problem haben
- PYG-ASR-Strategie (erst Pinyin, dann Text) als Prompt-Inspiration: wir koennten LLMs explizit auffordern, zuerst Pinyin zu generieren
- PYGEC (Chain-of-Thought ohne Finetuning) als methodische Anregung fuer unsere Prompt-Strategie
- Pinyin-Matching fuer Contextual Biasing relevant fuer die Diskussion, ob Kontextinformation die Ton-Accuracy verbessert

### Forschungsluecken
- "For future works, we intend to conduct larger-scale experiments and explore a more profound integration of Pinyin"
- Nur AISHELL-1; keine Generalisierung
- PYGEC erfordert zusaetzlichen Text-LLM (Latenz)
- Rolle der Toene in Pinyin nicht isoliert evaluiert

### Welche Luecken adressiert meine Arbeit?
- **Luecke 1:** PYG-ASR evaluiert nur CER — unsere Arbeit geht auf Phonem-/Ton-Granularitaet
- **Luecke 2:** "Rolle der Toene in Pinyin nicht isoliert evaluiert" — unsere Arbeit macht genau das mit TER als separater Metrik
- **Luecke 4:** PYG-ASR ist ein Einzelsystem — unsere Arbeit vergleicht systematisch mehrere multimodale Modelle

---

# Zusammenfassungstabelle

| # | Paper | Zentrale Learnings (Kurzfassung) | Konkret verwendbar fuer unsere Arbeit | Forschungsluecken (Kurzfassung) | Adressierte Luecken |
|---|-------|----------------------------------|--------------------------------------|---------------------------------|---------------------|
| 1 | Seed-ASR (ByteDance, 2024) | LLM-ASR uebertrifft E2E; Scaling Law; Kontext +15% Recall | CER-Benchmarkwerte als Referenz (RQ4); Architektur-Kontext §2.4.1 | Multi-Task; Langform; mehr Sprachen | L1 (nur WER/CER), L2 (keine Toene), L4 (Einzelsystem) |
| 2 | Baima ASR (Chirkova+, 2025) | IPA vs. Pinyin; Toene schwierigster Teil; LM hilft | Tonal CER-Methodik (RQ2c/RQ5); Transkriptionssystem-Wahl stuetzt Pinyin-Entscheidung | Nur 3 Sprecher; Tone Sandhi | L1 (kein multimod. LLM), L4 (kein FM-Vergleich) |
| 3 | AI Index 2026 (Stanford) | Benchmark-Konvergenz; "Jagged Intelligence"; Qualitaetsprobleme | "Jagged Intelligence" als Motivation; Benchmark-Qualitaet fuer Methodik | Speech/Audio-Benchmarks fehlen | L1 (fehlende Speech-Benchmarks), L4 (Konvergenz-Pruefung) |
| 4 | AudioBench (A*STAR, 2025) | Kein Modell dominant; Prompt-Sensitivitaet; Kaskade oft besser | Prompt-Sensitivitaet fuer Prompt-Design; Whisper-Baseline-Motivation (RQ4) | Nur Englisch; keine Multilingualitaet | L1 (nur Englisch), L2 (keine Toene) |
| 5 | PY-GEC (Huawei, 2024) | Pinyin verbessert Korrektur; Multitask aligniert Features | Pinyin-Repraesentierung bestaetigt; Future Work fordert multimodale LLMs — genau unsere Arbeit | Nur LLaMA-3-8B; keine multimodalen LLMs | L1+L4 (Extension auf multimod. LLMs), L2 (Toene nicht separat) |
| 6 | FireRedASR2S (Xiaohongshu, 2026) | All-in-One SOTA; Datenskalierung; hierarchische LID | CER-Referenzwerte (2.89%); potenzielle Baseline (RQ4) | Kein Streaming; Code-Switching | L1 (nur CER), L2 (keine Toene), L4 (kein LLM-Vergleich) |
| 7 | ASR DL Survey (Ahlawat+, 2025) | DNN: WER <5%; SSL transformativ; E2E vereinfacht | Architektur-Ueberblick §2.2; Metriken-Begruendung (PER+TER+CER) | Low-Resource; Akzente; Domain-ASR | L1 (nur WER/CER), L2 (keine Toene) |
| 8 | Pitch-Aware RNN-T (Wang+, 2024) | Pitch Fusion +3% PER; Training Native → Eval L2; Mel-F0 | **Baseline RQ4**; MDD-Metriken (PER/FRR/FAR/DER); F0-Methodik; LATIC-Dataset | Nur 4 L2-Sprecher; kein SLM-Vergleich | L3 (nur 4 L2), L4 (kein multimod. LLM-Vergleich) |
| 9 | Tone-syllable synchrony (Kang+, 2024) | CVT synchron (Bayes >200); Silbengrenze 125ms frueher | Phonetische Grundlage §2.2; stuetzt Silbenebene-Evaluation (Tone Perfect) | Nur 1 Sprecherin; keine L2 | L3 (keine L2); phonetischer Hintergrund fuer L2 |
| 10 | Sparks of LAMs (Latif+, 2023) | Erster LAM-Survey; Paralinguistik "untapped"; Tokenisation kritisch | Prosodie/Toene als "untapped" = direkte Motivation; Taxonomie §2.3 | Paralinguistik; Tokenisation; Halluzinationen | L1+L2 (Toene als "untapped"), L4 (systematische Eval gefordert) |
| 11 | ST SFM+LLM (Gaido+, 2024) | 5 Bausteine; kein Konsens ueber SFM; Length Adapter essentiell | 5-Bausteine-Schema §2.3; "kein Konsens" stuetzt Vergleich (RQ4) | Standards fehlen; Prosodie wenig untersucht | L2 (Prosodie/Toene wenig untersucht), L4 (kein fairer Vergleich) |
| 12 | LLMs Meet Speech (Yang+, 2025) | 3 Integrationsansaetze; Text-basiert verliert Prosodie | Taxonomie §2.3; "Prosodie-Verlust" motiviert RQ1 vs. RQ2 | Meist nur Englisch; kein fairer Vergleich | L1 (nur Englisch), L4 (kein unified setting) |
| 13 | Recent Advances SLMs (Cui+, 2025) | Kaskade: Info-Loss + Latenz + kumulative Fehler | E2E vs. Kaskade fuer RQ4-Diskussion; Taxonomie §2.3 | Komponentenauswahl unklar; Sicherheit | L1 (ob nativ multimodal besser), L2 (Toene als "lost info") |
| 14 | Survey Speech LLMs (Peng+, 2025) | Instruction Sensitivity (WER 4.61→48.48%); Reasoning Degradation | **Kritisch fuer Methodik:** Prompt-Design; IFR-Metrik; Taxonomie | Multitask; RL-Alignment; Multi-Speaker | L1 (nur WER), L2 (temporal alignment) |
| 15 | MMAU (Sakshi+, 2024) | Bestes Modell nur 53%; 27 Skills; Audio-Reasoning defizitaer | "Tonal transcription" als fehlender Skill; Modell-Overlap mit unserer Auswahl | Kein tonales Reasoning | L1+L2 (kein tonales Reasoning), L4 (Modelle ueberlappen) |
| 16 | Dynamic-SUPERB 2 (Huang+, 2024) | 180 Tasks; Whisper-LLaMA stark; kein universelles Modell | "ASR verwirft Paralinguistik" stuetzt Hypothese; Task fehlt | Mandarin unterrepraesentiert; keine Generation | L1 (Mandarin fehlt), L2 (keine Ton-Tasks), L4 (kein universelles Modell) |
| 17 | ASR-EC Bench (Wei+, 2024) | Multimodal am besten; Prompting ineffektiv; Over-Correction | Multimodale Augmentation; Over-Correction = target-leakage-Warnung | Keine Ton-Fehler; kein SLM-Vergleich | L2 (keine Ton-Fehler), L4 (kein multimod. LLM) |
| 18 | PERL (Liang+, 2025) | Pinyin unternutzt; 29% CER-Reduktion; 3ms Latenz | Pinyin als wertvolle Repraesentierung bestaetigt; Latenz-Diskussion | Nur Chinesisch; synthetische Daten; kein E2E | L1 (kein multimod. LLM), L2 (Toene nicht separat) |
| 19 | VoxEval (Cui+, 2025) | E2E-Benchmark; SLMs unter Zufall; Audio-Sensitivitaet | E2E-Methodik; "unter Zufall" motiviert unsere Frage | Nur Englisch; keine Aussprache | L1 (nur Englisch), L2 (keine Aussprache/Toene) |
| 20 | FireRedASR (Xiaohongshu, 2025) | SOTA CER 3.05%; Qualitaet > Quantitaet | **Baseline RQ4** (Open Source); CER-Referenz; Korpus-Design | Nur Mandarin CER; kein Streaming | L1 (nur CER), L2 (keine Toene), L3 (keine L2), L4 (Baseline) |
| 21 | Landscape SLMs (Arora+, 2025) | 3 SLM-Kategorien; Kaskade starke Baseline; Adapter zentral | Taxonomie §2.3; 50+ SLM-Tabelle; Kaskade-Baseline stuetzt Whisper (RQ4) | Optimale Repraesentation unklar; wenig Open Source | L1 (Repraesentation fuer Toene?), L4 (kein Vergleich auf Ton-Tasks) |
| 22 | Kimi-Audio (Moonshot, 2025) | Open-Source Audio FM; hybride Tokenisierung; Paralinguistik vernachlaessigt | **Testmodell-Kandidat**; "Toene vernachlaessigt" = direkte Motivation | Paralinguistik; ASR/TTS-Ceiling; keine L2 | L1+L2 (Toene vernachlaessigt), L3 (keine L2), L4 (Testmodell) |
| 23 | Holistic Eval LALMs (Yang+, 2026) | 4D-Taxonomie; domain-spezialisiert; LLM-as-Judge | Taxonomie: unsere Aufgabe als "General Auditory Awareness → tonal"; Benchmark-Luecke bestaetigt | Datenkontamination; Low-Resource | L1 (sublexikalisch uebersehen), L4 (systematische Eval gefordert) |
| 24 | ZIPA (Zhu+, 2025) | 64M > 300M; IPAPack++ 88 Sprachen; Variation geglaettet | **Baseline RQ4**; PFER-Metrik; "Glaettung" = auto-correction-Problem | Sprachen-Bias; G2P-Limitation | L1 (keine Toene), L2 (Toene nicht separat), L4 (kein multimod. LLM) |
| 25 | WildSpeech-Bench (Tencent, 2025) | Acoustic Features ignoriert; Query-aware Evaluation | Query-aware Methodik; "acoustic features ignoriert" = Motivation | Nur Englisch; Single-Turn | L1 (nur Englisch), L2 (akustische Features/Toene ignoriert) |
| 26 | ContextASR-Bench (Alibaba, 2025) | LALMs besser bei NE; Halluzinationen; 3 Kontextmodi | Halluzination bei Ton-Transkription; LLM-Weltwissen fuer RQ1 | Nur TTS; Halluzinationen | L1 (nur Wortebene), L2 (keine Toene) |
| 27 | Step-Audio 2 (StepFun, 2025) | E2E LLM; SOTA MMAU 78.0; Paralinguistik 83% | **Testmodell-Kandidat** (stark bei Paralinguistik); Performance-Referenz | Kein Ton-Assessment; keine L2 | L1+L2 (keine Ton-Eval), L3 (keine L2), L4 (Testmodell) |
| 28 | TELEVAL (TeleAI, 2026) | Nutzer-zentriert; Content + Interactional; Cues erkannt aber nicht genutzt | "Cues erkannt, nicht genutzt" relevant fuer Ton-Verarbeitung; Benchmark-Kritik | Synthetische Daten; Multi-Turn | L1 (nicht sublexikalisch), L2 (keine Toene trotz Chinese) |
| 29 | VocalBench-zh (SJTU, 2025) | Erstes Mandarin-S2S; LLM-Backbone entscheidend; Zeichenstruktur-Defizit | **Hochrelevant:** Pinyin-Schwaeche; 14 Modelle als Referenz; LLM-Backbone-Befund fuer Diskussion | Synthetische Daten; Dialekte | L1 (nur Satzebene), L2 (keine Ton-Eval), L3 (synth. Daten) |
| 30 | Qwen3-ASR (Qwen, 2026) | All-in-One 52 Sprachen; ForcedAligner; RL fuer Robustheit | **Baseline RQ4** (Open Source); ForcedAligner fuer Ton-Alignment | Kein Tone Assessment; keine L2 | L1 (nur CER), L2 (keine Toene), L3 (keine L2), L4 (Baseline) |
| 31 | Survey LALMs Trust (Luo+, 2026) | Modality Neglect; Cross-Modal Gap; 6 Trustworthiness-Saeulen | **"Modality Neglect" = target-leakage**; LALM-Tabelle §2.3 | Kein Safety Leaderboard; keine tonalen Sprachen | L1 (Modality Neglect bei Toenen?), L2 (keine Toene), L3 (keine L2) |
| 32 | Zipformer Mandarin (Du+, 2025) | WER 1.92%; 97% Phoneme >95%; Verwechslungsmatrix | **Hochrelevant RQ5:** Verwechslungsmatrix als Referenz; **Baseline RQ4**; Dataset-Methodik | Keine L2; keine Toene; kein SSL | L1 (keine Toene), L2 (Toene fehlen), L3 (keine L2), L4 (kein LLM) |
| 33 | SCCM (Chen+, 2025) | Pinyin-Ensemble; Pinyin→Initials/Finals→Characters; 45.7% CERR | Pinyin als Zwischenformat bestaetigt; Polyphone Zeichen fuer RQ1 | Nur AISHELL-1; keine Toene; kein LLM | L1 (kein LLM), L2 (keine Toene), L4 (kein Vergleich) |
| 34 | L2 Mandarin Tones Review (Huang+, 2026) | T2/T3-Verwechslung persistent; 4 Straenge; 0% QA | **Hochrelevant RQ5:** T2/T3-Muster; 4-Strand §2.2/2.4.2; XAI-Methoden; QA-Motivation | Keine multimod. LLMs; Classroom begrenzt; Reproduzierbarkeit | L1 (keine LLMs), L3 (Classroom begrenzt), L4 (kein FM-Vergleich) |
| 35 | ML Tone Recognition (Zou+, 2025) | DL 88.8% vs. 83.1% trad.; CNN 99.16% isoliert; GOT-Framework | **Hochrelevant §2.4.2/2.5:** Taxonomie; 99.16% als Referenz; GOT; Soft-Labels RQ3 | Dataset-Diversitaet; Prosodie; klinische Korpora | L1 (keine multimod. LLMs), L4 (kein FM-Vergleich) |
| 36 | ASR Tonal Languages (Kaur+, 2021) | F0 primaer; MFCC+HMM dominant; Asien gut erforscht | Historischer Kontext §2.2.1; F0 als primaerer Cue bestaetigt | Benchmark-Mangel; pre-Transformer | L1 (pre-LLM), L2 (keine separate Ton-Achse), L4 (kein moderner Vergleich) |
| 37 | ResNet Siamese Tones (Bu+, 2025) | Siamese+ResNet; 2D-Features; Ton 3 am besten unterschieden | T-value Normalisierung; Paar-Diskrepanz-Methode; L2-Korpus-Referenz | Vereinfachte Features; keine E2E; kleiner Korpus | L1 (kein multimod. LLM), L3 (nur tibetische L2), L4 (kein FM-Vergleich) |
| 38 | MSPB (Wang+, 2025) | 8 prosodische Tasks; Modelle nutzen Kontext statt Prosodie | **Hochrelevant:** "Kontext statt Prosodie" = target-leakage; Benchmark-Vorbild; Modell-Overlap | Keine T1-T4; nur Single-Round | L1+L2 (keine T1-T4-Eval), L4 (wir ergaenzen Ton-Dimension) |
| 39 | L2 Mandarin Rating (Wu+, 2024) | 80% Ton-Accuracy; Tone 3 FRR 30%; kommerzielle APIs limitiert | **Hochrelevant RQ3-5:** 80% als Referenz; FRR/FAR/DA; Tone 3 schwierigster | Kleine Stichprobe; nur Leseuebungen; wenig Metriken | L1 (keine multimod. LLMs), L3 (kleine L2-Stichprobe), L4 (nur 2 APIs) |
| 40 | PYG-ASR (Zhengjie+, 2025) | Pinyin-guided LLM-ASR; implizites Alignment scheitert; PYGEC ohne Finetuning | **Zentral RQ1/RQ2:** "Alignment scheitert bei Phonetik" direkt relevant; Prompt-Strategie | Nur AISHELL-1; Toene nicht isoliert; Latenz | L1 (nur CER), L2 (Toene nicht isoliert), L4 (Einzelsystem) |

**Legende:** L1 = Luecke 1 (Phonem-/Ton-Granularitaet), L2 = Luecke 2 (Toene als separate Achse), L3 = Luecke 3 (L2-Sprecher), L4 = Luecke 4 (systematischer Head-to-Head)
