# PowerPoint-Plan: Existing Work / Related Work
## Masterarbeit Stefan Dosch — *Can LLMs Hear Tones?*

> **Ziel:** Tims Fragen beantworten, Forschungslücke klar herausarbeiten, in ~8-10 Folien.
> **Prinzip:** Jede Folie hat 1 Hauptaussage + 2-3 Stützpunkte. Alles mit Beleg.

---

# TEIL I: FOLIENPLAN (exakter Inhalt pro Folie)

---

## Folie 1: Titel — Existing Work Overview

**Folientitel:** Existing Work: 42 Papers Analysiert

**Inhalt (3 Bullet Points):**
- 42 Paper systematisch analysiert: 7 Speech LLMs, 7 Mandarin-Ton/Pinyin-Systeme, 7 Pronunciation/MDD, 9 Benchmarks, 10 Surveys, 2 zentrale Datensätze
- Jede Aussage mit Originalzitat belegt
- Ergebnis: **Keine einzige Studie testet multimodale LLMs auf tonale Mandarin-Transkription**

**Sprechernotiz:** "Ich habe 42 Paper systematisch ausgewertet und nach vier Kriterien analysiert: Kernaussage, konkrete Verwendbarkeit, explizite Lücke, implizite Lücke für meine Arbeit. Das zentrale Ergebnis vorweg: Die Kombination LLM + Audio + Töne existiert in der Literatur nicht."

---

## Folie 2: Speech LLMs — Stand der Technik

**Folientitel:** Speech LLMs erreichen SOTA bei Mandarin-ASR — aber nur auf Zeichenebene

**Inhalt (Tabelle, 4 Spalten):**

| Modell | CER Mandarin | Trainingsumfang | Ton-Evaluation? |
|--------|-------------|-----------------|-----------------|
| Seed-ASR (2024) | 2,98% | 20M+ h | Nein |
| FireRedASR (2025) | 3,05% | 70k h | Nein |
| Kimi-Audio (2025) | 0,60% AISHELL-1 | 13M h | Nein |
| Qwen3-ASR (2026) | ~2% | 40M h | Nein |

**Schlüsselzitat (unten auf Folie):**
> *"text transcription focuses on the content of spoken words, neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, **tone**)."* — Kimi-Audio (2025)

**Sprechernotiz:** "Aktuelle Speech LLMs erreichen beeindruckende Ergebnisse auf Mandarin. Seed-ASR 2,98% CER, Kimi-Audio sogar 0,60% auf AISHELL-1. Aber: Alle evaluieren ausschließlich auf Zeichenebene — Character Error Rate. Keines dieser Systeme wird auf Tongenauigkeit getestet. Kimi-Audio benennt das Problem sogar selbst: 'tone' wird als vernachlässigte Information klassifiziert, aber nicht evaluiert. Das ist bemerkenswert: Die Modelle könnten theoretisch Töne erkennen — wir wissen es nicht, weil es niemand misst."

---

## Folie 3: Pinyin-Generierung — Fast am Ziel, aber ohne Töne

**Folientitel:** LLM-basierte Pinyin-Generierung existiert — aber ohne Tonmarker

**Inhalt (2 Paper hervorgehoben):**

**Zhengjie & Cheng (2025) — PYG-ASR** (nächstes Paper zu unserer Arbeit):
- HuBERT-Encoder → Qwen2-7B → generiert **Pinyin + chinesische Zeichen gleichzeitig**
- Pinyin Error Rate: nur 1,9%
- **Aber: Pinyin OHNE Tonnummern** (z.B. "ma" statt "ma1")
- Zitat: *"there has been little exploration of how the LLM-ASR model can directly generate Pinyin"*

**Chen SCCM (2025):**
- Multi-Task: Silben-Zeichen-Kollaboration
- Pinyin-CER: 2,41% — ebenfalls **ohne Tonmarker**

**Fazit-Box:**
→ Die technische Pipeline Audio→LLM→Pinyin funktioniert. Was fehlt, ist der letzte Schritt: **Töne mittranskribieren.**

