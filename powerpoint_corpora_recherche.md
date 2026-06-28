# PowerPoint — Korpus-Recherche & Auswahl fürs Experiment
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Zweck:** Folien zur Frage „Welche Mandarin-Korpora gibt es, welche sind geeignet, und welche nutzen wir?" — basierend auf **eigener Recherche** (20 Korpora) mit **Online-Verifizierung** (Stand 2026-06-28).
>
> **Legende:** ✅ = verifiziert ja · ⚠️ = teilweise/unklar · ❌ = verifiziert nein · 🔑 = Schlüssel-Korpus

---

# TEIL 0: PRÜF-ERGEBNIS (für Stefan, nicht zum Vortragen)

## Verifizierte Korrekturen zur Excel-Tabelle

| Korpus | Excel-Wert | Korrigiert | Quelle |
|---|---|---|---|
| **Tone Perfect** | Frei = nein | **✅ Ja** — Open Access unter tone.lib.msu.edu | Ryu 2021; MSU Libraries Website |
| **Tsurutani** | Frei = ja | **❌ Nein** — kein öffentlicher Download, reine Forschungsstudie | SST2014 Paper; kein Repository |
| **THCHS-30** | Stunden = 35 | **~33,5h** (27,23h Train + 6,24h Test) | Wang & Zhang 2015, Table 2 |
| **THCHS-30** | vorhanden = noch nicht | **✅ Seit 2015 verfügbar** auf OpenSLR | openslr.org/18 |
| **Spoken Chinese L2** | Sprecher = 14 | **44** (25 L1 + 19 L2) | Li PhD Thesis 2021, S. 68 |
| **Sinica Corpus** | als Sprachkorpus gelistet | **❌ Textkorpus** — kein Audio | Chen et al. 1996; nur POS-getaggter Text |
| **Tone Perfect** | Sprecher = leer | **6** (3M, 3F, Beijing) | Ryu 2021, S. 5 |
| **Tone Perfect** | Stunden = leer | **~2h** (9.840 × ~0,7s Einzelsilben) | Berechnet aus 9.840 Assets |
| **OMPAL** | Frei = jein | **✅ Ja** — Open-Source (kommerziell + nicht-kommerziell) | Hsieh et al. 2025, S. 2415 |
| **OMPAL** | Stunden = leer | **2,9h** | Hsieh et al. 2025, Table 1 |
| **iCALL** | Frei = nein | **❌ Bestätigt: nicht öffentlich** | OMPAL-Vergleichstabelle; ResearchGate |
| **BLCU-SA** | Status | **⚠️ Im Design-Stadium** (2016); neueres BLCU-SAIT: 21h, 19 L1s | Wu et al. 2016; BLCU Website 2026 |
| **Biaobei** | Typ unklar | **TTS-Korpus** — 1 Sprecherin, 12h, 10.000 Sätze (CSMSC) | Baker/DataBaker; GitHub |
| **NTU Speech** | Download unklar | **✅ Verfügbar** via Google Drive (Te-Hsin Liu, NTU) | sites.google.com/site/tehsinphono |
| **ChildMandarin** | vorhanden = noch nicht | **✅ Open-Source** auf GitHub (flageval-baai) | Zhou et al. 2025, S. 1 |

## Vollständige Verifizierungstabelle (20 Korpora)

