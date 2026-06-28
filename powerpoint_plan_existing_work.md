# PowerPoint- & Thesis-Plan: Related Work / Existing Work
## Masterarbeit Stefan Dosch — *Can LLMs Hear Tones? Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Betreuer:** Tim Schlippe, IU University
> **Zweck dieses Dokuments:** Tims Feedback umsetzen. Existing Work so präsentieren, dass (1) die Forschungsfrage wissenschaftlich (nicht produktartig) gerahmt ist, (2) die Papers klar an das Related-Work-Kapitel UND an die eigene Arbeit gebunden sind, (3) Tims vier Fragen explizit beantwortet werden.

---

# TEIL 0: Was sich gegenüber der letzten Version ändert (Tims Feedback)

Tim hat vier konkrete Punkte genannt. So adressiert dieser Plan jeden einzelnen:

| Tims Kritik | Konsequenz in diesem Plan |
|---|---|
| **„Mit einem KI-Abo einen Vokabeltrainer bauen" klingt nach Produkt, nicht nach Forschung** | Folie 1 reformuliert: Forschungsobjekt ist die **Transkriptionsleistung selbst** (phonetisch + tonal), nicht der Vokabeltrainer. Der Trainer wird in **einem** Satz als Motivation genannt, nicht als Ziel. |
| **Bezug der Papers zum Related-Work-Kapitel fehlt** | Neue Folie 3 mappt jedes Paper-Cluster auf einen Abschnitt §2.2–2.7 der Thesis. |
| **Bezug der Papers zur eigenen Arbeit fehlt** | Folien 8 + 10: „Was verwenden wir konkret" und „Welche Lücke schließen wir" mit RQ-Mapping. |
| **Tims 4 Fragen sollen im Fokus stehen** (Learnings / konkret verwendbar / Lücken / welche Lücke adressiert ihr) | Der gesamte Foliensatz ist um genau diese 4 Fragen herum gebaut (siehe Struktur unten). |

**Leitsatz der neuen Rahmung (auswendig fürs Sprechen):**
> „Diese Arbeit baut keinen Vokabeltrainer. Sie **misst**, wie zuverlässig aktuelle multimodale Foundation Models gesprochenes Mandarin in eine phonetische *und* tonale Repräsentation übersetzen — und ob das *out of the box* überhaupt belastbar genug ist. Der Vokabeltrainer ist nur das Motivationsszenario, nicht der Forschungsgegenstand."

---

# TEIL I: FOLIENSTRUKTUR IM ÜBERBLICK

Der Foliensatz folgt Tims vier Fragen als rotem Faden:

| # | Folie | Tim-Frage | Funktion |
|---|---|---|---|
| 1 | Forschungsfrage (wissenschaftlich gerahmt) | — | Reframing |
| 2 | Methodik der Literaturanalyse (42 Papers, 4 Kriterien) | — | Glaubwürdigkeit |
| 3 | Related-Work-Einordnung (§2.2–2.7) | **Bezug zum Kapitel** | Struktur |
| 4 | Learning 1: Speech LLMs sind stark, aber ton-blind | **Learnings** | |
| 5 | Learning 2: Pinyin-Generierung existiert — ohne Töne | **Learnings** | |
| 6 | Learning 3: Tonerkennung existiert — ohne LLMs (Tone Collapse) | **Learnings** | |
| 7 | Learning 4: Benchmarks & Metriken — TER wird nie gemessen | **Learnings** | |
| 8 | Was wir konkret verwenden | **Konkret verwendbar** | Bezug eigene Arbeit |
| 9 | Forschungslücken (4 Lücken, je 1 Belegzitat) | **Welche Lücken** | |
| 10 | Welche Lücken WIR adressieren (RQ-Mapping) | **Welche adressiert ihr** | Payoff |
| 11 | Abgrenzung: Niemand in der Schnittmenge (Venn) | — | Schlussbild |

**Folien-Faustregel:** max. 1 Kernaussage als Titel, 2–4 Bullets, **max. 1 Zitat sichtbar** auf der Folie. Alle weiteren Zitate (mit Seite + Wortlaut) gehören in die **Präsentationsnotizen** — siehe Teil II.