**Sprechernotiz:** "Das Paper von Zhengjie und Cheng kommt meiner Arbeit am nächsten. Sie zeigen, dass ein LLM aus Audio direkt Pinyin generieren kann — mit nur 1,9% Fehlerrate. Aber sie lassen die Töne komplett weg. Kein 'ma1', kein 'ma3', einfach nur 'ma'. Chen et al. machen dasselbe. Das heißt: Die Pipeline existiert, aber niemand hat den entscheidenden Schritt gemacht, Töne als Teil der Transkription zu fordern. Genau das macht meine Arbeit."

---

## Folie 4: Ton-Erkennung — Existiert, aber ohne LLMs

**Folientitel:** Mandarin-Tonerkennung ist ein aktives Feld — aber ohne LLMs

**Inhalt (3 Stränge):**

**Klassische Ansätze (kein LLM):**
- Zou et al. (2025) Review: DL erreicht 88,8% vs. traditionell 83,1% Ton-Accuracy
- Bu et al. (2025): Siamese-Netzwerk, MSE 0,189 auf L2-Daten
- Wang MDD (2024): Pitch-Aware RNN-T mit Tonal-Phoneme-Set (T1-T5)

**Bekannte Schwierigkeiten:**
- T2↔T3-Verwechslung ist **persistent** über alle Studien (Huang 2025)
- L2-Sprecher erreichen nur 53-73% Ton-Accuracy (Kaur 2021)
- Zitat: *"complex tones remain the most difficult part of the phonology to transcribe"* — Chirkova (2025)

**Was fehlt:**
- Alle diese Studien nutzen dedizierte Modelle (CNNs, RNNs, HuBERT)
- **Kein einziges Paper testet ein multimodales LLM auf Tonerkennung**

**Sprechernotiz:** "Ton-Erkennung bei Mandarin ist kein neues Thema. Es gibt Reviews, die 61 Studien zusammenfassen, Siamese-Netzwerke für L2-Daten, Pitch-Aware Modelle. Die Schwierigkeit ist bekannt: T2 und T3 werden chronisch verwechselt, L2-Sprecher schaffen nur 53-73% Accuracy. Aber: All diese Arbeiten nutzen klassische Deep-Learning-Modelle. Kein einziges Paper hat ein multimodales LLM wie GPT-4o oder Gemini auf Tonerkennung getestet. Das ist meine Lücke."

---

## Folie 5: Tone Collapse — Whisper verliert Ton-Information

**Folientitel:** Sogar Whisper "kollabiert" bei Tönen — SITA (Xu et al. 2026)

**Inhalt (visuell: 2 Spalten):**

**Links — Das Problem:**
- Whisper-Embeddings: Ton-Repräsentationen **überlappen** und werden ununterscheidbar
- Zitat: *"Whisper embeddings largely collapse into overlapping clusters"* — Xu et al. Sec. 5.1
- Gleiche Silbe, verschiedene Töne → gleiche Embedding-Position

**Rechts — Die Lösung (SITA):**
- "Tone-Repulsive Loss" drückt gleiche Silben mit verschiedenen Tönen auseinander
- SITA erreicht 99,27% Top-1 Accuracy auf Tone Perfect
- Zitat: *"tone-repulsive loss prevents tone collapse by explicitly separating same-word different-tone realizations"*

**Fazit-Box:**
→ Wenn Whisper Ton-Information verliert, was passieren dann bei GPT-4o, Gemini, Qwen — die alle auf Whisper-Features aufbauen?
→ **Genau diese Frage beantwortet meine Arbeit.**

**Sprechernotiz:** "Xu et al. haben 2026 gezeigt, dass Whisper ein fundamentales Problem hat: die Embeddings für verschiedene Töne der gleichen Silbe kollabieren — 'ma1' und 'ma3' landen praktisch an der gleichen Stelle im Embedding-Raum. Das ist hochrelevant, weil die meisten multimodalen LLMs Whisper-basierte Audio-Encoder verwenden. Wenn der Encoder die Ton-Information schon verliert, kann das LLM sie nicht mehr rekonstruieren. SITA löst das mit einem speziellen Loss — aber nur für ein dediziertes System. Meine Arbeit fragt: Wie schlimm ist das Problem bei den LLMs, die wir tatsächlich als Off-the-Shelf-Tools nutzen wollen?"