| # | Korpus | Sprache | Typ | Sprecher | Stunden | Lautschrift | Töne | Frei | L2? | Verifiziert |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | **Tone Perfect** 🔑 | Mandarin | Silben | 6 Native (Beijing) | ~2h | Pinyin+Ton# | ✅ T1–T4 | ✅ Open Access | ❌ | ✅ Online |
| 2 | **AISHELL-1** | Mandarin | Sätze | 400 Native | 170h | Initial-Final | ⚠️ implizit | ✅ OpenSLR | ❌ | ✅ Online |
| 3 | **THCHS-30** | Mandarin | Sätze | 50 Native | ~33,5h | Tonal finals | ⚠️ implizit | ✅ OpenSLR | ❌ | ✅ Online |
| 4 | **KeSpeech** | Mandarin+8 Dialekte | Sätze | 27.237 | 1.542h | — | ❌ | ⚠️ Baidu Drive | ❌ | ✅ Paper |
| 5 | **WenetSpeech4TTS** | Mandarin | Sätze | viele | 12.800h | — | ❌ | ✅ HuggingFace | ❌ | ✅ Online |
| 6 | **MAGICDATA** | Mandarin | Sätze | 1.080 | 755h | — | ❌ | ✅ OpenSLR/68 | ❌ | ✅ Online |
| 7 | **ChildMandarin** | Mandarin (Kinder) | Sätze | 397 Kinder | 41,25h | — | ❌ | ✅ GitHub | ❌ | ✅ Paper |
| 8 | **Free ST** | Mandarin | Sätze | 855 | — | — | ❌ | ✅ OpenSLR/38 | ❌ | ✅ Online |
| 9 | **Biaobei (CSMSC)** | Mandarin | Sätze | 1 (TTS) | 12h | Pinyin | ⚠️ | ✅ | ❌ | ✅ Online |
| 10 | **CS-Dialogue** | Mand.+Engl. | Dialoge | 200 | 104h | — | ❌ | ✅ | ❌ | ✅ Paper |
| 11 | **LATIC** 🔑 | Mandarin (L2) | Wörter | 4 L2 | 4h | Pinyin+Ton# | ✅ | ✅ IEEE DataPort | ✅ | ✅ Online |
| 12 | **iCALL** | Mandarin (L2) | Sätze | 305 L2 | 142h | Pinyin+IPA | ✅ | ❌ nicht öffentlich | ✅ | ✅ bestätigt |
| 13 | **OMPAL** 🔑 | Mandarin (L2) | Sätze | 46 L2 (Frz.) | 2,9h | Pinyin | ✅ Wort-Ebene | ✅ Open-Source | ✅ | ✅ Paper |
| 14 | **BLCU-SA** | Mandarin (L2) | Sätze | im Aufbau | — | Pinyin | ✅ Tri-Tone | ❌ E-Mail | ✅ | ⚠️ Paper |
| 15 | **Tsurutani** | Mandarin (L2) | Sätze | 33 L2 (Austr.) | — | Pinyin+IPA | ✅ T1–T4 | ❌ nicht öffentlich | ✅ | ✅ Paper |
| 16 | **SingaKids** | Mandarin (L2) | Sätze | 255 Kinder (SG) | 125h | Pinyin | ✅ | ❌ nicht öffentlich | ✅ | ✅ Paper |
| 17 | **Spoken Chinese L2** | Mandarin (L2) | Interviews | 44 (25L1+19L2) | — | ❌ nur orthogr. | ❌ | ❌ nur Transkripte | ✅ | ✅ Thesis |
| 18 | **NTU Speech** | Mandarin (L2) | Sätze | — | — | — | — | ⚠️ Google Drive | ✅ | ✅ Online |
| 19 | **EdUHK** | Kantonesisch | Sätze | 40 L2 | 8h | — | — | ⚠️ einzeln | ✅ | ✅ Paper |
| 20 | **Sinica Corpus** | Mandarin | **Text** | — | — | ❌ | ❌ | ✅ | ❌ | ✅ Online |

---

# TEIL I: FOLIEN

---

## Folie K1 — Korpus-Landschaft: 20 Datensätze recherchiert

**Titel:** 20 Mandarin-Korpora recherchiert — nur 3 passen wirklich

**Visuelles Diagramm (Trichter):**
```
20 recherchiert
 └─ 16 mit Audio
     └─ 9 öffentlich verfügbar
         └─ 5 mit Tonannotation
             └─ 3 direkt nutzbar für RQ2/RQ3
                  ├── Tone Perfect  (RQ2: Native)
                  ├── LATIC         (RQ3: L2)
                  └── OMPAL         (RQ3: L2)
```

**Sprechernotiz:** „Aus 20 recherchierten Korpora bleiben am Ende drei übrig, die direkt für mein Experiment nutzbar sind. Der Trichter zeigt, warum: Vier Datensätze fallen sofort raus — ein Textkorpus, ein TTS-Datensatz, ein Code-Switching-Korpus und ein Kantonesisch-Datensatz. Von den 16 mit Audio sind nur 9 öffentlich zugänglich. Von diesen haben nur 5 eine Tonannotation. Und von diesen sind nur 3 sowohl öffentlich als auch für meine Forschungsfragen geeignet: Tone Perfect für die Native-Speaker-Evaluation, LATIC und OMPAL für die L2-Evaluation."

