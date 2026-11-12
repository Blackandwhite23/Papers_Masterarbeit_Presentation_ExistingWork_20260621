# PowerPoint — Evaluation Metrics
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Vocabulary*

> **Zweck:** Folien zum Thema „Wie wird Transkription/Aussprache ausgewertet?" — mit Angabe, **welche Metrik in welchem Paper** genutzt wird, **was häufig** verwendet wird und **was eine neue Metrik** für diese Arbeit sein könnte.
>
> **Legende:** `[Standard]` = dominiert die Literatur · `[selten]` = wenige Paper · `[1 Paper]` = Einzelfall · `[NEU]` = in diesem Kontext (multimodale LLMs + Mandarin-Ton) noch nicht angewandt → Beitrag dieser Arbeit.

---

# TEIL I: Master-Übersichtstabelle (Backup-Folie / Handout)

| Metrik | Was sie misst | Genutzt in (Paper) | Häufigkeit | Für unsere Arbeit |
|---|---|---|---|---|
| **CER** (Character Error Rate) | Editierdistanz auf chinesischen Zeichen | Seed-ASR (1), FireRedASR (2,3), Kimi (4), Qwen3 (5), Step-Audio2 (6), PYG (13), Chen (14), Benchmarks (20–24) | **`[Standard]`** (20+ Paper) | nur Vergleichsanker (RQ4) |
| **WER** (Word Error Rate) | Editierdistanz auf Wörtern | Speech LLMs (1–6), Du (8), Chirkova (21), ContextASR (23), WildSpeech (30) | **`[Standard]`** | kaum (Chinesisch → CER) |
| **PER** (Phone Error Rate) | Editierdistanz auf Phonemen | Wang MDD (15), VocalBench-zh (24), Dyn-SUPERB (27) | `[selten]` | Segment-Ebene (RQ2b) |
| **Pinyin ERR / Pinyin-CER** | CER auf Pinyin-String | Zhengjie PYG (13), Chen SCCM (14) | `[selten]` | Segment-Ebene (RQ2b) |
| **tonal CER/WER** | CER/WER inkl. Tonmarker | Chirkova (21) | `[1 Paper]` | Ton eingebettet |
| **PFER** (Phone **Feature** Error Rate) | feature-gewichtete Distanz (PanPhon, 24 Merkmale) | Zhu ZIPA (18) | `[1 Paper]` | **fair gewichtet (RQ2b/RQ5)** |
| **TER** (Tone Error Rate) | Anteil Silben mit falschem Ton | empfohlen von Zou (9) — **von keinem angewandt** | `[0 Paper]` | **`[NEU]` Kernmetrik (RQ2c)** |
| **FRR / FAR / DA** | False Reject/Accept, Diagnostic Accuracy | Wu (16), Wang MDD (15) | `[selten]` | Mispronunciation (RQ3/RQ5) |
| **DER** (Diagnostic Error Rate) | Fehler in der Fehlerdiagnose | Wang MDD (15) | `[1 Paper]` | optional (RQ5) |
| **F1 / Precision / Recall** | Klassifikationsgüte | Zou (9), PYG (13), Wang MDD (15), Huang (17) | `[häufig in Klass.]` | per-Ton-F1 (RQ5) |
| **MSE / RMSE** | Ton-Distanz (Regression) | Bu Siamese (10) | `[1 Paper]` | nein |
| **Accuracy** | Klassifikationsgenauigkeit | Zou (9), Huang (17), MSPB (20), Benchmarks (25–29) | `[häufig in Benchm.]` | Ton-Acc (RQ2c) |
| **BLEU / COMET** | Übersetzungsgüte | WildSpeech (30), Gaido (32) | `[selten]` | nein |
| **MOS** | subjektive Qualität (Hörurteil) | AudioBench (29), Latif (31) | `[selten]` | nein |
| **WPER** (Weighted Phoneme ER) | Phonem-Ähnlichkeitsmatrix, abgestuft | online (arXiv:2507.14346) — **nicht in Korpus** | `[extern]` | **`[NEU]`-Kandidat** |
| **Artik. gew. Editierdistanz** | Aussprache-Scoring, gewichtet | online (arXiv:1905.02639) — **nicht in Korpus** | `[extern]` | **`[NEU]`-Kandidat** |

