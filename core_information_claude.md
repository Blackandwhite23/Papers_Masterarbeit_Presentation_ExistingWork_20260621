# Core Information: Literaturanalyse aller Paper

> Automatisch erstellt am 2026-06-22. Fuer jedes Paper werden zentrale Learnings, konkrete Verwendbarkeit, Forschungsluecken und deren Relevanz fuer die eigene Arbeit dokumentiert.

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
- AcLLM-Framework als Referenzarchitektur fuer LLM-basierte Spracherkennung
- Context-SFT als Vorlage fuer kontextbewusste ASR
- Scaling-Law-Befunde fuer Audio-Encoder als Orientierung bei Modellgroessenentscheidungen
- LUISE-Pretraining (Large-scale Unsupervised Iterative Speech Encoder) als SSL-Ansatz

### Forschungsluecken
- "In future, we will focus on extending Seed-ASR's ability to deal with multiple tasks within a single model" (Conclusion)
- Langform-Erkennung auf max. 5 Minuten beschraenkt
- Derzeit nur 8+ Sprachen; Erweiterung auf 40+ geplant
- Keine systematische Untersuchung von Low-Resource-Sprachen/Dialekten ausserhalb Chinesisch

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Methodik fuer ASR-Evaluation bei tonalen Sprachen (Tonal CER/WER)
- Erkenntnis, dass groessere vortrainierte Modelle nicht zwingend besser sind
- KenLM als effektive, leichtgewichtige Methode zur Verbesserung der Tonerkennung

### Forschungsluecken
- Nur 3 Sprecher und 186 Minuten Daten
- Geliehene Toene (55, 35) werden extrem schlecht erkannt (bis 100% Fehlerrate)
- Keine multilinguale Cross-linguale Transfer-Learning-Analyse
- Tone Sandhi in polysyllabischen Woertern "remains largely understudied"

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Ueberblick ueber aktuellen Stand der AI-Benchmarks als Kontextkapitel
- Benchmark-Konvergenz als Argument fuer domain-spezifische Evaluation
- Diskussion zu Benchmark-Qualitaetsproblemen als Motivation fuer sorgfaeltige Evaluationsmethodik

### Forschungsluecken
- "Benchmarks for multiagent coordination, human-AI interaction, tool-using agents remain underdeveloped"
- Fehlende standardisierte Benchmarks fuer Speech/Audio im Vergleich zu Text/Vision
- "The field should adopt centaur evaluations" (Human-AI-Kollaborationsbenchmarks fehlen)

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- AudioBench als Referenzbenchmark fuer AudioLLM-Evaluation
- Model-as-Judge-Methodik mit Llama-3-70B als GPT-4-Ersatz
- Robustheitstests mit multiplen Prompts als Evaluationsstrategie

### Forschungsluecken
- "The current AudioBench exclusively includes English datasets" (Limitations)
- "Multilingual Capabilities, Code-Switching, and Dialects" fehlen komplett
- "Long Audio Processing and Understanding" bleibt offen
- "Multi-round Query Handling" limitiert bei Open-Source-Modellen

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Pinyin-gestuetzte Fehlerkorrektur als Vorlage fuer phonetische Repraesentationen
- Multitask-Training-Strategie zur Verbesserung des phonetischen Verstaendnisses
- Synthetische Fehlererzeugung (homophon-basiert) als dateneffiziente Trainingsmethode
- Attention- und PCA-Analyse als Interpretationsmethode

### Forschungsluecken
- "For future research, we aim to extend our experiments to larger-scale LLMs and multi-modal LLMs" (Conclusion)
- Nur LLaMA-3-8B-Chinese getestet; Generalisierung unklar
- Nur 1-best Hypothesis; N-best koennte Verbesserungen bringen
- Keine Untersuchung fuer andere tonale Sprachen

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- All-in-One-Architektur als Referenz fuer industrielle ASR-Systeme
- Hierarchische LID fuer mehrsprachige/dialektale Szenarien
- Open-Source-Modelle als potenzielle Baseline

### Forschungsluecken
- "Future work will focus on further improving performance and expanding support for more languages" (Conclusion)
- Keine Evaluation der End-to-End-Pipeline-Fehlerfortpflanzung
- Kein Streaming-ASR evaluiert
- Code-Switching nicht systematisch evaluiert

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Systematische Uebersicht ueber ASR-Architekturen als theoretische Grundlage
- Vergleich von Evaluationsmetriken (WER, CER, PER) und Datasets
- Diskussion von Data Augmentation-Techniken

