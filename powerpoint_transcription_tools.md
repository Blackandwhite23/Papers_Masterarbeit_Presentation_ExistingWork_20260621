# PowerPoint — Bestehende Tools für Mandarin-Lautschrift (ohne LLMs)
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Zweck:** Folien zur Frage „Welche Tools gibt es JETZT SCHON, um Mandarin in Pinyin/IPA umzuwandeln — ohne LLMs?" — mit **Online-Verifizierung** (Stand 2026-06-28).
>
> **Kontext:** Diese Tools bilden die **Baseline** gegen die multimodalen LLMs verglichen werden. Die Forschungslücke ist: Keines kann Audio→Pinyin+Ton direkt.

---

# TEIL 0: PRÜF-ERGEBNIS (für Stefan, nicht zum Vortragen)

## Verifizierungsergebnis der DeepSeek-Analyse

| Tool | DeepSeek sagt | Verifiziert? | Korrektur |
|---|---|---|---|
| **g2pW** | BERT-basiert, ~98%, Interspeech 2022 | ✅ Korrekt | GitHub: GitYCC/g2pW, `pip install g2pw` |
| **g2pM** | Bi-LSTM, ~97%, 2020 | ✅ Korrekt | GitHub: kakaobrain/g2pm, Interspeech 2020 |
| **pypinyin** | Regelbasiert, ~90–95% | ✅ Korrekt | GitHub: mozillazg/python-pinyin |
| **pypinyin-g2pW** | Kombination | ✅ Existiert | `pip install pypinyin-g2pw` |
| **Whisper** | Audio→Hanzi, kein Pinyin | ✅ Korrekt | Braucht G2P-Nachbearbeitung |
| **MFA** | „Offizielle Mandarin-Modelle" | ⚠️ **Übertrieben** | Nur Zeichen-basiertes Modell, **kein Pinyin-Modell**; G2P für logographische Schriften limitiert |
| **Praat** | F0-Analyse, Goldstandard | ✅ Korrekt | Kostenlos, Open Source |
| **Pulse** (leeqingming/mychinesemultipa) | „Spezialisiertes Modell" | ❌ **Nicht verifizierbar** | Repository nicht auffindbar — wahrscheinlich halluziniert |
| **Qwen3-ForcedAligner** | NAR Timestamp Predictor | ⚠️ Teilweise | Existiert, aber nur Text+Audio Alignment, kein Pinyin-Output |

## Von DeepSeek fehlende Tools (online verifiziert)

| Tool | Was es macht | pip | Verifiziert |
|---|---|---|---|
| **epitran** | Text→IPA für 100+ Sprachen inkl. Mandarin (braucht CC-CEDict) | `epitran` | ✅ GitHub: dmort27/epitran |
| **dragonmapper** | Hanzi↔Pinyin↔Zhuyin↔IPA Konvertierung | `dragonmapper` | ✅ GitHub: tsroten/dragonmapper |
| **pinyin-to-ipa** | Pinyin→IPA mit Tonmarkern am Vokal | `pinyin-to-ipa` | ✅ GitHub: stefantaubert/pinyin-to-ipa |
| **PanPhon** | IPA→artikulatorische Merkmalsvektoren (21 Features), phonologische Distanz | `panphon` | ✅ GitHub: dmort27/panphon |
| **MultIPA** | Multilinguales Speech→IPA (Wav2Vec2-basiert) | — | ✅ GitHub: ctaguchi/multipa |

---

# TEIL I: FOLIEN

---

## Folie T1 — Die Tool-Landschaft: 4 Aufgaben, viele Tools, eine Lücke

**Titel:** Mandarin-Transkription ohne LLMs — was geht, was fehlt

**Visuelles Diagramm:**
```
AUFGABE                    TOOLS                           STATUS
───────────────────────────────────────────────────────────────────
Text → Pinyin (G2P)        g2pW, g2pM, pypinyin            ✅ Gelöst (~98%)
Pinyin → IPA               epitran, dragonmapper,          ✅ Gelöst (deterministisch)
                           pinyin-to-ipa
Audio → Hanzi (ASR)        Whisper, Qwen3-ASR, FireRed     ✅ Gelöst (~2–5% CER)
Audio → Pinyin+Ton         ❌ KEIN TOOL                     ← FORSCHUNGSLÜCKE
    (direkt)
───────────────────────────────────────────────────────────────────
Hilfstools:
  Forced Alignment         MFA (limitiert für Mandarin)
  F0-Analyse               Praat (manuell)
  Phonologische Distanz    PanPhon (21 artikulat. Features)
```

