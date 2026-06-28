# PowerPoint — Lautschrift-Entscheidungen in der Literatur + Verfügbare Systeme
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Vocabulary*

> **Zweck:** Folien zu: (a) Welche Lautschrift nutzen die Papers warum? (b) Welche Transkriptionssysteme gibt es? (c) Warum wählen wir Pinyin+Tonnummern?

---

# TEIL A: Welche Lautschrift haben sie warum verwendet?

---

# Folie L1: Lautschrift — Überblick über 42 Papers

**Folientitel:** Phonetic Transcription in 42 Papers: A Rare Choice

```
42 Papers
├── 31 Papers (74%): KEINE Lautschrift
│   └── Output: chinesische Schriftzeichen (Hanzi) → CER
│
├── 7 Papers (17%): Pinyin
│   ├── Zhengjie PYG-ASR — Pinyin (initials/finals), Qwen2-7B [LLM!]
│   ├── Chen SCCM — Pinyin (initials/finals), Conformer
│   ├── Li PY-GEC — Pinyin (mit Tonmarkern), LLaMA-3-8B [LLM!]
│   ├── Liang PERL — Pinyin Embeddings, BERT
│   ├── Wu L2 Prosody — Pinyin (Annotation)
│   ├── Wang MSPB — Pinyin+Ton# (Stimulus-Labels)
│   └── Tone Perfect — Pinyin+Ton# (Datensatz-Labels)
│
├── 2 Papers (5%): IPA
│   ├── Zhu ZIPA — IPA (broad, 88 Sprachen)
│   └── Chirkova — IPA + Chao + Pinyin + Simple (Vergleich!)
│
├── 1 Paper (2%): Phonem-Set
│   └── Du Zipformer — 66 Phonemkategorien (Mandarin)
│
└── 1 Paper (2%): G2P/PER (nur intern)
    └── Wang ContextASR — PER für TTS-Qualitätssicherung
```

> **Sprecher-Notizen:**
> - 74% aller Papers geben chinesische Schriftzeichen aus und messen CER — Töne gehen komplett verloren
> - "Chinese uses pinyin as a phonetic notation system, where each character is transcribed into a syllable composed of an initial and a final." (Chen SCCM, Section 1)
> - Pinyin dominiert (7 Papers), aber meist OHNE explizite Begründung der Systemwahl — es wird als "gegeben" betrachtet
> - IPA erscheint nur bei multilingualer Arbeit (ZIPA: 88 Sprachen) oder systematischem Vergleich (Chirkova: Baima)
> - **Kein einziges Paper begründet die Wahl zwischen Pinyin und IPA für Mandarin-ASR**

---

# Folie L2: Tonmarker — Wer markiert Töne?

**Folientitel:** Tone Markers: The Missing Dimension

| Paper | Lautschrift | Tonmarker? | Begründung |
|---|---|---|---|
| **Tone Perfect** (Ryu 2021) | Pinyin+Ton# | ✅ T1-T4 | Datensatz-Labels |
| **Wu L2 Prosody** (2024) | Pinyin | ✅ T1-T4 + Sandhi | Aufnahmeprotokoll |
| **Wang MSPB** (2025) | Pinyin+Ton# | ✅ Tonnummern | Stimulus-Labels |
| **Li PY-GEC** (2024) | Pinyin | ✅ Diakritika | Phonetische Brücke |
| **Liang PERL** (2025) | Pinyin | ✅ Diakritika | Phonologische Info |
| **Chirkova** (2025) | IPA+Chao | ✅ Chao-Tonnummern | Systematischer Vergleich |
| **Zhu ZIPA** (2025) | IPA | ⚠️ Numerisch/Chao | Multilingual |
| **Zhengjie PYG** (2025) | Pinyin (Pypinyin) | ⚠️ Standard tone style | Implementierung |
| **Chen SCCM** (2025) | Pinyin (I/F) | ⚠️ Implizit | Pädagogisch |
| **Du Zipformer** (2025) | 66 Phoneme | ⚠️ In Phonemen kodiert | Phonemerkennung |

**Kernaussage:** *Selbst Papers die Pinyin nutzen, evaluieren Töne selten SEPARAT.*

