# Related Work — Entwurf (Kapitel 2)
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Vocabulary*

> **Zweck:** Ausformulierter Entwurf des Related-Work-Kapitels, der die 42 analysierten Papers in die bestehende Gliederung (§2.1–2.7) einsetzt. Englisch, da die Thesis englisch ist. Zitate als `(Autor, Jahr)` mit `\cite{}`-Platzhalter-Vorschlag. Wörtliche Zitate sind gegen die PDFs verifiziert (Seite angegeben).
>
> **Stil nach Tims Kommentaren:** wissenschaftlich, keine Vermenschlichung ("hear" nur im Titel, im Fließtext "transcribe/recognise"), jede Behauptung mit Beleg, konsistente Terminologie (durchgehend **LLM** bzw. **multimodal foundation model**, nicht wechselnd).

---

# TEIL A: Passt unser Material in die Struktur?

**Kurzantwort: Ja, zu ~85 %.** Die sechs Analyse-Cluster decken §2.2, §2.4, §2.5, §2.6 und §2.7 sehr gut ab. Drei Unterabschnitte sind dünn besetzt (Teil B).

| Thesis-Abschnitt | Abgedeckt durch (Paper-Nr.) | Dichte |
|---|---|---|
| 2.1 Methodology to find papers | (meta — Vorgehen, 6 Cluster, 4 Kriterien) | ✅ |
| 2.2.1 Pre-Transformer methods | Kaur (38), Zou (9, trad. Teil), Ahlawat (37) | ⚠️ dünn |
| 2.2.2 Transformer-based methods | Du (8), Wang MDD (15), Bu (10), Zhu ZIPA (18), Kang (19) | ✅ |
| 2.3.1 FM for vocabulary learning | — | ❌ Lücke |
| 2.3.2 FM for pronunciation learning | Wu (16), Huang Review (17), Wang MDD (15) | ⚠️ dünn (kaum LLM-basiert) |
| 2.4.1 FM for Chinese speech recognition | Seed-ASR (1), FireRedASR (2,3), Kimi (4), Qwen3 (5), Step-Audio2 (6), Peng (7), Li (11), Liang (12), Zhengjie (13), Chen (14) | ✅ stark |
| 2.4.2 FM for Chinese tone recognition | Zou (9), Wang MSPB (20), Chirkova (21), Xu SITA (42), Bu (10) | ✅ |
| 2.5 Evaluation of pron. & tone | Benchmarks (22–30), Yang Eval (40), Gaido (32), Metriken aus 15/16 | ✅ stark |
| 2.6 Corpora | Ryu Tone Perfect (41), Zhu ZIPA/IPAPACK++ (18), AISHELL-1 (quer), LATIC/iCALL | ✅ |
| 2.7 Learnings & research gaps | SITA (42), Gaido (32), Peng (7), Surveys (31,33,34,35,36,39) | ✅ stark |

---

# TEIL B: Wo fehlt noch etwas? (3 Lücken)

**Lücke 1 — §2.3.1 „FM for vocabulary learning": echte inhaltliche Lücke.**
Keines unserer 42 Papers behandelt LLMs/Foundation Models *für Vokabellernen* (Wortschatzerwerb, Karteikarten/SRS, adaptives Üben). Unser Korpus ist transkriptions-/ASR-zentriert. → **Empfehlung:** 2–4 Papers gezielt nachrecherchieren (Suchbegriffe: *LLM vocabulary acquisition*, *GPT flashcards spaced repetition*, *generative AI CALL vocabulary*, *intelligent tutoring vocabulary LLM*). Alternativ den Abschnitt bewusst knapp halten und die Dünne als Teil der Motivation framen („vocabulary trainers increasingly adopt LLMs, but the underlying transcription capability is rarely assessed"). Genau hierauf zielt Stefans Introduction-Kommentar #11 („und wir sind erster").