---

## Folie 6: Benchmarks — Mandarin kommt vor, Töne nicht

**Folientitel:** 9+ Audio-LLM-Benchmarks — keiner misst Tongenauigkeit

**Inhalt (kompakte Tabelle):**

| Benchmark | Mandarin? | Phonem-Eval? | Ton-Eval? |
|-----------|----------|-------------|----------|
| MSPB (2025) | ✅ | ❌ | ⚠️ nur Satzprosodie |
| VocalBench-zh (2025) | ✅ | ❌ | ❌ |
| TELEVAL (2026) | ✅ | ❌ | ❌ |
| ContextASR (2025) | ✅ | ❌ | ❌ |
| Dynamic-SUPERB (2025) | ⚠️ | ✅ (1 Task) | ⚠️ Sandhi-Klass. |
| MMAU (2024) | ❌ | ⚠️ | ❌ |
| AudioBench (2025) | ❌ | ❌ | ❌ |

**Schlüsselzitat:**
> *"current speech LLMs remain limited in Mandarin speech prosody comprehension"* — Wang MSPB (2025)

**Fazit:** GPT-4o erreicht nur 59,7% auf MSPB-Prosodie-Aufgaben. Und das testet nur Satzprosodie — **lexikalische Töne werden gar nicht gemessen.**

**Sprechernotiz:** "Ich habe 9 aktuelle Audio-LLM-Benchmarks analysiert. Mehrere davon inkludieren Mandarin — MSPB, VocalBench-zh, TELEVAL, ContextASR. Aber keiner misst die Fähigkeit, einzelne Töne korrekt zu transkribieren. Der nächste Kandidat ist MSPB, der Satzprosodie testet — also Betonung und Intonation auf Satzebene. Selbst dabei erreicht GPT-4o nur 59,7%. Lexikalische Töne — ob 'ma1' oder 'ma4' — werden in keinem einzigen Benchmark evaluiert."

---

## Folie 7: Die Metrik-Lücke — TER existiert als Konzept, wird aber nie angewandt

**Folientitel:** Tone Error Rate (TER) wird empfohlen — aber von niemandem gemessen

**Inhalt (3 Punkte):**

**Was gemessen wird:**
- CER/WER: 20+ Papers → Standardmetrik, aber ignoriert Töne komplett
- PER: 3 Papers (Wang MDD, VocalBench-zh, Dyn-SUPERB) → misst Phoneme, nicht Töne separat
- Pinyin ERR: 2 Papers (Zhengjie, Chen) → Pinyin-Fehler, aber **ohne Tonmarker**

**Was NICHT gemessen wird:**
- **TER (Tone Error Rate):** Zou et al. (2025) empfehlen TER → *kein einziges Paper wendet sie an*
- **tonal CER/WER:** Nur Chirkova (2025) für Baima (6-Ton-System), **nicht für Mandarin mit LLMs**
- **PFER:** Zhu ZIPA (2025) verwendet Phone Feature Error Rate → aber nicht mandarin-spezifisch

**Fazit-Box:**
→ Stefans Arbeit führt TER **erstmals systematisch für multimodale LLMs** ein.

**Sprechernotiz:** "Das ist vielleicht der überraschendste Befund meiner Literaturanalyse: Die Metrik 'Tone Error Rate' wird von Zou et al. als Best Practice empfohlen. Aber kein einziges Paper in meinem Korpus wendet sie tatsächlich an. Chirkova misst 'tonal CER' — aber für Baima, eine andere Sprache, und ohne LLMs. Zhengjie misst 'Pinyin ERR' — aber ohne Tonmarker. Es gibt eine offensichtliche Lücke zwischen dem, was die Community als wichtig anerkennt, und dem, was tatsächlich gemessen wird."

---

## Folie 8: L2-Lernende — Die vernachlässigte Perspektive

**Folientitel:** L2-Mandarin-Lernende: Wenige Daten, keine LLM-Studien

**Inhalt:**

