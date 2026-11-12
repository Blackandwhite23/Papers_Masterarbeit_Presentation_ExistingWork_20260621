# PowerPoint — Metrik-Methodik: Wer misst WAS, auf WELCHER Ebene, WIE?
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Vocabulary*

> **Zweck:** Ergänzungsfolien zu `powerpoint_evaluation_metrics.md` (M1–M6). Dort: **welche Metriken** existieren. Hier: **wie genau** jedes Paper seine Metrik berechnet — Referenz-Einheit, Hypothesen-Einheit, Granularität, Ton-Behandlung.
>
> **Basiert auf:** 9-Fragen-Analyse von 37 Papers (3 Batches: 12 ASR+Speech, 10 Tone/Pinyin/MDD, 15 MDD/Benchmark/Survey)

---

# TEIL I: FOLIEN

---

## Folie MM1 — Die zentrale Frage: Was vergleichst du eigentlich?

**Titel:** „CER" ist nicht gleich „CER" — die versteckte Referenz-Einheit

**Visuelles Diagramm (3 Zeilen):**
```
Audio: [diànnǎo]

Paper A (CER auf Hanzi):      Ref: 电脑    Hyp: 电闹    → CER = 50%
Paper B (CER auf Pinyin):     Ref: diannao  Hyp: diannao  → CER = 0%
Paper C (CER auf Pinyin+Ton): Ref: dian4nao3 Hyp: dian4nao2 → CER = 11%
```

**Bullets:**
- **Gleicher Name, verschiedenes Ergebnis:** „CER" in Seed-ASR (Hanzi) misst etwas völlig anderes als „CER" in Chirkova (IPA+Ton)
- 27 von 42 Papers vergleichen auf **Hanzi-Ebene** — phonetische Fehler sind unsichtbar
- Nur 8 Papers messen auf einer Ebene, die für Aussprache-Bewertung relevant ist
- **Kernproblem:** Ein Tonfehler (mā→má) erzeugt auf Hanzi-Ebene 0 % Fehler, auf Pinyin+Ton-Ebene aber einen klaren Fehler

**Sprechernotiz:** „Wenn ein Paper ‚CER' sagt, muss man fragen: CER auf WAS? In Seed-ASR oder FireRedASR ist es CER auf chinesischen Zeichen — ob das Hanzi stimmt. In Chirkovas Arbeit ist es CER auf IPA-Symbolen inklusive Tonmarker. In Zhengjies PYG-ASR ist es CER auf Pinyin, aber ohne Töne. Alle drei sagen ‚CER', alle drei messen etwas völlig Verschiedenes. Das Diagramm zeigt es am Beispiel diànnǎo: Auf Hanzi-Ebene wird ein Tonfehler gar nicht sichtbar, wenn das System zufällig das richtige Zeichen errät. Auf Pinyin+Ton-Ebene ist der Fehler klar messbar. Diese Unterscheidung wird in keinem der 42 Papers explizit diskutiert."

---

## Folie MM2 — Wer misst auf welcher Ebene? Übersichtskarte

**Titel:** 42 Papers — 5 Evaluations-Ebenen