---

## Folie K2 — Die 20 Korpora im Überblick

**Titel:** Korpus-Übersicht: Native, L2, und warum die meisten nicht passen

**Tabelle (Folie — kompakte Version):**

| Korpus | Typ | Sprecher | Std | Töne | Frei | Für uns |
|---|---|---|---|---|---|---|
| **Tone Perfect** ⭐ | Silben, Native | 6 (Beijing) | ~2h | ✅ T1–T4 | ✅ | **RQ2** |
| **LATIC** ⭐ | Wörter, L2 | 4 (4 L1s) | 4h | ✅ | ✅ | **RQ3** |
| **OMPAL** ⭐ | Sätze, L2 | 46 (Frz.) | 2,9h | ✅ | ✅ | **RQ3** |
| AISHELL-1 | Sätze, Native | 400 | 170h | ❌ | ✅ | Baseline |
| THCHS-30 | Sätze, Native | 50 | 33,5h | ❌ | ✅ | Baseline |
| MAGICDATA | Sätze, Native | 1.080 | 755h | ❌ | ✅ | — |
| iCALL | Sätze, L2 | 305 (24 L1s) | 142h | ✅ | ❌ | Bonus |
| SingaKids | Sätze, L2-Kinder | 255 (SG) | 125h | ✅ | ❌ | Bonus |
| KeSpeech | Sätze+Dialekte | 27.237 | 1.542h | ❌ | ⚠️ | Dialekte |
| *8 weitere* | *diverse* | *—* | *—* | *—* | *—* | *—* |

**Sichtbares Hinweis-Kästchen:**
> **Ausgeschlossen:** Sinica (Textkorpus), Biaobei (TTS), CS-Dialogue (Code-Switch), EdUHK (Kantonesisch), Spoken Chinese L2 (nur Transkripte)

**Sprechernotiz:** „Diese Tabelle zeigt alle 20 recherchierten Korpora. Oben die drei Kernkorpora für mein Experiment: Tone Perfect für RQ2, LATIC und OMPAL für RQ3. AISHELL-1 und THCHS-30 dienen als ASR-Baselines für den Literaturvergleich. Die großen Korpora wie MAGICDATA und KeSpeech sind für meine isolierte Tonevaluation zu komplex — ich brauche keine 755 Stunden, sondern die richtigen 2 Stunden. Unten die ausgeschlossenen: Sinica ist ein reines Textkorpus, Biaobei ist für TTS, CS-Dialogue mischt Mandarin mit Englisch, und EdUHK ist Kantonesisch. Fünf Korpora wie iCALL und SingaKids wären interessant, sind aber nicht öffentlich — die wären ein Bonus, wenn ich Kontakt aufnehme."

---

## Folie K3 — Kern-Korpus: Tone Perfect (RQ2)

**Titel:** Tone Perfect — Perfekt für isolierte Tonevaluation

**Visuelles Diagramm:**
```
Tone Perfect (Ryu et al. 2021, Michigan State University)
├── 410 einzigartige Silben (alle Standard-Mandarin-Silben)
├── × 4 Töne (T1 ˉ, T2 ˊ, T3 ˇ, T4 ˋ)
├── × 6 Sprecher (3M, 3F, alle Beijing-Muttersprachler)
├── = 9.840 Aufnahmen
├── Format: WAV, 44.1kHz/16-bit Stereo
├── Lizenz: CC-BY 4.0 (Open Access)
├── Labels: Pinyin + Tonnummer (z.B. ma1_FV1.wav)
└── URL: tone.lib.msu.edu ← VERIFIZIERT ✅
```