**Existierende L2-Studien (alle ohne LLMs):**
| Studie | L2-Sprecher | Methode | Ergebnis |
|--------|------------|---------|---------|
| Wang MDD (2024) | 4 Sprecher | HuBERT RNN-T | PER -3% |
| Wu L2 Prosd (2024) | 12 Sprecher | Proprietäre API | DA ~80% |
| Bu Siamese (2025) | 40 (tibetisch) | Siamese ResNet | MSE 0,189 |
| Kaur Survey (2021) | Review | Diverse | 53-73% Acc |

**Schlüsselzitate:**
> *"L2 corpora exist but receive less attention than native-speaker datasets"* — Zou et al. (2025)
> *"scarcity of [...] annotated non-native speech data"* — Wang MDD (2024)

**Fazit:** Maximal 40 L2-Sprecher in einer Studie. Keine einzige nutzt multimodale LLMs. → **RQ3 (Stretch) wäre komplett neu.**

**Sprechernotiz:** "Für mein Anwendungsszenario — ein Vokabeltrainer — sind L2-Sprecher die eigentliche Zielgruppe. Aber die Literatur ist dünn: Die größte Studie hat 40 tibetische Lerner, die kleinste nur 4 Sprecher. Und keine davon nutzt ein multimodales LLM. Zou et al. bestätigen explizit, dass L2-Korpora weniger Aufmerksamkeit bekommen. Meine RQ3 — falls ich zeitlich dazu komme — wäre damit komplett neues Territorium."

---

## Folie 9: Tone Perfect — Mein Evaluationsdatensatz

**Folientitel:** Tone Perfect: Idealer Datensatz für systematische Ton-Evaluation

**Inhalt:**

**Eckdaten:**
- 410 einzigartige Silben × 4 Töne × 6 Beijing-Sprecher = **9.840 Audio-Assets**
- Lizenz: CC-BY (frei nutzbar)
- Format: Isolierte Monosilben, professionell aufgenommen
- Zitat: *"nothing like this existed so we had to make them"* — Ryu et al. (2021)

**Warum ideal für meine Arbeit:**
- ✅ Jede Silbe mit allen 4 Tönen → systematischer Ton-Vergleich möglich
- ✅ Isolierte Silben → kein Kontexteffekt, reiner Ton-Test
- ✅ Native Speaker → kontrollierte Baseline (RQ2)
- ✅ Bereits von SITA (Xu 2026) validiert → SITA als obere Referenz nutzbar

**Was fehlt:**
- ❌ Keine L2-Sprecher (für RQ3 müsste ich eigene Daten erheben)
- ❌ Keine Mehrsilben-Wörter (kein Tone Sandhi testbar)

**Sprechernotiz:** "Tone Perfect ist der Datensatz, auf dem ich meine Evaluation aufbaue. Er hat 9.840 Audio-Clips — jede Mandarin-Silbe in allen vier Tönen, gesprochen von sechs Beijing-Muttersprachlern. Der Datensatz ist CC-BY lizenziert und wurde genau für den Zweck erstellt, tonale Unterschiede systematisch zu testen. SITA hat ihn bereits validiert und erreicht 99,27% Top-1 Accuracy — das ist meine obere Referenzlinie. Zwei Einschränkungen: keine L2-Sprecher und keine Mehrsilben-Wörter, also kein Tone Sandhi."

---

## Folie 10: Die Forschungslücke — Zusammenfassung

**Folientitel:** Die Lücke: Kein Paper verbindet LLM + Audio + Töne

**Inhalt (Venn-Diagramm oder Matrix-Visual):**

| | Töne evaluiert | LLM genutzt | Audio-Input |
|---|:---:|:---:|:---:|
| Zou Review, Huang Review | ✅ | ❌ | ❌/✅ |
| Wang MDD, Bu Siamese | ✅ | ❌ | ✅ |
| Zhengjie PYG-ASR | ❌ (Pinyin ohne Ton) | ✅ | ✅ |
| Wang MSPB | ⚠️ (Satzprosodie) | ✅ | ✅ |
| Seed-ASR, Kimi-Audio | ❌ | ✅ | ✅ |
| **Stefan (diese Arbeit)** | **✅** | **✅** | **✅** |

**Stärkstes Zitat:**
> *"extracting richer and finer acoustic information from speech [...] remains limited."* — Peng Survey (2025)