**Sprechernotiz:** „Diese Folie zeigt die bestehende Tool-Landschaft für Mandarin-Transkription — alles ohne LLMs. Drei der vier Aufgaben sind gelöst: Text zu Pinyin funktioniert mit g2pW bei circa 98 Prozent Genauigkeit. Pinyin zu IPA ist deterministisch, da gibt es mehrere Python-Bibliotheken. Und Audio zu Hanzi — also Spracherkennung — macht Whisper mit unter 5 Prozent Fehlerrate. Aber die vierte Aufgabe — Audio direkt zu Pinyin mit Ton — dafür gibt es kein einziges Tool. Das ist genau meine Forschungslücke: Kann ein multimodales LLM diese Lücke füllen?"

---

## Folie T2 — Text → Pinyin: Ein gelöstes Problem

**Titel:** G2P für Mandarin — 3 Tools, ein klarer Sieger

**Tabelle (Folie):**

| Tool | Typ | Genauigkeit | Polyphone (多音字) | Open Source |
|---|---|---|---|---|
| **g2pW** (2022) ⭐ | BERT-basiert | **State-of-the-Art** | ✅ Kontext-sensitiv | ✅ GitYCC/g2pW |
| **g2pM** (2020) | Bi-LSTM | ~97% (CPP-Benchmark) | ✅ Kontext-sensitiv | ✅ kakaobrain/g2pm |
| **pypinyin** | Regelbasiert | ~90–95% | ⚠️ Schwach bei 多音字 | ✅ mozillazg/python-pinyin |

**Sichtbares Code-Beispiel:**
```python
from g2pw import G2PWConverter
conv = G2PWConverter(style='pinyin', enable_non_tradional_chinese=True)
conv('他红了20年')
# → [['ta1', 'hong2', 'le5', '20', 'nian2']]
```

**Bullets:**
- **Polyphone** (多音字) sind die Hauptschwierigkeit: 了 = le5 oder liǎo3? 行 = xíng oder háng?
- g2pW löst das per BERT-Kontext → übertrifft alle anderen auf dem CPP-Benchmark
- **Für RQ1:** g2pW ist die **Baseline** — wenn ein LLM schlechter als g2pW ist, taugt es nicht

**Sprechernotiz:** „Text zu Pinyin ist ein gelöstes Problem. g2pW von 2022 nutzt BERT, um Polyphone kontextabhängig aufzulösen — also ob 了 als ‚le' oder ‚liǎo' gelesen wird. Es ist der klare State of the Art und wird meine Baseline für RQ1 sein. Wenn ein LLM bei Text-zu-Pinyin schlechter abschneidet als g2pW, dann taugt es für diese Aufgabe nicht. g2pM ist die ältere Alternative mit Bi-LSTM, und pypinyin ist regelbasiert — gut für einfache Fälle, aber schwach bei Polyphonen."

---

## Folie T3 — Pinyin ↔ IPA: Deterministische Konvertierung

**Titel:** Pinyin ↔ IPA — kein ML nötig, reine Regelabbildung

**Tabelle (Folie):**

| Tool | Richtung | Töne? | Beispiel |
|---|---|---|---|
| **dragonmapper** | Hanzi↔Pinyin↔Zhuyin↔IPA | ✅ Tonmarker | Wǒ → wɔ˧˩˧ |
| **pinyin-to-ipa** | Pinyin→IPA | ✅ Am Vokal | ma1 → ma˥˥ |
| **epitran** | Text→IPA (100+ Sprachen) | ⚠️ Optional | 我 → wɔ˧˩˧ (braucht CC-CEDict) |

**Sichtbares Beispiel:**
```
Pinyin:   mā    má    mǎ    mà
IPA:      ma˥˥  ma˧˥  ma˨˩˦  ma˥˩
Chao:     55    35    214   51
```

**Bullets:**
- Pinyin→IPA ist **deterministisch** — keine ML-Modell-Fehler möglich
- Nützlich für **PFER-Berechnung**: IPA→PanPhon→artikulatorische Merkmale→gewichtete Distanz
- **dragonmapper** ist das flexibelste (alle Richtungen), **pinyin-to-ipa** das einfachste