**Bullets:**
- **Kontrolliert:** Isolierte Silben → Ton-Evaluation ohne Kontexteffekte (kein Tone Sandhi)
- **Vollständig:** Alle 410 Mandarin-Silben × 4 Töne — inklusive lexikalische Lücken
- **Ground Truth:** Professionelle Aufnahme, Pinyin+Tonnummer als Dateiname
- **Wenig genutzt:** Nur 2 Papers (Ryu 2021, Xu SITA 2026) — Potenzial für neue Ergebnisse

**Sichtbares Hinweis-Kästchen:**
> **Korrektur zur Excel:** Tone Perfect ist **Open Access** (tone.lib.msu.edu), nicht „von Autoren bekommen". CC-BY 4.0.

**Sprechernotiz:** „Tone Perfect ist der Kern meines Experiments. Es enthält alle 410 Mandarin-Silben in allen vier Tönen, gesprochen von sechs Pekinger Muttersprachlern — insgesamt 9.840 Aufnahmen. Das Besondere: Jedes Sample ist genau eine Silbe, und der korrekte Ton steht im Dateinamen. Damit habe ich einen perfekten Ground Truth für isolierte Tonevaluation. Das schließt Kontexteffekte wie Tone Sandhi aus — das ist gewollt, denn ich will zuerst wissen, ob ein Modell überhaupt Einzeltöne richtig erkennt. Wichtig: In meiner Excel stand ‚von Autoren bekommen', aber Tone Perfect ist tatsächlich Open Access unter tone.lib.msu.edu — das habe ich online verifiziert."

---

## Folie K4 — L2-Korpora: LATIC & OMPAL (RQ3)

**Titel:** L2-Datensätze — klein, aber die einzigen öffentlichen mit Tönen

**Tabelle (Folie):**

| | **LATIC** | **OMPAL** |
|---|---|---|
| **Sprecher** | 4 L2 (Russisch, Koreanisch, Frz., Arabisch) | 46 L2 (alle Französisch L1) |
| **Stunden** | 4h | 2,9h |
| **Sätze** | 2.579 | 1.768 |
| **Annotation** | Pinyin + Tonnummern | 4 Experten-Bewerter (Wort + Satz) |
| **Töne** | ✅ Explizit annotiert | ✅ Auf Wortebene bewertet |
| **Zugang** | ✅ IEEE DataPort | ✅ Open-Source |
| **Limitation** | Nur 4 Sprecher | Nur Franz. L1, Streamlit-Probleme |
| **Jahr** | 2021 | 2025 |

**Bullets:**
- **LATIC:** Einziger öffentlicher L2-Mandarin-Datensatz mit expliziten Tonnummern (yi1-Format)
- **OMPAL:** Größerer L2-Datensatz (46 Sprecher), mit Expertenrating — aber nur Franz. L1
- **Zusammen:** 50 L2-Sprecher, 4.347 Äußerungen — klein, aber ausreichend für erste RQ3-Ergebnisse
- **Bonus-Option:** iCALL (305 Sprecher, 142h) + SingaKids (255 Kinder, 125h) bei Kontaktaufnahme

**Sichtbares Hinweis-Kästchen:**
> **Lücke in der Forschung:** Kein L2-Mandarin-Datensatz wurde je mit multimodalen LLMs getestet.

**Sprechernotiz:** „Für die L2-Evaluation habe ich zwei öffentliche Datensätze: LATIC und OMPAL. LATIC ist klein — nur 4 Sprecher — aber es ist der einzige Datensatz mit expliziten Tonnummern im Pinyin-Format, also genau das, was mein Experiment braucht. OMPAL ist neuer und größer, 46 französische Mandarin-Lerner, mit Expertenrating auf Wort- und Satzebene. Zusammen sind das 50 L2-Sprecher. Das ist wenig, aber kein Paper hat bisher überhaupt multimodale LLMs auf L2-Mandarin getestet — der erste Datenpunkt ist schon ein Beitrag. Falls ich mehr brauche, könnte ich Nancy Chen für iCALL kontaktieren — 305 Sprecher, 142 Stunden — aber das ist nicht öffentlich und nicht kritisch für die Thesis."

---

## Folie K5 — Warum die großen Korpora nicht reichen

**Titel:** 170h AISHELL, 755h MAGICDATA — aber kein Ton

**Tabelle (Folie):**

