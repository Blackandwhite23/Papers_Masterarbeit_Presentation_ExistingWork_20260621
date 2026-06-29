# PowerPoint — Experiment-Methodik
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Zweck:** Folien zur Beschreibung des experimentellen Vorgehens — Forschungsfragen, Experimente, Modelle, Korpora, Kosten, Phasen, Pflicht/Kür, eigener Korpus, Metriken, Zeitplan.
>
> **Quelle:** `methodik_experiment.md` (Stand 2026-06-29)

---

## Folie M1 — Forschungsfragen & methodischer Kern

**Titel:** 5 Forschungsfragen — 3 Bewertungsachsen statt nur CER

```
RQ   FRAGE                                          EINGABE → AUSGABE         STATUS
────────────────────────────────────────────────────────────────────────────────────────
RQ1  Wie gut wandeln Modelle Text/Hanzi → Pinyin+Ton?  Text → Pinyin           Pflicht
RQ2  Wie gut transkribieren LLMs native Sprache?     ★ Audio (native) → Pinyin  Pflicht-Kern
RQ3  Wie gut transkribieren LLMs L2-Sprache?         ★ Audio (L2) → Pinyin      Pflicht-Kern
RQ4  LLM vs. ASR vs. Spezialmodell?                    Vergleich               Pflicht
RQ5  Welche Fehlerstruktur? (Töne, Phoneme)             Analyse                Pflicht
```

```
METHODISCHER KERN (= Beitrag der Arbeit)
╔═══════════════════════════════════════════════════════════════╗
║  3 entkoppelte Bewertungsachsen statt nur CER:               ║
║                                                               ║
║  TER   Tone Error Rate       → Tonebene (Zou 2025)          ║
║  PFER  Phone Feature Error   → artikulatorisch gewichtet     ║
║        Rate                    (PanPhon, 21 Features)        ║
║  CER   Character Error Rate  → nur für Literaturvergleich   ║
╚═══════════════════════════════════════════════════════════════╝
```

**Sprechernotiz:** „Wir haben fünf Forschungsfragen. Der Kern sind RQ2 und RQ3 — können multimodale LLMs Mandarin-Töne hören, und wie gut funktioniert das bei L2-Lernern? Der methodische Beitrag liegt in den drei Bewertungsachsen: TER misst die Tonebene separat — das hat noch keine vergleichbare Studie gemacht. PFER gewichtet Segmentfehler artikulatorisch über PanPhon mit 21 Features, statt binäres richtig/falsch. CER rechnen wir nur für den Literaturanschluss, weil alle bisherigen Papers nur CER berichten."

---

## Folie M2 — 7 Experiment-Blöcke: Pflicht & Kür

**Titel:** Die Experimente — 5 Pflicht + 2 Kür

```
PFLICHT (E1–E5)
╔══════════════════════════════════════════════════════════════════════════╗
║  E1  Text/Hanzi → Pinyin+Ton       alle 14 Modelle                    ║
║      Baseline: g2pW (99,08 %)      Daten: Tone-Perfect-Labels         ║
║                                                                        ║
║  E2  Audio NATIVE → Pinyin+Ton  ★  11 audio-fähige Modelle            ║
║      Daten: Tone Perfect (9.840)    CNN-Obergrenze (~99 %)             ║
║                                                                        ║
║  E3  Audio L2 → Pinyin+Ton  ★      11 audio-fähige Modelle            ║
║      Daten: LATIC + OMPAL (~4.350)  perzeptive Ground Truth            ║
║                                                                        ║
║  E4  Vergleich LLM/ASR/CNN         3-Wege auf denselben Daten         ║
║      + CER auf AISHELL-1            (Literaturanschluss)               ║
║                                                                        ║
║  E5  Fehlerstruktur                 Konfusionsmatrizen, TER/Ton,       ║
║      T2↔T3, Retroflexe              PFER nach Initial/Final            ║
╚══════════════════════════════════════════════════════════════════════════╝

KÜR (E6–E7)
┌──────────────────────────────────────────────────────────────────────────┐
│  E6  Format-Pilot: 6 Formate × 400 Samples × 3 Modelle = 7.200 Calls  │
│      Hypothese: Pinyin+Ton# > IPA bei LLMs (Chirkova-Umkehr)   < 10 $ │
│                                                                        │
│  E7  Eigener L2-Korpus: 8–12 deutsche Lerner, ~800–2.000 Aufnahmen     │
│      Höchster Neuheitswert — kein dt. L2-Mandarin-Korpus existiert     │
└──────────────────────────────────────────────────────────────────────────┘
```