**Fazit:** Aus 42 Papers: **0 Papers** in der Schnittmenge aller drei Dimensionen. Stefan schließt diese Lücke.

**Sprechernotiz:** "Das ist die Kernfolie. Ich habe drei Dimensionen: Werden Töne evaluiert? Wird ein LLM genutzt? Wird Audio als Input verwendet? Es gibt Papers, die zwei von drei erfüllen — Zou und Huang evaluieren Töne, aber ohne LLMs. Zhengjie nutzt ein LLM mit Audio, lässt aber Töne weg. Wang MSPB testet LLMs auf Prosodie, aber nur auf Satzebene. In der Schnittmenge aller drei Dimensionen steht: niemand. Das ist mein Beitrag."

---

# TEIL II: AUSFÜHRLICHE TEXTVERSION (für Thesis / Vorbereitung)

Die Textversion ist nach Tims Fragen strukturiert und enthält alle Details, die auf den Folien keinen Platz haben.

---

## Tim-Frage 1: Was ist der Stand der Technik bei Speech LLMs?

Aktuelle Speech LLMs erreichen beeindruckende Ergebnisse bei der Mandarin-Spracherkennung auf Zeichenebene. Seed-ASR (2024) nutzt ein Audio-Conditioned Large Language Model mit einem selbstüberwachten Encoder (LUISE, ~2B Parameter) und erreicht 2,98% durchschnittliche CER auf 6 chinesischen Testsets, trainiert auf über 20 Millionen Stunden Sprachdaten (*"Compared to recently released large ASR models, Seed-ASR achieves 10%-40% reduction in word (or character, for Chinese) error rates"* — Abstract). FireRedASR (2025) bietet ein open-source Mandarin-ASR mit Qwen2-7B-Backbone und 3,05% CER (*"achieving an 8.4% relative CER reduction"* — Abstract). FireRedASR2S (2025) erweitert dies auf 200k Stunden Trainingsdaten und erreicht 2,89% CER. Kimi-Audio (2025) erreicht mit hybrider Tokenisierung (diskrete semantische Tokens + kontinuierliche Whisper-Features) sogar 0,60% CER auf AISHELL-1 und ist das erste Modell, das "tone" explizit als vernachlässigte Information benennt: *"text transcription focuses on the content of spoken words, neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)"* (Section 1). Qwen3-ASR (2026) führt erstmals LLM-basiertes Forced Alignment ein, evaluiert aber nur auf Wort-/Zeichenebene.

**Kritische Bewertung für Stefans Arbeit:** Alle sieben analysierten Speech LLMs evaluieren ausschließlich Character Error Rate (CER) oder Word Error Rate (WER). Keines gibt Pinyin aus. Keines misst Tongenauigkeit. FireRedASR kritisiert sogar explizit, dass multilinguales Training die Mandarin-Leistung beeinträchtigt (*"suboptimal performance for specific languages like Mandarin"*), geht aber selbst nicht auf tonale Aspekte ein. Die Implikation: Diese Modelle könnten Töne korrekt erkennen — oder auch nicht. Wir wissen es schlicht nicht, weil es niemand misst. Genau das macht Stefans Arbeit.

---

## Tim-Frage 2: Gibt es schon Arbeiten zu LLM-basierter Pinyin-Transkription?

Ja, aber ohne Töne. Die relevantesten Arbeiten sind:

**Zhengjie & Cheng PYG-ASR (2025):** Dieses Paper kommt Stefans Ansatz am nächsten. Es nutzt einen HuBERT-Encoder mit Qwen2-7B als LLM-Backbone und generiert simultan chinesische Zeichen und Pinyin aus Audio. Die Ergebnisse sind stark: 25% relative CER-Reduktion gegenüber dem Basismodell und nur 1,9% Pinyin Error Rate. Das Paper benennt die Lücke explizit: *"there has been little exploration of how the LLM-ASR model can directly generate Pinyin"*. Der entscheidende Mangel: **Das generierte Pinyin enthält keine Tonnummern.** "mā" (Ton 1) und "mà" (Ton 4) werden beide als "ma" transkribiert. Für einen Vokabeltrainer, der Aussprache-Feedback geben soll, ist diese Information aber unverzichtbar.