---

# TEIL II: FOLIEN

---

## Folie M1 — Übersicht: Die Metrik-Landschaft

**Titel:** Wie Transkription ausgewertet wird — und wo die blinden Flecken liegen

**Bullets:**
- **Dominanz:** CER und WER tragen die gesamte Speech-LLM-Literatur (20+ Paper) — aber nur auf Zeichen-/Wortebene
- **Selten:** Phonem-Ebene (PER, Pinyin-CER) — nur einzelne Paper
- **Fast leer:** Ton als eigene Metrik (TER) — empfohlen, aber von **keinem** Paper angewandt
- **Unbekannt im LLM-Kontext:** phonetisch *gewichtete* Distanz (PFER/WPER)

**Sichtbares Mini-Diagramm (Pyramide):**
```
   CER / WER          ← Standard (20+ Paper)
   PER / Pinyin-CER    ← selten (≈4 Paper)
   PFER                ← 1 Paper (ZIPA)
   TER                 ← 0 Paper (nur empfohlen)
```

**Sprechernotiz:** „Die Metrik-Landschaft ist eine Pyramide: Unten, breit, CER und WER — die gesamte Speech-LLM-Literatur misst praktisch nur das. Darüber, schon dünn, die Phonem-Ebene. Ganz oben, fast leer, die Tonebene. Die Tone Error Rate wird empfohlen, aber in keinem einzigen der 42 Paper tatsächlich berechnet. Je relevanter eine Metrik für mein Thema wird, desto seltener wird sie benutzt."

---

## Folie M2 — Standard: CER & WER `[Standard]`

**Titel:** CER & WER — der Standard, der Töne versteckt

**Bullets:**
- **CER** = Editierdistanz auf chinesischen **Zeichen**; **WER** = auf **Wörtern**
- Genutzt von **allen** Speech LLMs: Seed-ASR (1), FireRedASR (2/3), Kimi-Audio (4), Qwen3-ASR (5), Step-Audio 2 (6)
- Misst **orthografische** Korrektheit — **nicht** Initial/Final/Ton getrennt
- Ton ist im Zeichen „eingebacken" → ein Tonfehler ist nicht von einem Lautfehler unterscheidbar

**Sichtbares Zitat:**
> ASR-Evaluation = *„Word Error Rate (WER), Character Error Rate (CER), Match Error Rate (MER)"* — Peng Survey (2025), Sec. VII.A, S. 18

**Sprechernotiz:** „CER und WER sind der unangefochtene Standard — der Peng-Survey listet für ASR genau diese drei Metriken, mehr nicht. CER rechnet Editierdistanz auf den chinesischen Zeichen, WER auf Wörtern. Das Problem für mein Thema: Beide messen die orthografische Ausgabe. Bei 电脑 zählt nur, ob die Zeichen 电 und 脑 stimmen. Der Ton steckt implizit im Zeichen — ich kann nicht sehen, ob das Modell den Laut oder den Ton verfehlt hat. Deshalb tauge diese Metriken bei mir nur als Vergleichsanker zur Literatur, nicht als inhaltliche Bewertung."

---

## Folie M3 — Phonem-Ebene: PER, Pinyin-CER, PFER `[selten]`

**Titel:** Phonem-Ebene — und die faire, *gewichtete* Distanz

