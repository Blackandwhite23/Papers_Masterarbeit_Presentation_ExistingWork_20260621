# Wie wird Mandarin-Spracherkennung wirklich ausgewertet?
## Was CER, WER, PER, TER bei einem Wort wie 电脑 (diàn nǎo) konkret messen

> **Kontext:** Klärung der Frage „Ist die Einheit ‚dian' oder ‚dian1'? Wie prüft man, ob Konsonanten/Vokale korrekt sind UND ob die Töne korrekt sind?" — direkt relevant für Thesis §2.5 (Evaluation) und Kapitel 3 (Metriken).
>
> **Beispielwort:** 电脑 = „Computer", Pinyin **diàn nǎo** = `dian4 nao3`. (Hinweis: tatsächlich ist 电 = dian**4** und 脑 = nao**3**; im Folgenden mit korrekten Tönen.)

---

## 1. Die Kurzantwort

**In der Standard-Auswertung (CER) ist die Einheit weder „dian" noch „dian1" — sondern das chinesische Schriftzeichen 电.**

Reguläre Mandarin-ASR gibt **chinesische Zeichen** aus, kein Pinyin und keine Tonzahlen. CER zählt, ob 电 und 脑 als Zeichen korrekt sind. Der Ton ist dabei **nie ein eigener Output** — er steckt implizit im Zeichen (电 *ist* dian4). Ein Tonfehler wird nur sichtbar, wenn er zu einem **falschen Zeichen** führt (meist einem Homophon).

**Konsequenz — und genau dein Punkt:** Weder CER noch WER trennen „sind die Konsonanten/Vokale richtig" von „sind die Töne richtig". Beides verschwimmt in einer einzigen Zahl. Wer beide Achsen getrennt wissen will, muss die Silbe **zerlegen** und **zwei getrennte Metriken** rechnen. Das ist die Lücke, die deine Arbeit adressiert.

---

## 2. Die drei Ebenen, auf denen Mandarin ausgewertet wird

### Ebene 1 — Zeichenebene: CER (Character Error Rate)

Die mit Abstand häufigste Metrik. Einheit = **chinesisches Zeichen (汉字)**.

```
CER = (S + D + I) / N
```
S = Substitutionen, D = Deletionen, I = Insertionen, N = Anzahl Referenzzeichen.

Für 电脑 ist **N = 2** (电, 脑). Berechnung über die Zeichenfolge, per Levenshtein-Alignment.

- **Was CER misst:** Ist die *Zeichenausgabe* korrekt?
- **Was CER NICHT misst:** Initialen, Finalen oder Töne als separate Größen. Der Ton ist im Zeichen „eingebacken".
- Alle Speech-LLMs im Korpus (Seed-ASR, FireRedASR, Kimi-Audio, Step-Audio 2, Qwen3-ASR) berichten **ausschließlich CER**. Der Peng-Survey bestätigt: *„Automatic Speech Recognition (ASR): Word Error Rate (WER), Character Error Rate (CER), Match Error Rate (MER)"* (Sec. VII.A, S. 18) — mehr nicht.

### Ebene 2 — Wortebene: WER (Word Error Rate)

Einheit = **Wort** (nach Wortsegmentierung). 电脑 wäre **1 Wort**.

- Für Chinesisch unüblich, weil es keine Leerzeichen gibt und Segmentierungsstandards uneinheitlich sind → **CER ist im Chinesischen Standard**, WER eher für Englisch.
- Misst, wie CER, die *orthografische* Korrektheit, nicht Phoneme/Töne.

### Ebene 3 — Phonetische Ebene: Pinyin / IPA (hier lebt deine Frage)

Erst wenn die Ausgabe **Pinyin oder IPA** ist, taucht überhaupt die Frage auf, ob die Einheit „dian" oder „dian4" ist. Und genau hier muss man eine **Einheit festlegen** — diese Wahl bestimmt, was gemessen wird.

---

## 3. Die Mandarin-Silbe: Initial + Final + Ton