> **Sprecher-Notizen:**
> - Von 11 Papers die Lautschrift nutzen, haben nur 6 explizite Tonmarker
> - Zhengjie PYG-ASR ist ein wichtiger Grenzfall: Das System verwendet Pypinyin "with the standard tone style, the phonetic tone is on the first letter of the final" (Section 2.1) — also MIT Diakritika. Aber die Evaluation (Pinyin-ERR) trennt nicht zwischen Silben- und Tonfehlern
> - Chirkova (2025) ist das EINZIGE Paper mit systematischem Vergleich verschiedener Tondarstellungen: "IPA reduced character errors by 2.7% and tone errors by 5.2% over Pinyin" (Section 5) — aber für Baima, nicht Mandarin, und mit Wav2Vec2/Whisper, nicht LLMs

---

# Folie L3: Die kritische Lücke — LLM + Lautschrift + Töne

**Folientitel:** The Gap: No LLM + Phonetic Transcription + Tones

| Paper | LLM? | Audio→LLM? | Lautschrift? | Töne separat? |
|---|---|---|---|---|
| **Zhengjie PYG-ASR** | ✅ Qwen2-7B | ✅ HuBERT→LLM | ✅ Pinyin | ❌ |
| **Li PY-GEC** | ✅ LLaMA-3-8B | ❌ Text-only | ✅ Pinyin+Ton | ❌ |
| **Chirkova** | ❌ Wav2Vec2 | ✅ Direkt | ✅ IPA+Ton | ✅ (Baima) |
| **Wang MSPB** | ✅ GPT-4o etc. | ✅ Direkt | ✅ Pinyin+Ton | ❌ (Prosodie) |
| **Du Zipformer** | ❌ Zipformer | ✅ Direkt | ✅ Phoneme | ⚠️ Implizit |
| **Unsere Arbeit** | ✅ Multimodal | ✅ Direkt | ✅ Pinyin+Ton# | **✅ TER + PER** |

> **Sprecher-Notizen:**
> - Zhengjie PYG-ASR kommt am nächsten: Audio→LLM→Pinyin — aber explizit OHNE separate Tonevaluation
> - Zhengjie selbst schreibt: "there has been little exploration of leveraging LLMs to map audio features directly to Pinyin representations" (Introduction, S. 1) [VERIFIZIERT]
> - Chirkova vergleicht IPA vs. Pinyin systematisch mit Tonerkennung — aber mit Wav2Vec2/Whisper, nicht LLMs
> - **Unsere Arbeit füllt das letzte Kästchen: LLM ✅ + Audio ✅ + Lautschrift ✅ + Töne separat ✅**
> - Das ist ein genuiner Forschungsbeitrag — kein Paper in der Literatur hat alle 4 Kriterien gleichzeitig

---

# Folie L4: Warum wird Pinyin nicht begründet?

**Folientitel:** Why Pinyin? Nobody Asks.

| Paper | Begründung für Pinyin? |
|---|---|
| Li PY-GEC | ❌ "Pinyin—the phonetic representation of Chinese characters" (als gegeben) |
| Liang PERL | ❌ "The Chinese Pinyin system provides a Latin-based transcription... that carries rich phonological information" (beschreibend) |
| Zhengjie PYG-ASR | ❌ "we utilize the Pinyin system as presented in Pypinyin" (Implementierungsentscheidung) |
| Chen SCCM | ✅ "Chinese language learners typically begin by mastering initials and finals" (pädagogisch) |
| Wu L2 Prosody | ❌ "the pronunciation transcription, also called Pinyin is provided" (Konvention) |
| Wang MSPB | ❌ (Konvention) |
| **Chirkova** | **✅✅** Ausführliche Begründung IPA vs. Pinyin vs. Simple |

**Erkenntnis:** *Nur Chirkova (2025) begründet ihre Systemwahl. Alle anderen behandeln Pinyin als selbstverständlich.*

> **Sprecher-Notizen:**
> - Dies ist ein methodologisches Problem in der Literatur: Die Wahl des Transkriptionssystems wird fast nie diskutiert
> - Chirkova ist die Ausnahme und begründet ihre Wahl mehrstufig: "We chose Chao tone numbers for several reasons. First, the complexity of the tonal system of Baima has only recently begun to be unravelled... Chao tone numbers offer the most accurate and reliable method for noting tone variation" (Section 2.2)
> - Für unsere Arbeit: Wir sollten unsere Wahl (Pinyin+Tonnummern) explizit begründen — das stärkt die Methodologie
> - Chen SCCM gibt die beste Pinyin-Begründung: "pinyin directly corresponds to the pronunciation of speech and can provide more phonetic information" (Section 5)

---

# TEIL B: Welche Transkriptionssysteme gibt es?

---

# Folie T1: Transkriptionssysteme für Mandarin — Überblick

**Folientitel:** Phonetic Transcription Systems for Mandarin Chinese