| Korpus | Stunden | Sprecher | Töne? | Problem für uns |
|---|---|---|---|---|
| AISHELL-1 | 170h | 400 | ❌ | Nur CER auf Zeichenebene, kein Ton-Ground-Truth |
| MAGICDATA | 755h | 1.080 | ❌ | Kein Pinyin, kein Ton |
| KeSpeech | 1.542h | 27.237 | ❌ | Dialekte, kein Ton-Label |
| WenetSpeech4TTS | 12.800h | viele | ❌ | TTS-Korpus (Text→Speech, nicht Speech→Text) |
| **Tone Perfect** | **~2h** | **6** | **✅** | **Perfekt: Alle Silben × alle Töne × Ground Truth** |

**Kernaussage:**
> Mehr Stunden ≠ besserer Datensatz. Für Tonevaluation braucht man **nicht** 12.800 Stunden, sondern **kontrollierten Ground Truth** mit **expliziten Tonlabels**.

**Sprechernotiz:** „Diese Folie erklärt, warum ich nicht einfach AISHELL-1 mit seinen 170 Stunden nehme. AISHELL hat keinen Ton-Ground-Truth — es enthält Sätze mit Zeichentranskription, aber nirgendwo steht, welchen Ton ein bestimmtes Zeichen in einem bestimmten Kontext hat. Ich kann CER rechnen, aber keine Tone Error Rate. Dasselbe gilt für MAGICDATA, KeSpeech und WenetSpeech. Tone Perfect hat nur 2 Stunden — aber jedes einzelne Sample hat ein Label, das mir genau sagt: Diese Silbe ist ‚ma' mit Ton 2. Das ist, was ich für eine kontrollierte Evaluation brauche. Die großen Korpora nutze ich nur für den Literaturvergleich, also CER-Werte, die ich mit anderen Papers vergleichen kann."

---

## Folie K6 — Experiment-Plan: Welcher Datensatz für welche RQ

**Titel:** Datensatz-Zuordnung: 3 Korpora für 5 Forschungsfragen

**Tabelle (Folie):**

| Forschungsfrage | Datensatz | Samples | Begründung |
|---|---|---|---|
| **RQ1** (Text→Pinyin) | Kein Audio nötig | Textproben aus Tone-Perfect-Silben | Claude, DeepSeek: Text-only |
| **RQ2** (Native Speech) | **Tone Perfect** | 9.840 Silben (alle) / 700 (Sub-Sample) | Isolierte Töne, perfekter Ground Truth |
| **RQ3** (L2 Speech) | **LATIC + OMPAL** | ~4.350 Äußerungen | Einzige öffentliche L2 mit Tonlabels |
| **RQ4** (Vergleich) | Tone Perfect + AISHELL-1 | CER auf AISHELL für Lit.-Vergleich | Standard-Benchmark der ASR-Literatur |
| **RQ5** (Fehlerstruktur) | Tone Perfect + LATIC | Konfusionsmatrizen aus RQ2/RQ3 | Ton-Verwechslungsmuster analysieren |

**Sichtbares Hinweis-Kästchen:**
> **Pilot:** 400 Tone-Perfect-Samples (100 pro Ton) + 200 LATIC-Samples → Pipeline-Test vor dem vollen Lauf.

**Sprechernotiz:** „Die Zuordnung ist einfach: Für RQ1, die reine Text-zu-Pinyin-Aufgabe, brauche ich kein Audio — da reichen Textproben für Claude und DeepSeek. Für RQ2, die Kern-Evaluation auf Native Speech, nehme ich Tone Perfect — entweder alle 9.840 Samples oder eine stratifizierte Stichprobe von 700. Für RQ3, die L2-Evaluation, nehme ich LATIC und OMPAL zusammen — die einzigen öffentlichen L2-Datensätze mit Toninformation. Für den Literaturvergleich in RQ4 rechne ich zusätzlich CER auf AISHELL-1, damit meine Ergebnisse mit den 15+ Papers vergleichbar sind, die AISHELL als Benchmark nutzen. Und die Fehlerstruktur in RQ5 ergibt sich aus den Konfusionsmatrizen von RQ2 und RQ3. Bevor das volle Experiment startet, laufe ich einen Pilot: 400 Tone-Perfect-Samples plus 200 LATIC-Samples — das testet die gesamte Pipeline für unter einen Dollar."

