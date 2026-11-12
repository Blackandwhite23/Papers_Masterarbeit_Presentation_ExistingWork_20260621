# PowerPoint — Bestehende Tools für Mandarin-Lautschrift & Tonerkennung (ohne LLMs)
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Zweck:** Folien zur Frage „Welche Tools können JETZT SCHON gesprochenes Mandarin in Pinyin/Ton/IPA umwandeln — ohne LLMs? Wie zuverlässig? Können sie teures manuelles Labeling ersetzen?"
>
> **Quellen:** 4 unabhängige Deep-Research-Läufe (Gemini, Mistral, Claude, Perplexity), **online gegengeprüft** (Stand 2026-06-29).
>
> **Kernkontext:** Diese Tools sind (a) die **Nicht-LLM-Baseline** für den Vergleich, (b) ein möglicher **automatischer Cross-Check** für die Annotation. Die Forschungslücke wird durch sie **bestätigt und geschärft**.

---

# TEIL 0: PRÜF-ERGEBNIS (für Stefan, nicht zum Vortragen)

## Was die 4 Deep-Research-Berichte übereinstimmend sagen

| These | Belege (4 Berichte) | Online verifiziert? |
|---|---|---|
| **Native + isolierte Silben = praktisch gelöst** | CNN 99,8 %, HuBERT ~100 %, wav2vec2 6 % TER | ✅ Tone-Perfect-CNN (alicex2020) = 99,8 %; ToneNet ~99 %; Yuan et al. 2021 = 6 % TER |
| **L2-Lerner = alle Tools brechen ein** | DER 26–32 % bei SOTA-MDD | ✅ Pitch-Aware RNN-T DER 31,8 %; Tone-CAT 27,86 %; Tone-PT 26,05 % (arXiv 2606.22022) |
| **CAPT-APIs liefern Ton-Score + Ton-Prädiktion** | SpeechSuper, iFlytek | ✅ SpeechSuper: Initial-/Final-/Ton-Score je Zeichen + Ton-Prädiktion; Wort/Phrase „in Arbeit" |
| **Azure hat KEINEN separaten Ton-Score (zh-CN)** | Ton versteckt im Phonem-Score | ✅ bestätigt (Azure-Doku: SAPI-Phoneme, NBestPhonemes, kein Tone-Score) |
| **Kein Frontier-LLM auf isolierter Silben-Tonebene gebenchmarkt** | MMSU aggregiert; Phonologie schwach | ✅ MMSU: Gemini-1.5-Pro 60,68 %, Phonologie nur **53,6 %**, GPT-4o-Audio 56,38 %, Mensch 89,72 % |

## Tool-Verifizierung (neu hinzugekommen ggü. gestern)

| Tool | Kategorie | Verifiziert? | Notiz |
|---|---|---|---|
| **SpeechSuper API** | CAPT (Audio→Ton-Score) | ✅ | Initial/Final/Ton getrennt + Ton-Prädiktion; kommerziell, Free-Trial |
| **iFlytek ISE / 讯飞** | CAPT (Audio→Ton-Score) | ✅ | Rückgrat des offiziellen PSC-Examens; phone_score, tone_score |
| **Microsoft Azure Pron. Assessment** | CAPT (Audio→Phonem) | ✅ | NBestPhonemes (IPA/SAPI), **kein** separater Ton-Score für zh-CN |
| **CNN-Tonklassifikator** (alicex2020) | Akademisch (Audio→Ton) | ✅ | Tone Perfect, bis 99,8 % (nur native, isoliert) |
| **ToneNet** (CNN, Interspeech 2019) | Akademisch (Audio→Ton) | ✅ | ~97–99 %, SCSC-Korpus |
| **wav2vec2-Tonklassifikator** (Yuan 2021) | Akademisch (Audio→Ton) | ✅ | 6 % Tonfehlerrate, 50 % Reduktion ggü. Vorarbeit |
| **Pitch-Aware RNN-T** (Wang MDD, IS 2024) | Akademisch L2-MDD | ✅ | HuBERT+Pitch, eval. auf LATIC, DER ~31,8 % |
| **Tone-CAT / Tone-PT** (arXiv 2606.22022) | Akademisch L2-MDD | ✅ | DER 27,86 % / 26,05 %, segmentale AER bis 1,83 % |
| **Parselmouth** | Pitch (Praat in Python) | ✅ | Nativer C-Zugriff auf Praat-Kern, für Batch-Pipelines |
| **ProsodyPro** (Yi Xu, UCL) | Pitch (F0-Kontur) | ✅ | Zeitnormalisierte F0-Konturen (10 Punkte/Silbe), keine Auto-Klassifikation |
| **cpp-pinyin** | G2P (Text→Pinyin) | ✅ | 90,3 % „mit Ton" (CPP-Datensatz), ohne Ton 99,95 % |
| **SpeakGoodChinese (SGC)** | CAPT (Praat-basiert) | ✅ | ~85 % Tonerkennung bisilbisch; älter, Forschungstool |
| **MMSU** (Benchmark) | LLM-Eval | ✅ | 22 SpeechLLMs, 47 Tasks, 5.000 Audio-QA |
| **TonePerfect-App / MandaTone / Yutone** | Lern-Apps | ⚠️ teilw. | Echte Produkte, **keine** Peer-Review-Zahlen → nur als Kontext nennen |
| **„9M CAPT model" (98,29 %)** | Blog/Show-HN | ⚠️ unbelegt | Aus Blog, kein Peer-Review → nicht als Headline zitieren |