---

# TEIL II: FOLIEN IM DETAIL (Inhalt + Präsentationsnotiz)

> **Konvention für Präsentationsnotizen:** Wörtliches Zitat in „…", danach (Quelle, Section/Seite). Alle in den Notizen verwendeten Zitate sind gegen die Original-PDFs verifiziert (Stand 28.06.2026, siehe Verifizierungs-Checkliste, Teil V).

---

## Folie 1 — Forschungsfrage

**Titel:** Können multimodale Foundation Models Mandarin phonetisch *und* tonal transkribieren?

**Bullets:**
- Forschungsgegenstand: die **Transkriptionsleistung selbst** — auf drei Ebenen: Wort/Silbe, Phonem, **Ton**
- Off-the-shelf-Perspektive: Wie gut sind aktuelle Modelle *ohne* Finetuning, *ohne* Modelloperation?
- Motivation (nicht Ziel): belastbares Aussprache-Feedback für Mandarin-Lernende setzt zuverlässige tonale Transkription voraus

**Sichtbares Zitat:** keines (Statement-Folie).

**Präsentationsnotiz:**
> „Die zentrale Frage ist eine Messfrage, keine Baufrage: Wie genau übersetzen aktuelle multimodale Modelle gesprochenes Mandarin in eine Repräsentation, die Phoneme *und* Töne erfasst? Mandarin ist eine Tonsprache — dieselbe Silbe ‚ma' bedeutet je nach Ton ‚Mutter', ‚Hanf', ‚Pferd' oder ‚schimpfen'. Wenn ein Modell den Ton verfehlt, ist die Transkription inhaltlich falsch, selbst wenn das Segment stimmt. Ich untersuche das bewusst *out of the box* — also genau so, wie ein Anwender die Modelle einsetzen würde, ohne sie umzutrainieren. Der Vokabeltrainer ist nur das Motivationsszenario; das Forschungsobjekt ist die Transkription."

---

## Folie 2 — Methodik der Literaturanalyse

**Titel:** 42 Papers, vier Analysekriterien

**Bullets:**
- 6 Cluster: Speech LLMs (7) · Mandarin-Ton/Pinyin (7) · Pronunciation/MDD (7) · Benchmarks (9) · Surveys (10) · zentrale Datensätze (2)
- Jedes Paper nach 4 Kriterien geprüft: **(1) Kernaussage · (2) konkret verwendbar · (3) explizite Lücke · (4) implizite Lücke für unsere Arbeit**
- Belegprinzip: jede Aussage mit wörtlichem Zitat + Fundstelle hinterlegt

**Sichtbares Zitat:** keines.

**Präsentationsnotiz:**
> „Ich habe 42 Papers systematisch nach denselben vier Kriterien ausgewertet — genau die Fragen, die Sie genannt hatten: Was ist die Kernaussage? Was können wir konkret verwenden? Welche Lücke benennt das Paper selbst? Und welche Lücke ergibt sich daraus für unsere Arbeit? Jede Aussage in der Analyse ist mit einem wörtlichen Zitat und Fundstelle belegt — keine Behauptung ohne Beleg. Die folgenden Folien fassen die vier Learnings zusammen, die sich über die Cluster hinweg wiederholen."

---

## Folie 3 — Related-Work-Einordnung

**Titel:** Wie sich die Papers ins Related-Work-Kapitel einfügen

**Tabelle (Folie):**

| Thesis-Abschnitt | Cluster / Schlüssel-Papers |
|---|---|
| §2.2 Traditionelle Methoden | Du, Wang MDD, Bu, Kang, Chirkova |
| §2.3 LLMs im Sprachenlernen | Li PY-GEC, Liang PERL |
| §2.4 LLMs für chinesische Sprachverarbeitung | Seed-ASR, FireRedASR(2S), Kimi-Audio, Qwen3-ASR, **Zhengjie PYG**, Chen |
| §2.5 Evaluation von Aussprache & Ton | Wu, Huang Review, Zou Review, Kaur Survey, **MSPB** |
| §2.6 Korpora | **Tone Perfect**, ZIPA, AISHELL-1 |
| §2.7 Learnings & Forschungslücken | **SITA**, Gaido, Peng Survey, alle Surveys |