**Visuelles Diagramm (horizontale Balken, sortiert nach Häufigkeit):**
```
EBENE                         PAPERS                               ANZAHL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Hanzi (Zeichen)      Seed, FireRed, FireRed2S, Kimi, Qwen3,          ~27
                     Step-Audio, Wei, TELEVAL, Benchmarks, Surveys...
────────────────────────────────────────────────────────────────────────
Wort (WER)           ContextASR, WildSpeech, Chirkova (auch)            3
────────────────────────────────────────────────────────────────────────
Pinyin (Silbe)       Zhengjie PYG-ASR, Chen SCCM                       2
────────────────────────────────────────────────────────────────────────
Phonem               Du Zipformer, Wang MDD, Zhu ZIPA                  3
────────────────────────────────────────────────────────────────────────
Ton (F0/Ton-Label)   Bu Siamese, Wu L2 Prosody, Zou Review             3
────────────────────────────────────────────────────────────────────────
Prosodie (Items)     Wang MSPB                                         1
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Sprechernotiz:** „Die Übersichtskarte zeigt ein klares Muster: Etwa 27 der 42 Papers messen auf Hanzi-Ebene — Zeichen rein, Zeichen raus, fertig. Auf der Pinyin-Ebene, also der Lautschrift-Silbe, arbeiten nur zwei Papers: Zhengjie und Chen. Auf Phonem-Ebene — wo einzelne Laute verglichen werden — sind es drei: Du mit seinem Zipformer, Wang mit dem Pitch-Aware MDD-Modell und Zhu mit ZIPA. Explizit den Ton als eigene Dimension messen nur drei Papers: Bu über F0-Konturen, Wu mit FRR/FAR pro Ton, und der Zou-Review, der TER empfiehlt. Je relevanter die Ebene für Aussprache-Bewertung wird, desto weniger Papers messen dort."

---

## Folie MM3 — Was genau wird verglichen? Die Referenz-Hypothese-Tabelle

**Titel:** Referenz vs. Hypothese — die 8 phonetisch relevanten Papers im Detail

**Tabelle (Folie):**

| Paper | Referenz (Goldstandard) | Hypothese (System-Output) | Vergleichs-Ebene | Metrik |
|---|---|---|---|---|
| **Du Zipformer** (2025) | Phonem-Sequenzen (66 Kat.) | Phonem-Sequenzen | Phonem | WER (=PER) |
| **Wang MDD** (2024) | Tonal-Phoneme (66 Kat., T1–T5) | Tonal-Phoneme | Phonem | PER, FRR, FAR, F1 |
| **Zhu ZIPA** (2025) | IPA-Phoneme (broad) | IPA-Phoneme | Phonem | **PFER** (gewichtet) |
| **Zhengjie PYG** (2025) | Pinyin (ohne Ton) | Pinyin + Hanzi (dual) | Silbe | Pinyin-ERR, CER |
| **Chen SCCM** (2025) | Pinyin (Initial+Final) | Pinyin + Hanzi | Silbe | CER, Pinyin-ERR |
| **Chirkova** (2025) | IPA + Pinyin (mit Ton) | IPA / Pinyin / Simple | Wort | tCER, tWER |
| **Bu Siamese** (2025) | F0-Konturen (Standard-Mand.) | Ton-Diskrepanz-Score | Silbe (F0) | MSE, RMSE |
| **Wu L2 Prosody** (2024) | Human-annotierte Töne (T1–T4) | Automatische Engines | Silbe | FRR, FAR, DA |

**Sprechernotiz:** „Diese Tabelle ist der Kern der Methodik-Analyse. Sie zeigt für die acht phonetisch relevanten Papers exakt, was als Goldstandard dient und was das System ausgibt. Bei Du sind es Phonemsequenzen aus 66 Kategorien — inklusive Tonphonem. Bei Zhengjie ist es Pinyin, aber ohne Tonmarker — ein wichtiger Unterschied. Bei Chirkova werden IPA-Transkriptionen mit Tönen verglichen, und sie definiert sogar eine eigene tonal CER, die nur Tonfehler zählt. Bei Bu werden gar keine diskreten Einheiten verglichen, sondern F0-Konturen als Vektoren — ein fundamentaler methodischer Unterschied. Was sofort auffällt: Kein Paper vergleicht Pinyin MIT Tonnummern als atomare Einheiten — also ‚ma1' gegen ‚ma2'. Das wäre genau die Ebene, die für einen Vokabeltrainer optimal wäre."

---

## Folie MM4 — Wie werden Töne behandelt? Vier Strategien

**Titel:** Vier Strategien für Töne — und ihre Konsequenzen

**Visuelles 2×2-Raster:**
```
┌──────────────────────────────────┬───────────────────────────────────┐
│  IGNORIERT (27 Papers)           │  IMPLIZIT IM PHONEM (3 Papers)   │
│                                  │                                   │
│  Seed, FireRed, Kimi, Qwen3,    │  Du Zipformer: Phonem-Set mit     │
│  Step-Audio, Surveys, Benchm.   │   Ton als Teil des Inventars      │
│                                  │  Wang MDD: 66 tonal phonemes      │
│  → Tonfehler = unsichtbar        │  Zhu ZIPA: Chao tone letters      │
│                                  │                                   │
│  "CER on characters treats       │  "tonal phonemes to assess        │
│  tones as invisible"             │  pronunciation with greater       │
│                                  │  granularity" — Wang (2024)        │
├──────────────────────────────────┼───────────────────────────────────┤
│  SEPARATE METRIK (4 Papers)      │  F0-BASIERT (2 Papers)            │
│                                  │                                   │
│  Chirkova: tCER, tWER           │  Bu: MSE/RMSE auf F0-Konturen     │
│  Wu: FRR/FAR pro T1–T4          │  Kang: Divergenzpunkt-Analyse     │
│  Zou: TER empfohlen (!)         │                                   │
│  Huang: Review (pro Ton)        │  → Akustisch, nicht symbolisch     │
│                                  │                                   │
│  "Confusion matrices reveal      │  "pitch contour for each syllable │
│  Tone 3 as most error-prone"    │  normalized on a 0-5 scale"       │
│  — Zou (2025)                    │  — Bu (2025)                      │
└──────────────────────────────────┴───────────────────────────────────┘
```

**Sprechernotiz:** „Es gibt genau vier Strategien, wie Papers mit Mandarin-Tönen umgehen. Die Mehrheit — 27 Papers — ignoriert sie komplett: CER auf Hanzi misst Zeichen, nicht Töne. Drei Papers betten den Ton implizit ins Phonem ein — bei Wang etwa ist ‚a1' ein anderes Phonem als ‚a2', beide aus einem 66-Phonem-Set mit fünf Tönen. Vier Papers messen Töne als eigene Dimension: Chirkova mit tonal CER, Wu mit FRR und FAR pro Ton, Zou empfiehlt TER. Und zwei Papers arbeiten akustisch auf F0-Konturen statt auf symbolischen Einheiten. Der Haken an Strategie 2 — implizit — ist: Man kann nicht nachher sagen, ob ein Fehler ein Segment-Fehler oder ein Ton-Fehler war, weil beides in einer Zahl verschmilzt. Strategie 3 — separat — ist die informativste, wird aber am seltensten angewandt. Diese Lücke motiviert meinen zweiachsigen Ansatz."

---

## Folie MM5 — Die Berechnung im Detail: Levenshtein und darüber hinaus

**Titel:** Wie wird gerechnet? — Von Levenshtein bis Feature-Distanz

**Drei Berechnungs-Strategien (visuell):**

```
STRATEGIE 1: Levenshtein (Standard)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Ref:  d i a n 4  n a o 3       (9 Token)
  Hyp:  d i a n 4  n a o 2       (9 Token)
                                ↑ 1 Substitution
  PER = S+D+I / N = 1/9 = 11,1%
  
  → Jeder Fehler kostet genau 1 (egal wie ähnlich)
  → Genutzt von: fast alle (CER, WER, PER, Pinyin-ERR, TER)