**Sprechernotiz:** „Die Umwandlung zwischen Pinyin und IPA ist rein regelbasiert — kein Machine Learning, keine Fehlerquelle. dragonmapper kann in alle Richtungen konvertieren: Hanzi, Pinyin, Zhuyin, IPA. Das ist nützlich für die PFER-Berechnung: Ich wandle den LLM-Output in IPA um, füttere das in PanPhon, und bekomme eine phonetisch gewichtete Fehlerdistanz. Die Chao-Tonnummern — 55, 35, 214, 51 — beschreiben den Pitch-Verlauf auf einer 5-Stufen-Skala und sind international Standard in der Phonetik."

---

## Folie T4 — Audio → Hanzi: Whisper & Co. (ASR-Pipeline)

**Titel:** Die Standard-Pipeline — und warum sie Töne versteckt

**Visuelles Diagramm:**
```
Audio "mǎ" (Ton 3)
  │
  ├─→ Whisper large-v3  →  马 (Hanzi)  →  g2pW  →  mǎ (T3) ✓
  │                                                  
  │   ABER bei L2-Lerner:
  │   Audio "má" (Ton 2, FALSCH)
  │     └─→ Whisper  →  麻 (Hanzi)  →  g2pW  →  má (T2)
  │                                    ↑ RICHTIG für dieses Zeichen,
  │                                      FALSCH als Fehlerdiagnose!
  │                                      (Whisper rät das ZEICHEN,
  │                                       nicht den GESPROCHENEN Ton)
  │
  └─→ Multimodales LLM (unsere Methode):
      Prompt: "Transcribe to Pinyin with tone number"
      → "ma2" ← direkt prüfbar gegen Ground Truth "ma3"
```

**Bullets:**
- **Whisper + g2pW** ist die Standard-Pipeline: Audio→Hanzi→Pinyin
- Problem: Whisper wählt das **wahrscheinlichste Zeichen**, nicht den **gesprochenen Ton**
- Bei L2-Sprechern: Whisper „korrigiert" Fehler → Tonfehler werden **unsichtbar**
- **Unsere Methode:** LLM gibt Pinyin+Ton **direkt** aus → Fehler bleiben sichtbar

**Sprechernotiz:** „Die Standard-Pipeline ist Whisper plus g2pW: Audio rein, Hanzi raus, dann G2P zu Pinyin. Das funktioniert für Native Speaker gut — Whisper erkennt ‚ma' mit Ton 3 und schreibt 马. Aber bei L2-Lernern bricht es zusammen: Wenn ein Lerner ‚má' mit Ton 2 statt Ton 3 sagt, schreibt Whisper trotzdem 麻 — das Zeichen, das zu Ton 2 passt. g2pW gibt dann ‚má' aus, und es sieht so aus, als wäre alles korrekt. Der Tonfehler ist verschwunden. Deshalb ist der direkte Weg über ein multimodales LLM methodisch sauberer: Ich prompte es, Pinyin mit Tonnummer auszugeben, und kann den Output direkt mit dem Ground Truth vergleichen."

---

## Folie T5 — Hilfstools: PanPhon, MFA, Praat

**Titel:** Analyse-Werkzeuge — für faire Bewertung und manuelle Annotation

**Tabelle (Folie):**

| Tool | Aufgabe | Für unsere Arbeit | Status |
|---|---|---|---|
| **PanPhon** | IPA→21 artikulatorische Merkmale→Distanz | **PFER-Berechnung** (RQ2b/RQ5) | ✅ `pip install panphon` |
| **MFA** | Audio + Text → Phonem-Alignment | Tone-Perfect-Alignment (optional) | ⚠️ Mandarin limitiert |
| **Praat** | F0-Kontur → Tonverlauf sichtbar | L2-Ground-Truth (Stichprobe) | ✅ Goldstandard |

**Sichtbares PanPhon-Beispiel:**
```python
from panphon.distance import Distance
dst = Distance()
dst.feature_edit_distance('ma˥˥', 'na˥˥')   # → 0.08 (nah: m↔n)
dst.feature_edit_distance('ma˥˥', 'tsa˥˥')  # → 0.42 (fern: m↔ts)
# → Near-Misses werden FAIR bestraft, nicht alles-oder-nichts
```

**Bullets:**
- **PanPhon:** Berechnet Distanz basierend auf 21 artikulatorischen Merkmalen (z.B. ±nasal, ±stimmhaft) — Grundlage für die PFER-Metrik (Phone Feature Error Rate)
- **MFA:** Kann Audio mit Referenztext alignen, aber Mandarin-Support ist **limitiert** (kein Pinyin-Modell, nur Zeichen-basiert)
- **Praat:** Manuelle F0-Analyse → sieht man, ob der Lerner wirklich T3 (214) oder T2 (35) produziert hat