**Sprechernotiz:** „Fünf Pflicht-Experimente. E1 testet, ob Modelle überhaupt das Pinyin-System beherrschen — rein linguistisch, ohne Audio. E2 ist der Kern: Muttersprachler-Silben aus Tone Perfect, isoliert, kontrolliert. E3 verschiebt den Fokus auf L2-Lerner — hier ist die Forschungslücke am größten, kein LLM wurde je darauf getestet. E4 vergleicht drei Ansätze auf denselben Daten. E5 ist die linguistische Analyse: welche Töne, welche Phoneme werden verwechselt? Als Kür haben wir den Format-Pilot — welches Transkriptionsformat funktioniert am besten — und einen eigenen L2-Korpus mit deutschen Lernern, den es so noch nicht gibt."

---

## Folie M3 — 14 Modelle in 3 Gruppen

**Titel:** Modellauswahl — 3 Märkte × 3 Systemtypen

```
GRUPPE 1 — Multimodal (Audio → Pinyin direkt) — der Kern          7 Modelle
┌──────────────────┬───────────┬────────┬──────────────────────────────────┐
│ GPT-5.5          │ OpenAI    │ 🇺🇸 USA│ teuerste → Sub-Sampling (700)    │
│ Gemini 3.1 Pro   │ Google    │ 🇺🇸 USA│                                  │
│ Grok 4.3         │ xAI       │ 🇺🇸 USA│                                  │
│ Qwen3.5-Omni     │ Alibaba   │ 🇨🇳 CN │ Alibaba Cloud API                │
│ Kimi-Audio       │ Moonshot  │ 🇨🇳 CN │ Replicate                        │
│ GLM-4-Voice      │ Zhipu     │ 🇨🇳 CN │ DeepInfra                        │
│ Step-Audio 2.5   │ StepFun   │ 🇨🇳 CN │ günstigste ($0,022/Std)          │
└──────────────────┴───────────┴────────┴──────────────────────────────────┘

GRUPPE 2 — Reine ASR (→ Hanzi, G2P nötig) = Baseline               4 Modelle
┌──────────────────┬───────────┬────────┬──────────────────────────────────┐
│ Whisper large-v3 │ OpenAI    │ 🇺🇸 USA│ Standard-Baseline                │
│ Scribe v2        │ ElevenLabs│ 🇺🇸 USA│ ASR-Vergleich                    │
│ Voxtral 2        │ Mistral   │ 🇪🇺 EU │ EU-Vertreter                     │
│ FireRedASR2S     │ Xiaohongshu│🇨🇳 CN │ nur lokal (optional)             │
└──────────────────┴───────────┴────────┴──────────────────────────────────┘

GRUPPE 3 — Text-only (nur RQ1)                                     3 Modelle
┌──────────────────┬───────────┬────────┬──────────────────────────────────┐
│ Claude Opus 4.8  │ Anthropic │ 🇺🇸 USA│ kein Audio-Input                 │
│ Claude Sonnet 4.6│ Anthropic │ 🇺🇸 USA│ kein Audio-Input                 │
│ DeepSeek V4      │ DeepSeek  │ 🇨🇳 CN │ kein Audio-Input                 │
└──────────────────┴───────────┴────────┴──────────────────────────────────┘
```

**Sprechernotiz:** „14 Modelle, abgedeckt über drei Märkte — USA, China, Europa — und drei Systemtypen. Gruppe 1 ist der Kern: sieben multimodale LLMs, die Audio direkt in Pinyin+Ton umwandeln sollen. Die spannende Frage ist, ob sie Töne hören können. Gruppe 2 sind reine ASR-Systeme — die geben Hanzi aus, dann braucht man g2pW für Pinyin. Das verdeckt aber L2-Tonfehler bei Homophonen, deshalb nur Baseline. Gruppe 3 testet nur RQ1: ob Text-only-Modelle Hanzi in Pinyin umwandeln können. Wichtig: Nur GPT-5.5 bekommt ein Sub-Sample wegen der hohen Kosten — alle anderen laufen auf dem vollen Korpus."