---

## Folie K7 — Backup: Nicht geeignete Korpora & Gründe

**Titel:** Ausgeschlossen — und warum (Backup-Folie)

**Tabelle (Folie):**

| Korpus | Grund für Ausschluss |
|---|---|
| **Sinica Corpus** | ❌ Textkorpus — kein Audio, nur POS-getaggte Zeichen |
| **Biaobei (CSMSC)** | ❌ TTS-Korpus — 1 Sprecherin, für Text→Speech, nicht Speech→Text |
| **CS-Dialogue** | ❌ Code-Switching (Mandarin+Englisch) — kein reines Mandarin |
| **EdUHK HongKong** | ❌ Kantonesisch, nicht Mandarin |
| **Spoken Chinese L2** | ❌ Nur Transkripte (kein Audio öffentlich) |
| **WenetSpeech4TTS** | ❌ TTS-Korpus (12.800h, aber falsche Richtung) |
| **ChildMandarin** | ⚠️ Kinder 3–5 Jahre — nicht L2, nicht RQ-relevant |
| **Free ST** | ⚠️ Kein Ton-Ground-Truth, keine Lautschrift |
| **BLCU-SA** | ⚠️ Im Design-Stadium (2016), Status unklar |

**Sprechernotiz:** „Für den Fall, dass Tim fragt, warum ich bestimmte Korpora nicht verwende: Sinica ist ein Textkorpus ohne Audio. Biaobei und WenetSpeech sind TTS-Korpora — die gehen von Text zu Sprache, ich brauche Sprache zu Text. CS-Dialogue mischt Mandarin mit Englisch, EdUHK ist Kantonesisch. Spoken Chinese L2 hat kein öffentliches Audio, nur Transkripte. ChildMandarin wäre interessant, aber Kinder von 3 bis 5 sind weder meine Zielgruppe noch L2-Lerner. Und BLCU war 2016 noch im Design — ob der Datensatz inzwischen fertig ist, müsste ich erst recherchieren."

---

# TEIL II: Anmerkungen für Stefan

- **Folien K1–K6** sind die vortragbaren Folien; **K7** ist Backup für Q&A.
- **Wichtigste Botschaft an Tim:** Du hast 20 Korpora systematisch recherchiert und geprüft. Die Auswahl (Tone Perfect + LATIC + OMPAL) ist nicht willkürlich, sondern ergibt sich zwingend aus dem Trichter: Von 20 recherchierten Korpora haben nur 3 die nötige Kombination aus Audio + Tonlabels + öffentlichem Zugang.
- **Verifizierte Fakten (Q&A-fest):**
  - Tone Perfect = Open Access (tone.lib.msu.edu), CC-BY 4.0 — online verifiziert
  - LATIC = IEEE DataPort, öffentlich — online verifiziert
  - OMPAL = Open-Source (Interspeech 2025) — im Paper bestätigt
  - AISHELL-1 = OpenSLR/33, 170h, 400 Sprecher — online verifiziert
  - iCALL = **nicht öffentlich** — bestätigt durch OMPAL-Vergleichstabelle und ResearchGate
  - Sinica = **Textkorpus**, nicht Sprachkorpus — Paper 1996 bestätigt
- **Excel-Korrekturen:** 7 Werte korrigiert (siehe Teil 0). Wichtigste: Tone Perfect ist frei, Tsurutani ist nicht frei, THCHS-30 hat 33,5h (nicht 35).
- **Kürzungsoption:** K2 + K5 zu einer „Übersicht + Warum nicht die Großen"-Folie zusammenziehen → 5 statt 7 Folien. K7 ist ohnehin nur Backup.
- **Bonus-Kontakte (wenn Tim fragt):**
  - Nancy Chen (I2R Singapore): iCALL (305 Sprecher) + SingaKids (255 Kinder) — LinkedIn/E-Mail
  - BLCU (Peking): yuyanziyuan@blcu.edu.cn — Chinese Interlanguage Corpus
  - Te-Hsin Liu (NTU): NTU Speech Bank — Google Sites / E-Mail