**Chen SCCM (2025):** Verwendet einen Syllable-Character-Collaborative-Mechanismus mit Pinyin-Ensemble-Ansatz. Erreicht 2,41% Pinyin-CER und beeindruckende 45,7% relative CER-Reduktion bei der Zeichenerkennung. Aber auch hier: Pinyin ohne Tonmarker.

**Li PY-GEC (2024):** Integriert Pinyin-Features in LLaMA-3-8B für grammatische Fehlerkorrektur. Zeigt, dass Pinyin-Informationen die Cosine Similarity von phonetisch ähnlichen Zeichen von 0,26 auf 0,82 erhöhen. Aber: Arbeitet nur auf Textebene (Text→Pinyin), nicht auf Audio.

**Liang PERL (2025):** Pinyin Enhanced Rephrasing mit GRU+Transformer-Encoder. 29,11% CER-Reduktion bei der ASR-Nachkorrektur. Nutzt Pinyin als intermediäre Repräsentation, generiert es aber nicht aus Audio.

**Fazit:** Die technische Machbarkeit von Audio→LLM→Pinyin ist nachgewiesen (Zhengjie/Cheng). Was fehlt, ist der letzte Schritt: Töne als expliziten Teil des Pinyin-Outputs zu fordern und zu evaluieren. Stefan kann direkt auf der PYG-ASR-Architektur aufbauen und als Evaluation den Schritt "Pinyin mit Tonnummern" ergänzen.

---

## Tim-Frage 3: Was wissen wir über Mandarin-Tonerkennung?

Das Feld ist aktiv, aber vollständig im Bereich klassischer Machine-Learning-Modelle verankert:

**Systematische Reviews:** Zou et al. (2025) fassen 61 Studien zusammen und berichten, dass Deep-Learning-Ansätze 88,8% durchschnittliche Tonerkennungsgenauigkeit erreichen gegenüber 83,1% bei traditionellen Methoden. CNNs erreichen auf isolierten Silben bis zu 99,16% Accuracy. T3 (der Dipping-Ton) wird als schwierigster Ton identifiziert. Kaur et al. (2021) liefern den umfassendsten Survey zu tonaler ASR und berichten L2-Ton-Accuracy von 53-73% über verschiedene Quellsprachen. Huang et al. (2025) identifizieren die T2↔T3-Verwechslung als *"persistent difficulty"* über alle Studien hinweg.

**Dedizierte Systeme:** Wang MDD (2024) entwickelt ein Pitch-Aware RNN-T mit HuBERT-Features und einem Tonal-Phoneme-Set (5 Töne inkl. neutralem Ton), das PER um 3% relativ reduziert. Evaluiert auf AISHELL-1 (native) und LATIC (4 L2-Sprecher). Bu et al. (2025) trainieren ein Siamese ResNet-18 auf F0-Features mit 145 Stunden Daten (40 tibetische L2-Lerner) und erreichen MSE 0,189 für Ton-Distanz-Messung. Wu et al. (2024) evaluieren eine proprietäre API auf Mandarin-Töne im Klassenraum (12 L2-Sprecher) und erreichen ~80% Diagnostic Accuracy, mit Human-Korrelation von nur 0,31-0,60 — *"applicability to L2 classrooms remains questionable"*.

**Der blinde Fleck:** Xu et al. (2026) zeigen mit SITA, dass Whisper-Embeddings bei Tönen "kollabieren" — verschiedene Töne derselben Silbe werden im Embedding-Raum ununterscheidbar (*"Whisper embeddings largely collapse into overlapping clusters"*). SITA löst dies mit einem tone-repulsive Loss und erreicht 99,27% Top-1 Accuracy auf Tone Perfect. Aber: SITA ist ein dediziertes System (XLS-R/wav2vec-basiert), kein multimodales LLM. Die Frage, ob multimodale LLMs (die oft Whisper-Features als Audio-Input nutzen) dasselbe Tone-Collapse-Problem haben, wird von keinem Paper adressiert. Das ist die zentrale Motivation für Stefans RQ4.