**Sprechernotiz:** „Drei Hilfstools ergänzen die Pipeline. PanPhon ist das Wichtigste für mich: Es rechnet phonologische Distanz basierend auf 21 artikulatorischen Merkmalen. Wenn ein LLM ‚na' statt ‚ma' transkribiert, ist das ein kleiner Fehler — beide sind Nasale. Aber ‚tsa' statt ‚ma' ist ein großer Fehler. PanPhon macht diese Abstufung messbar und ist die Grundlage für meine PFER-Metrik. MFA, der Montreal Forced Aligner, kann Audio mit Text alignen, aber der Mandarin-Support ist limitiert — es gibt kein Pinyin-Modell. Praat ist der Goldstandard der Phonetik für manuelle F0-Analyse: Damit kann ich in einer Stichprobe nachprüfen, ob ein L2-Lerner wirklich Ton 3 oder Ton 2 produziert hat."

---

## Folie T6 — Die Lücke: Kein Tool für Audio→Pinyin+Ton

**Titel:** Die Forschungslücke — bestätigt durch die Tool-Analyse

**Tabelle (Folie):**

| Aufgabe | Bestes Tool | Genauigkeit | Kosten | Töne? |
|---|---|---|---|---|
| Text → Pinyin | g2pW | ~98% | $0 | ✅ (aus Kontext) |
| Audio → Hanzi | Whisper large-v3 | ~2–5% CER | $0 (lokal) | ❌ (im Zeichen versteckt) |
| Hanzi → Pinyin | g2pW | ~98% | $0 | ⚠️ (Standardton, nicht gesprochen) |
| Pinyin → IPA | dragonmapper | 100% | $0 | ✅ (deterministisch) |
| IPA → Features | PanPhon | 100% | $0 | ✅ (deterministisch) |
| **Audio → Pinyin+Ton** | **❌ Existiert nicht** | **—** | **—** | **—** |

**Sichtbares Hinweis-Kästchen:**
> **Forschungslücke:** Es gibt kein Tool, das Audio direkt in Pinyin mit korrektem Ton umwandelt — insbesondere nicht für L2-Sprecher. Genau das testen wir mit multimodalen LLMs.

**Sprechernotiz:** „Diese Tabelle fasst die gesamte Tool-Landschaft zusammen. Jede einzelne Teilaufgabe ist gelöst — Text zu Pinyin, Audio zu Hanzi, Hanzi zu Pinyin, Pinyin zu IPA. Aber die Kombination — Audio direkt zu Pinyin mit dem tatsächlich gesprochenen Ton — dafür existiert kein Tool. Man kann die Pipeline zusammenstecken: Whisper plus g2pW. Aber dann verliert man die Toninformation, weil Whisper das wahrscheinlichste Zeichen wählt und g2pW den Standardton dieses Zeichens ausgibt. Genau hier setze ich an: Können multimodale LLMs diese Lücke füllen? Können sie Audio hören und direkt ‚ma2' ausgeben, ohne den Umweg über Zeichen? Das ist die Kernfrage meiner Arbeit."

---

## Folie T7 — Unser Tool-Stack fürs Experiment

**Titel:** Was wir nutzen — Tools + LLMs = vollständige Pipeline

**Visuelles Diagramm:**
```
                    BASELINE (bestehende Tools)
                    ┌─────────────────────────────┐
   Text ──────────→│ g2pW (RQ1 Baseline)          │──→ Pinyin+Ton
                    └─────────────────────────────┘

   Audio ─────────→ Whisper → g2pW ──────────────────→ Pinyin+Ton*
                    *(Standardton, nicht gesprochener Ton)

                    EXPERIMENT (multimodale LLMs)
                    ┌─────────────────────────────┐
   Audio ──────────→│ GPT-5.5 / Gemini / Qwen /   │──→ Pinyin+Ton
                    │ Grok / Kimi / Step / GLM     │   (gesprochener Ton)
                    └─────────────────────────────┘

                    EVALUATION
                    ┌─────────────────────────────┐
   LLM-Output ────→│ pinyin-to-ipa → PanPhon      │──→ PFER (gewichtet)
   vs. Ground Truth │ Ton-Vergleich                │──→ TER  (Tonebene)
                    │ Zeichen-Vergleich             │──→ CER  (Literatur)
                    └─────────────────────────────┘
```