| System | Beispiel: 妈/麻/马/骂 | Zeichen | Verbreitung | LLM-Trainingsdaten |
|---|---|---|---|---|
| **Hanyu Pinyin** | mā, má, mǎ, mà | 26 lat. Buchstaben + Diakritika | 🌍 Internationaler Standard (UN, ISO 7098) | ✅ Massiv |
| **Pinyin + Tonnummer** | ma1, ma2, ma3, ma4 | 26 lat. Buchstaben + Ziffern | 🌍 Standard (digital) | ✅ Massiv |
| **IPA** | [ma˥], [ma˧˥], [ma˨˩˦], [ma˥˩] | ~100+ IPA-Symbole | 🎓 Akademisch | ⚠️ Wenig |
| **IPA + Chao-Nummern** | [ma55], [ma35], [ma214], [ma51] | ~100+ Symbole + Ziffern | 🎓 Sinologie | ⚠️ Wenig |
| **Zhuyin (Bopomofo)** | ㄇㄚ, ㄇㄚˊ, ㄇㄚˇ, ㄇㄚˋ | 37 Symbole | 🇹🇼 Taiwan | ⚠️ Taiwan-only |
| **Wade-Giles** | ma¹, ma², ma³, ma⁴ | 26 lat. Buchstaben + Hochzahlen | 📚 Historisch | ⚠️ Ältere Texte |
| **Gwoyeu Romatzyh** | ma, mar, maa, mah | 26 lat. Buchstaben (Ton im Buchstaben) | 📚 Veraltet | ❌ Sehr selten |

> **Sprecher-Notizen:**
> - Hanyu Pinyin ist der offizielle internationale Standard seit 1958 (VR China) / 1982 (ISO)
> - In der Literatur: 7 Papers nutzen Pinyin, 2 nutzen IPA, 0 nutzen Zhuyin/Wade-Giles/GR
> - Für LLMs ist Pinyin optimal: lateinische Buchstaben sind in den Trainingsdaten massiv vertreten
> - IPA-Symbole wie ˥, ˧˥, ˨˩˦ sind seltene Unicode-Zeichen → Token-Fragmentierung bei LLMs möglich
> - Chirkova (2025): "favouring Chao tone numbers over IPA diacritics for tone representation" (Section 1) — selbst innerhalb der IPA-Tradition werden Chao-Nummern bevorzugt

---

# Folie T2: Ton-Darstellung — 5 Methoden im Vergleich

**Folientitel:** How to Represent Mandarin Tones

| # | Methode | Beispiel (T1/T2/T3/T4) | Maschinen-lesbar? | LLM-geeignet? |
|---|---|---|---|---|
| 1 | **Pinyin-Diakritika** | mā, má, mǎ, mà | ⚠️ Unicode nötig | ⚠️ Token-Fragmentierung |
| 2 | **Tonnummern (1-4)** | ma1, ma2, ma3, ma4 | ✅ Klar | ✅ Optimal |
| 3 | **Chao-Tonnummern** | ma55, ma35, ma214, ma51 | ✅ Präzise | ⚠️ Ungewöhnlich |
| 4 | **IPA Tone Letters** | ma˥, ma˧˥, ma˨˩˦, ma˥˩ | ⚠️ Exotisch | ❌ Seltene Zeichen |
| 5 | **Keine Tonmarker** | ma, ma, ma, ma | ✅ Einfach | ✅ Aber Toninfo verloren! |

**Erklärung Chao-System (5-Stufen-Skala):**
```
5 ─ ˥  hoch         T1 (high level):   55  ──────
4 ─ ˦  halbhoch     T2 (rising):       35    ╱
3 ─ ˧  mittel       T3 (dipping):     214  ╲╱
2 ─ ˨  halbtief     T4 (falling):      51  ╲
1 ─ ˩  tief                                  ╲
```

> **Sprecher-Notizen:**
> - Chao Yuen Ren (赵元任) entwickelte 1930 das 5-Stufen-System, das bis heute der Standard in der Tonforschung ist
> - Chirkova (2025): "Chao tone numbers offer the most accurate and reliable method for noting tone variation" (Section 2.2)
> - Für LLMs ist "ma1" (Tonnummer) optimal: (1) lateinische Buchstaben + Ziffer, (2) kein Unicode-Sonderzeichen, (3) klar maschinenparsbar, (4) massiv in Online-Lernmaterialien vertreten
> - Zou (2025) empfiehlt separate Tonevaluation: "Tone Error Rate (TER) analogous to Word Error Rate is preferred"
> - Unsere Wahl: **Pinyin + Tonnummer (ma1)** — optimal für LLM-Ausgabe UND maschinenlesbare Evaluation