---

## Folie M4 — Korpora: Pflicht & Kür

**Titel:** Welche Daten? — Pflicht-Korpora + Kür-Korpus

```
PFLICHT-KORPORA (bestehend, sofort verfügbar)
╔══════════════════════════════════════════════════════════════════════════╗
║  Tone Perfect 🔑   RQ2   410 Silben × 4 Töne × 6 Sprecher = 9.840    ║
║                          CC-BY 4.0, Open Access                        ║
║                          → VOLL für günstige Modelle                   ║
║                          → 700er-Sub-Sample (stratifiziert) für GPT-5.5║
║                                                                        ║
║  LATIC 🔑          RQ3   4 L2-Sprecher, 2.579 Wörter                  ║
║                          IEEE DataPort, öffentlich                     ║
║                                                                        ║
║  OMPAL 🔑          RQ3   46 L2-Sprecher (Frz. L1), 1.768 Sätze       ║
║                          Open-Source (Interspeech 2025)                 ║
║                                                                        ║
║  AISHELL-1         RQ4   170h, 400 native Sprecher                    ║
║                          → nur CER-Teilmenge (Literaturanschluss)     ║
╚══════════════════════════════════════════════════════════════════════════╝

KÜR-KORPUS (eigene Erhebung)
┌──────────────────────────────────────────────────────────────────────────┐
│  8–12 deutsche L2-Lerner, A2–B2, ~80–100 Items, ~800–2.000 Aufnahmen  │
│  → kein dt. L2-Mandarin-Korpus existiert öffentlich = echter Beitrag   │
└──────────────────────────────────────────────────────────────────────────┘

WARUM NICHT die großen Korpora (AISHELL 170h, KeSpeech 1.542h)?
→ Kein Ton-Ground-Truth! Man kann CER rechnen, aber keine TER.
→ Für Tonevaluation zählt Label-Qualität, nicht Stundenzahl.
```

**Sprechernotiz:** „Vier Pflicht-Korpora. Tone Perfect ist das Herzstück: 9.840 isolierte Silben, native Sprecher, exakte Tonlabels — klein aber perfekt kontrolliert. Für L2 haben wir LATIC und OMPAL — zusammen rund 4.350 Äußerungen von 50 Lernern. AISHELL-1 nutzen wir nur für CER, um an die Literatur anzuschließen. Warum nicht die großen Korpora wie KeSpeech mit 1.500 Stunden? Weil die keinen Ton-Ground-Truth haben — man kann CER rechnen, aber unsere Kernmetrik TER braucht ein Tonlabel pro Silbe. Und als Kür: ein eigener Korpus mit deutschen Lernern, den es so noch nicht gibt."

---

## Folie M5 — Kosten

**Titel:** API-Kosten — Pflicht-Kern für 15–30 €

```
╔══════════════════════════════════════════════════════════════════╗
║  SZENARIO                                    KOSTEN             ║
║  ─────────────────────────────────────────────────────────────  ║
║  Pflicht-Kern                                ~15–30 €    ← Ziel║
║  (günstige voll + GPT-5.5 sub-sample + text)                   ║
║                                                                 ║
║  Alle Modelle, alle 9.840 Samples             ~25–44 $         ║
║  (ohne Sub-Sampling)                                            ║
║                                                                 ║
║  Minimal (700 überall)                         ~4–7 $           ║
║                                                                 ║
║  Format-Pilot (Kür)                            < 10 $           ║
╚══════════════════════════════════════════════════════════════════╝

  Teuerste:   GPT-5.5      $5/$30 pro 1M Token + Audio → Sub-Sampling
  Günstigste: Step-Audio   $0,022/Std
              Voxtral      ~$0,001/Aufruf
              Whisper       $0,006/Min

  Kein Cloud-GPU nötig  (11/12 Audio-Modelle per API)
  Annotationskosten      nur für Kür-Korpus, separat (~150–500 €)
```