**Bullets:**
- **Baseline:** g2pW (Text→Pinyin), Whisper+g2pW (Audio→Pinyin, aber Ton unsicher)
- **Experiment:** 12 multimodale LLMs: Audio→Pinyin+Ton direkt
- **Evaluation:** pinyin-to-ipa + PanPhon für PFER; direkter Tonvergleich für TER; CER für Literaturvergleich
- **Ground Truth:** Tone Perfect (Dateiname = Pinyin+Ton), LATIC (annotiert)

**Sprechernotiz:** „Hier sieht man, wie alles zusammenspielt. Oben die Baseline: g2pW für die reine Text-Aufgabe, und Whisper plus g2pW für die Audio-Pipeline — die gibt zwar Pinyin aus, aber mit dem Standardton statt dem gesprochenen. In der Mitte das eigentliche Experiment: 12 multimodale LLMs, die Audio direkt zu Pinyin mit Ton umwandeln. Und unten die Evaluation: Ich nehme den LLM-Output, wandle ihn per pinyin-to-ipa in IPA um, füttere das in PanPhon für die gewichtete PFER-Distanz, rechne separat die TER für die Tonebene, und behalte CER für den Literaturvergleich. Damit habe ich drei Achsen: Segment, Ton und Zeichen — entkoppelt und phonetisch fair gewichtet."

---

# TEIL II: Anmerkungen für Stefan

- **Folien T1–T7** sind die vortragbaren Folien; **Teil 0** ist deine Prüf-Checkliste.
- **Wichtigste Botschaft an Tim:** Die bestehenden Tools decken jede Teilaufgabe ab, aber die **Kombination Audio→Pinyin+Ton** existiert nicht. Die Pipeline Whisper+g2pW verliert Toninformation. Multimodale LLMs könnten diese Lücke füllen — genau das testen wir.
- **Verifizierte Fakten (Q&A-fest):**
  - g2pW = Interspeech 2022, BERT-basiert, State of the Art für Polyphone → verifiziert (GitHub, Paper)
  - g2pM = Interspeech 2020, Bi-LSTM, 99k-Satz-Benchmark → verifiziert (GitHub, Paper)
  - pypinyin = regelbasiert, schwach bei Polyphonen → verifiziert (GitHub, PyPI)
  - PanPhon = 21 artikulatorische Merkmale, IPA→Feature-Vektoren → verifiziert (COLING 2016, GitHub)
  - MFA Mandarin = **limitiert** (kein Pinyin-Modell, nur Zeichen-basiert) → verifiziert (Doku, arXiv 2606.18466)
  - Praat = Goldstandard für F0-Analyse → verifiziert (Standardwissen + Online)
  - dragonmapper, pinyin-to-ipa, epitran = alle verifiziert (GitHub, PyPI)
- **DeepSeek-Fehler korrigiert:**
  - „Pulse" (leeqingming/mychinesemultipa) → **nicht verifizierbar**, aus Folien entfernt
  - MFA „offizielle Mandarin-Modelle" → **übertrieben**, nur Zeichen-Modell, kein Pinyin
- **Kürzungsoption:** T2+T3 zusammenziehen (G2P + Konvertierung), T5 weglassen → 5 statt 7 Folien.

## Quellen (alle online verifiziert, 2026-06-28)

- g2pW: [GitHub](https://github.com/GitYCC/g2pW) | [Paper (Interspeech 2022)](https://arxiv.org/abs/2203.10430)
- g2pM: [GitHub](https://github.com/kakaobrain/g2pm) | [Paper (Interspeech 2020)](https://arxiv.org/abs/2004.03136)
- pypinyin: [GitHub](https://github.com/mozillazg/python-pinyin)
- PanPhon: [GitHub](https://github.com/dmort27/panphon) | [Paper (COLING 2016)](https://aclanthology.org/C16-1328.pdf)
- epitran: [GitHub](https://github.com/dmort27/epitran)
- dragonmapper: [GitHub](https://github.com/tsroten/dragonmapper)
- pinyin-to-ipa: [GitHub](https://github.com/stefantaubert/pinyin-to-ipa)
- MultIPA: [GitHub](https://github.com/ctaguchi/multipa)
- MFA: [Dokumentation](https://montreal-forced-aligner.readthedocs.io) | [arXiv (2026)](https://arxiv.org/html/2606.18466v1)
- Praat: [praat.org](https://www.fon.hum.uva.nl/praat/)