---

# Folie T3: Mandarin-Silbenstruktur — Was evaluieren wir?

**Folientitel:** Mandarin Syllable Structure: What We Evaluate

```
Beispiel: 电 (diàn, Tone 4) = "Elektrizität/Strom"

┌─────────────────────────────────────────┐
│             Silbe: dian4                │
│                                         │
│   ┌──────────┐   ┌──────────┐   ┌───┐  │
│   │ Initial  │ + │  Final   │ + │Ton│  │
│   │ (声母)   │   │ (韵母)   │   │   │  │
│   │   "d"    │   │  "ian"   │   │ 4 │  │
│   └──────────┘   └──────────┘   └───┘  │
│                                         │
│   23 Initiale     35 Finale    4 Töne   │
│   (b,p,m,f,       (a,o,e,     (+ Ton 5 │
│    d,t,n,l,        ai,ei,      neutral) │
│    g,k,h,          an,en,               │
│    j,q,x,          ang,eng,             │
│    zh,ch,sh,r,     ia,ie,               │
│    z,c,s)          iao,ian...)           │
└─────────────────────────────────────────┘

Unsere 3 Evaluationsebenen:
1. SEGMENT:  d + ian → PER (Phone Error Rate)
2. TON:      4 → TER (Tone Error Rate) ← NEU!
3. KOMBINIERT: dian4 → Combined Accuracy
```

> **Sprecher-Notizen:**
> - Ein Mandarin-Wort besteht aus: Initial (Konsonant) + Final (Vokal/Diphthong) + Ton
> - Standard: 23 Initiale × 35 Finale × 4 Töne — aber nicht alle Kombinationen existieren → 410 gültige Silben (Tone Perfect)
> - Die meisten Papers (31/42) evaluieren nur auf Hanzi-Ebene (CER): 电→电 korrekt oder nicht
> - Unsere Arbeit evaluiert auf ALLEN 3 Ebenen: Segment (d+ian) + Ton (4) + Kombiniert (dian4)
> - "Chinese uses pinyin as a phonetic notation system, where each character is transcribed into a syllable composed of an initial and a final" (Chen SCCM, Section 1)

---

# Folie T4: Unsere Wahl — Pinyin + Tonnummern

**Folientitel:** Our Choice: Pinyin + Tone Numbers — Why?

| Kriterium | Pinyin+Ton# | IPA+Chao | Pinyin+Diakritika |
|---|---|---|---|
| **LLM-Training** | ✅ Massiv (Wikipedia, Lehrbücher, Online-Kurse) | ⚠️ Selten (Linguistik-Fachtexte) | ✅ Häufig |
| **Tokenisierung** | ✅ "ma1" = klares Token | ❌ "[ma˥]" = Subword-Fragmentierung | ⚠️ "mā" = Unicode-abhängig |
| **Maschinen-Parsing** | ✅ Regex: `([a-z]+)(\d)` | ⚠️ IPA-Parser nötig | ⚠️ Unicode-Normalisierung nötig |
| **Ton-Evaluation** | ✅ Ton als eigene Ziffer → TER trivial | ✅ Chao-Nummern → aber LLM-Output unzuverlässig | ⚠️ Diakritika-Extraktion nötig |
| **Literatur-Standard** | ✅ 5 von 7 Pinyin-Papers | ⚠️ 2 Papers (ZIPA, Chirkova) | ✅ 2 Papers (PY-GEC, PERL) |
| **Nutzer-Verständlichkeit** | ✅ Jeder Mandarin-Lernende kennt es | ❌ Nur Linguisten | ✅ Standard |

**Unsere Entscheidung:** `ma1, ma2, ma3, ma4`

> **Sprecher-Notizen:**
> - Pinyin+Tonnummern ist der optimale Kompromiss für unsere Arbeit:
>   1. LLMs kennen Pinyin aus den Trainingsdaten
>   2. Tonnummern sind maschinenlesbar → automatische TER-Berechnung
>   3. Es ist der De-facto-Standard in der digitalen Mandarin-Didaktik
> - Chirkova (2025) fand: "IPA reduced character errors by 2.7% and tone errors by 5.2% over Pinyin" — ABER mit Wav2Vec2/Whisper (ASR-Modelle), nicht LLMs
> - Bei LLMs könnte Pinyin BESSER sein als IPA — wegen Token-Vertrautheit. Das ist eine offene Frage.
> - Die Entscheidung für Pinyin+Tonnummern wird durch die Literatur gestützt:
>   - Tone Perfect nutzt Pinyin+Ton# als Ground Truth
>   - 5 von 7 Pinyin-Papers nutzen Tonnummern oder Initials/Finals
>   - Chao-Tonnummern (55/35/214/51) wären präziser, aber in LLM-Trainingsdaten selten