Jede Mandarin-Silbe zerfällt in drei Teile (das **„initial-final-tone system"**):

| Komponente | 中文 | Beispiel `dian4` | Beispiel `nao3` |
|---|---|---|---|
| **Initial** (Konsonant-Anlaut) | 声母 | `d` | `n` |
| **Final** (Vokal + evtl. Koda) | 韵母 | `ian` | `ao` |
| **Ton** (Tonkontur) | 声调 | `4` (fallend) | `3` (Tiefton) |

Die fünf Töne: T1 hoch, T2 steigend, T3 fallend-steigend, T4 fallend, T5 neutral/tonlos.

> Wang, Shi & Wang (2024, Pitch-Aware RNN-T, Paper 15) nutzen genau dieses System: *„Pronunciations are represented using the initial-final-tone system, where syllables are broken down into their initial consonant sounds, final vowel sounds, and tones."* — mit **214 tonalen Phonem-Tokens** im Vokabular.

**Konsonanten/Vokale = Initial + Final. Töne = die Tonzahl.** Das ist die Zerlegung, die du brauchst, um deine zwei Fragen getrennt zu beantworten.

---

## 4. Der Kern: „dian" vs. „dian4" — die Wahl der Einheit

Wenn die Ausgabe Pinyin ist, gibt es **drei gängige Token-Definitionen**, und sie liefern unterschiedliche Zahlen:

| Token-Definition | 电脑 wird zu | Was ein Fehler bedeutet |
|---|---|---|
| **(a) Tonale Silbe** | `dian4`, `nao3` | Tonfehler UND Segmentfehler sind **nicht unterscheidbar** — beides „Token falsch" |
| **(b) Initial + tonaler Final** | `d` + `ian4`, `n` + `ao3` | Initial-Fehler trennbar; Final- und Tonfehler noch vermischt |
| **(c) Voll zerlegt** | `d` / `ian` / `4`, `n` / `ao` / `3` | **Segment und Ton vollständig getrennt** → genau was du willst |

Dass die Einheit die Zahlen verschiebt, ist empirisch belegt: berichtete Tonfehlerraten von **5,0 % mit „Initialen + tonalen Finalen" vs. 2,8 % mit „tonalen Silben"** als Einheit (Modeling-Units-Literatur). → **Du musst die Einheit fixieren und im Methodenteil begründen**, sonst sind Fehlerraten künstlich beeinflusst.

→ **Antwort auf deine Frage:** „dian" oder „dian4" ist eine *Designentscheidung*. In Standard-CER ist es ohnehin keins von beiden (sondern 电). Für *deine* Fragestellung ist **(c) voll zerlegt** die richtige Wahl.

---

## 5. So trennt man „Segmente korrekt?" von „Töne korrekt?" — zwei Wege

### Weg A — Eingebettet, dann vergleichen (einfach, grob)

Transkribiere **mit** Tonmarkern und rechne CER (= „tonal CER"). Rechne zusätzlich CER **ohne** Töne. Die **Differenz** ist der Beitrag der Töne.

> Chirkova et al. (2025, Baima, Paper 21) machen genau das: *„using tone increases the error rate. It increases CER by 6.9±3.6 points"* (Sec. 3.1, S. 175). Sie isolieren so, wie viel Fehler allein auf Töne entfällt — und zerlegen die Fehler weiter nach **Konsonant / Vokal / Ton** (*„the way vowels and consonants are transcribed can affect tonal … "*, Sec. 5).

- **Vorteil:** leicht, literatur-kompatibel.
- **Nachteil:** indirekt; sagt nicht pro Silbe „Segment ok, Ton falsch".

### Weg B — Entkoppelt (sauber, empfohlen für deine RQs)

Silben-Alignment, dann **zwei unabhängige Achsen** scoren:

1. **Segmentale Fehlerrate** — Initial + Final korrekt? (= PER auf **tonlosem** Pinyin, oder getrennte Initial-Accuracy / Final-Accuracy)
   → beantwortet **„sind Konsonanten und Vokale korrekt?"**
2. **Tone Error Rate (TER)** — Anteil Silben mit falschem Ton (unabhängig vom Segment)
   → beantwortet **„sind die Töne korrekt?"**

> Zou et al. (2025, Review 61 Studien, Paper 9) empfehlen **TER** explizit als Metrik — wenden sie aber in keiner Studie an. **In keinem der 42 Papers wird TER tatsächlich berechnet.** Genau das führst du erstmals systematisch für multimodale LLMs ein.

**Wichtig:** Wang MDDs 214 „tonale Phoneme" (Weg-A-artig, Token = `ian4`) vermischen Ton und Segment in *einem* Token — der WebFetch bestätigt: das Paper trennt Ton und Segment in der Auswertung **nicht**. Das ist die Falle, die du mit Weg B vermeidest.

---

## 6. Worked Example — 电脑 (Ziel: `dian4 nao3`)

| Modell-Ausgabe | Was passierte | Standard-CER (Zeichen) | Segmental (Init+Final) | TER (Ton) |
|---|---|---|---|---|
| `dian4 nao3` | alles korrekt | 0 % | 0 % | 0 % |
| `dian4 **l**ao3` | Initial n→l | Zeichen falsch ⇒ Fehler | **1 Initial falsch** | Ton korrekt |
| `dian**1** nao3` | Ton 4→1 | Zeichen falsch/Homophon | Segment korrekt | **1/2 = 50 %** |
| `dian4 nao**2**` | Ton 3→2 (T3↔T2!) | Zeichen falsch | Segment korrekt | **1/2 = 50 %** |

**Die Pointe:** In Spalte „Standard-CER" sehen Zeile 2, 3 und 4 **gleich** aus („ein Zeichen falsch") — du erfährst **nicht**, ob das Modell den *Laut* oder den *Ton* verfehlt hat. Erst die Entkopplung (Spalten 4–5) zeigt: Zeile 2 ist ein Segmentproblem, Zeile 3–4 sind reine **Tonprobleme** (und T3↔T2/T3 ist laut Huang-Review die häufigste Verwechslung). Das ist exakt die Information, die ein Aussprache-Feedback braucht.