## Wichtigste Korrektur ggü. meinen Folien von gestern
> Gestern: „Audio→Pinyin+Ton: **KEIN Tool existiert**." → **zu pauschal.**
> Korrekt: Audio→**Ton** existiert (CAPT-APIs, CNN/HuBERT-Klassifikatoren), aber (a) als isolierte **Ton-Klassifikation/-Scores**, nicht als vollständige Pinyin+Ton-**Transkription**; (b) **nur für native, isolierte Silben gelöst** — bei L2 DER 26–32 %; (c) **kein Frontier-Audio-LLM** wurde auf isolierter Silben-Tonebene systematisch evaluiert. → **stärkere, präzisere Lücke.**

---

# TEIL I: FOLIEN

---

## Folie T1 — Die Tool-Landschaft in 3 Ebenen (Tiers)

**Titel:** Mandarin-Transkription & Tonerkennung ohne LLMs — die 3 Tiers

```
                    EINGABE        →   AUSGABE              STATUS
─────────────────────────────────────────────────────────────────────────
TIER A   Text → Pinyin (+Ton)     g2pW, pypinyin,        ✅ gelöst (~99 %)
(G2P)    Pinyin ↔ IPA             cpp-pinyin, dragonmap.   = SOLL-Referenz

TIER B   Audio → Ton-Klasse       CAPT-APIs (SpeechSuper, ⚠️ ZWEIGETEILT:
(Akustik)  / Ton-Score            iFlytek), CNN/HuBERT      native ✅ ~99 %
                                  /wav2vec2-Klassifikator   L2 ❌ DER 26–32 %

TIER C   Audio → F0-Kontur        Praat, Parselmouth,     ✅ Messung gelöst
(Signal)   (Pitch)               ProsodyPro, MFA          ❌ Klassifik. selbst
─────────────────────────────────────────────────────────────────────────
NICHT vorhanden:
  • Audio → vollständige Pinyin+Ton-TRANSKRIPTION (Silbe+Ton, robust für L2)
  • Frontier-Audio-LLM systematisch auf isolierter Silben-Tonebene gebenchmarkt
                                                          ← FORSCHUNGSLÜCKE
```

**Sprechernotiz:** „Die Tool-Landschaft hat drei Ebenen. Tier A wandelt Text in Pinyin um — g2pW erreicht 99 Prozent. Das ist aber die kanonische Soll-Aussprache, nicht das, was tatsächlich gesprochen wurde. Tier B ist die spannende Ebene: Tools, die aus Audio den Ton bestimmen — kommerzielle APIs wie SpeechSuper und akademische CNN-Klassifikatoren. Hier ist der Befund zweigeteilt: Für Muttersprachler mit isolierten Silben ist das praktisch gelöst, bis 99 Prozent. Aber für L2-Lerner brechen alle Werte ein, mit Diagnose-Fehlerraten von 26 bis 32 Prozent. Tier C misst die physikalische Pitch-Kontur mit Praat — die Messung ist gelöst, aber die Klassifikation in Tonkategorien muss man selbst bauen. Und ganz unten: Was fehlt? Ein Tool, das Audio robust in vollständige Pinyin-plus-Ton-Transkription umwandelt — gerade für L2. Und kein Frontier-LLM wurde je auf isolierter Silben-Tonebene gebenchmarkt. Das ist meine Lücke."

---

## Folie T2 — Tier A: Text → Pinyin & IPA (die SOLL-Referenz)

**Titel:** Tier A — Text→Pinyin: gelöst, aber nur die kanonische Referenz

**Tabelle (Folie):**

| Tool | Typ | Genauigkeit „mit Ton" | Polyphone (多音字) |
|---|---|---|---|
| **g2pW** (2022) ⭐ | BERT | **99,08 %** (CPP-Benchmark) | ✅ kontext-sensitiv |
| **g2pM** (2020) | Bi-LSTM | 97,31 % | ✅ kontext-sensitiv |
| **cpp-pinyin** | wörterbuchbasiert | 90,3 % (ohne Ton 99,95 %) | ⚠️ begrenzt |
| **pypinyin** | regelbasiert + Sandhi-Modul | ~87 % (Satzkorpus) | ⚠️ schwach |
| **dragonmapper / pinyin-to-ipa** | Pinyin→IPA | deterministisch | — (reine Abbildung) |