### Forschungsluecken
- Grosse Diskrepanz zwischen High- und Low-Resource-Sprachen (WER 1.4% Englisch vs. 20.6% Odiya)
- Akzent-Robustheit bleibt problematisch
- Domain-spezifisches ASR (Healthcare, Education) unterentwickelt

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Pitch Fusion Block-Architektur direkt anwendbar auf tonbewusste Aussprachebewertung
- Ansatz des Trainings auf Native-Speaker-Daten bei L2-Datenmangel
- F0-Extraktionsmethode (DIO/WORLD) mit Mel-Skalierung und Diskretisierung
- Metriken PER, FRR, FAR, DER fuer MDD-Evaluation

### Forschungsluecken
- Nur 4 Non-Native-Sprecher im LATIC-Dataset evaluiert
- Keine Evaluation suprasegmentaler Features jenseits von Ton
- Nur Mandarin getestet; Transferierbarkeit auf andere tonale Sprachen nicht untersucht
- Kein Vergleich mit SLM-basierten Ansaetzen

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Fundamental fuer Verstaendnis der Mandarin-Tonproduktion
- Minimal Contrast Paradigm (GAMMs + Bayes-Faktor) als Gold-Standard fuer tonale Analyse
- Implikationen fuer Target Approximation Modelle und F0-Kontur-Modellierung in ASR/TTS

### Forschungsluecken
- Basiert auf nur einer Sprecherin -- Generalisierung noetig
- "Even articulatory divergence is just the result of muscle activities" -- praezisere Messungen noetig
- Revision des Target Approximation Models moeglicherweise erforderlich
- Keine L2-Sprecher untersucht

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Umfassende Taxonomie der Large Audio Models als Grundlage
- Identifizierte Herausforderungen: Data Issues, Tokenisation, Computational Cost, Paralinguistic Information
- Timeline und Architektur-Uebersichten als Referenz

### Forschungsluecken
- "Understanding paralinguistic information" -- Emotionen und Prosodie "largely untapped and understudied"
- "Tokenisation for speech/audio data processing requires further attention"
- Halluzinationen auch bei Large Audio Models
- Computational Cost enorm (AudioPaLM 530B: ~$530M)

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- 5-Bausteine-Framework als analytisches Schema fuer SFM+LLM-Architekturen
- Empfehlung: COMET zusaetzlich zu BLEU als Evaluationsmetrik
- CoVoST2 als Standard-Benchmark

### Forschungsluecken
- Fehlende standardisierte Trainingseinstellungen
- In-Context Learning: "The only attempt in ST has not been similarly successful"
- Inferenz-Effizienz: SFM+LLM vs. traditionelle Modelle (100-300M Parameter)
- Nutzung prosodischer Information wenig untersucht

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Drei-Wege-Taxonomie als Kategorisierungsrahmen
- Detaillierte Uebersicht der Modality-Adaptation-Strategien
- Computational Trade-offs der verschiedenen Ansaetze

### Forschungsluecken
- "There is a notable gap in comparing the different integration approaches under a unified setting"
- "Most existing speech-LLM models are designed to handle only English"
- "Speech-based applications more often require real-time processing"
- "The alignment of text and speech data remains underexplored"

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Taxonomie der SpeechLM-Komponenten (Speech Tokenizer, Language Model, Vocoder)
- Argumentation End-to-End vs. kaskadierte Pipelines
- Systematische Evaluationsmethoden-Klassifikation

### Forschungsluecken
- "There remains a significant gap in understanding the advantages and disadvantages of different component selections"
- End-to-End-Training mit Gradientenfluss von Vocoder zu Tokenizer wenig erforscht
- Echtzeit-Sprachgenerierung "remains underexplored"
- Sicherheitsfragen "not thoroughly investigated"

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Dreidimensionale Taxonomie als theoretisches Framework
- Instruction-Sensitivity-Analyse und IFR-Metrik
- Vergleichstabellen fuer Modell-Performance (ASR, ST, ER, SEC)

### Forschungsluecken
- "Speech LLMs still have a long way to go in achieving true multitask generalizability"
- "Preference alignment based on reinforcement learning has received limited attention"
- "Managing speaker turns in multi-party conversations remain open problems"
- "Temporal alignment of speech segments has been relatively underexplored"

### Welche Luecken adressiert meine Arbeit?
unbekannt

---

## 15. MMAU (2410.19168v1)

**Titel:** MMAU: A Massive Multi-Task Audio Understanding and Reasoning Benchmark  
**Autoren:** Sakshi et al., U Maryland/Adobe (2024)

### Zentrale Learnings
- Erster umfassender Benchmark fuer multimodales Audio-Reasoning: 10.000 QA-Paare ueber Speech, Sound, Music
- "Even the most advanced Gemini Pro v1.5 achieves only 52.97% accuracy" -- massive Defizite bei Audio-Reasoning
- 27 distinct Skills, darunter "multi-speaker role mapping, emotional shift detection, temporal acoustic event analysis"