---

## 7. Empfehlung für deine Thesis (Kapitel 3, Metriken)

Lass die Modelle **Pinyin mit expliziten Tonnummern** ausgeben (`ma1`), fixiere die Einheit auf **voll zerlegt (Weg c)** und werte auf **drei entkoppelten Achsen** aus — passend zu deiner RQ-Struktur:

| Ebene | Metrik | Beantwortet | RQ |
|---|---|---|---|
| Wort/Zeichen | **CER** | Literatur-Vergleichbarkeit | RQ2a |
| Phonem/Segment | **PER** bzw. Initial-/Final-Accuracy (tonloses Pinyin) | „Konsonanten & Vokale korrekt?" | RQ2b |
| Ton | **TER** (+ per-Ton-Konfusionsmatrix) | „Töne korrekt?" | RQ2c / RQ5 |

**Warum das neu ist:** Die Standard-LLMs geben Zeichen aus und verstecken den Ton darin. Indem du Pinyin **mit** Tonnummer als Output erzwingst und Segment vs. Ton getrennt scorest, machst du den Ton zur **eigenen, messbaren Achse** — was bislang niemand für multimodale LLMs getan hat (Lücke 2).

---

## 8. Belege / Quellen

**Aus dem Korpus (verifiziert):**
- Wang, Shi & Wang (2024), Pitch-Aware RNN-T (Paper 15): initial-final-tone system, 214 tonale Phoneme; Ton/Segment in der Auswertung **nicht** getrennt.
- Chirkova et al. (2025), Baima (Paper 21): tonal CER; *„using tone increases the error rate … by 6.9±3.6 points"* (S. 175); Zerlegung nach Konsonant/Vokal/Ton.
- Zou et al. (2025), Review (Paper 9): empfiehlt **TER**; DL-Tonerkennung ~88,8 % vs. traditionell 83,1 %.
- Peng et al. (2025), Survey (Paper 7): ASR-Evaluation auf WER/CER/MER beschränkt (Sec. VII.A, S. 18).