**Kernunterscheidung:**
```
Tier A liefert:  was gesagt werden SOLLTE  (kanonisch, aus dem Wörterbuch)
NICHT:           was tatsächlich GEHÖRT wurde (das macht Tier B)
```

**Bullets:**
- g2pW ist **State-of-the-Art** und meine **Baseline für RQ1** (Text→Pinyin)
- Bei isolierten Vokabeln ist die Soll-Referenz faktisch perfekt (Wort bekannt → Wörterbuch greift)
- **Wichtig:** g2pW „rät" den Standardton des Zeichens — es **hört nicht** den gesprochenen Ton

**Sprechernotiz:** „Tier A ist gelöst. g2pW nutzt BERT, um Polyphone kontextabhängig aufzulösen, und erreicht 99 Prozent auf dem Standard-Benchmark. Es ist meine Baseline für die erste Forschungsfrage, Text zu Pinyin. Aber — und das ist der entscheidende Punkt — Tier A liefert nur die kanonische Soll-Aussprache aus dem Wörterbuch. Es sagt mir, wie ein Wort ausgesprochen werden sollte, nicht, was ein Sprecher tatsächlich produziert hat. Für die Audio-Aufgabe brauche ich Tier B."

---

## Folie T3 — Tier B: CAPT-APIs — kommerzielle Aussprachebewertung

**Titel:** Tier B (kommerziell) — APIs, die aus Audio den Ton bewerten

**Tabelle (Folie):**