### Konkret verwendbar fuer unsere Arbeit
- Skill-Taxonomie als Referenzrahmen fuer Audio-LLM-Evaluation
- Identifizierte LALMs und Architekturen als Baseline-Modelle

### Forschungsluecken
- Kein spezifisches prosodisches oder tonales Reasoning getestet
- LALMs haben sich "in isolation" mit Fokus auf einzelne Domaenen entwickelt

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Task-Taxonomie als Referenz (Content, Semantics, Speaker, Paralinguistics, etc.)
- Erkenntnis: kein universelles Modell -- Motivation fuer spezialisierte Ansaetze
- LLM-as-Judge Evaluationsmethodik

### Forschungsluecken
- "It lacks comprehensive speech-generation tasks"
- Mandarin und andere Sprachen unterrepraesentiert
- Ca. 7% der Audiodateien laenger als 30s -- Whisper-Limitation

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Multimodale Augmentation als vielversprechender Ansatz fuer Post-Processing
- LoRA-Finetuning fuer domain-spezifische ASR-Fehlerkorrektur
- Fehlertyp-Klassifikation (Substitution, Deletion, Insertion)

### Forschungsluecken
- "LLMs still face challenges in correcting errors requiring deep contextual understanding"
- Keine tonspezifischen Fehler untersucht (obwohl Mandarin)
- Kein Vergleich mit neueren SLM-Modellen

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Phonetische Embedding-Methodik (Pinyin Encoder)
- N-Best-Korrektur-Pipeline mit Length Predictor + Semantic+Phonetic Fusion
- DoAD-Datensatz als Referenz fuer domain-spezifische ASR-Evaluation

### Forschungsluecken
- Nur Chinesisch evaluiert; Uebertragbarkeit unklar
- DoAD basiert auf synthetischer Sprache (TTS + Rauschen)
- Keine Integration in End-to-End-Systeme
- Keine Evaluation auf realen Sprachdaten

### Welche Luecken adressiert meine Arbeit?
unbekannt

---

## 19. VoxEval (2501.04962v4)

**Titel:** VoxEval: Benchmarking Knowledge Understanding of End-to-End Spoken Language Models  
**Autoren:** Cui et al., CUHK (2025)

### Zentrale Learnings
- Erster End-to-End (Speech-in, Speech-out) Benchmark: 13.938 QA-Paare, 56 Faecher, 26 Audio-Bedingungen
- Aktuelle SLMs: "low accuracies often below random guessing, sensitivity to audio conditions"
- Mathematisches Reasoning in gesprochener Form besonders herausfordernd

### Konkret verwendbar fuer unsere Arbeit
- Methodik zur Evaluation unter diversen Audio-Bedingungen (Noise, Sprechstile)
- Erkenntnis, dass End-to-End-Evaluation andere Ergebnisse als Speech-to-Text liefert

### Forschungsluecken
- Nur Englisch; keine tonalen Sprachen
- Keine Aussprachebewertung oder MDD
- "Other critical aspects such as fairness, toxicity, hallucination" nicht abgedeckt

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Encoder-Adapter-LLM Architektur mit LoRA
- Datenqualitaet > Datenquantitaet als Designprinzip
- Benchmark-Vergleichswerte fuer Mandarin ASR

### Forschungsluecken
- Primaer Mandarin-fokussiert
- Kein kontextuelles/domain-spezifisches ASR
- Kein Streaming/Real-Time ASR evaluiert

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Umfassende Taxonomie als Rahmen fuer Einordnung
- Architekturformulierung (Encoder -> Adapter -> Sequence Model -> Decoder)
- Modell-Tabelle (50+ SLMs) als Referenz

### Forschungsluecken
- "The optimal representation of speech within SLMs remains unclear"
- "There is a lack of public high-quality training data"
- "Very few SLMs are fully open-source"
- "Scaling studies for SLMs are needed"
- "Improving inclusiveness and safety" fuer unterrepraesentierte Sprachen/Dialekte

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Audio-Foundation-Model fuer Downstream-Tasks (Open Source)
- Hybride Tokenisierung als Architektur-Inspiration
- Datenpipeline als Referenz

### Forschungsluecken
- "Text transcription neglects important information such as paralanguage (emotion, style, timbre, tone)"
- "Current audio foundation models rely heavily on ASR and TTS -- can hardly achieve performance far beyond the ceiling of ASR/TTS"
- Kein Fokus auf Mandarin-Ton-Bewertung oder L2-Pronunciation Assessment