**Online (maßgebliche Definitionen):**
- *Research on Modeling Units of Transformer Transducer for Mandarin Speech Recognition* (arXiv:2004.13522): Vergleich „syllable initial/final with tone" vs. „syllable with tone" vs. „Chinese character"; tonale Silbe als Einheit am besten.
- *Decoupling Recognition and Transcription in Mandarin ASR* (arXiv:2108.01129): Trennung von Aussprache-Ebene (Pinyin/Initial/Final/Ton) und Zeichen-Transkription.
- *Pitch-Aware RNN-T* HTML (arXiv:2406.04595): initial-final-tone-Definition, 214 tonale Phoneme.

---

## 9. Pinyin-CER statt Hanzi-CER — und phonetisch *gewichtete* Metriken

> **Frage:** Gibt es Papers, die CER nicht auf Zeichen, sondern auf Pinyin messen? Und: „dian vs. lian" (1 Laut daneben) sollte doch viel weniger bestraft werden als „dian vs. cao" (komplett daneben). Außerdem: Hanzi-CER ist für einen Vokabeltrainer irrelevant, oder?

**Kurz: Ja zu beidem — und deine Intuition hat einen Namen.** Es gibt drei Stufen von „streng" zu „phonetisch fair":

### Stufe 1 — Hanzi-CER (Editierdistanz auf Zeichen)
Alles-oder-nichts pro Zeichen, Ton implizit. 电 falsch = 1 Fehler, egal *wie* falsch. **Schlechteste Wahl für deinen Zweck.**

### Stufe 2 — Pinyin-CER / Pinyin Error Rate (Editierdistanz auf dem Pinyin-*String*)
Hier wird auf der Buchstabenfolge des Pinyin gerechnet. **Das löst dein erstes Beispiel schon teilweise:**
- `dian` → `lian`: **1** Substitution (d→l) bei 4 Zeichen
- `dian` → `cao`: **~3** Editieroperationen

Der Near-Miss kostet also automatisch weniger als der Total-Miss. **Papers in deiner Liste, die genau das tun:**

| Paper | Metrik | Wert | Töne? |
|---|---|---|---|
| **Zhengjie & Cheng (13)** | Pinyin Error Rate | 1,9 % | nein (Pinyin ohne Ton) |
| **Chen SCCM (14)** | Pinyin-CER | 2,41 % | nein |
| **Li PY-GEC (11)** | Pinyin-Feature-Integration; Cosine-Similarity phonetisch ähnlicher Zeichen 0,26→0,82 | — | teils |
| **Chirkova (21)** | CER/WER auf Romanisierung (Pinyin/IPA), getrennt nach Konsonant/Vokal/Ton | — | ja |

→ Pinyin-CER existiert also und ist literatur-etabliert. **Aber:** String-Editierdistanz behandelt jede Buchstaben-Substitution gleich teuer (`b→p` kostet so viel wie `b→x`). Das ist noch nicht ganz, was du willst.

### Stufe 3 — Phonetisch *gewichtete* Distanz (genau deine Intuition, formalisiert)
Hier wird der Substitutionspreis **nach phonetischer Ähnlichkeit** gewichtet: ein Laut, der nur in *einem* Merkmal abweicht (z. B. Stimmhaftigkeit `p`/`b`), kostet weniger als ein völlig anderer Laut. **Etablierte Metriken:**

- **PFER (Phone/Phonetic Feature Error Rate)** — Editierdistanz über **24 artikulatorische PanPhon-Merkmale**; jede Substitution bekommt Teilpunkte je nach Merkmalsabstand. **Liegt in deiner Liste:** Zhu et al. (2025, ZIPA, Paper 18) berichten PFER **0,44** für Mandarin. Zitat: *„the top errors are the substitution of vowels that are close in the vowel space"* (Sec. 7, S. 8) — d. h. die Metrik bestraft genau die *nahen* Verwechslungen mild.
- **WPER (Weighted Phoneme Error Rate)** — ersetzt die Substitutions-*Zählung* durch die Summe der Phonem-Ähnlichkeiten (Ähnlichkeitsmatrix ∈ (0,1)); abgestufte statt binärer Strafen (Phoneme-Similarity-Modeling, arXiv:2507.14346).
- **Artikulatorisch gewichtete Phonem-Editierdistanz** — explizit für **Aussprache-Scoring im Sprachenlernen** entwickelt; `p`/`b` (nur Stimmhaftigkeit) kostet weniger als `p`/`k` (arXiv:1905.02639). Das ist quasi die Metrik-Blaupause für einen Vokabeltrainer.