STRATEGIE 2: Feature-Distanz (PFER, nur Zhu ZIPA)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Ref: /d/  = [−cont, +voice, +alveolar, ...]  (24 Features)
  Hyp: /l/  = [+cont, +voice, +alveolar, ...]  (24 Features)
  
  Feature-Distanz(/d/, /l/) = Hamming(Ref, Hyp) / 24 = 1/24 = 0,042
  
  → Ähnliche Laute kosten weniger als verschiedene
  → "the top errors are substitution of vowels close
     in the vowel space" — Zhu (2025)

STRATEGIE 3: F0-Regression (nur Bu Siamese)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Ref:  F0-Vektor = [5, 5, 5, 5, 5]     (Tone 1: high level)
  Hyp:  F0-Vektor = [3, 5, 4, 2, 1]     (Tone 4: falling)
  
  MSE = Σ(Ref_i − Hyp_i)² / N = 5,2
  
  → Kontinuierliche Distanz statt kategorischer Fehler
  → "MSE and RMSE between model evaluation and 
     expert evaluators" — Bu (2025)
```

**Sprechernotiz:** „Alle 37 untersuchten Papers verwenden eine von drei Berechnungsstrategien. Erstens, mit Abstand am häufigsten: Levenshtein-Distanz. Das ist egal ob CER, WER, PER oder TER — immer wird die Editierdistanz berechnet, jede Substitution, Deletion oder Insertion kostet genau 1. Wei's ASR-EC Paper ist das einzige, das die S/D/I-Anteile separat aufschlüsselt. Zweitens, nur in Zhus ZIPA-Paper: Feature-Distanz auf 24 artikulatorischen Merkmalen aus PanPhon. Statt ‚falsch' oder ‚richtig' gibt es ein Maß der phonetischen Ähnlichkeit. Drittens, nur bei Bu: eine Regression auf F0-Konturen, also auf den Tonhöhenverlauf. Das ergibt einen kontinuierlichen MSE-Wert statt einer diskreten Fehlerrate. Für meine Arbeit ist die erste Frage: Auf welchen Token wird Levenshtein berechnet — Hanzi, Pinyin, Pinyin+Ton, oder Phonem? Die zweite Frage: Soll man gewichten wie PFER, oder jeder Fehler gleich?"

---

## Folie MM6 — Die Gewichtungslücke: Alle Fehler gleich?

**Titel:** 41 von 42 Papers: Jeder Fehler kostet genau 1

**Bullets:**
- **Standard (41 Papers):** Alle Substitutionen, Deletionen, Insertionen kosten gleich viel
- **Einzige Ausnahme:** Zhu ZIPA — PFER gewichtet nach phonetischer Feature-Distanz (24 Merkmale)
- **Partielle Ausnahme:** Seed-ASR — WWER gewichtet Keyword-Fehler stärker (aber nicht phonetisch)

**Konsequenz für Aussprache-Bewertung:**
```
Standard (ungewichtet):
  "dian" → "tian"  (d↔t, stimmhaft↔stimmlos)     = 1 Fehler
  "dian" → "zhao"  (komplett anderer Laut)          = 1 Fehler
  → Identische Bestrafung!