**Sprechernotiz:** „Das Budget ist sehr überschaubar. Der Pflicht-Kern kostet realistisch 15 bis 30 Euro — weil wir eine klare Strategie haben: günstige Modelle laufen auf dem vollen Korpus, nur GPT-5.5 bekommt ein Sub-Sample wegen der hohen Token-Kosten. Selbst wenn wir alles ohne Sub-Sampling rechnen, bleiben wir unter 50 Dollar. Die billigsten Modelle kosten Bruchteile eines Cents pro Aufruf. Wichtig: Kein GPU-Server nötig — 11 von 12 Audio-Modellen laufen über APIs. Annotationskosten fallen nur beim eigenen Kür-Korpus an."

---

## Folie M6 — 8 Phasen: günstig anfangen, teuer nur bei Bedarf

**Titel:** Phasenstrategie — günstig → teuer → Kür

```
Phase   Inhalt                          Modelle              Kosten    Status
──────────────────────────────────────────────────────────────────────────────
P0      Setup & API-Checks              —                      ~0      Pflicht
        Replicate/StepFun/DeepInfra
        aus DE testen; Prompt-Templates

P1      Pipeline-Pilot                  billigste 2–3        < 1 $    Pflicht
        400+200 Samples; Eval testen

P2      Günstige APIs, voller Korpus    Step-Audio, Whisper   7–12 $  Pflicht
        E2 + E3                         Voxtral, Scribe,
                                        GLM, Qwen-Omni

P3      Mittlere APIs                   Gemini, Grok,        +5–15 $  Pflicht
        E2 + E3                         Kimi-Audio

P4      Premium (Sub-Sample)            GPT-5.5              +1–2 $   Pflicht
        E2 + E3 auf 700er-Sample

P5      Text-only (RQ1)                 Claude, DeepSeek     +0,9 $   Pflicht
        E1

P6      Auswertung                      —                      0      Pflicht
        TER/PFER/CER, Konfusionsmatrizen

P7      Kür                             —                   +10 $ +   Kür
        Format-Pilot (E6) und/oder                          Annotation
        eigener L2-Korpus (E7)
──────────────────────────────────────────────────────────────────────────────
                                              Pflicht gesamt: ~15–30 €
```

**Sprechernotiz:** „Die Phasenstrategie ist: günstig anfangen, teuer nur bei Bedarf. P0 ist Setup — API-Zugänge testen, vor allem die chinesischen Anbieter von Deutschland aus. P1 ist ein Mini-Pilot: 600 Samples durch die ganze Pipeline, um Bugs im Eval-Code zu finden, bevor wir Geld ausgeben. Ab P2 gehen die billigen Modelle auf den vollen Korpus — das sind nur 7 bis 12 Dollar. P3 sind die mittleren — Gemini, Grok, Kimi. GPT-5.5 kommt erst in P4 und nur auf dem Sub-Sample. Text-only in P5 kostet unter einem Dollar. P6 ist reine Auswertung. Und P7 ist Kür — erst wenn der Pflicht-Kern sauber steht."

---

## Folie M7 — Pflicht vs. Kür (Entscheidungsregel)

**Titel:** Was MUSS rein, was KANN rein?

```
╔══════════════════════════════════════════════════════════════════╗
║                        PFLICHT                                  ║
║  E1  Text→Pinyin          alle 14 Modelle, g2pW-Baseline       ║
║  E2  Audio native          11 Modelle, Tone Perfect (voll)     ║
║  E3  Audio L2              11 Modelle, LATIC + OMPAL           ║
║  E4  Vergleich             LLM / ASR / CNN + CER (AISHELL)     ║
║  E5  Fehlerstruktur        TER, PFER, Konfusionsmatrizen       ║
║  Phasen P0–P6              ~15–30 €                             ║
╚══════════════════════════════════════════════════════════════════╝
                              ↓
                Erst wenn Pflicht-Kern steht:
                              ↓
┌──────────────────────────────────────────────────────────────────┐
│                          KÜR                                    │
│  ① Eigener L2-Korpus (E7)  → höchster Neuheitswert             │
│  ② Format-Pilot (E6)      → methodische Verstärkung, < 10 $   │
│  ③ iCALL/SingaKids        → mehr L2-Daten                      │
│  ④ FireRedASR2S lokal      → vollständige ASR-Baseline          │
│  ⑤ GPT-5.5 voller Lauf     → statt Sub-Sample                  │
└──────────────────────────────────────────────────────────────────┘

Entscheidungsregel: Erst wenn der gesamte Pflicht-Kern
(inkl. sauberer Auswertung) steht, wird Kür angefasst.
```