→ **Das ist die rigorose Antwort auf „dian/lian << dian/cao":** phonetisch gewichtete Editierdistanz (PFER/WPER). Kein Paper im Korpus wendet sie auf **multimodale LLMs für Mandarin mit Tönen** an — erneut deine Lücke.

### Dein konzeptioneller Punkt: Ist Hanzi-CER „Quatsch" für einen Vokabeltrainer?

**Im Kern ja — mit einer Nuance.** Für Aussprache-Feedback ist die *Zeichenidentität* tatsächlich irrelevant: Wenn Initial, Final und Ton stimmen, war die Aussprache korrekt — *welches* Zeichen das Modell daraus rät, ist egal. Zwei Gründe, warum phonetische Bewertung sogar *besser* ist:

1. **Hanzi-CER misst das Falsche.** Es belohnt Homophon-Disambiguierung (Sprachmodell-Wissen), nicht Aussprache. Für einen Trainer ist das die falsche Zielgröße.
2. **Hanzi-CER lädt das Target-Leakage-Problem ein** (Nordstern „linguistic trap"): Sobald das Modell auf ein plausibles Zeichen normalisiert, „korrigiert" es den Lernerfehler weg. Phonetische Bewertung dessen, *was tatsächlich gesprochen wurde* (Pinyin+Ton), umgeht das.

**Die eine Nuance:** Hanzi-CER trotzdem *zusätzlich* berichten — aber nur als **Vergleichsanker zur ASR-Literatur** (RQ4 vs. Whisper/Seed-ASR, die alle in Hanzi-CER berichten). Deine *inhaltliche* Bewertung baust du auf der phonetischen Ebene auf.

### Empfehlung (ergänzt Abschnitt 7)
Für den Vokabeltrainer-Zweck ist die ideale Metrik-Hierarchie:

| Zweck | Metrik | Begründung |
|---|---|---|
| Literatur-Vergleich | Hanzi-CER | nur Vergleichbarkeit (RQ4) |
| Segment „streng" | Pinyin-CER (tonlos) | Initial+Final, Near-Miss schon abgestuft |
| Segment „phonetisch fair" | **PFER / WPER** | `d→l` < `d→c`; Teilpunkte nach Merkmalen (RQ2b, RQ5) |
| Ton | **TER** + Konfusionsmatrix | Töne separat (RQ2c, RQ5) |

So bestrafst du Near-Misses fair, trennst Laut von Ton, und vermeidest die Zeichen-Falle — genau was ein Aussprache-Trainer braucht.

**Belege Stufe 3 (online):**
- *Phoneme Similarity Modeling for Phonetic Error Detection* (arXiv:2507.14346): WPER, Phonem-Ähnlichkeitsmatrix, abgestufte Strafen.
- *Transparent pronunciation scoring using articulatorily weighted phoneme edit distance* (arXiv:1905.02639): gewichtete Editierdistanz fürs Aussprache-Scoring.
- PanPhon-Merkmals-Editierdistanz als Grundlage von PFER (Zhu et al. ZIPA, Paper 18).

---

Sources:
- https://arxiv.org/abs/2004.13522
- https://arxiv.org/abs/2108.01129
- https://arxiv.org/html/2406.04595v1
- https://pmc.ncbi.nlm.nih.gov/articles/PMC12233228/ (Chen et al., SCCM — Pinyin/Zeichen-Erkennung)
- https://arxiv.org/html/2507.14346v1 (WPER — Phoneme Similarity Modeling)
- https://arxiv.org/abs/1905.02639 (Articulatorily weighted phoneme edit distance — pronunciation scoring)