Gewichtet (PFER-Stil):
  "dian" → "tian"  Feature-Distanz = 0.042          = milder Fehler
  "dian" → "zhao"  Feature-Distanz = 0.375          = schwerer Fehler
  → Phonetisch faire Bewertung
```

**Sichtbares Zitat:**
> *"the top errors are the substitution of vowels that are close in the vowel space"* — Zhu et al., ZIPA (2025), Sec. 7

**Sprechernotiz:** „Von 42 Papers gewichtet genau eines Fehler nach phonetischer Ähnlichkeit: Zhus ZIPA mit PFER. Alle anderen behandeln jede Verwechslung gleich — ob ein Lerner ‚dian' statt ‚tian' sagt, also nur den Stimmhaftigkeitsunterschied verwechselt, oder ob er ‚dian' statt ‚zhao' sagt, also einen komplett anderen Laut produziert, beides ist genau 1 Fehler. Für einen Aussprache-Trainer ist das unfair: Der erste Fehler ist ein kleiner Patzer, der zweite ein fundamentales Problem. Seed-ASR gewichtet zwar auch, aber nach Keyword-Wichtigkeit, nicht nach phonetischer Ähnlichkeit — das hilft für Diktierfunktionen, nicht für Aussprache. Die Forschungslücke ist klar: Gewichtete tonale Metriken existieren nicht."

---

## Folie MM7 — Tonale Evaluation: Was fehlt

**Titel:** Gewichtete Ton-Fehler? — Niemand.

**Visualisierung (Tone Confusion Landscape):**
```
                   EXISTIERT                FEHLT
                   ─────────               ─────
Ton-Klassifikation  ✅ Zou (2025):          
(T1/T2/T3/T4)      "Tone 3 most            
                    error-prone"            