**Sprechernotiz:** „Klare Trennung: Pflicht ist das, was in zwei bis drei Monaten fertig sein muss — fünf Experimente, alle Phasen bis P6, Kosten unter 30 Euro. Kür kommt nur, wenn der Kern steht. Die Prioritätsreihenfolge: Erstens der eigene L2-Korpus — der hat den höchsten Neuheitswert, weil kein deutscher L2-Mandarin-Korpus existiert. Zweitens der Format-Pilot für unter 10 Dollar. Dann weitere Kontakte wie iCALL für mehr L2-Daten. FireRedASR2S lokal und der volle GPT-5.5-Lauf ganz am Ende."

---

## Folie M8 — Eigener L2-Korpus (Kür): Design

**Titel:** Kür — Eigener L2-Korpus: 8–12 deutsche Lerner

```
WARUM?
  • Öffentliche L2-Korpora: LATIC = 4 Sprecher, OMPAL = nur Frz. L1
  • Kein dt. L2-Mandarin-Korpus existiert → eigene L1-Gruppe = Beitrag
  • Passt zum geplanten Vokabeltrainer (deutschsprachige Zielgruppe)

DESIGN
┌────────────────┬──────────────────────────────────────────────┐
│ Sprecher       │ 8–12 deutsche L2-Lerner                     │
│ Niveau         │ A2–B2 (gestaffelt)                          │
│ Items          │ ~80–100 Zielsilben (Tonpaare, Retroflexe)   │
│ Wiederholungen │ 1–2× pro Item                               │
│ Gesamt         │ ~800–2.000 Aufnahmen                        │
└────────────────┴──────────────────────────────────────────────┘

ERHEBUNG
  1. Rekrutierung: IU-Netzwerk, Mandarin-Sprachkurse
  2. Stimulus: Hanzi + Referenz-Pinyin (Vergleichbarkeit mit Tone Perfect)
  3. Aufnahme: ruhiger Raum, Headset, WAV 16 kHz mono, Einzelaufnahme
  4. Ethik/DSGVO: Einwilligung, Pseudonymisierung, Löschkonzept

PROCESSING
  1. Normalisierung (Lautstärke, Sampling-Rate, Stille trimmen)
  2. Segmentierung (1 Datei/Item; MFA mit L2-Nachkontrolle)
  3. Benennung: sprecherID_item_wiederholung.wav + Metadaten-CSV
```

**Sprechernotiz:** „Falls wir zur Kür kommen: 8 bis 12 deutsche Mandarin-Lerner, Niveau A2 bis B2, ungefähr 80 bis 100 Zielsilben — fokussiert auf die schwierigen: Tonminimalpaare, Retroflexe, Diphthonge. Ergibt 800 bis 2.000 Aufnahmen, vergleichbar mit LATIC. Rekrutierung über das IU-Netzwerk. Aufnahmen standardisiert als WAV, 16 kHz mono. Stimulus-Design so, dass Vergleichbarkeit mit Tone Perfect besteht. Ethik und DSGVO vorher mit Tim klären."

---

## Folie M9 — Annotation des eigenen Korpus

**Titel:** Kür — Annotationsworkflow: perzeptiv, halbautomatisch