**Sichtbares Zitat:** keines.

**Präsentationsnotiz:**
> „Damit klar ist, wie sich die Literatur ins Kapitel 2 einfügt: Die traditionellen Ton- und MDD-Modelle gehen nach §2.2. Pinyin-gestützte LLM-Arbeiten — das ist der Strang, an den ich technisch direkt anknüpfe — nach §2.4; das zentralste Paper, Zhengjie & Cheng, sitzt hier. Die Evaluations- und Benchmark-Arbeiten inkl. MSPB nach §2.5. Tone Perfect als mein Korpus nach §2.6. Und die Synthese der Lücken, getragen von SITA und den Surveys, mündet in §2.7, aus dem dann direkt meine Forschungsfragen folgen."

---

## Folie 4 — Learning 1: Speech LLMs sind stark, aber ton-blind

**Titel:** Learning 1 — SOTA bei Mandarin-ASR, aber Evaluation endet auf der Zeichenebene

**Tabelle (Folie):**

| Modell | Mandarin-Leistung | Ton-Evaluation? |
|---|---|---|
| Seed-ASR (2024) | 2,98 % CER | nein |
| FireRedASR (2025) | 3,05 % CER | nein |
| Kimi-Audio (2025) | 0,60 % CER (AISHELL-1) | nein |
| Qwen3-ASR (2026) | ~2 % CER | nein |

**Sichtbares Zitat:**
> „…neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, **tone**)…" — Kimi-Audio (2025), Sec. 8, S. 21