Ton-Fehlerrate     ✅ Chirkova (2025):      ❌ TER auf Mandarin
pro Ton            tCER, tWER für Baima         + LLM

FRR/FAR pro Ton    ✅ Wu (2024):            ❌ FRR/FAR für
                   T1: 25%/16%                  LLM-Transkription
                   T2: 19%/8%
                   T3: 30%/13%
                   T4: 15%/7%

Gewichtete          ❌ NIEMAND              ❌ T2↔T3 anders
Ton-Distanz                                      als T1↔T4
```

**Sichtbares Zitat:**
> *"Confusion matrices consistently reveal Tone 3 as the most error-prone category"* — Zou et al. (2025), S. 10

**Sprechernotiz:** „Die tonale Evaluation ist der schwächste Punkt der gesamten Literatur. Zou zeigt in seinem Review über 61 Studien, dass Ton 3 durchgängig die meisten Fehler verursacht — in Confusion Matrices sieht man das klar. Wu liefert FRR und FAR pro Ton und zeigt, dass Ton 3 mit 30 % die höchste False Rejection Rate hat. Chirkova definiert sogar eine eigene tonal CER — aber für Baima, nicht Mandarin. Was komplett fehlt: Erstens, TER als eigene Metrik auf Mandarin-LLM-Transkription. Zweitens, und das ist meine zentrale Erkenntnis, gewichtete Ton-Fehler. Niemand modelliert, dass eine Verwechslung von Ton 2 und Ton 3 — die akustisch ähnlich sind — anders zu bewerten ist als eine Verwechslung von Ton 1 und Ton 4, die akustisch maximal verschieden sind. Das ist die Forschungslücke, die meine Arbeit adressiert."

---

## Folie MM8 — Unsere Methodik: Zweiachsige Evaluation

**Titel:** Unser Ansatz — Segment und Ton entkoppelt, fair gewichtet

**Visuelles Diagramm (Pipeline):**
```
Audio → LLM → "ma2"

            ┌─── ACHSE 1: Segment ───────────────────────────┐
            │                                                 │
            │  Ref: "ma"    Hyp: "ma"   → SyllER = 0%        │
            │  (oder: Ref: m-a  Hyp: m-a → PER_seg = 0%)     │
            │                                                 │
            │  Optional: PFER (gewichtet, Zhu-Stil)           │
            └─────────────────────────────────────────────────┘
            
            ┌─── ACHSE 2: Ton ────────────────────────────────┐
            │                                                 │
            │  Ref: Ton 1   Hyp: Ton 2   → ToneAcc = 0%      │
            │                             → TER = 1/1 = 100%  │
            │                                                 │
            │  Per-Ton-F1: T1=?, T2=?, T3=?, T4=?             │
            │  Tone Confusion Matrix: welche Töne verwechselt? │
            └─────────────────────────────────────────────────┘

            ┌─── KOMBINIERT ──────────────────────────────────┐
            │  Silbe "ma" korrekt + Ton 1≠2 falsch            │
            │  → Combined Accuracy = 0%                        │
            └─────────────────────────────────────────────────┘
```

**Bullets:**
- **Achse 1 (Segment):** Sind Konsonant+Vokal korrekt? → Syllable Error Rate oder PER (segmental)
- **Achse 2 (Ton):** Ist der Ton korrekt? → TER + per-Ton-F1 + Confusion Matrix
- **Kombiniert:** Beides korrekt? → Combined Accuracy
- **Neu im LLM-Kontext:** Diese Entkopplung wurde für multimodale Foundation Models noch nie durchgeführt

**Sprechernotiz:** „Mein Ansatz entkoppelt die Evaluation in zwei Achsen. Achse 1 — das Segment: Stimmen Konsonant und Vokal? Das messe ich über Syllable Error Rate oder segmentale PER, also Pinyin ohne Ton. Achse 2 — der Ton: Stimmt der Ton? Das messe ich über TER plus per-Ton-F1 und eine Confusion Matrix, die zeigt, welche Töne das Modell verwechselt. Erst die Kombination — Silbe UND Ton korrekt — ergibt die Combined Accuracy. Diese Entkopplung hat zwei Vorteile: Erstens sehe ich, ob ein Modell gut bei Lauten aber schlecht bei Tönen ist — oder umgekehrt. Zweitens kann ich gezielt nach Ton-3-Schwächen suchen, die laut Zou der häufigste Fehlertyp sind. Für multimodale LLMs auf Mandarin wurde diese entkoppelte Evaluation noch nie gemacht — das ist der methodische Beitrag."

---

## Folie MM9 — Token-Definition: Was ist EIN Vergleichs-Token?

**Titel:** Drei Token-Definitionen für Mandarin — mit verschiedenen Ergebnissen

**Visuelles Beispiel (gleicher Input, drei Definitionen):**
```
Audio: "diànnǎo" (电脑)