**Lücke 2 — §2.2.1 „Pre-Transformer methods": nur über Surveys abgedeckt.**
Wir haben die klassische Ära nur sekundär (Kaur 2021, Zou 2025). Für HMM / statistische LMs / CRF — die in §1.3 explizit genannt sind — fehlen Primärquellen. → **Empfehlung:** 2–3 kanonische Referenzen ergänzen (z. B. HMM-basierte ASR-Grundlagen, ein GMM-HMM-Tonerkennungs-Paper, ein CRF-Sequenzlabeling-Paper). Diese müssen nicht mandarin-spezifisch sein; sie verankern nur die historische Linie.

**Lücke 3 — §2.3.2 „FM for pronunciation learning": vorhanden, aber kaum LLM-basiert.**
Unsere Pronunciation-Papers (Wu, Huang, Wang MDD) sind überwiegend *dedizierte* Systeme (API, HuBERT, CNN), keine LLM-/CAPT-Systeme. → **Empfehlung:** 1–2 Papers zu LLM-gestütztem Aussprache-Feedback / Computer-Assisted Pronunciation Training (CAPT) mit LLMs nachtragen. Falls keine existieren, ist das wiederum ein Beleg für die Forschungslücke und kann so benannt werden.

> **Fazit für Tim:** Die Struktur trägt. Die drei dünnen Stellen sind kein Konstruktionsfehler, sondern decken sich mit der Forschungslücke selbst — besonders §2.3.1/§2.3.2: dass es kaum Arbeiten zu LLMs im Vokabel-/Ausspracheunterricht *mit belastbarer Transkriptionsbewertung* gibt, ist Teil der Begründung dieser Arbeit.

---

# TEIL C: Ausformulierter Entwurf (englisch, mit Zitaten)

> **Zitations-Konvention im Entwurf:** `(Author Year)` lesbar + `\cite{key}`-Vorschlag in eckigen Klammern für die `.bib`. Verifizierte wörtliche Zitate mit Seitenangabe. Eine Referenz-Mapping-Tabelle (arXiv/DOI → Vorschlag-bibkey) steht in Teil D.

---

## 2.1 Methodology to find relevant papers

This chapter is based on a structured review of 42 publications selected for their relevance to multimodal transcription of Mandarin speech. Candidate papers were identified through keyword searches on Google Scholar, arXiv, and the ACL/Interspeech anthologies, using combinations of the terms *Mandarin*, *tone recognition*, *Pinyin*, *speech LLM / audio language model*, *mispronunciation detection*, and *phonetic transcription*, complemented by backward and forward citation tracing from the most central works. Each paper was assessed against four criteria: (i) its central claim, (ii) what can be concretely reused for the present work, (iii) the research gap the authors state explicitly, and (iv) the implicit gap it leaves with respect to this thesis. The resulting corpus was grouped into six clusters — speech LLMs, Mandarin tone and Pinyin systems, pronunciation assessment, evaluation benchmarks, surveys, and reference corpora — which map onto the sections below. Inclusion prioritised recent work (2024–2026) for the foundation-model landscape, while older surveys were retained to trace the historical development of tone recognition.

*(Stefan: hier ggf. konkrete Datenbanken/Zeitraum/Anzahl-Treffer ergänzen, falls du ein PRISMA-artiges Vorgehen dokumentieren willst.)*

---

## 2.2 Traditional methods for pronunciation and tone recognition

### 2.2.1 Pre-Transformer methods

Before the advent of Transformer architectures, automatic recognition of Mandarin tones relied on explicit acoustic modelling of fundamental frequency (F0) combined with statistical sequence models. Tone is acoustically carried primarily by the F0 contour, and classical systems extracted F0 trajectories and modelled them with Gaussian Mixture Models, Hidden Markov Models, or, for sequence labelling, Conditional Random Fields `[\cite{HMM_ASR_ref}, \cite{CRF_ref}]`. Comprehensive surveys of this era report that traditional, feature-engineering–based approaches reach on average around 83 % tone-classification accuracy, clearly below later deep-learning systems (Zou et al. 2025) `[\cite{zou2025}]`. Kaur et al. (2021) `[\cite{kaur2021}]`, in a state-of-the-art survey of ASR for tonal languages, document that tone modelling was historically treated as an add-on to phoneme recognition rather than as a first-class target, and that reported tone-classification accuracy for second-language (L2) speech remained in the range of roughly 53–73 % across source languages.