### Welche Luecken adressiert meine Arbeit?
unbekannt

---

## 23. Holistic Evaluation LALMs (2505.15957v4)

**Titel:** Towards Holistic Evaluation of Large Audio-Language Models: A Comprehensive Survey  
**Autoren:** Yang, Ho, Lee, NTU (2026)

### Zentrale Learnings
- Vierdimensionale Taxonomie: (1) General Auditory Awareness, (2) Knowledge and Reasoning, (3) Dialogue, (4) Fairness/Safety/Trustworthiness
- "Different LALMs excel in different domains, each with their own strengths"
- LLM-as-a-judge als "emerging trend" fuer skalierbare Evaluation

### Konkret verwendbar fuer unsere Arbeit
- Vierdimensionale Taxonomie als Rahmen fuer eigene Arbeit
- Ueberblick ueber existierende Benchmarks zur Auswahl relevanter Metriken

### Forschungsluecken
- Datenkontamination: "Models may have seen these datasets during training"
- "Many overlook crucial aspects such as low-resource languages and code-switching"
- "Safety should cover auditory comfort, not just content harmlessness"
- Personalisierung: "LALMs must become familiar with users' voice characteristics"

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- ZIPA als Phone-Recognizer fuer Preprocessing-Pipelines
- IPAPack++ als Trainingsressource
- PFER (Phonetic Feature Error Rate) als alternative Metrik

### Forschungsluecken
- Sprachenverteilung stark verzerrt (Bias zu High-Resource-Sprachen)
- G2P-Trainingsdaten bilden soziophonische Variation nicht ab
- "Simply scaling up G2P might not solve phone recognition"

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Query-aware Evaluierungsmethode mit Checklisten
- Kategorisierung paralinguistischer Phaenomene
- Noise-Augmentation-Strategie

### Forschungsluecken
- "Our current benchmark focuses solely on single-turn dialogue evaluation"
- "The lack of actual in-the-wild speech data"
- Nur Englisch; multilinguale Abdeckung fehlt

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Evaluierungsmodi als methodisches Vorbild fuer kontextuelle ASR
- NE-WER/NE-FNR als spezifischere Metriken

### Forschungsluecken
- "Current LALMs still struggle in contextual ASR"
- Basiert auf TTS, keine realen Aufnahmen
- Halluzinationsproblem ungeloest

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- RAG-Integration als Architektur-Pattern
- StepEval-Audio-Paralinguistic als Evaluierungsrahmen

### Forschungsluecken
- Kein Fokus auf Tonbewertung/L2-Pronunciation Assessment
- Keine Evaluation auf tonspezifischen Benchmarks

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Unterscheidung Content Fulfillment vs. Interactional Appropriateness als Evaluationsdimensionen
- Kritik an bestehenden Benchmarks (Modality Leakage, unrealistische Formate)

### Forschungsluecken
- Synthetische Audio-Daten, keine natuerliche Sprache
- Multi-Turn-Interaktional-Angemessenheit nicht bewertet
- "LLM-Judge-Korrelation" nicht explizit analysiert

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Benchmark-Taxonomie (Semantic, Acoustic, Chat, Robust) als Referenz
- Pinyin/Zeichenstruktur-Schwaeche als Motivation fuer Pinyin-basierte Ansaetze
- 13 Beobachtungen zu Modellschwaechen als Motivationsrahmen

### Forschungsluecken
- "Relies exclusively on synthetic data" -- keine menschlichen Aufnahmen
- Dialekt-Evaluation fehlt
- Paralinguistische Kontrolle und emotionale Generierung bleiben offen

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Baseline-ASR fuer Mandarin (Open Source, Apache 2.0)
- ForcedAligner fuer Timestamp-Generierung bei Ton-Alignment
- Dialekt-Unterstuetzung (22 chinesische Dialekte)

### Forschungsluecken
- Kein Fokus auf Tone Assessment/Pronunciation Evaluation
- Keine Evaluation fuer L2-Learner
- Forced Alignment auf Wort-Ebene, nicht Ton-Level

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Trustworthiness-Framework als Bewertungskriterium
- "Modality Neglect" als Motivation fuer tonspezifische Audio-Verarbeitung
- Umfassende LALM-Architektur-Tabelle (2022-2026)

### Forschungsluecken
- "Community lacks a comprehensive Safety Leaderboard"
- "Audio-aware alignment" fehlt -- RLHF mit multimodalen Praeferenzsignalen noetig
- Kein Fokus auf tonale Sprachen oder L2-Szenarien

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Leichtgewichtige Phonem-Erkennungs-Architektur fuer Real-Time-Anwendungen
- Verwechslungsmatrix fuer Mandarin-Phoneme
- AISHELL1-PHONEME Dataset-Konstruktionsmethodik