**Mandarin-spezifische LLM-Prosodie:** Wang MSPB (2025) ist die einzige Studie, die Speech LLMs (GPT-4o, Gemini, Qwen2-Audio) auf Mandarin-Prosodie testet. Ergebnis: GPT-4o erreicht nur 59,7% — *"current speech LLMs remain limited in Mandarin speech prosody comprehension"*. Aber MSPB testet Satzprosodie (Betonung, Intonation, Pausen), nicht lexikalische Töne. Die Frage "Kann GPT-4o 'mā' von 'mà' unterscheiden?" wird nicht gestellt.

---

## Tim-Frage 4: Welche Datensätze und Metriken existieren?

**Datensätze für Mandarin-Ton-Evaluation:**

| Datensatz | Umfang | Töne? | L2? | Öffentlich? |
|-----------|--------|-------|-----|-------------|
| **Tone Perfect** (Ryu 2021) | 9.840 Assets (410×4×6) | ✅ Alle 4 | ❌ | ✅ CC-BY |
| AISHELL-1 | 170h, 400 Spr. | ❌ (nur Zeichen) | ❌ | ✅ |
| THCHS-30 | 30h | ❌ | ❌ | ✅ |
| IPAPACK++ (ZIPA) | 17.132h, 88 Sprachen | ⚠️ IPA-Töne | ⚠️ | ✅ |
| LATIC (Wang MDD) | 4h, 4 L2-Spr. | ✅ | ✅ | ❓ |
| Bu et al. L2 | 145h, 40 tib. Spr. | ✅ | ✅ | ❌ |
| Wu Classroom | ~12 Spr. | ✅ | ✅ | ❌ |

**Stefan nutzt Tone Perfect** weil es der einzige öffentlich verfügbare Datensatz ist, der alle vier Mandarin-Töne für jede Silbe systematisch abdeckt. SITA hat ihn bereits als Evaluation-Benchmark validiert (99,27% Top-1 = obere Referenz).

**Metriken — was existiert vs. was Stefan einführt:**
Die Literatur misst überwiegend CER/WER (20+ Papers). Tone Error Rate (TER) wird von Zou et al. als Best Practice empfohlen, aber in keinem der 42 Papers tatsächlich angewandt. Pinyin Error Rate messen Zhengjie und Chen — aber ohne Tonmarker. Tonal CER/WER nutzt nur Chirkova (2025) für Baima, nicht für Mandarin mit LLMs. Stefan führt TER erstmals systematisch für multimodale LLMs ein und ergänzt sie mit PER, CER, F1 und per-Ton-/per-Kontrast-Breakdowns (RQ5).

---

## Tim-Frage 5: Wie grenzt sich Stefans Arbeit ab?

**Die Dreifach-Lücke (aus 42 Papers bestätigt):**

1. **Dimension "Töne":** 13 Papers evaluieren Mandarin-Töne in irgendeiner Form — aber keines davon nutzt ein multimodales LLM.
2. **Dimension "LLM":** Mindestens 9 LLM-basierte Systeme verarbeiten Mandarin-Audio direkt (Seed-ASR, FireRedASR, Kimi-Audio, Qwen3-ASR, Step-Audio 2, Zhengjie PYG, GPT-4o via MSPB etc.) — aber keines evaluiert Tongenauigkeit.
3. **Dimension "Pinyin mit Tönen":** 4 Papers generieren Pinyin (Zhengjie, Chen, Li, Liang) — aber keines davon mit Tonnummern.

**In der Schnittmenge aller drei Dimensionen steht: kein Paper.**

Stefan ist der Erste, der:
- multimodale LLMs (GPT-4o, Gemini, Qwen-Audio etc.)
- auf Mandarin-Audio (Tone Perfect Datensatz)
- mit tonaler Granularität (TER, per-Ton F1)
- systematisch vergleicht (Head-to-Head + Whisper-Baseline)

Das stärkste Zitat für die Abgrenzung: *"no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework"* — Gaido et al. (2024). Und das bezieht sich nur auf Speech Foundation Models allgemein — für tonale Evaluation gilt es noch stärker.

---

## Tim-Frage 6: Was sind die stärksten Beleg-Zitate für die Thesis?

Sortiert nach Einsatzort in der Thesis:

**Einleitung / Motivation:**
1. *"text transcription focuses on the content of spoken words, neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)."* — Kimi-Audio (2025)
2. *"extracting richer and finer acoustic information from speech [...] remains limited."* — Peng Survey (2025)
3. *"nothing like this existed so we had to make them"* — Ryu/Tone Perfect (2021), über den Datensatzmangel

**Related Work / Forschungslücke:**
4. *"there has been little exploration of how the LLM-ASR model can directly generate Pinyin"* — Zhengjie PYG-ASR (2025)
5. *"current speech LLMs remain limited in Mandarin speech prosody comprehension"* — Wang MSPB (2025)
6. *"L2 corpora exist but receive less attention than native-speaker datasets"* — Zou Review (2025)
7. *"complex tones remain the most difficult part of the phonology to transcribe"* — Chirkova (2025)

**Methodik / Evaluation:**
8. *"Whisper embeddings largely collapse into overlapping clusters"* — Xu SITA (2026), motiviert den Vergleich
9. *"tone-repulsive loss prevents tone collapse by explicitly separating same-word different-tone realizations"* — Xu SITA (2026)
10. *"no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework"* — Gaido (2024)
11. *"scarcity of [...] annotated non-native speech data"* — Wang MDD (2024)

**Diskussion / Limitationen:**
12. *"applicability to L2 classrooms remains questionable"* — Wu L2 Prosody (2024)
13. *"inherent trade-off between tone separation and ASR accuracy"* — Xu SITA (2026)

---

# TEIL III: OPTIONALE ZUSATZFOLIEN

## Folie A (optional): Related-Work-Einordnung nach Nordstern-Schema

**Folientitel:** Einordnung in die Thesis-Struktur (§2.2–2.7)

| Thesis-Abschnitt | Papers | Anzahl |
|------------------|--------|--------|
| §2.2 Traditionelle Methoden | Du, Bu, Wang MDD, Kang, Chirkova | 5 |
| §2.3 LLMs in Language Learning | Li PY-GEC, Liang PERL | 2 |
| §2.4 LLMs für Chinese Speech | Seed-ASR, FireRedASR(2S), Kimi, Qwen3, Step-Audio, Zhengjie, Chen | 9 |
| §2.5 Evaluation Pron./Ton | Wu, Huang Review, Zou Review, Kaur Survey | 4 |
| §2.6 Korpora | Tone Perfect, ZIPA, AISHELL-1 | 3 |
| §2.7 Learnings & Gaps | MSPB, SITA, alle Surveys | 10+ |

## Folie B (optional): Timeline — Von ASR zu Speech LLMs

2019: Kaur Survey (letzter tonaler ASR-Review vor LLM-Ära)
2021: Tone Perfect veröffentlicht
2024: Erste Speech LLMs für Mandarin (Seed-ASR, Kimi-Audio)
2025: Explosion an Speech LLMs + Benchmarks (FireRedASR, MSPB, VocalBench-zh, Zhengjie PYG-ASR)
2026: SITA zeigt Tone Collapse; Stefans Arbeit schließt die Lücke

---

# TEIL IV: ZUSAMMENFASSUNG

## Für die PowerPoint (10 Folien, je ~1 Min):
- Folie 1: Überblick (42 Papers, zentrale Erkenntnis)
- Folie 2: Speech LLMs — SOTA, aber nur CER
- Folie 3: Pinyin-Generierung — fast am Ziel, aber ohne Töne
- Folie 4: Ton-Erkennung — aktives Feld, aber ohne LLMs
- Folie 5: Tone Collapse — Whisper-Problem (SITA)
- Folie 6: Benchmarks — Mandarin ja, Töne nein
- Folie 7: Metrik-Lücke — TER fehlt komplett
- Folie 8: L2-Lernende — kaum Daten, keine LLMs
- Folie 9: Tone Perfect — Stefans Datensatz
- Folie 10: Die Lücke — Venn-Diagramm (Schlussfolie)

## Für die Thesis:
Die ausführliche Textversion (Teil II) enthält alle Details mit Zitaten und kann direkt als Grundlage für das Related-Work-Kapitel (§2.2–2.7) dienen. Die Struktur folgt den Nordstern-Abschnitten.