---

# Folie T5: IPA vs. Pinyin — Chirkova (2025): Der einzige Vergleich

**Folientitel:** Chirkova (2025): The Only Systematic Comparison

| System | Char CER | Tonal CER | Tonal WER |
|---|---|---|---|
| **IPA + Chao** | **Niedrigste** | **20** | **Niedrigste** |
| **Pinyin-style** | +2,7% | 28 | Höher |
| **Simple (ohne Ton)** | +5,0% | 31 | Höchste |

> "IPA is again the transcription with the least error. For tonal CER, IPA has tonal CER=20, compared to Pinyin tonal CER=28 and Simple tonal CER=31 (χ²(2)=28, p<0.0001)." (Section 3)

**Aber:** Chirkova nutzte Wav2Vec2/Whisper (traditionelle ASR), nicht LLMs!

**Offene Frage für unsere Arbeit:**
- *Gilt dieses Ergebnis auch bei multimodalen LLMs?*
- *Oder kehrt sich das Ergebnis um, weil LLMs mehr Pinyin als IPA in den Trainingsdaten haben?*

> **Sprecher-Notizen:**
> - Chirkova (2025) ist das EINZIGE Paper das IPA vs. Pinyin systematisch auf Tonerkennung vergleicht
> - Ergebnis: IPA ist 5,2% besser bei Tonfehlern (tonal CER 20 vs. 28) und 2,7% besser bei Zeichenfehlern
> - ABER: (1) Sprache ist Baima (Tibeto-Burman), nicht Standard-Mandarin; (2) Modelle sind Wav2Vec2 und Whisper, keine LLMs
> - Für LLMs gibt es Gründe, warum Pinyin besser sein KÖNNTE:
>   - "ma1" ist ein häufiges Token in LLM-Trainingsdaten
>   - IPA-Symbole wie ˥˧˥˨˩˦˥˩ sind seltene Unicode-Zeichen
>   - LLMs tokenisieren IPA-Zeichen möglicherweise als einzelne Bytes → schlechtere Repräsentation
> - Chirkova selbst schreibt: "While conversion from Simple romanisation or Baima Pinyin to IPA is impossible as too many details are lost, it is possible to convert into Pinyin and Simple romanised script from the better-performing IPA model" (Section 4) — IPA erfasst mehr Detail, aber Pinyin ist praktischer

---

# Folie T6 (Backup): Pilot-Experiment — Brauchen wir einen Vortest?

**Folientitel:** Optional Pilot: Which Format Works Best for LLMs?

**Design (falls durchgeführt):**
| Format | Beispiel (T1) | LLM-Eignung |
|---|---|---|
| A: Pinyin + Tonnummer | ma1 | ✅ Optimal |
| B: Pinyin + Diakritika | mā | ⚠️ Token-Fragmentierung? |
| C: IPA + Chao-Nummern | [ma55] | ⚠️ Selten in Trainingsdaten |
| D: IPA + Tone Letters | [ma˥] | ❌ Exotische Unicode-Zeichen |

- 50 Silben × 4 Töne × 2 Sprecher = 400 Samples
- 3 Modelle × 4 Formate = 4.800 API-Calls
- Geschätzte Kosten: <$10, Zeit: 1 Tag
- Metrik: Tone Accuracy (%)

**Entscheidungsmatrix:**
- Falls Pinyin+Ton# klar gewinnt → Bestätigung unserer Wahl
- Falls IPA besser → Chirkova-Ergebnis bestätigt auch für LLMs
- Falls kein Unterschied → Einfachstes Format (Pinyin+Ton#) wählen

> **Sprecher-Notizen:**
> - Dieses Pilot-Experiment ist OPTIONAL — es stärkt die Methodologie, ist aber nicht zwingend nötig
> - Begründung: Pinyin+Tonnummern ist bereits der Standard in der Literatur und am LLM-freundlichsten
> - Wenn wir den Pilot durchführen, haben wir ein eigenes empirisches Ergebnis zu einem offenen methodologischen Problem — das wäre ein zusätzlicher Beitrag
> - Zeitaufwand: 1 Tag (Wochenende), Kosten: <$10
> - Tim fragen: Lohnt sich der Aufwand für einen Pilot-Vergleich, oder reicht die Literatur-Begründung?