> **GAP-Hinweis (Lücke 2):** `\cite{HMM_ASR_ref}` und `\cite{CRF_ref}` sind Platzhalter — hier 2–3 klassische Primärquellen einsetzen (HMM-ASR-Grundlage, GMM-HMM-Tonerkennung, CRF-Sequenzlabeling).

### 2.2.2 Transformer-based methods

With self-supervised speech representations and Transformer encoders, phoneme- and tone-recognition accuracy improved substantially. Du et al. (2025) `[\cite{du2025}]` apply a Zipformer-based RNN-T/CTC model to Mandarin phoneme recognition and report a word error rate of 2.12 % on a 66-phoneme inventory, illustrating the strength of modern encoders for segmental recognition; tones, however, are not evaluated as a separate axis. For pronunciation assessment specifically, Wang, Shi and Wang (2024) `[\cite{wang2024mdd}]` propose a pitch-aware RNN-T for Mandarin mispronunciation detection and diagnosis that fuses HuBERT features with an explicit pitch-fusion block over a tonal phoneme set (T1–T5), reducing the phone error rate while motivating the design with the "scarcity of [...] annotated non-native speech data". Bu et al. (2025) `[\cite{bu2025}]` target L2 tone evaluation directly with a ResNet-based Siamese network that measures tonal discrepancy on 145 hours of learner data from 40 speakers, reporting a mean squared error of 0.189. Zhu et al. (2025) `[\cite{zhu2025}]` push multilingual phone recognition to 88 languages with their ZIPA models trained on the 17,132-hour IPAPACK++ corpus, introducing a phone-feature error rate (PFER); their IPA-based representation captures tone only partially and is not Mandarin-specific. Finally, Kang and Xu (2024) `[\cite{kang2024}]` provide phonetic evidence on tone-syllable synchrony in Mandarin, clarifying the temporal alignment of tonal and segmental information that downstream models must capture.

A consistent pattern across these Transformer-based systems is that they are dedicated, single-purpose models; none is a general-purpose multimodal LLM, and tone is evaluated — if at all — only implicitly within character or phoneme error rates.

---

## 2.3 LLMs and Foundational Models in language learning

### 2.3.1 LLMs and Foundational Models for vocabulary learning

> **GAP-Hinweis (Lücke 1):** Dieser Abschnitt ist im Korpus nicht belegt. Vorschlag für die Rahmung, sobald 2–4 Quellen vorliegen:

Large language models have begun to permeate digital language-learning tools, where they are used to generate example sentences, adaptive exercises, and conversational practice for vocabulary acquisition `[\cite{TODO_vocab1}, \cite{TODO_vocab2}]`. *(Stefan: 2–4 Papers zu LLM-gestütztem Wortschatzlernen / SRS / generativer CALL ergänzen.)* While such tools increasingly rely on foundation models for content generation and feedback, the *transcription* capability that any pronunciation-aware vocabulary trainer ultimately depends on — turning a learner's spoken attempt into an accurate phonetic and tonal representation — is rarely assessed in this literature. The present thesis isolates and evaluates exactly this capability.

### 2.3.2 LLMs and Foundational Models for pronunciation learning

Research on automated pronunciation assessment for Mandarin has focused on lexical tone and prosody, the dimensions most difficult for L2 learners. Wu and Shen (2024) `[\cite{wu2024}]` evaluate an automated rating system for L2 Mandarin prosody (lexical tones and pauses), reporting a diagnostic accuracy of around 80 % but only moderate correlation with human raters (0.31–0.60), and conclude that "applicability to L2 classrooms remains questionable". Huang et al. (2025) `[\cite{huang2025}]`, in a review of interpretable computational methods for L2 Mandarin tone perception and production, identify the T2–T3 contrast as a persistent source of confusion across studies and stress the limited interpretability of current feedback systems. These systems demonstrate that fine-grained pronunciation feedback is feasible, but they rely on dedicated acoustic models or commercial APIs rather than general-purpose LLMs, and they operate on narrowly defined tasks. The use of multimodal LLMs as the engine for pronunciation feedback — and the prerequisite question of whether such models can transcribe tones reliably in the first place — remains largely unexplored.