```
KERNPRINZIP: Annotiert wird was GEHÖRT wurde, nicht die Soll-Aussprache.

SCHEMA (pro Aufnahme)
  Initial (gehörter Konsonant) + Final (gehörter Reim) + Ton (1–5)

WORKFLOW
┌────────────────────────────────────────────────────────────────────────┐
│  1  CNN-Vorlabel     Tonklassifikator (~99 % native, Vorschlag für L2)│
│     + g2pW           kanonische Referenz                              │
│     + SpeechSuper    2. Maschinen-Meinung (optional)                  │
│                              ↓                                        │
│  2  2 Muttersprachler annotieren unabhängig was sie HÖREN             │
│                              ↓                                        │
│  3  Cohen's κ        Ziel: κ > 0,85                                   │
│                              ↓                                        │
│  4  Konflikt?  → 3. Annotator ODER Praat-F0 als Schiedsrichter       │
│                              ↓                                        │
│  5  Stichproben-Audit: ≥ 15–20 % doppelt prüfen                      │
└────────────────────────────────────────────────────────────────────────┘

AUFWAND
  ~1.000–2.000 Items × 2 Annotatoren = 2.000–4.000 Labels
  ~20–40 Annotator-Stunden → ~150–500 € (separates Budget)
  Dauer: ~1 Woche bei paralleler Arbeit
```

**Sprechernotiz:** „Der Annotationsworkflow ist halbautomatisch. Zuerst läuft ein CNN-Tonklassifikator als Vorlabel — der ist für native Sprache bei 99 Prozent, gibt aber für L2 nur einen Vorschlag. Dann annotieren zwei Muttersprachler unabhängig, was sie tatsächlich hören — perzeptiv, nicht kanonisch. Das ist der Schlüssel: Whisper plus g2pW kann nur die Soll-Aussprache liefern, nicht die Realisierung. Wir messen Inter-Rater-Agreement mit Cohen's Kappa, Ziel über 0,85. Bei Konflikten entscheidet ein dritter Annotator oder die Praat-F0-Kontur als objektiver Schiedsrichter. Aufwand: 20 bis 40 Stunden, 150 bis 500 Euro."

---

## Folie M10 — Metriken (Querschnitt)

**Titel:** 6 Metriken — Ton, Phonem, Zeichen

```
METRIK              MISST                  BERECHNUNG                   RQ
─────────────────────────────────────────────────────────────────────────────
Syllable Accuracy   Silbe (Init.+Final)    Exact Match                RQ2/3
                    korrekt

Tone Accuracy       Ton korrekt            1 − Tonfehlerrate      ★ RQ2/3/5
/ TER               (1–4 + neutral 5)                              Kernbeitrag

Combined Accuracy   Silbe UND Ton korrekt  beide korrekt            RQ2/3

PFER                artikulatorisch        pinyin-to-ipa          ★ RQ5
                    gewichtete Distanz     → PanPhon (21 Features) Kernbeitrag

CER                 Zeichenfehlerrate      Levenshtein auf Hanzi    RQ4
                                                                  Lit.-anschluss

Tone Confusion      welcher Ton            Häufigkeitsmatrix        RQ5
Matrix              → welcher Ton
─────────────────────────────────────────────────────────────────────────────

TOOLING
  g2pW             Baseline + kanonische Referenz
  pinyin-to-ipa    Konvertierung für PFER
  PanPhon          21 artikulatorische Features
  CNN-Klassifik.   native Obergrenze
  Praat/Parselm.   F0-Schiedsrichter bei L2
```

**Sprechernotiz:** „Sechs Metriken. Syllable Accuracy und Tone Accuracy werden getrennt gemessen — das ist wichtig, weil ein Modell die Silbe richtig haben kann, aber den Ton falsch. Combined Accuracy fordert beides. TER, die Tone Error Rate, ist unser Kernbeitrag — das hat in der bestehenden Literatur noch niemand systematisch auf LLMs angewandt. PFER nutzt PanPhon mit 21 artikulatorischen Features, um Segmentfehler graduell zu bewerten — ein falsches Retroflexes ‚zh' statt ‚z' wiegt weniger als ein komplett falscher Konsonant. CER rechnen wir nur, damit unsere Ergebnisse mit den 15+ Papers vergleichbar sind, die AISHELL als Benchmark nutzen. Und die Tone Confusion Matrix zeigt, welche Töne verwechselt werden — wir erwarten T2 versus T3 als häufigstes Muster."

---

## Folie M11 — Zeitplan (12 Wochen)

**Titel:** Zeitplan — 8 Wochen Pflicht + 4 Wochen Puffer/Kür