### Forschungsluecken
- "Scarcity of Chinese reading evaluation datasets"
- Keine Evaluation auf L2-Sprecher
- Keine Toninformation (F0/Pitch) integriert
- Kein Self-Supervised Pre-training (HuBERT, wav2vec2.0)

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Dreistufige Dekodierarchitektur (Pinyin -> Initials/Finals -> Characters) als Referenz
- Pinyin-Ensemble-Konzept zur Fehlerreduktion
- Joint-Loss-Funktion als Vorlage fuer Multi-Task-Optimierung

### Forschungsluecken
- Nur AISHELL-1 evaluiert
- Keine Beruecksichtigung von Toenen
- Kein LLM-Einsatz
- Kein Streaming/Real-Time

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- 4-Strand-Systematik als Strukturierungsrahmen
- Parameter-aligned Feedback: Slope, Turning-Point-Timing, F0-Range
- Explainability-Methoden: Integrated Gradients, SHAP, Counterfactual Explanations
- GNN-basierte Ansaetze fuer relationale Modellierung

### Forschungsluecken
- "Classroom evidence remains limited relative to laboratory studies"
- "Many studies measure perception and production separately"
- "85.2% of studies had unclear transparency/reproducibility"
- "0% had adequate measurement reliability reporting"

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Umfassende Taxonomie der ML-Ansaetze als Literaturgrundlage
- Modell-Empfehlungen je nach Aufgabe (Table 3)
- Soft-Label-Ansatz fuer L2-Bewertung
- 6 praktische Empfehlungen fuer Modellentwicklung

### Forschungsluecken
- "Lack of diverse datasets, weak prosody and dialect modeling, insufficient validation rigor"
- "Most models trained on young adult male speakers"
- "Speech corpora for clinical populations remain scarce"
- "Existing methods are highly Mandarin-specific"

### Welche Luecken adressiert meine Arbeit?
unbekannt

---

## 36. ASR Tonal Languages Survey (s11831-020-09414-4)

**Titel:** Automatic Speech Recognition System for Tonal Languages: State-of-the-Art Survey  
**Autoren:** Kaur, Singh, Kadyan (2021)

### Zentrale Learnings
- Umfassender Survey ueber ASR fuer tonale Sprachen weltweit (Asien, Indo-Europa, Afrika)
- F0 als primaerer Cue; MFCC und HMM am haeufigsten verwendet
- "Lot of work for Asian tonal languages (Chinese, Thai, Vietnamese) but little for Mizo, Bodo, Indo-European tonal languages"

### Konkret verwendbar fuer unsere Arbeit
- Historischer Kontext ueber traditionelle ASR-Ansaetze fuer tonale Sprachen
- Klassifikation tonaler Sprachen und ihrer phonologischen Features
- "Better results when phones defined as syllable rather than individual letters" -- relevant fuer Mandarin

### Forschungsluecken
- "Lack of benchmark speech corpus for majority languages"
- "ASR for tonal languages of American and Austral-Asia not studied"
- Keine LLM/Deep-Learning-Beruecksichtigung (pre-Transformer-Aera)

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Five-Degree Tone Scale Normalisierung (T-value) als Feature-Engineering
- Kontrastive Loss-Funktion fuer Ton-Paarvergleiche
- Ansatz: Tonbewertung als Paar-Diskrepanz statt Klassifikation

### Forschungsluecken
- "Method relies on computed tone features, which may oversimplify tone characteristics"
- Keine End-to-End-Verarbeitung; erfordert Silben-Segmentierung und F0-Extraktion
- Beschraenkt auf Silbenebene; keine Satz-/Diskursebene
- Korpus klein und auf tibetische L2-Sprecher beschraenkt

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- MSPB als Vorbild/Vergleichsbenchmark fuer Mandarin-Evaluation
- 8-Task-Taxonomie (prosodic disambiguation, emotional prosody etc.)
- Getestete Modelle und Performance als Baseline (GPT-4o, Gemini, Qwen2-Audio, GLM-4-Voice)

### Forschungsluecken
- "Future work should explore multi-turn conversational settings"
- Keine Tone-spezifische Evaluation (T1-T4) -- nur suprasegmentale Prosodie
- Nur Mandarin; multilingualer Benchmark fehlt

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- Evaluationsframework (FRR, FAR, DA) fuer Vergleich automatisiert vs. menschlich
- Tone 3 als schwierigster Ton fuer automatische Systeme
- Erkenntnis: standardisierte Test-Engines nicht auf Klassenzimmer uebertragbar