---

## 2.4 LLMs and Foundational Models for Chinese speech processing

### 2.4.1 LLMs and Foundational Models for Chinese speech recognition

The integration of LLMs with speech encoders has produced a rapid succession of strong Mandarin ASR systems. Seed-ASR (Bai et al. 2024) `[\cite{seedasr2024}]` couples a large self-supervised audio encoder with an LLM in an audio-conditioned framework and reports an average character error rate (CER) of 2.98 % across six Chinese test sets. FireRedASR (Xu et al. 2025) `[\cite{firered2025}]` provides an open-source Mandarin system reaching 3.05 % CER and explicitly criticises that general multilingual models show "suboptimal performance for specific languages like Mandarin"; its successor FireRedASR2S `[\cite{firered2s}]` scales training data to roughly 200k hours. Kimi-Audio (Kimi Team 2025) `[\cite{kimiaudio2025}]` combines discrete semantic tokens with continuous Whisper features and achieves a state-of-the-art 0.60 % CER on AISHELL-1. Notably, Kimi-Audio is the only system in this group to name tone explicitly, listing it among the information that text-centric transcription discards: "text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone), acoustic scene, and non-linguistic sounds" (Kimi Team 2025, Sec. 8, p. 21). Qwen3-ASR (Qwen Team 2025) `[\cite{qwen3asr}]` adds LLM-based forced alignment across 52 languages, and Step-Audio 2 (Wu et al. 2025) `[\cite{stepaudio2}]` follows an end-to-end multimodal design. A survey by Peng et al. (2025) `[\cite{peng2025}]` organises this landscape and identifies fine-grained acoustic understanding as an open challenge, observing that the models' "ability to capture the full range of acoustic cues [...] remains limited" (Peng et al. 2025, Sec. IX, p. 25).

A second line of work targets Pinyin explicitly. Li et al. (2024) `[\cite{li2024pygec}]` integrate Pinyin features into an LLM for Chinese ASR error correction, and Liang and Zhang (2025) `[\cite{liang2025perl}]` use Pinyin-enhanced rephrasing for N-best correction. Most relevant to this thesis, Zhengjie and Cheng (2025) `[\cite{zhengjie2025}]` couple a HuBERT encoder to a Qwen2-7B LLM to generate Pinyin and Chinese characters directly from audio, reaching a Pinyin error rate of 1.9 % and stating that "there has been little exploration of how the LLM-ASR model can directly generate Pinyin and Chinese characters during ASR" (Zhengjie & Cheng 2025, p. 1). Chen et al. (2025) `[\cite{chen2025sccm}]` pursue a similar syllable–character collaborative approach. Crucially, the Pinyin produced by these systems carries no tone markers: the syllable "ma" is transcribed identically regardless of whether it was spoken as mā, má, mǎ, or mà. The phonetic pipeline therefore exists, but the tonal dimension that distinguishes meaning in Mandarin is omitted — precisely the gap this thesis addresses.

### 2.4.2 LLMs and Foundational Models for Chinese tone recognition