| API | Ton-Score? | Ton-Prädiktion? | Granularität | IPA-Ausgabe? | L2-Fehlerrate |
|---|---|---|---|---|---|
| **SpeechSuper** | ✅ separat | ✅ („du sagtest T2, soll T4") | Phonem/Silbe/Wort | ⚠️ | ❌ nicht publiziert |
| **iFlytek ISE** (讯飞) | ✅ tone_score | ✅ | Frame (10 ms) | ❌ | ❌ nicht publiziert |
| **Microsoft Azure** | ❌ **keiner** (zh-CN) | ❌ | Phonem/Silbe/Wort | ✅ NBestPhonemes | ❌ nicht publiziert |
| **SpeakGoodChinese** | ✅ (Praat-basiert) | ⚠️ | Mono-/Bisilbe | — | ~85 % (Studie, native) |

**Bullets:**
- **SpeechSuper/iFlytek** sind dem chinesischen EdTech-Markt entsprungen → **echte, getrennte Ton-Bewertung** (Initial / Final / Ton). iFlytek treibt das offizielle PSC-Staatsexamen.
- **Azure** ist stark bei Segmenten (N-Best-Phoneme im IPA-Format), hat aber **keinen separaten Ton-Score** für Chinesisch → Tonfehler verstecken sich im Phonem-Score.
- **Black-Box-Problem:** Keine dieser APIs publiziert **unabhängig verifizierte Tonfehlerraten** für L2 → vor Nutzung selbst auf Pilotstichprobe validieren.

**Sprechernotiz:** „Auf der kommerziellen Seite von Tier B gibt es spezialisierte Aussprachebewertungs-APIs. Die beiden chinesischen Marktführer, SpeechSuper und iFlytek, sind genau auf Mandarin zugeschnitten: Sie bewerten Initial, Final und Ton getrennt und liefern sogar eine Ton-Prädiktion — sie sagen dem Lerner: ‚Du hast Ton 2 produziert, korrekt wäre Ton 4.' iFlytek ist so etabliert, dass es das offizielle chinesische Sprachexamen antreibt. Microsoft Azure ist technisch exzellent bei Segmenten und gibt sogar IPA-Alternativen mit Konfidenzwerten zurück — aber für Chinesisch hat Azure keinen separaten Ton-Score, der Ton versteckt sich im Phonem-Score. Das große Problem aller drei: Es sind Black Boxes ohne publizierte Fehlerraten. Bevor ich sie als Cross-Check nutze, muss ich sie selbst auf einer Stichprobe validieren."

---

## Folie T4 — Tier B: Akademische Tonklassifikatoren — native = gelöst

**Titel:** Tier B (akademisch) — Native + isolierte Silben: praktisch gelöst

**Tabelle (Folie):**

| Modell | Architektur | Datensatz | Genauigkeit / TER | Sprecher |
|---|---|---|---|---|
| **CNN** (alicex2020) | 5-Layer CNN | Tone Perfect | **99,8 %** | native |
| **HuBERT-Tonklassifik.** | fine-getunt | Tone Perfect | ~100 % Ton, 98 % Silbe | native |
| **ToneNet** (IS 2019) | CNN + MLP | SCSC | 97–99,16 % | native |
| **wav2vec2** (Yuan 2021) | fine-getunt | Mandarin | **6 % TER** | native |
| **Systematic Review 2025** | 61 Studien (PRISMA) | div. | Ø DL 88,8 % vs. klassisch 83,1 % | meist native |

**Kernaussage:**
```
Isolierte Silbe + Muttersprachler  →  Tonklassifikation ≈ GELÖST (99–100 %)
Durchgängig schwächster Ton: Ton 3 (tief-fallend-steigend)
```

**Bullets:**
- Für **Phase 1 (Tone Perfect, native)** kann ein CNN als **automatischer Cross-Check** dienen (≈99 % gegen menschliche Labels, frei reproduzierbar)
- **Schwellenwert:** Wenn Klassifikator-Genauigkeit gegen Tone-Perfect-Labels >98 %, ist manuelle Annotation für Phase 1 verzichtbar
- Diese Werte sind die **idealen Nicht-LLM-Baselines** für meine Vergleichstabelle (Spezialmodell ~99 % vs. LLM ?)

**Sprechernotiz:** „Auf der akademischen Seite von Tier B ist die Tonklassifikation für Muttersprachler mit isolierten Silben praktisch gelöst. Ein simples CNN auf dem Tone-Perfect-Datensatz erreicht 99,8 Prozent, fine-getunte HuBERT-Modelle melden bis zu 100 Prozent. Eine systematische Review von 61 Studien bestätigt: Deep Learning erreicht im Schnitt 89 Prozent, bei isolierten Silben bis 99 Prozent. Durchgängig der schwierigste Ton ist Ton 3 — der tief-fallend-steigende. Für meine Arbeit hat das zwei Konsequenzen: Erstens kann ich für Phase 1, die Muttersprachler-Daten, einen CNN-Klassifikator als automatischen Cross-Check nutzen — wenn er über 98 Prozent gegen die bekannten Labels liegt, spare ich mir die manuelle Annotation. Zweitens sind diese 99-Prozent-Werte die perfekten Nicht-LLM-Baselines: Spezialmodell 99 Prozent — wie schlägt sich ein generalistisches LLM dagegen?"

---

## Folie T5 — Tier B: Der L2-Einbruch — warum Annotation teuer bleibt

**Titel:** L2-Lerner — hier brechen ALLE Tools ein (DER 26–32 %)

**Tabelle (Folie):**

| System | Architektur | Eval-Korpus (L2) | Diagnose-Fehlerrate (DER) |
|---|---|---|---|
| **Pitch-Aware RNN-T** (Wang, IS 2024) | HuBERT + Pitch-Fusion | LATIC | **31,80 %** |
| **Tone-CAT** (arXiv 2606.22022) | Phonolog. wav2vec2 | LATIC | **27,86 %** |
| **Tone-PT** (arXiv 2606.22022) | Phonolog. wav2vec2 | LATIC | **26,05 %** |
| **Siamese ResNet** (2025) | Referenz-vs-Test | — | MSE 0,189 (korreliert mit Mensch) |

**Kontrast:**
```
Native isolierte Silbe:  99 %+ Genauigkeit   →  Tool als Cross-Check OK
L2-Lerner:               DER 26–32 %          →  Mensch unverzichtbar
                                                 (Tool nur als Vor-Filter)
```

**Bullets:**
- **Warum?** L2-Realisierungen weichen stark ab (aspiriert/unaspiriert verwechselt, Ton 3 zu flach → klingt wie Ton 2), + Datenknappheit
- **Konsequenz für Phase 2 (L2):** Menschliche Annotation **bleibt nötig**; Tool nur als Vorab-Label + Doppelannotation
- **Schwellenwert für Vollautomatik:** erst wenn Tool–Mensch-Agreement (Cohens κ) >0,85 auf Pilotstichprobe
- **Das ist genau, warum mein L2-Experiment wertvoll ist:** Können LLMs hier besser/anders abschneiden als spezialisierte MDD-Systeme?

**Sprechernotiz:** „Und jetzt der entscheidende Kontrast. Sobald wir von Muttersprachlern zu L2-Lernern wechseln, brechen alle Tools ein. Die besten spezialisierten Mispronunciation-Detection-Systeme — Pitch-Aware RNN-T, Tone-CAT, Tone-PT, alle auf dem L2-Korpus LATIC evaluiert — haben Diagnose-Fehlerraten zwischen 26 und 32 Prozent. Warum? Weil L2-Lerner Laute produzieren, die das Modell nie im Training gesehen hat: Ein zu flacher Ton 3 klingt wie ein Ton 2, Aspiration wird verwechselt, dazu kommt massive Datenknappheit. Die Konsequenz für meine Arbeit ist klar: Für die L2-Phase bleibt menschliche Annotation unverzichtbar — die Tools taugen nur als Vorab-Filter, nicht als Ground Truth. Erst bei einem Tool-Mensch-Agreement über Kappa 0,85 könnte man automatisieren. Und genau das macht mein L2-Experiment wertvoll: Können multimodale LLMs hier vielleicht anders oder besser abschneiden als die spezialisierten Systeme?"

---

## Folie T6 — Tier C: F0-Kontur — die physikalische Wahrheit

**Titel:** Tier C — Pitch-Analyse: Praat, Parselmouth, ProsodyPro, MFA

**Tabelle (Folie):**

| Tool | Aufgabe | Output | Für unsere Arbeit |
|---|---|---|---|
| **Praat** | F0-Extraktion (Autokorrelation) | Pitch-Kontur, Spektrogramm | Goldstandard, manuelle L2-Stichprobe |
| **Parselmouth** | Praat-Kern in Python | F0-Werte programmatisch | **Batch-Pipeline** für alle Samples |
| **ProsodyPro** (Yi Xu) | zeitnormalisierte F0 | 10 Punkte/Silbe, F0-Velocity, Range | **Soll-vs-Ist-Konturvergleich** |
| **MFA** (Montreal Forced Aligner) | Audio+Text→Zeitgrenzen | TextGrid (Phonem-Timing) | **Segmentierung** vor F0-Analyse |

**Visuelles Beispiel (T3-Diagnose):**
```
Ton 3 (Soll, 214):   ╲___╱      L2-Lerner (Ist):  ╲___      → „Anstieg fehlt"
                     tief-steigend                 nur tief-fallend (≈ T2-Verwechslung)
```

**Bullets:**
- **Pipeline:** MFA (Segmentierung) → Parselmouth/ProsodyPro (F0) → Vergleich gegen Tone-Perfect-Referenzkonturen
- Liefert **quantitatives, erklärbares** Feedback: „Ton-2-Anstieg begann zu spät" — Rohdaten für künftigen Vokabeltrainer
- **Aber:** Tier C **klassifiziert nicht selbst** in Tonkategorien — das muss man aufsetzen (CNN oder Regel)
- **MFA-Einschränkung:** Akustikmodelle sind auf native Aussprache trainiert → bei starken L2-Abweichungen kann das Alignment verfehlen

**Sprechernotiz:** „Tier C ist die Signalebene — die physikalische Wahrheit der Tonhöhe. Praat ist der phonetische Goldstandard zur F0-Extraktion, und Parselmouth bringt den Praat-Kern nach Python, sodass ich tausende Samples automatisch verarbeiten kann. ProsodyPro normalisiert die Konturen zeitlich auf zehn Punkte pro Silbe — erst dadurch kann ich die Kurve eines L2-Lerners direkt über die Muttersprachler-Sollkurve legen und sehen: Hier fehlt der Anstieg von Ton 3. Der Montreal Forced Aligner liefert die nötigen Zeitgrenzen, damit ich überhaupt weiß, wo im Audiosignal die Silbe liegt. Diese Pipeline gibt mir quantitatives, erklärbares Feedback — die Rohdaten für den späteren Vokabeltrainer. Wichtig ist aber: Tier C misst nur, es klassifiziert nicht. Die Einteilung in Tonkategorien muss ich selbst aufsetzen. Und der Aligner ist auf native Aussprache trainiert, kann also bei starken L2-Abweichungen die Grenzen verfehlen."

---

## Folie T7 — Warum die Whisper-Pipeline Töne versteckt

**Titel:** Die naheliegende Pipeline (Whisper→g2pW) — und ihr blinder Fleck

```
Audio "mǎ" (Ton 3, native)
  └─→ Whisper large-v3 → 马 → g2pW → mǎ (T3) ✓   funktioniert für native

ABER bei L2-Lerner:
Audio "má" (Ton 2 GESPROCHEN, gemeint war 马/Ton 3)
  └─→ Whisper → 马 (rät das ZEICHEN) → g2pW → mǎ (T3)
                                        ↑ Tonfehler VERSCHWINDET
                                          (Whisper wählt wahrscheinlichstes Zeichen,
                                           g2pW gibt dessen STANDARDton aus)

Unsere Methode (multimodales LLM):
  └─→ Prompt „Transcribe to Pinyin with tone number"
       → "ma2" ← direkt prüfbar gegen Ground Truth "ma3" ✓
```

**Bullets:**
- **Standard-ASR (Whisper, iFlytek-ASR) gibt Zeichen aus**, keine expliziten Tonlabels → für Ton-Cross-Check ungeeignet ohne Nachbearbeitung
- Die Pipeline Whisper→g2pW „repariert" L2-Fehler unbeabsichtigt → **Tonfehler werden unsichtbar**
- Spezielle Pinyin-ASR (Whisper+LoRA ~6,5 % WER, Branchformer 4,7 %) verbessert Pinyin, löst aber **nicht** das L2-Ton-Diagnoseproblem
- **Unser direkter Weg** (Audio→Pinyin+Ton via LLM) hält Tonfehler **sichtbar**

**Sprechernotiz:** „Die naheliegendste Pipeline ist Whisper plus g2pW: Audio rein, Hanzi raus, dann G2P zu Pinyin. Für Muttersprachler funktioniert das. Aber bei L2-Lernern hat es einen fatalen blinden Fleck. Wenn ein Lerner ‚má' mit Ton 2 sagt, obwohl er das Zeichen 马 mit Ton 3 meint, dann wählt Whisper trotzdem das wahrscheinlichste Zeichen — 马 — und g2pW gibt brav dessen Standardton aus, Ton 3. Der Tonfehler ist spurlos verschwunden. Selbst spezialisierte Pinyin-ASR mit LoRA oder Branchformer verbessert nur die Pinyin-Genauigkeit, nicht die L2-Ton-Diagnose. Mein direkter Weg über ein multimodales LLM, das ich anweise, Pinyin mit Tonnummer auszugeben, hält den Fehler sichtbar — ich bekomme ‚ma2' und kann es direkt gegen den Ground Truth ‚ma3' prüfen."

---

## Folie T8 — Die LLM-Lücke: kein Frontier-Modell auf Silben-Tonebene

**Titel:** Die Benchmark-Lücke — LLMs sind bei Phonologie schwach & ungetestet

**Tabelle (Folie):**

| Benchmark | Was getestet | Bestes LLM | Mandarin-Ton isoliert? |
|---|---|---|---|
| **MMSU** (CUHK, ICLR 2026) | 47 Tasks, inkl. Phonologie | Gemini-1.5-Pro **60,68 %** (Mensch 89,72 %) | ❌ aggregiert, real-world |
| → Phonologie-Domäne | Ton/Prosodie/Phoneme | nur **53,60 %** | ❌ keine Per-Ton-Zahlen |
| → GPT-4o-Audio | — | **56,38 %** (< Open-Source!) | ❌ |
| **PRiSM** (2026) | Phonerkennung + LALMs | „LALMs lag behind specialized" | ❌ multilingual/IPA |
| **SSL-Probing** (2024–26) | Ton in HuBERT-Layern | F1 0,80–0,87 (mittlere Layer) | ⚠️ zeigt: Tokenisierung zerstört Ton |

**Kernbefund:**
```
• Frontier-Audio-LLMs erreichen bei Phonologie nur ~54 % (Mensch ~90 %)
• GPT-4o-Audio schlägt sich SCHLECHTER als Open-Source (Qwen2.5-Omni, Kimi-Audio)
• KEIN Paper benchmarkt LLMs auf isolierter Silben-Tonebene (T1/T2/T3/T4)
• SSL-Studien erklären WARUM: diskrete Tokenisierung zerstört Toninformation
```

**Sprechernotiz:** „Jetzt zur eigentlichen Lücke. Es gibt zwar Benchmarks, die Audio-LLMs testen — der wichtigste ist MMSU von der CUHK, 22 Sprach-LLMs über 47 Aufgaben. Das beste Modell, Gemini-1.5-Pro, erreicht insgesamt nur 60 Prozent, während Menschen bei 90 liegen. Und in der Phonologie-Domäne — Töne, Prosodie, Phoneme — fällt selbst das beste Modell auf 54 Prozent. Bemerkenswert: GPT-4o-Audio schneidet schlechter ab als Open-Source-Modelle wie Qwen und Kimi. Aber — und das ist mein Einstieg — MMSU testet aggregiert mit Real-World-Audio, es gibt keine Zahlen für isolierte Silben und keine Aufschlüsselung pro Ton. Kein einziges Paper benchmarkt Frontier-LLMs auf isolierter Silben-Tonebene. SSL-Probing-Studien liefern sogar die Erklärung, warum LLMs hier scheitern könnten: Die diskrete Tokenisierung, die LLMs nutzen, zerstört gerade die feine Toninformation, die in den mittleren Layern eigentlich vorhanden wäre."

---

## Folie T9 — Die geschärfte Forschungslücke & unsere Positionierung

**Titel:** Was genau fehlt — und wie wir uns abgrenzen

**Tabelle (Folie):**

| Arbeit | LLM? | Audio→Pinyin+Ton? | Isolierte Silbe? | Per-Ton? | L2? |
|---|---|---|---|---|---|
| **CNN/HuBERT** (Tone Perfect) | ❌ | ⚠️ nur Ton-Klasse | ✅ | ✅ | ❌ |
| **SpeechSuper/iFlytek** | ❌ | ⚠️ Ton-Score | ✅ | ✅ | ⚠️ Black-Box |
| **MMSU** | ✅ | ❌ | ❌ aggregiert | ❌ | ❌ |
| **Tone-CAT/PT** (MDD) | ❌ | ⚠️ Diagnose | ⚠️ | ⚠️ | ✅ DER 26 % |
| **Unsere Arbeit** | ✅ | ✅ direkt | ✅ Tone Perfect | ✅ TER + Konfusion | ✅ LATIC |

**Positionierungs-Satz:**
> „Erstes systematisches **isolierte-Silben-Mandarin-Ton-+-Phonem-Benchmark von Frontier-Audio-LLMs** — abgegrenzt von MMSU (aggregierte Phonologie), PRiSM (multilinguale Phonerkennung) und SSL-Probing (kein instruction-tuned LLM)."

**Sprechernotiz:** „Diese Tabelle bringt die Abgrenzung auf den Punkt. Die akademischen CNN-Klassifikatoren und die kommerziellen APIs können Töne aus Audio bestimmen — aber sie sind keine LLMs und geben nur eine Ton-Klasse oder einen Score, keine vollständige Transkription. MMSU testet LLMs, aber aggregiert und ohne isolierte Silben. Die MDD-Systeme testen L2, sind aber keine LLMs. Meine Arbeit füllt jede Spalte gleichzeitig: ein multimodales LLM, das Audio direkt in Pinyin plus Ton umwandelt, auf isolierten Silben aus Tone Perfect, mit einer eigenen Tonfehlerrate und Konfusionsmatrix, und in der zweiten Phase auf L2-Daten aus LATIC. Ich positioniere das als das erste systematische Benchmark von Frontier-Audio-LLMs auf isolierter Silben-Tonebene für Mandarin — klar abgegrenzt von MMSU, PRiSM und den Probing-Studien."

---

## Folie T10 — Unser Tool-Stack: Baseline + Cross-Check + Evaluation

**Titel:** Wie wir die Tools nutzen — drei Rollen

```
ROLLE 1: NICHT-LLM-BASELINE (Vergleichsmaßstab)
  Text  → g2pW (99 %)                          → RQ1-Baseline
  Audio → CNN/HuBERT-Tonklassifik. (~99 % native) → Tier-B-Baseline

ROLLE 2: AUTOMATISCHER CROSS-CHECK (Annotationskosten senken)
  Phase 1 (native):  Tone-Perfect-Labels + CNN-Klassifik. → manuelles Labeln entfällt (κ>0,85)
  Phase 2 (L2):      SpeechSuper/iFlytek als VOR-Label → Mensch prüft Stichprobe + Disagreements

ROLLE 3: EVALUATION DES LLM-OUTPUTS
  LLM → pinyin-to-ipa → PanPhon  → PFER (artikulatorisch gewichtet)
  LLM vs. Ground Truth: Tonvergleich → TER ;  Zeichen → CER (Literatur)
  Strittige L2-Fälle:  MFA → Parselmouth/ProsodyPro → F0-Kontur (Schiedsrichter)
```

**Bullets:**
- **Komplett kostenlos möglich:** g2pW, CNN, MFA, Parselmouth, ProsodyPro, PanPhon = Open Source; CAPT-APIs nur als optionaler Gegencheck (Free-Trials)
- **Kostenkontrolle (passt zum <50 €-Budget):** keine teuren manuellen Annotatoren für Phase 1
- **PanPhon** bleibt zentral für faire **PFER** (Near-Miss m↔n wird milder bestraft als m↔ts)

**Sprechernotiz:** „Zum Abschluss: So setze ich die Tools konkret ein, in drei Rollen. Erstens als Nicht-LLM-Baseline — g2pW für Text, der CNN-Klassifikator für Audio. Daran messe ich, ob ein LLM überhaupt mithalten kann. Zweitens als automatischer Cross-Check, um Annotationskosten zu senken: In Phase 1 mit Muttersprachlern brauche ich dank der bekannten Tone-Perfect-Labels und dem 99-Prozent-CNN gar kein manuelles Labeling. In Phase 2 mit L2-Lernern nutze ich SpeechSuper als Vorab-Label, aber ein Mensch prüft eine Stichprobe und alle Fälle, wo die Tools uneinig sind. Drittens als Evaluations-Werkzeuge: Den LLM-Output wandle ich über pinyin-to-ipa und PanPhon in eine phonetisch gewichtete Fehlerdistanz, die PFER. Dazu TER für die Tonebene und CER für den Literaturvergleich. Und bei strittigen L2-Fällen ist die Praat-Pipeline mein Schiedsrichter. Das Schöne: Bis auf die optionalen API-Gegenchecks ist alles Open Source — das passt zu meinem Budget von unter 50 Euro."

---

# TEIL II: Anmerkungen für Stefan

- **Vortragbare Folien:** T1–T10. **Kürzungsoption** (auf ~6): T1, T2, dann T3+T4+T5 zu „Tier B" zusammenziehen, T7, T8, T10.
- **Die wichtigste neue Botschaft an Tim** (ggü. gestern verschärft):
  1. Audio→Ton-Tools **existieren** — aber gespalten: **native gelöst (~99 %), L2 kaputt (DER 26–32 %)**.
  2. Daraus folgt direkt: Phase 1 braucht **kein** teures Labeling (CNN-Cross-Check reicht); Phase 2 **schon** (Mensch unverzichtbar).
  3. Die LLM-Lücke ist **schärfer als „kein Tool"**: kein Frontier-LLM wurde je auf isolierter Silben-Tonebene gebenchmarkt (MMSU-Phonologie nur 53,6 %).
- **Verifizierte Headline-Zahlen (Q&A-fest, Stand 2026-06-29):**
  - MMSU: Gemini-1.5-Pro 60,68 % (Phonologie 53,6 %), GPT-4o-Audio 56,38 %, Mensch 89,72 % → arXiv 2506.04779
  - CNN Tone Perfect 99,8 % (alicex2020); ToneNet ~99 %; wav2vec2 6 % TER (Yuan 2021)
  - L2-MDD DER: Pitch-Aware RNN-T 31,8 % → Tone-CAT 27,86 % → Tone-PT 26,05 % (arXiv 2606.22022)
  - SpeechSuper: getrennter Ton-Score + Ton-Prädiktion; Wort/Phrase „in Arbeit"
  - Azure zh-CN: **kein** separater Ton-Score
- **Bewusst NICHT als Headline zitiert** (unbelegt): „9M-CAPT-Modell 98,29 %" (Blog/Show-HN); App-Genauigkeiten (TonePerfect/MandaTone/Yutone — keine Peer-Review-Zahlen). Nur als Markt-Kontext nennen.
- **Offene Validierungs-Aufgabe vor Nutzung:** CAPT-APIs auf eigener Pilotstichprobe prüfen (κ gegen menschliche Labels), bevor sie als L2-Cross-Check dienen.

## Quellen (online verifiziert, 2026-06-29)

- MMSU: [arXiv 2506.04779](https://arxiv.org/abs/2506.04779) | [HTML](https://arxiv.org/html/2506.04779v2)
- Phonological-Level Wav2Vec2 MDD (Tone-CAT/PT): [arXiv 2606.22022](https://arxiv.org/html/2606.22022v1) | [Speech Communication](https://www.sciencedirect.com/science/article/pii/S0167639325000640)
- Pitch-Aware RNN-T MDD (Wang, IS 2024): [arXiv 2406.04595](https://arxiv.org/abs/2406.04595)
- CNN Tone Classification (Tone Perfect, 99,8 %): [GitHub alicex2020](https://github.com/alicex2020/Mandarin-Tone-Classification) | [Tone Perfect MSU](https://tone.lib.msu.edu/related)
- ToneNet (IS 2019): [ISCA Archive](https://www.isca-archive.org/interspeech_2019/gao19c_interspeech.pdf)
- Systematic Review ML Mandarin Tone (2025): [Preprints.org 202510.2478](https://www.preprints.org/manuscript/202510.2478)
- SpeechSuper Tone Prediction: [Medium](https://medium.com/@speechsuper1024/unveiling-speechsupers-tone-prediction-feature-for-improved-mandarin-chinese-pronunciation-2b6c2e30e3cb) | [API Samples](https://github.com/speechsuper/SpeechSuper-API-Samples)
- iFlytek ISE: [Produkt](https://global.xfyun.cn/products/ise) | [API-Doku](https://global.xfyun.cn/doc/voiceservice/ise/API.html)
- Azure Pronunciation Assessment: [MS Learn](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-pronunciation-assessment)
- SpeakGoodChinese: [UvA](https://www.fon.hum.uva.nl/sgc/) | [IS2007 Poster](https://www.fon.hum.uva.nl/rob/Publications/is2007_sgc_Poster.pdf)
- Parselmouth: [Doku](https://parselmouth.readthedocs.io) | ProsodyPro (Yi Xu, UCL): [TRASP 2013](https://www.homepages.ucl.ac.uk/~uclyyix/yispapers/Xu_TRASP2013.pdf)
- g2pW: [GitHub](https://github.com/GitYCC/g2pW) | [Paper IS2022](https://arxiv.org/abs/2203.10430) · g2pM: [Paper IS2020](https://arxiv.org/abs/2004.03136)
- PanPhon: [GitHub](https://github.com/dmort27/panphon) | [Paper COLING 2016](https://aclanthology.org/C16-1328.pdf)
- pinyin-to-ipa / dragonmapper / epitran: [stefantaubert](https://github.com/stefantaubert/pinyin-to-ipa) | [tsroten](https://github.com/tsroten/dragonmapper) | [dmort27](https://github.com/dmort27/epitran)
- MFA: [Doku](https://montreal-forced-aligner.readthedocs.io) | [arXiv 2606.18466](https://arxiv.org/html/2606.18466v1)
- Praat: [praat.org](https://www.fon.hum.uva.nl/praat/)