**Bullets:**
- **PER** (Phone Error Rate): Wang MDD (15), VocalBench-zh (24), Dyn-SUPERB (27)
- **Pinyin-CER / Pinyin ERR**: Zhengjie PYG (13, 1,9 %), Chen SCCM (14, 2,41 %) — Pinyin statt Hanzi, aber **ohne Töne**
- **PFER** (Phone *Feature* Error Rate): Zhu ZIPA (18) — gewichtet nach **24 artikulatorischen Merkmalen**
- → PFER bestraft `d→l` (nah) **milder** als `d→c` (fern) — Teilpunkte statt alles-oder-nichts

**Sichtbares Zitat:**
> *„the top errors are the substitution of vowels that are close in the vowel space"* — Zhu et al., ZIPA (2025), Sec. 7, S. 8

**Sprechernotiz:** „Eine Ebene tiefer wird es dünn. Phone Error Rate nutzen nur wenige, etwa Wang's Pitch-Aware-Modell. Pinyin-CER — also Editierdistanz auf der Pinyin-Schreibung statt auf Zeichen — gibt es bei Zhengjie und Chen, aber ohne Tonmarker. Am interessantesten für mich ist PFER, die Phone Feature Error Rate aus dem ZIPA-Paper: Sie gewichtet Fehler nach 24 artikulatorischen Merkmalen. Damit kostet eine knappe Verwechslung wie d gegen l weniger als ein völliger Fehlgriff wie d gegen c — genau die abgestufte Bewertung, die ein Aussprache-Trainer braucht. Auf multimodale LLMs für Mandarin wurde PFER aber noch nie angewandt."

---

## Folie M4 — Ton-Ebene: TER `[NEU]`

**Titel:** Tone Error Rate — empfohlen, aber von niemandem angewandt

**Bullets:**
- **TER** = Anteil der Silben mit **falschem Ton** (unabhängig vom Segment)
- **Empfohlen** als Best Practice von Zou et al. (2025, Review über 61 Studien)
- **In 0 von 42 Papern tatsächlich berechnet** — die Lücke zwischen „anerkannt wichtig" und „gemessen"
- Nahester Verwandter: *tonal CER* bei Chirkova (21) — aber für Baima, nicht Mandarin, ohne LLM

**Sichtbares Hinweis-Kästchen:**
> **Beitrag dieser Arbeit:** TER erstmals systematisch für multimodale LLMs auf Mandarin — als eigene Evaluationsachse neben dem Segment.

**Sprechernotiz:** „Ganz oben die Tonebene. Es gibt eine Metrik dafür — die Tone Error Rate, der Anteil der Silben mit falschem Ton. Zou et al. empfehlen sie in ihrem Review über 61 Studien ausdrücklich. Aber: In keinem einzigen der 42 Paper wird sie tatsächlich berechnet. Das ist die auffälligste Lücke meiner ganzen Analyse — eine Metrik, die alle für wichtig halten und niemand benutzt. Am nächsten kommt Chirkovas tonal CER, aber das ist für die Sprache Baima, nicht Mandarin, und ohne LLM. Die TER systematisch für multimodale LLMs einzuführen, ist ein konkreter methodischer Beitrag meiner Arbeit."

---

## Folie M5 — Aussprache-spezifisch: FRR/FAR/DA, F1 `[selten]`

**Titel:** Mispronunciation-Detection-Metriken — fürs Feedback-Szenario

**Bullets:**
- **FRR / FAR** (False Rejection / Acceptance Rate) + **DA** (Diagnostic Accuracy): Wu (16), Wang MDD (15)
- **DER** (Diagnostic Error Rate): Wang MDD (15)
- **F1 / Precision / Recall** (per Ton/Phonem): Zou (9), Wang MDD (15), Huang (17), PYG (13)
- Relevant, sobald es um **Lerner-Feedback** geht (RQ3/RQ5): nicht „wie viel % falsch", sondern „welcher Fehler, wie zuverlässig erkannt"

**Sichtbares Zitat:**
> Human-Korrelation der automatischen Bewertung nur 0,31–0,60 — *„applicability to L2 classrooms remains questionable"* — Wu & Shen (2024)