Whereas segmental recognition is mature, tone recognition with modern models is far less consolidated. Zou et al. (2025) `[\cite{zou2025}]`, reviewing 61 studies, report that deep-learning approaches reach about 88.8 % average tone-recognition accuracy versus 83.1 % for traditional methods, identify T3 as the most difficult tone, and recommend tone error rate (TER) as an evaluation metric — a metric that, as Section 2.5 shows, is almost never actually applied. The only study to test general-purpose multimodal LLMs on Mandarin tonal/prosodic material is Wang et al. (2025) `[\cite{wang2025mspb}]`, whose Mandarin Speech Prosody Benchmark (MSPB) finds that even the strongest model, GPT-4o, reaches only 59.70 % accuracy (Wang et al. 2025, Sec. 3.2.2, p. 5380); the models "generally struggled with tasks relying heavily on subtle speech prosody variations" (p. 5378/5381). MSPB, however, evaluates sentence-level prosody (focus, intonation), not lexical tone (T1–T4) at the syllable level. At the representational level, Xu et al. (2026) `[\cite{xu2026sita}]` show that standard speech encoders lose tonal information: "Whisper embeddings largely collapse into overlapping clusters" (Sec. 5.1), a phenomenon they term *tone collapse*. Their SITA model counteracts this with a tone-repulsive loss and reaches near-ceiling accuracy (~0.99 Top-1) on the Tone Perfect corpus (Sec. 5.4) — but as a dedicated, non-LLM system. Since most multimodal LLMs build on Whisper-style encoders, tone collapse motivates the central question of whether such models can recognise tone at all when used out of the box. Chirkova et al. (2025) `[\cite{chirkova2025}]`, studying the related tonal language Baima, confirm from a transcription perspective that "complex tones remain the most difficult part of the phonology to transcribe" (Sec. 5, p. 178).

---

## 2.5 Evaluation of pronunciation and tone recognition

Evaluation practice in the audio-LLM field has expanded rapidly but remains misaligned with the needs of tonal transcription. General audio-understanding benchmarks such as MMAU (Sakshi et al. 2024) `[\cite{mmau}]`, AudioBench (Wang et al. 2025) `[\cite{audiobench}]`, VoxEval (Cui et al. 2025) `[\cite{voxeval}]`, and WildSpeech (Zhang et al. 2025) `[\cite{wildspeech}]` are predominantly English and do not test Mandarin tone. Mandarin-capable benchmarks exist — ContextASR (Wang et al. 2025) `[\cite{contextasr}]`, VocalBench-zh (Liu et al. 2025) `[\cite{vocalbench}]`, and TELEVAL (Li et al. 2026) `[\cite{televal}]` — but they measure conversational ability or character-level accuracy rather than tonal transcription. Dynamic-SUPERB (Huang et al. 2024) `[\cite{dynsuperb}]` spans 180 tasks including phoneme recognition and a third-tone-sandhi classification task, yet offers no systematic Mandarin tone-transcription evaluation. Surveys of audio-LLM evaluation (Yang et al. 2026) `[\cite{yang2026eval}]` confirm that the benchmark landscape is fragmented and that no standard benchmark reports tone error rate or tonal Pinyin accuracy. With respect to metrics, the field relies overwhelmingly on CER and WER; phone error rate appears only sporadically (e.g. Wang et al. 2024 `[\cite{wang2024mdd}]`), false-rejection/acceptance and diagnostic-accuracy measures are used in pronunciation assessment (Wu & Shen 2024 `[\cite{wu2024}]`), and the tone error rate recommended by Zou et al. (2025) is not applied in any of the reviewed studies. Gaido et al. (2024) `[\cite{gaido2024}]` make the broader methodological point for speech foundation models that "no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework" (Sec. 2.1) — an observation that holds with even greater force for tonal evaluation.

---

## 2.6 Corpora for Chinese pronunciation evaluation

The primary corpus used in this thesis is Tone Perfect (Ryu et al. 2021) `[\cite{ryu2021}]`, an open-access database of 9,840 audio assets comprising 410 monosyllables produced in all four tones by six native Beijing speakers. Its systematic, fully crossed design (syllable × tone × speaker) makes it uniquely suited to a controlled tonal evaluation, and it has already served as a benchmark for tone-aware representation learning (Xu et al. 2026 `[\cite{xu2026sita}]`). Its limitations are equally relevant: it contains only native, isolated monosyllables, so it covers neither L2 speech nor multi-syllabic tone sandhi. For broader phonetic coverage, the multilingual IPAPACK++ corpus (Zhu et al. 2025) `[\cite{zhu2025}]` provides 17,132 hours across 88 languages with IPA annotation, and AISHELL-1 `[\cite{aishell1}]` is the de-facto standard for Mandarin ASR, used by the majority of the speech-LLM systems discussed above, though it provides character transcripts without tone labels. For L2 Mandarin, smaller resources such as LATIC (used by Wang et al. 2024) and iCALL (142 hours, 305 learners; reported by Zou et al. 2025, p. 10) exist but, as Zou et al. note, "receive less attention than native-speaker datasets".