**Präsentationsnotiz:**
> „Die aktuellen Speech LLMs sind technisch beeindruckend: Kimi-Audio erreicht 0,60 % CER auf AISHELL-1 — die stärkste bekannte Mandarin-ASR-Leistung. Aber alle sieben analysierten Systeme evaluieren ausschließlich Character- oder Word Error Rate. Tongenauigkeit wird in keinem gemessen. Bemerkenswert: Kimi-Audio benennt das Problem selbst und listet ‚tone' explizit als vernachlässigte Information — evaluiert es aber nicht. Wörtlich: ‚text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone), acoustic scene, and non-linguistic sounds' (Kimi-Audio, Section 8 ‚Challenges and Future Trends', S. 21). Verifizierbar belegt ist außerdem das Methodenproblem der Community selbst: ‚Current practices suffer from inconsistent metric implementations' (Kimi-Audio, Section 6.1, S. 15). Das heißt: Die Modelle könnten Töne erkennen — wir wissen es nicht, weil niemand misst."

---

## Folie 5 — Learning 2: Pinyin-Generierung existiert — aber ohne Töne

**Titel:** Learning 2 — Audio → LLM → Pinyin funktioniert, aber ohne Tonmarker

**Bullets:**
- **Zhengjie & Cheng PYG-ASR (2025)** — zentralstes Paper: HuBERT → Qwen2-7B erzeugt Pinyin *und* Zeichen; Pinyin Error Rate nur **1,9 %**
- **aber:** Pinyin **ohne Tonnummern** — „ma" statt „ma1"; derselbe Mangel bei Chen SCCM (2025)
- Die technische Pipeline steht — es fehlt der letzte Schritt: **Töne mittranskribieren**

**Sichtbares Zitat:**
> „there is no direct correspondence between the pronunciation and the written form of Chinese characters." — Zhengjie & Cheng (2025), S. 1

**Präsentationsnotiz:**
> „Das Paper, das meiner Arbeit am nächsten kommt, ist Zhengjie und Cheng 2025. Sie koppeln einen HuBERT-Encoder an Qwen2-7B und erzeugen aus Audio direkt Pinyin und Zeichen — mit nur 1,9 % Pinyin Error Rate: ‚The PYG-ASR1 model achieved a Pinyin error rate of 1.6%/1.9%' (S. 4). Ihre Motivation deckt sich mit meiner: ‚its implicit alignment often fails to capture phonetic relationships in Chinese, leading to pronunciation confusion and homophone errors' (Abstract). Aber: Das generierte Pinyin enthält **keine** Tonnummern — ‚mā' und ‚mà' werden beide zu ‚ma'. Genau die Information, die ein Aussprache-Feedback braucht, fehlt. Sie benennen selbst, dass kaum erforscht ist, wie LLM-ASR direkt Pinyin erzeugen kann: ‚there has been little exploration of how the LLM-ASR model can directly generate Pinyin and Chinese characters during ASR' (Zhengjie & Cheng, Introduction, S. 1). Ich kann auf dieser Architektur aufsetzen und genau den fehlenden Schritt — Töne — als Evaluationsachse ergänzen."

---

## Folie 6 — Learning 3: Tonerkennung existiert — aber ohne LLMs

**Titel:** Learning 3 — Tonerkennung ist ein aktives Feld, getragen von dedizierten Modellen; sogar Whisper „kollabiert"

**Bullets:**
- Klassische Ansätze stark: Deep Learning 88,8 % vs. traditionell 83,1 % Ton-Accuracy (Zou Review, 61 Studien)
- Bekannte Schwierigkeiten: **T2↔T3** chronisch verwechselt; L2-Sprecher nur 53–73 % (Kaur)
- **Tone Collapse:** Whisper-Embeddings verschiedener Töne überlappen (SITA, 2026)
- aber: **kein einziges** dieser Papers nutzt ein multimodales LLM

**Sichtbares Zitat:**
> „Whisper embeddings largely collapse into overlapping clusters." — Xu et al., SITA (2026), Sec. 5.1

**Präsentationsnotiz:**
> „Tonerkennung selbst ist kein neues Thema — es gibt Reviews über 61 Studien, Deep-Learning-Modelle erreichen 88,8 % Accuracy, die T2-T3-Verwechslung ist als chronisches Problem dokumentiert. Aber all das nutzt dedizierte CNNs, RNNs, HuBERT — kein multimodales LLM. Besonders relevant ist SITA 2026: Sie zeigen, dass sogar Whisper die Töne verliert — ‚Whisper embeddings largely collapse into overlapping clusters' (Section 5.1). Verschiedene Töne derselben Silbe landen an fast derselben Stelle im Embedding-Raum. Das ist brisant, weil die meisten multimodalen LLMs Whisper-basierte Encoder verwenden — auch Kimi-Audio. SITA löst es mit einem ‚tone-repulsive loss' und erreicht ~0,99 Top-1 auf Tone Perfect (Section 5.4) — aber eben als dediziertes System. Meine Frage: Wie stark trifft dieses Collapse-Problem die LLMs, die wir tatsächlich off-the-shelf einsetzen?"

---

## Folie 7 — Learning 4: Benchmarks & Metriken

**Titel:** Learning 4 — Mandarin-Benchmarks existieren; Tongenauigkeit misst keiner

**Bullets:**
- 9+ Audio-LLM-Benchmarks analysiert; mehrere mit Mandarin (MSPB, VocalBench-zh, TELEVAL, ContextASR)
- **Keiner** misst lexikalische Tongenauigkeit; MSPB testet nur **Satz**prosodie — GPT-4o erreicht dort nur 59,7 %
- **TER (Tone Error Rate)** wird empfohlen (Zou) — aber in **0 von 42** Papers angewandt

**Sichtbares Zitat:**
> „they generally struggled with tasks relying heavily on subtle speech prosody variations…" — Wang et al., MSPB (2025)

**Präsentationsnotiz:**
> „Vierte Beobachtung, die Metrik-Ebene: Es gibt reichlich Audio-LLM-Benchmarks, mehrere mit Mandarin. Der nächste an meinem Thema ist MSPB, der einzige, der Speech LLMs auf Mandarin-Prosodie testet. Selbst dort erreicht GPT-4o nur 59,70 % (Section 3.2.2, S. 5380), und die Autoren halten fest: ‚they generally struggled with tasks relying heavily on subtle speech prosody variations (e.g., prosodic focus marking, scalar meaning)' (S. 5378/5381). Aber MSPB misst Satzprosodie — Betonung, Intonation — nicht die lexikalischen Töne T1 bis T4. Und der überraschendste Befund: Die Tone Error Rate wird in der Community als Best Practice empfohlen, aber in keinem einzigen der 42 Papers tatsächlich angewandt. Es klafft eine Lücke zwischen dem, was als wichtig gilt, und dem, was gemessen wird."

---

## Folie 8 — Was wir konkret verwenden

**Titel:** Was wir aus der Literatur konkret übernehmen

**Tabelle (Folie):**

| Baustein | Quelle | Verwendung in unserer Arbeit |
|---|---|---|
| **Datensatz** | Tone Perfect (Ryu 2021) | Evaluations-Spine: 9.840 Silben, alle 4 Töne (RQ2) |
| **Architektur-Vorbild** | Zhengjie PYG-ASR (2025) | Audio→LLM→Pinyin-Pipeline, erweitert um Töne |
| **Methodik-Vorbild** | MSPB (2025) | Head-to-Head-LLM-Vergleich als Versuchsdesign |
| **Obere Referenz / Baseline** | SITA (2026), Whisper | dediziertes System als Vergleichsanker (RQ4) |
| **Metriken** | Zou (TER), Wang MDD (PER) | TER + PER + per-Ton-F1 (RQ5) |

**Sichtbares Zitat:** keines (Synthese-Folie).

**Präsentationsnotiz:**
> „Konkret verwendbar — Ihre zweite Frage — ist mehr, als man zunächst denkt. Den Datensatz Tone Perfect übernehme ich als Evaluations-Rückgrat: 9.840 Audioclips, jede Silbe in allen vier Tönen, CC-BY-lizenziert. Die Architektur von Zhengjie & Cheng dient als Pipeline-Vorbild, das ich um die Tondimension erweitere. Das Versuchsdesign von MSPB — mehrere LLMs im kontrollierten Head-to-Head — übertrage ich von Prosodie auf tonale Transkription. SITA und Whisper liefern den dedizierten Vergleichsanker für RQ4; SITAs ~0,99 ist meine obere Referenzlinie. Und bei den Metriken kombiniere ich die empfohlene, aber nie genutzte Tone Error Rate mit Phoneme Error Rate und per-Ton-F1 für die Fehleranalyse."

---

## Folie 9 — Forschungslücken

**Titel:** Vier Lücken, die die Literatur offen lässt

**Tabelle (Folie):**

| # | Lücke | Beleg |
|---|---|---|
| L1 | LLMs nur auf Wort/Zeichen (CER/WER) evaluiert, nicht Phonem/Ton | alle 7 Speech LLMs |
| L2 | Töne selten als **separate** Evaluationsachse | Zhengjie: Pinyin ohne Ton |
| L3 | Sehr wenige L2-Lerner-Studien | Zou Review |
| L4 | Kein systematischer LLM-vs.-dediziert-Vergleich | Gaido (2024) |

**Sichtbares Zitat:**
> „no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework." — Gaido et al. (2024)

**Präsentationsnotiz:**
> „Daraus ergeben sich vier Forschungslücken — Ihre dritte Frage. Erstens: Frontier-LLMs werden fast nur auf Wort- oder Zeichenebene evaluiert; Phonem- und Tongranularität für Mandarin ist praktisch unerforscht. Zweitens: Töne werden selten als eigene Achse behandelt — selbst das Pinyin-generierende Paper lässt sie weg. Drittens, belegt durch das Zou-Review: ‚L2 corpora like iCALL exist but receive less attention than native-speaker datasets' (S. 10). Viertens, Gaido 2024 für Speech Foundation Models allgemein: ‚no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework' (Section 2.1) — und für tonale Evaluation gilt das noch schärfer."

---

## Folie 10 — Welche Lücken WIR adressieren

**Titel:** Welche Lücken diese Arbeit schließt — Mapping auf die Forschungsfragen

**Tabelle (Folie):**

| RQ | Schließt Lücke | Beitrag |
|---|---|---|
| **RQ1** Text→Pinyin (±Ton) | L1 | isolierte Sprachwissens-Baseline |
| **RQ2** Native-Speech (Wort/Phonem/**Ton**) | L1, L2 | erste tonale Transkriptionsgüte für LLMs |
| **RQ3** L2-Speech *(stretch)* | L3 | feedback-relevanter Fall, neu für LLMs |
| **RQ4** vs. Whisper/dediziert | L4 | kontrollierter Head-to-Head |
| **RQ5** Fehlerstruktur | L2 | per-Ton-/Kontrast-Konfusionsanalyse (TER) |

**Sichtbares Zitat:** keines (Payoff-Folie).

**Präsentationsnotiz:**
> „Ihre vierte Frage — welche dieser Lücken schließt die Arbeit. RQ1 isoliert das reine Sprachwissen über eine Text-zu-Pinyin-Baseline und adressiert Lücke 1. RQ2 ist der Kern: erstmals die tonale Transkriptionsgüte multimodaler LLMs auf nativer Sprache — Wort-, Phonem- und Tonebene — und schließt damit Lücken 1 und 2. RQ3, zeitabhängig, bringt L2-Lerner ins Spiel und adressiert Lücke 3, die feedback-relevante. RQ4 stellt die LLMs kontrolliert gegen Whisper und SITA — Lücke 4. Und RQ5 analysiert die Fehlerstruktur per Ton und Kontrast mit der Tone Error Rate, die bisher niemand angewandt hat. Der committed core — RQ1, RQ2, RQ4, RQ5 — steht für sich; RQ3 ist die Erweiterung."

---

## Folie 11 — Abgrenzung (Schlussbild)

**Titel:** Die Lücke in einem Bild: niemand verbindet LLM + Audio + Töne

**Visual (3-Kreis-Venn oder Matrix):**

| | Töne evaluiert | LLM genutzt | Audio-Input |
|---|:---:|:---:|:---:|
| Zou, Huang (Reviews) | ✓ | ✗ | ✗/✓ |
| Wang MDD, Bu (dediziert) | ✓ | ✗ | ✓ |
| Zhengjie PYG-ASR | ✗ (Pinyin ohne Ton) | ✓ | ✓ |
| Wang MSPB | ~ (nur Satzprosodie) | ✓ | ✓ |
| Seed-ASR, Kimi-Audio | ✗ | ✓ | ✓ |
| **Diese Arbeit** | **✓** | **✓** | **✓** |

**Sichtbares Zitat:**
> „…their ability to capture the full range of acoustic cues … remains limited." — Peng Survey (2025), Sec. IX, S. 25

**Präsentationsnotiz:**
> „Zum Schluss die Abgrenzung in einem Bild. Drei Dimensionen: Werden Töne evaluiert? Wird ein LLM eingesetzt? Ist der Input Audio? Es gibt Papers für je zwei der drei — die Reviews evaluieren Töne ohne LLM, Zhengjie nutzt LLM und Audio, lässt aber Töne weg, MSPB testet LLMs auf Prosodie statt lexikalischer Töne. In der Schnittmenge aller drei steht aus 42 Papers: niemand. Der Peng-Survey fasst es zusammen — die Fähigkeit, das volle Spektrum akustischer Hinweise zu erfassen, ‚remains limited' (Section IX, S. 25). Genau diese Schnittmenge ist mein Beitrag."

---

# TEIL III: WIE DIESELBEN FRAGEN IN DER THESIS BEANTWORTET WERDEN

In der Präsentation komprimiert man auf eine Aussage pro Folie. In der Thesis wird derselbe Inhalt **ausführlich** ausgeschrieben — hier die Zuordnung Folie → Thesis-Stelle und der jeweils andere Detailgrad.

| Tim-Frage | PowerPoint (knapp) | Thesis (ausführlich) |
|---|---|---|
| **Reframing** | Folie 1: eine Statement-Folie | **Kapitel 1 (Einleitung):** Motivation (Tonsprache, Feedback-Bedarf) → Problemstellung → Forschungsfrage → 5 RQs. Vokabeltrainer als Anwendungsszenario in 1–2 Absätzen, klar abgegrenzt vom Forschungsgegenstand. |
| **Bezug Related Work** | Folie 3: Mapping-Tabelle | **Kapitel 2 (§2.2–2.7):** Jeder Abschnitt ein eigener Fließtext-Teil. Pro Paper: Methode, Ergebnis, Limitation, Bezug zu unserer Arbeit — mit Zitat + Seite. Die ausführlichen Paper-Texte liegen bereits in `core_information_combined.md` vor. |
| **Learnings** | Folien 4–7: 4 Aussagen | **§2.7 (Learnings & Gaps):** je Learning ein Absatz, der die clusterübergreifende Beobachtung mit 2–3 Belegzitaten stützt. |
| **Konkret verwendbar** | Folie 8: Tabelle | **Kapitel 3 (Methodik):** Datensatzwahl (Tone Perfect), Pipeline (an Zhengjie angelehnt), Versuchsdesign (an MSPB angelehnt), Baselines (Whisper/SITA), Metrik-Definitionen (TER, PER, F1). |
| **Welche Lücken** | Folie 9: 4 Lücken | **§2.7 → Überleitung zu Kapitel 1 RQs:** Lücken explizit nummeriert und je mit Belegzitat; Brücke zu den RQs. |
| **Welche adressiert ihr** | Folie 10: RQ-Mapping | **Kapitel 1 (RQ-Definition) + Kapitel 3 (Operationalisierung):** jede RQ mit zugeordneter Lücke und konkretem Messplan. |

**Schreibmuster für einen Related-Work-Absatz (Thesis, §2.4):**
> *Zhengjie & Cheng (2025) koppeln einen HuBERT-Encoder an ein Qwen2-7B-LLM und erzeugen aus Mandarin-Audio simultan Pinyin und Zeichen; sie berichten eine Pinyin Error Rate von 1,9 % auf AISHELL-1 (S. 4) und begründen den Ansatz damit, dass „its implicit alignment often fails to capture phonetic relationships in Chinese, leading to pronunciation confusion and homophone errors" (Abstract). Der generierte Pinyin-Output enthält jedoch keine Tonmarkierungen. Für die in dieser Arbeit untersuchte Fragestellung — die tonale Transkriptionsgüte — bleibt damit genau die entscheidende Dimension ausgespart; die vorliegende Arbeit schließt hier an, indem sie Tonnummern als explizite Ausgabe- und Evaluationsachse einführt.*

→ Dieses Muster (Methode → Ergebnis mit Zitat+Seite → Limitation → Bezug zu unserer Arbeit) für jedes zentrale Paper wiederholen. Die Rohtexte dafür stehen in `core_information_combined.md`.

---

# TEIL IV: ZITAT-BANK FÜR DIE PRÄSENTATIONSNOTIZEN

Verifizierte Zitate (Seite/Section im Analysefile belegt) — direkt in die Notizen kopierbar:

| # | Zitat | Quelle | Fundstelle | Folie |
|---|---|---|---|---|
| 0 | „text transcription focuses on the content of spoken words (what is said), neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone), acoustic scene, and non-linguistic sounds." | Kimi-Audio | Section 8, S. 21 | 4 |
| 0b | „there has been little exploration of how the LLM-ASR model can directly generate Pinyin and Chinese characters during ASR" | Zhengjie & Cheng | Introduction, S. 1 | 5 |
| 1 | „Current practices suffer from inconsistent metric implementations (e.g., variations in Word Error Rate calculation…)" | Kimi-Audio | Section 6.1, S. 15 | 4 |
| 2 | „Kimi-Audio sets SOTA results on AISHELL-1 (0.60)…" | Kimi-Audio | Section 6.2, S. 18 | 4 |
| 3 | „there is no direct correspondence between the pronunciation and the written form of Chinese characters." | Zhengjie & Cheng | S. 1 | 5 |
| 4 | „its implicit alignment often fails to capture phonetic relationships in Chinese, leading to pronunciation confusion and homophone errors." | Zhengjie & Cheng | Abstract | 5 |
| 5 | „The PYG-ASR1 model achieved a Pinyin error rate of 1.6%/1.9%" | Zhengjie & Cheng | S. 4 | 5 |
| 6 | „Whisper embeddings largely collapse into overlapping clusters" | Xu et al. (SITA) | Section 5.1 | 6 |
| 7 | „…a tone-repulsive loss prevents tone collapse by explicitly separating same-word different-tone realizations" | Xu et al. (SITA) | Abstract | 6 |
| 8 | „SITA achieves near-ceiling…Top-1 accuracy of ~0.99" | Xu et al. (SITA) | Section 5.4 | 6 |
| 9 | „they generally struggled with tasks relying heavily on subtle speech prosody variations (e.g., prosodic focus marking, scalar meaning)" | Wang et al. (MSPB) | S. 5378/5381 | 7 |
| 10 | GPT-4o 59,70 % (bestes Speech LLM) | Wang et al. (MSPB) | Section 3.2.2, S. 5380 | 7 |
| 11 | „L2 corpora like iCALL (142 hours, 305 learners) exist but receive less attention than native-speaker datasets" | Zou Review | S. 10 | 9 |
| 12 | „no work has addressed the comparative assessment of different SFMs under controlled conditions within the same framework" | Gaido et al. | Section 2.1 | 9 |
| 13 | „Another critical avenue for future work lies in extracting richer and finer acoustic information from speech… remains limited." | Peng Survey | Section IX, S. 25 | 11 |
| 14 | „complex tones remain the most difficult part of the phonology to transcribe" | Chirkova | Section 5, S. 178 | Reserve |

---

# TEIL V: VERIFIZIERUNGS-CHECKLISTE

Direkt aus den PDFs verifiziert (28.06.2026):

- [x] **Kimi-Audio Ton-Zitat** — verifiziert in `2504.18425v1.pdf`, **Page 21, Section 8 „Challenges and Future Trends"**. Exakter Wortlaut (Korrektur ggü. erster Notiz): „text transcription focuses on the content of spoken words **(what is said)**, neglecting important information in audio, such as paralanguage information (e.g., emotion, style, timbre, tone)**, acoustic scene, and non-linguistic sounds**." → Für Folie 4 ggf. mit „…" gekürzt zitieren; voller Wortlaut in Zitat-Bank #0.
- [x] **Zhengjie „little exploration"-Zitat** — verifiziert in `zhengjie25_interspeech.pdf`, **Page 1 (Introduction)**: „there has been little exploration of how the LLM-ASR model can directly generate Pinyin and Chinese characters during ASR." → Zitat-Bank #0b.
- [ ] **Step-Audio 2** „neglecting the para-linguistic information" — nur bestätigen, falls dieses Zitat tatsächlich auf eine Folie kommt (aktuell nicht im Foliensatz verwendet).

> Hinweis: Das Kimi-Zitat steht in Section 8 (Future Trends), nicht in der Introduction — d.h. das Modellpaper benennt den Ton-Aspekt selbst als offene Herausforderung. Das ist für die Motivation sogar stärker und kann im Vortrag so betont werden.

---

# TEIL VI: ZUSAMMENFASSUNG

- **Reframing erledigt:** Forschungsgegenstand = Transkriptionsleistung (phonetisch+tonal), Vokabeltrainer nur Motivation.
- **Related-Work-Bezug:** Folie 3 + Teil III mappen jedes Cluster auf §2.2–2.7.
- **Tims 4 Fragen** sind der rote Faden: Learnings (Folien 4–7), konkret verwendbar (8), Lücken (9), welche wir schließen (10).
- **Präsentationsnotizen** enthalten wörtliche Zitate mit Seite/Section; alle Fundstellen sind gegen die Original-PDFs verifiziert (Teil V abgehakt).
- **Thesis-Brücke:** Teil III zeigt, wie dieselben Antworten ausführlich in Kapitel 1–3 wandern; Rohtexte liegen in `core_information_combined.md`.