### Forschungsluecken
- "Future research should develop more additional metrics in automated rating"
- "Visualization of pitch curves and intonation patterns" empfohlen
- Nur Leseuebungen; keine freien Sprachaeusserungen
- Kleine Stichprobe (12 Teilnehmer, 1.388 Silben)

### Welche Luecken adressiert meine Arbeit?
unbekannt

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
- PYG-ASR-Architektur: gleichzeitige Pinyin- und Text-Generierung durch LLM-ASR
- PYGEC als leichtgewichtiges Post-Processing ohne Finetuning
- Contextual Biasing via Pinyin-Matching fuer domain-spezifische Korrektur
- Prompt-Templates als Referenz

### Forschungsluecken
- "For future works, we intend to conduct larger-scale experiments and explore a more profound integration of Pinyin"
- Nur AISHELL-1; keine Generalisierung
- PYGEC erfordert zusaetzlichen Text-LLM (Latenz)
- Rolle der Toene in Pinyin nicht isoliert evaluiert

### Welche Luecken adressiert meine Arbeit?
unbekannt

---

# Zusammenfassungstabelle

| # | Paper | Zentrale Learnings (Kurzfassung) | Konkret verwendbar | Forschungsluecken (Kurzfassung) | Luecken adressiert? |
|---|-------|----------------------------------|--------------------|---------------------------------|---------------------|
| 1 | Seed-ASR (ByteDance, 2024) | LLM-ASR uebertrifft E2E-Modelle; Scaling Law fuer Audio-Encoder; Kontextinformation +15% Recall | AcLLM-Framework; Context-SFT; Scaling-Law-Befunde | Multi-Task in Single Model; Langform-ASR; mehr Sprachen | unbekannt |
| 2 | Baima ASR (Chirkova+, 2025) | IPA vs. Pinyin fuer tonale ASR; Toene schwierigster Teil; LM verbessert Tonerkennung | Tonal CER/WER Methodik; KenLM fuer Tonerkennung | Nur 3 Sprecher; geliehene Toene schlecht erkannt; Tone Sandhi | unbekannt |
| 3 | AI Index 2026 (Stanford) | Benchmarks ueberholt; Modellkonvergenz; US-China-Luecke geschlossen | Benchmark-Kontext; Motivation fuer domain-spezifische Eval | Fehlende Speech/Audio-Benchmarks; Human-AI-Evaluation | unbekannt |
| 4 | AudioBench (A*STAR, 2025) | Kein Modell dominant; Kaskade oft besser; Prompt-Sensitivitaet | Referenzbenchmark; Model-as-Judge; Robustheitstests | Nur Englisch; keine Multilingualitaet; Long Audio | unbekannt |
| 5 | PY-GEC (Huawei, 2024) | Pinyin verbessert ASR-Korrektur; Multitask aligniert Feature-Raeume | Pinyin-Fehlerkorrektur; Multitask-Training; synthetische Fehler | Nur LLaMA-3-8B; nur 1-best; keine anderen tonalen Sprachen | unbekannt |
| 6 | FireRedASR2S (Xiaohongshu, 2026) | All-in-One ASR SOTA; Datenskalierung entscheidend; hierarchische LID | Industrielle Referenzarchitektur; Open Source | Kein Streaming; Code-Switching nicht systematisch | unbekannt |
| 7 | ASR DL Survey (Ahlawat+, 2025) | DNN reduziert WER auf <5%; SSL transformativ; E2E vereinfacht Pipeline | Architektur-Ueberblick; Metriken; Data Augmentation | Low-Resource-Luecke; Akzent-Robustheit; Domain-ASR | unbekannt |
| 8 | Pitch-Aware RNN-T (Wang+, 2024) | Pitch Fusion Block +3% PER; Training auf Native-Daten; Mel-F0 optimal | Pitch-Architektur; Native-Training-Ansatz; F0-Methodik | Nur 4 L2-Sprecher; nur Mandarin; keine SLM-Vergleiche | unbekannt |
| 9 | Tone-syllable synchrony (Kang+, 2024) | CVT vollstaendig synchron (Bayes >200); Silbengrenze 125ms frueher | Tonproduktions-Verstaendnis; GAMMs+Bayes Methodik | Nur 1 Sprecherin; keine L2; Target Approximation Revision | unbekannt |
| 10 | Sparks of LAMs (Latif+, 2023) | Erster LAM-Survey; Tokenisation kritisch; Paralinguistik untererforscht | LAM-Taxonomie; Herausforderungen-Katalog | Paralinguistik "untapped"; Tokenisation; Halluzinationen | unbekannt |
| 11 | ST SFM+LLM (Gaido+, 2024) | 5 Bausteine identifiziert; kein Konsens ueber beste SFM; Length Adapter essentiell | 5-Bausteine-Framework; COMET-Empfehlung | Fehlende Standards; ICL nicht transferiert; Effizienz | unbekannt |
| 12 | LLMs Meet Speech (Yang+, 2025) | 3 Integrationsansaetze; Text-basiert verliert Prosodie; Audio-Token vielversprechend | Drei-Wege-Taxonomie; Modality-Adaptation-Strategien | Kein fairer Vergleich; meist nur Englisch; Echtzeit fehlt | unbekannt |
| 13 | Recent Advances SLMs (Cui+, 2025) | ASR+LLM+TTS: Info-Loss, Latenz, kumulative Fehler; End-to-End besser | Komponentent-Taxonomie; E2E vs. Kaskade Argumentation | Komponentenauswahl unklar; E2E-Training; Sicherheit | unbekannt |
| 14 | Survey Speech LLMs (Peng+, 2025) | Instruction Sensitivity; Semantic Reasoning Degradation; 3D-Taxonomie | Taxonomie; IFR-Metrik; Vergleichstabellen | Multitask-Generalisierung; RL-Alignment; Multi-Speaker | unbekannt |
| 15 | MMAU (Sakshi+, 2024) | Bestes Modell nur 53%; 27 Skills; Audio-Reasoning massiv defizitaer | Skill-Taxonomie; Baseline-Modelle | Kein tonales Reasoning; Domain-Isolation | unbekannt |
| 16 | Dynamic-SUPERB 2 (Huang+, 2024) | 180 Tasks; Whisper-LLaMA stark bei SLU; kein universelles Modell | Task-Taxonomie; LLM-as-Judge | Keine Generation; Mandarin unterrepraesentiert | unbekannt |
| 17 | ASR-EC Bench (Wei+, 2024) | Prompting ineffektiv; Multimodal am besten; Mindest-CER-Schwelle | Multimodale Augmentation; LoRA-Finetuning | Keine Ton-Fehler; kein tiefes Kontextverstaendnis | unbekannt |
| 18 | PERL (Liang+, 2025) | Pinyin unternutzt; 29% CER-Reduktion; 3.09ms Latenz | Pinyin Encoder; N-Best Pipeline; DoAD-Dataset | Nur Chinesisch; synthetische Daten; kein E2E | unbekannt |
| 19 | VoxEval (Cui+, 2025) | E2E-Benchmark; SLMs unter Zufallsniveau; Audio-Sensitivitaet | E2E-Evaluationsmethodik; diverse Audio-Bedingungen | Nur Englisch; keine Aussprache; kein Ton | unbekannt |
| 20 | FireRedASR (Xiaohongshu, 2025) | SOTA CER 3.05%; Datenqualitaet > Quantitaet; Progressive Regularization | Encoder-Adapter-LLM; Training-Strategie | Nur Mandarin; kein kontextuelles ASR; kein Streaming | unbekannt |
| 21 | Landscape SLMs (Arora+, 2025) | 3 SLM-Kategorien; Kaskade starke Baseline; Adapter-Rolle zentral | Taxonomie; 50+ SLM-Tabelle; Architektur-Framework | Optimale Repraesentation unklar; wenig Open Source; Scaling | unbekannt |
| 22 | Kimi-Audio (2025) | Open-Source Audio FM; hybride Tokenisierung; >13M Stunden Pretraining | Foundation Model; Datenpipeline; Tokenisierung | Paralinguistik vernachlaessigt; ASR/TTS-Ceiling | unbekannt |
| 23 | Holistic Eval LALMs (Yang+, 2026) | 4D-Taxonomie; Modelle domain-spezialisiert; LLM-as-Judge Trend | Taxonomie-Rahmen; Benchmark-Ueberblick | Datenkontamination; Low-Resource-Sprachen; Personalisierung | unbekannt |
| 24 | ZIPA (Zhu+, 2025) | 64M uebertrifft 300M; IPAPack++ 88 Sprachen; soziophonische Variation verloren | Phone-Recognizer; Trainingsressource; PFER-Metrik | Sprachen-Bias; G2P-Limitation; Variation | unbekannt |
| 25 | WildSpeech-Bench (Tencent, 2025) | Drei Benchmark-Probleme; Query-aware Evaluation; Paralinguistik entscheidend | Evaluierungsmethode; Paralinguistik-Kategorisierung | Nur Single-Turn; nur Englisch; keine In-the-Wild-Daten | unbekannt |
| 26 | ContextASR-Bench (Alibaba, 2025) | LALMs besser bei NE; 3 Kontextmodi; Halluzinationen bei Fine-grained | Kontextuelle ASR-Evaluation; NE-WER/NE-FNR | Nur TTS; Halluzinationen; LALMs kaempfen mit Kontext | unbekannt |
| 27 | Step-Audio 2 (StepFun, 2025) | E2E LLM mit RAG; SOTA MMAU 78.0; Paralinguistik 83% | RAG-Pattern; Paralinguistik-Benchmark | Kein Ton-Assessment; keine L2-Evaluation | unbekannt |
| 28 | TELEVAL (TeleAI, 2026) | Nutzer-zentriert; Content + Interactional; sozial-pragmatisches Defizit | Evaluationsdimensionen; Benchmark-Kritik | Synthetische Daten; Multi-Turn-Appropriateness | unbekannt |
| 29 | VocalBench-zh (SJTU, 2025) | Erstes Mandarin-S2S-Benchmark; LLM-Backbone entscheidend; Zeichenstruktur-Defizit | Benchmark-Taxonomie; Pinyin-Motivation; 13 Beobachtungen | Synthetische Daten; Dialekte; paralinguistische Kontrolle | unbekannt |
| 30 | Qwen3-ASR (Qwen, 2026) | All-in-One ASR 52 Sprachen; ForcedAligner; RL fuer Robustheit | Baseline-ASR; ForcedAligner; Dialekt-Support | Kein Tone Assessment; keine L2; kein Ton-Alignment | unbekannt |
| 31 | Survey LALMs Trust (Luo+, 2026) | 6 Trustworthiness-Saeulen; Modality Neglect; Cross-Modal Gap | Trustworthiness-Framework; LALM-Tabelle | Kein Safety Leaderboard; Audio-aware Alignment; keine tonalen Sprachen | unbekannt |
| 32 | Zipformer Mandarin (Du+, 2025) | WER 1.92%; 97% Phoneme >95% Acc; RTF 0.002 | Leichtgewichtige Architektur; Verwechslungsmatrix; Dataset | Keine L2; keine Toninformation; kein SSL | unbekannt |
| 33 | SCCM (Chen+, 2025) | Multi-Task: Pinyin+Initials/Finals+Characters; 45.7% CER-Reduktion | Dekodier-Architektur; Pinyin-Ensemble; Joint-Loss | Nur AISHELL-1; keine Toene; kein LLM; kein Streaming | unbekannt |
| 34 | L2 Mandarin Tones Review (Huang+, 2026) | T2/T3-Kontrast persistente Schwierigkeit; 4 Straenge; 0% QA-Reporting | 4-Strand-Systematik; XAI-Methoden; Parameter-Feedback | Classroom-Evidenz begrenzt; Perception/Production getrennt; Reproduzierbarkeit | unbekannt |
| 35 | ML Tone Recognition (Zou+, 2025) | DL uebertrifft traditionell (88.8% vs. 83.1%); Tone 3 variabel; GOT-Framework | ML-Taxonomie; Modell-Empfehlungen; Soft-Labels | Datendiversitaet; Prosodie/Dialekt; klinische Korpora | unbekannt |
| 36 | ASR Tonal Languages (Kaur+, 2021) | F0 primaerer Cue; MFCC+HMM dominant; Asien gut erforscht, Rest nicht | Historischer Kontext; Sprach-Klassifikation | Benchmark-Mangel; Amerika/Austral-Asien; pre-Transformer | unbekannt |
| 37 | ResNet Siamese Tones (Bu+, 2025) | Siamese+ResNet-18; 2D-Features optimal; Ton 3 am besten unterschieden | T-value Normalisierung; kontrastive Loss; Paar-Diskrepanz | Vereinfachte Features; keine E2E; kleiner Korpus | unbekannt |
| 38 | MSPB (Wang+, 2025) | 8 prosodische Tasks; Modelle verlassen sich auf Kontext statt Prosodie | Benchmark-Vorbild; Task-Taxonomie; Modell-Baselines | Kein T1-T4; nur Single-Round; nur Mandarin | unbekannt |
| 39 | L2 Mandarin Rating (Wu+, 2024) | 80% Ton-Diagnose-Accuracy; Fluency-Korrelation nur 0.6; Tone 3 FRR 30% | FRR/FAR/DA Framework; Tone 3 schwierigster Ton | Mehr Metriken noetig; nur Leseuebungen; kleine Stichprobe | unbekannt |
| 40 | PYG-ASR (Zhengjie+, 2025) | Pinyin-guided LLM-ASR; PYGEC ohne Finetuning; 25% CER-Reduktion | PYG-Architektur; PYGEC Post-Processing; Contextual Biasing | Nur AISHELL-1; Ton-Rolle nicht isoliert; Latenz durch ext. LLM | unbekannt |