---

## 2.7 Learnings and research gaps

Synthesising the reviewed literature yields four central learnings and the corresponding research gaps that motivate this thesis.

**Learning 1 — Modern speech LLMs achieve strong Mandarin recognition but are evaluated only at the character/word level.** All reviewed speech LLMs (Sections 2.4.1) report CER or WER and none assesses tonal accuracy, even though models such as Kimi-Audio explicitly acknowledge tone as discarded information. *Gap 1: phoneme- and tone-level transcription granularity for Mandarin is largely unexamined for general-purpose multimodal LLMs.*

**Learning 2 — Direct audio-to-Pinyin generation with LLMs is feasible, but tone markers are omitted.** Zhengjie and Cheng (2025) and Chen et al. (2025) generate Pinyin from audio at low error rates, yet without tone numbers. *Gap 2: tone is rarely treated as an explicit, separate evaluation axis.*

**Learning 3 — Tone recognition is an active field, but it is carried by dedicated models, not LLMs, and even strong encoders lose tonal information.** Deep-learning tone classifiers perform well (Zou et al. 2025), yet Xu et al. (2026) show tone collapse in Whisper embeddings, and the only LLM prosody study (Wang et al. 2025) reports just 59.70 % for GPT-4o on sentence-level prosody. *Gap 3 (and the L2 dimension): very few studies address non-native learner speech, and none evaluate multimodal LLMs on Mandarin tone.*

**Learning 4 — Evaluation is fragmented and tone-blind.** No benchmark reports tone error rate or tonal Pinyin accuracy, and no controlled, like-for-like comparison of general-purpose LLMs against dedicated systems exists for this task (Gaido et al. 2024). *Gap 4: there is no systematic head-to-head comparison of frontier multimodal models, against each other and against dedicated systems, at phoneme/tone granularity.*

Taken together, across 42 publications, no single work combines all three of the dimensions relevant here — multimodal LLM, audio input, and explicit tonal evaluation. Dedicated systems evaluate tone without LLMs; LLM-based systems generate Pinyin without tone; prosody benchmarks test LLMs without lexical tone. This thesis addresses the resulting gap directly: it evaluates current multimodal foundation models on Mandarin transcription at the word, phoneme, and tone level (RQ1–RQ2), compares them against dedicated baselines such as Whisper (RQ4), and analyses the structure of their tonal errors (RQ5), with non-native learner speech (RQ3) as the target-relevant extension.

---

# TEIL D: Referenz-Mapping (Vorschlag bibkey → Quelle)