DEFINITION A: Tonal Syllable (unser Ansatz)
─────────────────────────────────────────────
  Token: "dian4"  "nao3"                    → 2 Token
  Fehler: "dian4"→"dian4" ✓  "nao3"→"nao2" ✗ → SyllER = 50%
  
DEFINITION B: Initial + Tonal Final (Wang MDD)
─────────────────────────────────────────────
  Token: "d" "ian4" "n" "ao3"              → 4 Token
  Fehler: "ao3"→"ao2"                       → PER = 25%
  
DEFINITION C: Fully Decomposed (feinste Granularität)
─────────────────────────────────────────────
  Token: "d" "i" "a" "n" "4" "n" "a" "o" "3"  → 9 Token
  Fehler: "3"→"2"                           → PER = 11%
```

**Bullets:**
- Gleicher Fehler (Ton 3→2 bei „nao"), **drei verschiedene Fehlerraten**: 50%, 25%, 11%
- **Wang MDD** nutzt Definition B mit 66 tonal phonemes: *"initial consonant sounds, final vowel sounds, and tones"*
- **Du Zipformer** nutzt ebenfalls 66 Phoneme auf Phonem-Ebene
- **Unsere Wahl:** Definition A (tonal syllable) — weil Tone Perfect silbenbasiert ist und jede Silbe exakt einen Ton hat

**Sprechernotiz:** „Die Token-Definition verändert die Fehlerrate dramatisch — bei exakt demselben Fehler. Wenn ich ‚dian4 nao3' als zwei tonal syllables tokenisiere und der Ton von nao falsch ist, habe ich 50 % SyllER. Wenn ich in Initial und tonal Final aufteile, wie Wang es tut, sind es 25 % PER. Und wenn ich bis auf einzelne Phoneme inklusive Ton als eigenes Token runterbreche, sind es nur 11 % PER — obwohl der Fehler identisch ist. Für meine Arbeit wähle ich Definition A, die tonale Silbe, aus zwei Gründen: Erstens ist Tone Perfect silbenbasiert — jeder Sample ist genau eine Silbe mit genau einem Ton. Zweitens ist die Silbe die natürliche Einheit im Mandarin — jede Silbe hat exakt einen Ton, was die Entkopplung in Segment und Ton trivial macht."

---

## Folie MM10 — Zusammenfassung: Die Forschungslücke

**Titel:** Was existiert, was fehlt — und was wir beitragen

**Tabelle (zentrale Folie):**

| Eigenschaft | Existiert? | Wer? | Unser Beitrag |
|---|---|---|---|
| CER/WER auf Hanzi | ✅ Standard (27 Papers) | Seed, FireRed, Kimi, Qwen3, ... | nur Vergleichsanker |
| PER auf Phonemen | ✅ selten (3 Papers) | Du, Wang MDD, Zhu ZIPA | — |
| CER auf Pinyin (ohne Ton) | ✅ selten (2 Papers) | Zhengjie (1,9%), Chen (2,41%) | Segment-Achse |
| tCER/tWER (mit Ton) | ✅ 1 Paper (Baima!) | Chirkova (2025) | Ton-Achse (Mandarin!) |
| TER (Tone Error Rate) | ⚠️ empfohlen, nie angewandt | Zou Review (2025) | **NEU: erstmals berechnet** |
| PFER (gewichtet) | ✅ 1 Paper | Zhu ZIPA (2025) | optional |
| Per-Ton-F1 + Confusion Matrix | ✅ teilweise | Wu (FRR/FAR pro Ton) | **systematisch für LLMs** |
| **Segment+Ton entkoppelt + LLM** | ❌ **niemand** | — | **Kernbeitrag** |
| **Gewichtete Ton-Distanz** | ❌ **niemand** | — | **offene Frage** |

**Sichtbares Hinweis-Kästchen:**
> **Forschungslücke in einem Satz:** Kein Paper kombiniert Pinyin+Ton als Referenz, entkoppelte Segment/Ton-Evaluation, und multimodale LLMs für Mandarin.

**Sprechernotiz:** „Diese Tabelle fasst die gesamte 9-Fragen-Analyse über 37 Papers zusammen. Links steht, was existiert: CER auf Hanzi als Standard, PER auf Phonemen bei drei Papers, Pinyin-CER ohne Ton bei zweien, tonal CER bei Chirkova aber nur für Baima. Rechts steht, was ich beitrage: TER erstmals berechnet — obwohl Zou sie in seinem Review über 61 Studien explizit empfiehlt, hat kein einziges Paper sie tatsächlich angewandt. Per-Ton-F1 mit Confusion Matrix systematisch für LLMs. Und die Entkopplung von Segment- und Ton-Evaluation — diese Kombination gibt es für multimodale LLMs auf Mandarin schlicht nicht. Die offene Frage für die Zukunft ist die gewichtete Ton-Distanz: Ob Ton 2 gegen Ton 3, die akustisch ähnlich sind, anders bestraft werden sollte als Ton 1 gegen Ton 4."

---

# TEIL II: Backup-Folie (Detail-Tabelle)

---

## Folie MM-Backup — Alle 42 Papers: 9-Fragen-Übersicht

**Titel:** Vollständige Metrik-Methodik-Übersicht (Backup / Handout)

| # | Paper | Referenz-Einheit | Metrik | Ton-Behandlung | Phon. Relevanz |
|---|---|---|---|---|---|
| 1 | Seed-ASR (2024) | Hanzi | CER/WER/WWER | Ignoriert | NEIN |
| 2 | FireRedASR (2025) | Hanzi | CER | Ignoriert | NEIN |
| 3 | FireRedASR2S (2026) | Hanzi | CER | Ignoriert | NEIN |
| 4 | Kimi-Audio (2025) | Hanzi | WER | Ignoriert | NEIN |
| 5 | Qwen3-ASR (2026) | Hanzi | CER/WER | Ignoriert | NEIN |
| 6 | Step-Audio 2 (2025) | Hanzi | CER/WER | Ignoriert | NEIN |
| 7 | Wei ASR-EC (2024) | Hanzi | CER (S/D/I) | Ignoriert | NEIN |
| 8 | ContextASR (2025) | Wort/NE | WER/NE-WER | Ignoriert | TEILW. |
| 9 | VocalBench-zh (2025) | Text+Phonem | PER/UTMOS/Acc | Ignoriert | TEILW. |
| 10 | **Du Zipformer** (2025) | **Phoneme (66)** | **WER (=PER)** | **Implizit** | **JA** |
| 11 | **Zou Review** (2025) | **Ton-Labels** | **TER/Acc/F1** | **Fokus** | **JA** |
| 12 | TELEVAL (2026) | Hanzi | CER/DNSMOS | Ignoriert | TEILW. |
| 13 | **Zhengjie PYG** (2025) | **Pinyin (o. Ton)** | **Pinyin-ERR** | **Ignoriert** | **JA** |
| 14 | Chen SCCM (2025) | Pinyin (I+F) | CER/Pinyin-ERR | Ignoriert | TEILW. |
| 15 | **Wang MDD** (2024) | **Tonal-Phoneme** | **PER/FRR/FAR/F1** | **Implizit** | **JA** |
| 16 | **Wu L2 Prosody** (2024) | **Human-Töne** | **FRR/FAR/DA** | **Separat** | **JA** |
| 17 | Huang Review (2026) | F0-Param. | Review | Fokus | TEILW. |
| 18 | **Zhu ZIPA** (2025) | **IPA-Phoneme** | **PFER** | **Implizit** | **JA** |
| 19 | **Bu Siamese** (2025) | **F0-Konturen** | **MSE/RMSE** | **Separat** | **JA** |
| 20 | **Wang MSPB** (2025) | Expert-Urteile | Accuracy | Prosodie | TEILW. |
| 21 | **Chirkova** (2025) | **IPA+Pinyin+Ton** | **tCER/tWER** | **Separat** | **JA** |
| 22 | Kang Tone-Sync (2024) | F2/f0-Trajekt. | GAMMs/Bayes | Fokus | TEILW. |
| 23 | Kaur Survey (2021) | — (Review) | WER | Diskutiert | TEILW. |
| 24 | Ahlawat Survey (2025) | — (Review) | WER/CER | Nicht addr. | NEIN |
| 25 | Li PY-GEC (2024) | Hanzi | CER | Pinyin-Hilfe | TEILW. |
| 26 | Liang PERL (2024) | Hanzi | CER/CERR | Pinyin-Embed. | TEILW. |
| 27 | Sakshi MMAU (2024) | Expert-Annot. | Micro-Acc | Nein | NEIN |
| 28 | Dyn-SUPERB (2024) | Task-Gold | LLM-Pipeline | Teilw. | TEILW. |
| 29 | Cui VoxEval (2025) | Wissensbasis | Accuracy | Nein | NEIN |
| 30 | Wang AudioBench (2024) | Gold-Datensätze | WER/MOS | Nein | NEIN |
| 31 | Zhang WildSpeech (2025) | Expert-Bew. | GPT-4o Judge | Paralinguis. | TEILW. |
| 32 | Yang LLM+Speech (2025) | Benchmarks | WER/BLEU | Nein | NEIN |
| 33 | Yang Eval Survey (2025) | Benchmarks | Diverse | Nein | NEIN |
| 34 | Latif LAM (2023) | Benchmarks | WER | Nein | NEIN |
| 35 | Gaido ST (2024) | ST-Benchmarks | BLEU | Nein | NEIN |
| 36 | Cui SpeechLM (2024) | Benchmarks | Diverse | Nein | NEIN |
| 37 | Arora SLM (2025) | Benchmarks | ABX/WER | Nein | NEIN |
| 38–42 | (Peng, SpeechIO, Xu SITA, etc.) | Hanzi/Bench. | CER/WER/Acc | Ignoriert | NEIN |

---

# TEIL III: Anmerkungen für Stefan

- **Folien MM1–MM10 ergänzen M1–M6:** M1–M6 zeigen **welche Metriken** es gibt. MM1–MM10 zeigen **wie genau** jedes Paper seine Metrik berechnet und was die methodischen Implikationen sind.
- **MM1 ist die Eröffnungsfolie** — sie macht sofort klar, warum die Metrik-Methodik wichtig ist.
- **MM3 (Referenz-Hypothese-Tabelle) ist die Schlüsselfolie** für die Q&A — hier kann man jedes Detail nachschlagen.
- **MM4 (Ton-Strategien) ist das 2×2-Raster**, das Tim sofort sieht: 27 ignorieren, 3 implizit, 4 separat, 2 akustisch.
- **MM8 (Unser Ansatz) ist die Antwort auf Tims Frage** „Wie macht ihr es?" — zweiachsig, Segment+Ton entkoppelt.
- **MM10 (Zusammenfassung) ist die Folie**, die direkt zur Forschungslücke führt.
- **Kürzungsoption:** MM5 (Berechnung) und MM6 (Gewichtung) können für einen kürzeren Vortrag zu einer Folie zusammengefasst werden. MM9 (Token-Definition) kann als Backup verschoben werden.
- **Alle Zitate sind aus der DeepSeek-Analyse verifiziert** gegen `core_information_combined.md` und die 37-Paper-Analyse.