**Sprechernotiz:** „Sobald es nicht mehr um reine Transkription, sondern um Lerner-Feedback geht, kommen andere Metriken ins Spiel: False Rejection und False Acceptance Rate, Diagnostic Accuracy, und per-Ton-F1-Werte. Wu und Wang nutzen diese in der Mispronunciation Detection. Sie beantworten nicht ‚wie viel Prozent falsch', sondern ‚welcher Fehlertyp, und wie zuverlässig erkennt das System ihn'. Für meine RQ5 — die Fehlerstruktur — und das L2-Szenario sind das die passenden Größen. Wichtig ist Wus Befund, dass selbst dedizierte Systeme nur 0,31 bis 0,60 mit menschlichen Bewertern korrelieren — die Latte für ein klassentaugliches Feedback liegt hoch."

---

## Folie M6 — Unser Metrik-Stack (Vorschlag)

**Titel:** Der Metrik-Stack dieser Arbeit — entkoppelt nach Laut und Ton

**Tabelle (Folie):**

| Ebene | Metrik | Beantwortet | Status |
|---|---|---|---|
| Zeichen | CER | Vergleich mit Literatur (RQ4) | Standard |
| Segment | Pinyin-CER (tonlos) / **PFER** | „Konsonanten & Vokale korrekt?" (RQ2b) | selten / `[NEU]` im LLM-Kontext |
| **Ton** | **TER** + per-Ton-F1 | „Töne korrekt?" (RQ2c, RQ5) | `[NEU]` |
| Feedback | FRR/FAR/DA | Diagnosegüte (RQ3/RQ5) | aus MDD übernommen |

**Sichtbares Hinweis-Kästchen:**
> Kernprinzip: **Laut und Ton getrennt** auswerten + Near-Misses **fair gewichten** (PFER) — das macht für multimodale LLMs noch niemand.

**Sprechernotiz:** „Daraus baue ich meinen Metrik-Stack — entkoppelt nach Ebene. Hanzi-CER behalte ich nur für die Vergleichbarkeit mit der ASR-Literatur. Die eigentliche Bewertung läuft auf zwei getrennten Achsen: das Segment — Konsonanten und Vokale — über Pinyin-CER oder, fairer, PFER; und der Ton separat über die Tone Error Rate plus per-Ton-F1 für die Konfusionsanalyse. Für das L2-Feedback ergänze ich die Diagnose-Metriken aus der Mispronunciation Detection. Das Kernprinzip in einem Satz: Laut und Ton getrennt messen und Near-Misses phonetisch fair gewichten — diese Kombination wurde für multimodale LLMs auf Mandarin noch nie umgesetzt."

---

# TEIL III: Anmerkungen für Stefan

- **Welche Metriken sind „neu" — und in welchem Sinn?**
  - **TER**: existiert als Konzept (Zou empfiehlt), aber **Anwendung auf multimodale LLMs + Mandarin ist neu**. Sauberster Beitrag.
  - **PFER**: existiert (ZIPA), aber **nicht für LLMs / nicht mit Mandarin-Ton-Fokus** → neue Anwendung.
  - **WPER / artik. gew. Editierdistanz**: existieren extern (arXiv:2507.14346, 1905.02639), **nicht im Korpus** → optionaler Zusatzbeitrag, falls du Near-Miss-Gewichtung formal begründen willst.
  - **„Decoupled Segment+Tone Scoring"**: die *Kombination* (Laut-Achse + Ton-Achse getrennt, mit Gewichtung) kannst du als kleine **methodische Eigenleistung** framen.
- **Häufigkeits-Aussagen (für Q&A belegbar):** CER/WER = Standard (20+ Paper); PER/Pinyin-CER = selten (≈4); TER = 0 angewandt; PFER = 1 (ZIPA).
- **Folienzahl:** M1–M6 = 6 Folien. Falls Zeit knapp: M3+M4 zu einer „Phonem- & Ton-Ebene"-Folie zusammenziehen → 5 Folien.