| Vorschlag `\cite{key}` | Paper | Quelle / arXiv-DOI |
|---|---|---|
| seedasr2024 | Seed-ASR (1) | ByteDance, 2024 |
| firered2025 | FireRedASR (2) | 2025 |
| firered2s | FireRedASR2S (3) | 2025 |
| kimiaudio2025 | Kimi-Audio (4) | arXiv:2504.18425 |
| qwen3asr | Qwen3-ASR (5) | 2025 |
| stepaudio2 | Step-Audio 2 (6) | 2025 |
| peng2025 | Peng Survey (7) | 2024/2025 |
| du2025 | Du et al. (8) | PLOS ONE 2025 |
| zou2025 | Zou et al. (9) | Preprints 2025 |
| bu2025 | Bu et al. (10) | Scientific Reports 2025 |
| li2024pygec | Li PY-GEC (11) | arXiv:2409.13262 |
| liang2025perl | Liang & Zhang PERL (12) | arXiv:2412.03230 |
| zhengjie2025 | Zhengjie & Cheng PYG-ASR (13) | Interspeech 2025 |
| chen2025sccm | Chen et al. SCCM (14) | PLOS ONE 2025 |
| wang2024mdd | Wang, Shi & Wang (15) | 2024 |
| wu2024 | Wu & Shen (16) | Speech Prosody 2024 |
| huang2025 | Huang et al. Review (17) | 2025 |
| zhu2025 | Zhu et al. ZIPA (18) | 2025 |
| kang2024 | Kang & Xu (19) | 2024 |
| wang2025mspb | Wang et al. MSPB (20) | 2025 (S. 5378–5381) |
| chirkova2025 | Chirkova et al. (21) | ComputEL 2025 |
| wei2024asrec | Wei et al. ASR-EC (22) | 2024 |
| contextasr | Wang et al. ContextASR (23) | 2025 |
| vocalbench | Liu et al. VocalBench-zh (24) | 2025 |
| televal | Li et al. TELEVAL (25) | 2026 |
| mmau | Sakshi et al. MMAU (26) | 2024 |
| dynsuperb | Huang et al. Dynamic-SUPERB (27) | 2024 |
| voxeval | Cui et al. VoxEval (28) | 2025 |
| audiobench | Wang et al. AudioBench (29) | 2025 |
| wildspeech | Zhang et al. WildSpeech (30) | 2025 |
| latif2023 | Latif et al. (31) | 2023 |
| gaido2024 | Gaido et al. (32) | 2024 |
| cui2025splm | Cui et al. SpeechLM Survey (33) | 2025 |
| arora2026 | Arora et al. (34) | 2026 |
| yang2026eval | Yang, Chih-Kai et al. (35/40) | 2026 |
| aiindex2026 | AI Index 2026 (36) | Stanford HAI |
| ahlawat2025 | Ahlawat et al. (37) | 2025 |
| kaur2021 | Kaur et al. (38) | 2021 |
| yang2025integration | Yang, Zhengdong et al. (39) | 2025 |
| luo2026 | Luo et al. (40/35) | 2026 |
| ryu2021 | Ryu et al. Tone Perfect (41) | DH 2021 |
| xu2026sita | Xu et al. SITA (42) | arXiv 2026 |
| aishell1 | AISHELL-1 corpus | Bu et al. 2017 (Standard-Ref) |

> **Achtung Doppelung:** In den Analysedateien tauchen „Yang" und „Luo" teils unter verschiedenen Nummern auf (35/40). Vor dem Einbau Referenzen einmal abgleichen, damit keine Dublette in die `.bib` kommt.

---

# TEIL E: To-dos für Stefan (priorisiert)

1. **Lücke 1 schließen** (§2.3.1): 2–4 Papers zu LLM-gestütztem Vokabellernen recherchieren — sonst Abschnitt bewusst kurz + als Motivationsbrücke.
2. **Lücke 2 schließen** (§2.2.1): 2–3 klassische HMM/CRF-Primärquellen ergänzen (`\cite{HMM_ASR_ref}`, `\cite{CRF_ref}`).
3. **Lücke 3 prüfen** (§2.3.2): existiert LLM-/CAPT-Aussprache-Feedback-Literatur? Falls nein → als Lücke benennen.
4. **Introduction-Kommentare abarbeiten:** alle `[citation needed]` mit obigen Quellen füllen; konsistent **LLM** verwenden; den Satz „Before an LLM can give a learner meaningful feedback … hear and transcribe" entanthropomorphisieren (Kommentare #14/#15: z. B. „A reliable pronunciation trainer first requires an accurate phonetic and tonal transcription of the learner's utterance, including tone.").
5. **`.bib` aufbauen** anhand Teil D; Dubletten (Yang/Luo) bereinigen.
6. **2.1 konkretisieren:** Suchstrings, Zeitraum, Trefferzahlen dokumentieren, falls systematisches Review gewünscht.