```
WOCHE   PHASE    INHALT
──────────────────────────────────────────────────────────────────
  1      P0      Korpora beschaffen (Tone Perfect, LATIC, OMPAL)
                 API-Zugänge testen · Prompt-Templates
                 Eval-Skripte (TER/PFER/CER)

  2      P1      Pipeline-Pilot (400+200 Samples)
                 Bugs fixen · Output-Parsing robust

  3–4    P2+P3   Günstige & mittlere APIs, voller Korpus
                 E2 + E3

  5      P4+P5   GPT-5.5 Sub-Sample + Text-only (RQ1)

  6–7    P6      Auswertung — TER/PFER/CER
                 Konfusionsmatrizen · Vergleichstabellen
                 E4 + E5

  8              Ergebnisse verschriftlichen · Plots
                 Limitationen

  9–12   P7      PUFFER + KÜR
                 ① eigener L2-Korpus
                 ② Format-Pilot
                 ③ iCALL-Kontakt
──────────────────────────────────────────────────────────────────
         █████████████████████████████  Pflicht (Wo 1–8)
                                      ░░░░░░░░░░░░  Kür (Wo 9–12)
```

**Sprechernotiz:** „Zwölf Wochen, davon acht für den Pflicht-Kern. Woche 1 ist Setup: Korpora runterladen, APIs testen, Eval-Skripte schreiben. Woche 2 der Pipeline-Pilot — bevor wir Geld ausgeben, muss der ganze Ablauf einmal durchlaufen. Woche 3 und 4 laufen die günstigen und mittleren Modelle auf dem vollen Korpus. Woche 5 kommt GPT-5.5 auf dem Sub-Sample plus die Text-only-Modelle. Woche 6 und 7 ist Auswertung: Konfusionsmatrizen, Vergleichstabellen. Woche 8 verschriftlichen. Und Wochen 9 bis 12 sind Puffer plus Kür — in der Prioritätsreihenfolge: eigener Korpus, dann Format-Pilot, dann iCALL-Kontakt."

---

## Folie M12 — Zusammenfassung für Tim

**Titel:** Methodik auf einen Blick

```
╔══════════════════════════════════════════════════════════════════════════╗
║                                                                        ║
║  EXPERIMENTE   E1 Text→Pinyin · E2 Audio-native · E3 Audio-L2         ║
║                E4 Vergleich (LLM/ASR/CNN) · E5 Fehlerstruktur          ║
║                [Kür] E6 Format-Pilot · E7 eigener L2-Korpus            ║
║                                                                        ║
║  MODELLE       14 = 7 multimodal + 4 ASR-Baseline + 3 text-only       ║
║                3 Märkte: USA / China / EU                              ║
║                                                                        ║
║  KORPORA       Pflicht: Tone Perfect (9.840) + LATIC+OMPAL (~4.350)   ║
║                Vergleich: AISHELL-1 (CER)                              ║
║                Kür: eigener dt. L2-Korpus (~8–12 Sprecher)             ║
║                                                                        ║
║  METRIKEN      TER + PFER (Kernbeitrag) + CER (Literaturanschluss)    ║
║                                                                        ║
║  KOSTEN        Pflicht 15–30 € · alles < 50 €                         ║
║                Annotation nur für Kür (~150–500 €)                     ║
║                                                                        ║
║  ZEIT          ~8 Wochen Pflicht + 4 Wochen Puffer/Kür                 ║
║                                                                        ║
╚══════════════════════════════════════════════════════════════════════════╝
```

**Sprechernotiz:** „Zusammenfassung: 7 Experimente, davon 5 Pflicht. 14 Modelle aus 3 Märkten. Kontrollierte Korpora mit echtem Ton-Ground-Truth statt großen Korpora ohne Tonlabels. Drei Metriken statt nur CER — das ist der methodische Beitrag. Budget unter 30 Euro für den Pflicht-Kern. Und ein klarer Zeitplan mit 8 Wochen Pflicht und 4 Wochen Puffer. Die Entscheidungsregel ist einfach: Erst wenn der Pflicht-Kern sauber steht, wird Kür angefasst."
