# Methodik des Experiments
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*
### Betreuer: Tim Schlippe · IU International University · Stand 2026-06-29

> **Zweck dieses Dokuments:** Vollständige Beschreibung des empirischen Teils — welche Experimente, welche Modelle, welche Korpora, welche Kosten, welche Phasen, was Pflicht/Kür ist, und ob/wie ein eigener Korpus gesammelt wird.
>
> **Rahmenbedingungen (festgelegt):**
> - **Zeit:** ~2–3 Monate für den empirischen Teil → Fokus auf einen schlanken, robusten **Pflicht-Kern**
> - **Korpus-Strategie:** **Hybrid** — bestehende Korpora = Pflicht; kleiner eigener L2-Korpus = Kür
> - **Budget:** API-Kosten < 50 € (Pflicht-Kern realistisch 15–30 €) + separates Annotationsbudget
> - **Ressourcen:** Zugang zu L2-Mandarin-Lernern, Muttersprachler-Annotatoren, Annotationsbudget vorhanden (Annotation nicht durch Stefan selbst)
> - **Format-Pilot:** Kür

---

# 0. Forschungsfragen (Bezugsrahmen)

| RQ | Frage | Eingabe → Ausgabe | Status |
|---|---|---|---|
| **RQ1** | Wie gut wandeln Modelle **Text/Hanzi → Pinyin+Ton** um? | Text → Pinyin | Pflicht |
| **RQ2** | Wie gut transkribieren multimodale LLMs **native Sprache → Pinyin+Ton**? | Audio (native) → Pinyin | **Pflicht (Kern)** |
| **RQ3** | Wie gut transkribieren sie **L2-Sprache → Pinyin+Ton**? | Audio (L2) → Pinyin | **Pflicht (Kern)** |
| **RQ4** | Wie schneiden LLMs im **Vergleich** zu reiner ASR und Spezialmodellen ab? | Vergleich | Pflicht |
| **RQ5** | Welche **Fehlerstruktur** zeigen die Modelle (welche Töne/Phoneme werden verwechselt)? | Analyse | Pflicht |

**Methodischer Kern (der eigentliche Beitrag):** Drei entkoppelte Bewertungsachsen statt nur CER —
**TER** (Tone Error Rate, Tonebene), **PFER** (Phone Feature Error Rate, artikulatorisch gewichtet via PanPhon), **CER** (Zeichenebene, nur für Literaturvergleich).

---

# (a) Welche Experimente/Tests?

Fünf Experiment-Blöcke (E1–E5) als Pflicht, zwei (E6–E7) als Kür.

## E1 — Text/Hanzi → Pinyin+Ton (RQ1)
- **Was:** Modell bekommt Hanzi-Text (bzw. einzelne Zeichen/Silben) und soll Pinyin + Tonnummer ausgeben.
- **Warum:** Isoliert die **linguistische** Fähigkeit von der **akustischen** — testet, ob ein Modell überhaupt das Pinyin-System + Polyphone (多音字) beherrscht, bevor Audio ins Spiel kommt.
- **Modelle:** **alle 14** (auch Text-only: Claude, DeepSeek — die haben keinen Audio-Input).
- **Baseline:** g2pW (BERT, 99,08 % auf CPP-Benchmark). Ein LLM, das unter g2pW liegt, taugt für diese Aufgabe nicht.
- **Daten:** Hanzi-Labels der Tone-Perfect-Silben (kein Audio nötig).

## E2 — Audio NATIVE → Pinyin+Ton (RQ2) ★ Kern
- **Was:** Multimodales LLM hört eine isolierte native Silbe und gibt direkt `ma2` aus.
- **Warum:** Die zentrale Forschungsfrage — *können LLMs Töne hören?* Kontrolliert (isolierte Silben, kein Tone Sandhi, kein Satzkontext).
- **Modelle:** 11 audio-fähige (7 multimodal + 4 ASR-Baseline).
- **Daten:** Tone Perfect (9.840 Silben; günstige Modelle auf allem, teures GPT-5.5 auf 700er-Sub-Sample).
- **Spezialmodell-Baseline:** CNN-Tonklassifikator auf Tone Perfect (~99 %) als Obergrenze.

## E3 — Audio L2 → Pinyin+Ton (RQ3) ★ Kern
- **Was:** Dasselbe, aber mit Aufnahmen von Mandarin-Lernern (fehlerhafte Aussprache).
- **Warum:** Hier ist die Forschungslücke am größten — kein L2-Mandarin-Korpus wurde je mit multimodalen LLMs getestet; spezialisierte MDD-Systeme erreichen hier nur DER 26–32 %.
- **Kritischer Punkt:** Ground Truth = **was tatsächlich produziert/gehört wurde** (perzeptiv annotiert), **nicht** die kanonische Soll-Aussprache.
- **Modelle:** 11 audio-fähige.
- **Daten:** LATIC + OMPAL (~4.350 Äußerungen, 50 L2-Sprecher).

## E4 — Vergleich: LLM vs. ASR vs. Spezialmodell (RQ4)
- **Was:** Drei-Wege-Vergleich auf denselben Daten:
  1. **Multimodale LLMs** (Audio→Pinyin direkt)
  2. **Reine ASR** (Whisper/FireRed → Hanzi → g2pW → Pinyin) — Tonfehler bei Homophonen verdeckt
  3. **Spezial-Tonklassifikator** (CNN ~99 % native) als Referenz-Obergrenze
- **Plus Literaturanschluss:** CER auf AISHELL-1, um mit den 15+ Papers vergleichbar zu sein, die AISHELL als Benchmark nutzen.

## E5 — Fehlerstruktur (RQ5)
- **Was:** Konfusionsmatrizen (welcher Ton → welcher Ton), TER pro Ton, PFER-Aufschlüsselung nach Initial/Final.
- **Erwartete Muster:** T2↔T3 (beide steigend am Ende), Ton 3 als schwierigster (Literatur durchgängig). Retroflexe (zh/ch/sh/r) als Segment-Fehlerquelle.
- **Output:** Die eigentliche linguistische Erkenntnis — *wo und warum* scheitern LLMs.

## E6 — Format-Pilot (Kür)
- **Was:** 6 Transkriptionsformate vergleichen (Pinyin+Ton#, Pinyin+Diakritika, Pinyin ohne Ton, IPA+Chao, IPA+Tone-Letters, Chao separat) → welches funktioniert am besten mit LLMs?
- **Hypothese:** Pinyin+Tonnummern > IPA bei LLMs (Chirkova-Umkehr), weil LLMs massiv mehr Pinyin im Training haben.
- **Umfang:** 400 Samples × 3 Modelle × 6 Formate = 7.200 Calls, < 10 $.
- **Status:** Kür — Pinyin+Ton# ist bereits durch Literatur begründet; der Pilot ist methodische Verstärkung.

## E7 — Eigener L2-Korpus (Kür) → siehe (f)/(g)

---

# (b) Welche Modelle — die finale Auswahl

Abdeckung von **3 Märkten** (USA/China/Europa) × **3 Systemtypen**. Preise online verifiziert (2026-06-28).

## Gruppe 1 — Multimodale LLMs (Audio → Pinyin direkt) — der Kern (7)
| Modell | Anbieter | Markt | API |
|---|---|---|---|
| **GPT-5.5** | OpenAI | USA | ✅ |
| **Gemini 3.1 Pro** | Google | USA | ✅ |
| **Grok 4.3** | xAI | USA | ✅ |
| **Qwen3.5-Omni** | Alibaba | China | ✅ Alibaba Cloud |
| **Kimi-Audio** | Moonshot | China | ✅ Replicate |
| **GLM-4-Voice** | Zhipu | China | ✅ DeepInfra |
| **Step-Audio 2.5** | StepFun | China | ✅ StepFun ($0,022/Std) |

## Gruppe 2 — Reine ASR (→ Hanzi, G2P nötig) = Baseline (4)
| Modell | Anbieter | Markt | API | Rolle |
|---|---|---|---|---|
| **Whisper large-v3** | OpenAI | USA | ✅ | Standard-Baseline |
| **Scribe v2** | ElevenLabs | USA | ✅ | ASR-Vergleich |
| **Voxtral 2** | Mistral | Europa | ✅ | EU-Vertreter |
| **FireRedASR2S** | Xiaohongshu | China | ❌ nur lokal | optional / durch Qwen3-ASR ersetzbar |

## Gruppe 3 — Text-only (nur RQ1) (3)
| Modell | Anbieter | Markt | Audio? |
|---|---|---|---|
| **Claude Opus 4.8** | Anthropic | USA | ❌ kein Audio |
| **Claude Sonnet 4.6** | Anthropic | USA | ❌ kein Audio |
| **DeepSeek V4** | DeepSeek | China | ❌ kein Audio |

**Gesamt: 14 Modelle** = 11 audio-fähige (E2/E3) + 3 text-only (nur E1).
**Wichtige methodische Trennung:** Gruppe 1 wird geprompted, Pinyin+Ton **direkt** auszugeben (Ton prüfbar). Gruppe 2 gibt **Hanzi** aus → G2P-Nachbearbeitung → Tonfehler bei Homophonen verschwinden → deshalb **nur Baseline**, kein gleichwertiger Kandidat.
**FireRedASR2S:** einziges Modell ohne API. Entweder lokale GPU oder Ersatz durch Qwen3-ASR. Für den Pflicht-Kern **nicht kritisch**.

---

# (h)/(i) Welche Korpora, wie viel, in welchem Status

## Pflicht-Korpora (bestehend, sofort verfügbar)

| Korpus | RQ | Inhalt | Umfang genutzt | Status |
|---|---|---|---|---|
| **Tone Perfect** 🔑 | RQ2 | 410 Silben × 4 Töne × 6 native Sprecher = 9.840 WAV | **Voll (9.840)** für günstige Modelle; **700er-Sub-Sample** (stratifiziert) für GPT-5.5 | CC-BY 4.0, Open Access (tone.lib.msu.edu) |
| **LATIC** 🔑 | RQ3 | 4 L2-Sprecher, 2.579 Wörter/Äußerungen, Pinyin+Tonnummern | **Voll** | IEEE DataPort, öffentlich |
| **OMPAL** 🔑 | RQ3 | 46 L2-Sprecher (Frz. L1), 1.768 Sätze, Experten-Wortrating | **Voll** | Open-Source (Interspeech 2025) |
| **AISHELL-1** | RQ4 | 170h, 400 native Sprecher | Nur **CER-Teilmenge** für Literaturvergleich | OpenSLR/33 |

**Sub-Sampling Tone Perfect (700):** stratifiziert nach (a) allen 4 Tönen gleich, (b) allen 6 Sprechern, (c) Schwierigkeit der Silbe (einfach/mittel/schwer, inkl. Retroflexe + Diphthonge). Konkret z. B. **30 Silben × 4 Töne × 6 Sprecher = 720**. Nur für das teuerste Modell (GPT-5.5) nötig; alle anderen laufen auf dem vollen Korpus.

## Warum nicht die großen Korpora?
AISHELL (170h), MAGICDATA (755h), KeSpeech (1.542h), WenetSpeech (12.800h) haben **keinen Ton-Ground-Truth** — man kann CER rechnen, aber keine TER. Für kontrollierte Tonevaluation zählt **Ground-Truth-Qualität**, nicht Stundenzahl. Tone Perfect hat nur ~2h, aber jedes Sample ein exaktes Label.

## Kür-Korpus
**Eigener L2-Mini-Korpus** (deutsche Mandarin-Lerner) → Details unter (f)/(g).
**Bonus-Kontakte** (falls mehr L2-Daten nötig): iCALL (Nancy Chen, 305 Sprecher), SingaKids (255 Kinder) — nicht öffentlich, nur per Anfrage.

---

# (c) Was kostet das?

Alle Preise online verifiziert (2026-06-28). API-Kosten in USD.

| Szenario | Kosten | Kommentar |
|---|---|---|
| **Pflicht-Kern** (günstige Modelle voll + GPT-5.5 sub-sample + Text-only) | **~15–30 €** | realistisches Arbeitsbudget |
| Alle Modelle, alle 9.840 Samples (ohne Sub-Sampling) | ~25–44 $ | obere Grenze, immer noch < 50 € |
| Mit Sub-Sampling (700) überall | ~4–7 $ | Minimal-Variante |
| Format-Pilot (Kür) | < 10 $ | 7.200 Calls |

**Teuerstes Modell:** GPT-5.5 ($5/$30 pro 1M Token + Audio-Token) → nur dort Sub-Sampling.
**Günstigste:** Step-Audio 2.5 ($0,022/Std), Voxtral, Whisper.
**Kein Cloud-GPU-Mietbedarf** (11/12 Audio-Modelle per API). Nur FireRedASR2S bräuchte lokale GPU.
**Annotationskosten** (nur Kür-Korpus) — separat, siehe (g).

---

# (d) Phasen

Strategie: **günstig anfangen, teuer nur bei Bedarf.**

| Phase | Inhalt | Modelle | Kosten | Pflicht? |
|---|---|---|---|---|
| **P0 — Setup & API-Checks** | Test-Calls für Replicate/StepFun/DeepInfra aus DE; Grok-Audio bestätigen; Prompt-Templates | — | ~0 | Pflicht |
| **P1 — Pipeline-Pilot** | 400 Tone-Perfect + 200 LATIC Samples durch die ganze Pipeline (Eval-Skripte testen) | billigste 2–3 | < 1 $ | Pflicht |
| **P2 — Günstige APIs, voller Korpus** | E2 + E3 | Step-Audio, Whisper, Voxtral, Scribe, GLM, Qwen-Omni | ~7–12 $ | Pflicht |
| **P3 — Mittlere APIs** | E2 + E3 | Gemini, Grok, Kimi-Audio | +5–15 $ | Pflicht |
| **P4 — Premium (Sub-Sample)** | E2 + E3 auf 700er-Sample | GPT-5.5 | +1–2 $ | Pflicht |
| **P5 — Text-only** | E1 (RQ1) | Claude Opus/Sonnet, DeepSeek | +0,9 $ | Pflicht |
| **P6 — Auswertung** | TER/PFER/CER, Konfusionsmatrizen (E4/E5) | — | 0 | Pflicht |
| **P7 — Kür** | Format-Pilot (E6) und/oder eigener L2-Korpus (E7) | — | +10 $ + Annotation | Kür |

---

# (e) Pflicht vs. Kür

## PFLICHT (muss in 2–3 Monaten fertig sein)
- E1 (RQ1): Text→Pinyin, alle 14 Modelle, g2pW-Baseline
- E2 (RQ2): Audio native, 11 Modelle, Tone Perfect (voll + GPT-5.5-Sub-Sample)
- E3 (RQ3): Audio L2, 11 Modelle, LATIC + OMPAL
- E4 (RQ4): Vergleich LLM/ASR/CNN + CER-Literaturanschluss (AISHELL)
- E5 (RQ5): Fehlerstruktur (TER, PFER, Konfusionsmatrizen)
- Phasen P0–P6

## KÜR (nur wenn Zeit & Budget bleiben — Priorität in dieser Reihenfolge)
1. **Eigener L2-Mini-Korpus** (E7) — höchster Neuheitswert, du hast die Ressourcen
2. **Format-Pilot** (E6) — methodische Verstärkung, < 10 $
3. **iCALL/SingaKids-Kontakt** — mehr L2-Daten, falls LATIC+OMPAL zu klein wirken
4. **FireRedASR2S lokal** — vollständige ASR-Baseline
5. **Voller Tone-Perfect-Lauf auch für GPT-5.5** (statt Sub-Sample)

**Entscheidungsregel:** Erst wenn der gesamte Pflicht-Kern (inkl. sauberer Auswertung) steht, wird Kür angefasst — beginnend mit dem eigenen L2-Korpus.

---

# (f) Eigener Korpus — ob, wie viel, wie sammeln, wie bearbeiten

**Entscheidung: JA, aber als Kür** — bestehende Korpora tragen das Pflicht-Experiment; der eigene Korpus ist ein gezielter Zusatzbeitrag, **wenn** der Kern steht.

## Warum überhaupt ein eigener Korpus?
- Öffentliche L2-Korpora sind winzig (LATIC: 4 Sprecher) oder einseitig (OMPAL: nur Frz. L1).
- **Kein** L2-Korpus mit **deutschen** Lernern ist öffentlich → eigene L1-Gruppe = echter Beitrag, passt zum geplanten Vokabeltrainer (deutschsprachige Zielgruppe).
- Du hast Zugang zu L2-Lernern + Annotatoren + Budget → machbar.

## Wie viel braucht man? (klein & kontrolliert halten)
| Parameter | Empfehlung | Begründung |
|---|---|---|
| **Sprecher** | 8–12 deutsche L2-Lerner | genug für Sprecher-Varianz, klein genug für 2–3 Wochen |
| **Niveau-Mix** | A2–B2 (gestaffelt) | Fehlerbandbreite abbilden |
| **Items** | ~80–100 Zielsilben/Wörter | Tonpaar-Minimalpaare + die schwierigen Tone-Perfect-Silben (Retroflexe, Diphthonge) |
| **Wiederholungen** | 1–2× pro Item | Konsistenz messbar |
| **Gesamt** | **~800–2.000 Aufnahmen** | vergleichbar mit LATIC, in der Annotation noch beherrschbar |

## Wie bekommt man es? (Erhebung)
1. **Rekrutierung:** über IU-Netzwerk, Mandarin-Sprachkurse, Hochschulgruppen.
2. **Stimulus-Design:** Zielwörter als **Hanzi + Referenz-Pinyin** zeigen (oder Bild/Audio-Vorgabe, um Lesefehler zu vermeiden). Items aus der Pilot-Silbenliste, damit Vergleichbarkeit mit Tone Perfect besteht.
3. **Aufnahme-Protokoll:** ruhiger Raum, Headset-Mikrofon, einheitlich **WAV 16 kHz mono**; pro Item Einzelaufnahme; standardisierte Instruktion.
4. **Ethik/DSGVO:** Einwilligungserklärung, Pseudonymisierung (Sprecher-IDs statt Namen), Speicher-/Löschkonzept. Vorab mit Tim/IU klären.

## Wie bearbeitet man es? (Processing)
1. **Normalisierung:** Lautstärke, Sampling-Rate vereinheitlichen; Stille trimmen.
2. **Segmentierung:** pro Item eine Datei; bei Bedarf MFA für Phonem-Zeitgrenzen (Achtung: MFA ist auf native Aussprache trainiert → bei starken L2-Abweichungen manuell nachkontrollieren).
3. **Struktur:** `sprecherID_item_wiederholung.wav` + Metadaten-CSV (Niveau, L1, Zielsilbe, Zielton).
4. **Qualitätskontrolle:** kaputte/abgebrochene Aufnahmen aussortieren.

---

# (g) Wie wird der eigene Korpus annotiert?

**Kernprinzip:** Annotiert wird, **was tatsächlich gehört wurde** (perzeptive Realisierung), nicht die Soll-Aussprache. Genau das macht den L2-Korpus wertvoll — und genau das können automatische Tools (Whisper+g2pW) nicht liefern.

## Annotations-Schema (pro Aufnahme)
- **Initial** (gehörter Konsonant), **Final** (gehörter Reim), **Ton** (1–4 + neutral 5) — getrennt.
- Optional Fehler-Flags: ausgelassen / ersetzt / hinzugefügt (analog iFlytek-Schema).

## Workflow (halbautomatisch, kostengünstig)
1. **Vor-Label automatisch:** CNN-Tonklassifikator (Tone Perfect, ~99 % native — bei L2 nur Vorschlag!) + g2pW für kanonische Referenz + optional SpeechSuper/iFlytek-API als zweite Maschinen-Meinung.
2. **Menschliche Annotation:** **2 Muttersprachler** annotieren unabhängig, **was sie hören**.
3. **Inter-Rater-Agreement:** Cohen's κ berechnen. Ziel **κ > 0,85**. Erst darüber wäre Teilautomatisierung vertretbar.
4. **Konfliktauflösung:** Bei Annotator-Disagreement oder Tool-vs-Mensch-Konflikt entscheidet ein 3. Annotator **oder** Praat/Parselmouth-F0-Kontur als objektiver Schiedsrichter (z. B. T2 vs. T3 anhand des Pitch-Verlaufs).
5. **Stichproben-Audit:** mindestens 15–20 % doppelt prüfen.

## Aufwand & Kosten (Annotation)
- ~1.000–2.000 Items × 2 Annotatoren = ~2.000–4.000 Labels.
- Bei ~100–200 Items/Std → **~20–40 Annotator-Stunden**.
- Mit bezahlten Muttersprachlern realistisch **~150–500 €** (separates Annotationsbudget).
- Dauer: ~1 Woche bei paralleler Arbeit.

**Wichtig:** Für die **bestehenden** Korpora (Pflicht) entfällt das — Tone Perfect ist bereits gelabelt (Dateiname = Ground Truth), LATIC/OMPAL bringen Annotationen mit. Manuelle Annotation fällt **nur** beim eigenen Kür-Korpus an.

---

# Querschnitt: Metriken (für alle Experimente)

| Metrik | Misst | Berechnung | Bei welchem RQ |
|---|---|---|---|
| **Syllable Accuracy** | Silbe (Initial+Final) korrekt | Exact Match | RQ2, RQ3 |
| **Tone Accuracy / TER** | Ton korrekt (1–4+5) | 1 − Tonfehlerrate | **RQ2, RQ3, RQ5** (Kernbeitrag) |
| **Combined Accuracy** | Silbe UND Ton korrekt | beide korrekt | RQ2, RQ3 |
| **PFER** | artikulatorisch gewichtete Distanz | pinyin-to-ipa → PanPhon (21 Features) | RQ5 |
| **CER** | Zeichenfehlerrate | Levenshtein auf Hanzi | RQ4 (Literaturanschluss) |
| **Tone Confusion Matrix** | welcher Ton → welcher Ton | Häufigkeitsmatrix | RQ5 |

**Tooling:** g2pW (Baseline + kanonische Referenz), pinyin-to-ipa + PanPhon (PFER), CNN-Klassifikator (native Obergrenze), Praat/Parselmouth (F0-Schiedsrichter bei L2).

---

# Realistischer Zeitplan (2–3 Monate, nur Pflicht)

| Woche | Inhalt |
|---|---|
| **1** | P0: Korpora beschaffen (Tone Perfect, LATIC, OMPAL); API-Zugänge testen; Prompt-Templates; Eval-Skripte (TER/PFER/CER) |
| **2** | P1: Pipeline-Pilot (400+200 Samples); Bugs fixen; Output-Parsing robust machen |
| **3–4** | P2 + P3: günstige & mittlere APIs auf vollem Korpus (E2 + E3) |
| **5** | P4 + P5: GPT-5.5 Sub-Sample + Text-only (RQ1) |
| **6–7** | P6: Auswertung — TER/PFER/CER, Konfusionsmatrizen, Vergleichstabellen (E4 + E5) |
| **8** | Ergebnisse verschriftlichen, Plots, Limitationen |
| **9–12** | **Puffer + Kür** (in Prioritätsreihenfolge): eigener L2-Korpus → Format-Pilot → iCALL-Kontakt |

---

# Zusammenfassung (eine Folie für Tim)

```
EXPERIMENTE   E1 Text→Pinyin · E2 Audio-native→Pinyin · E3 Audio-L2→Pinyin
              E4 Vergleich (LLM/ASR/CNN) · E5 Fehlerstruktur
              [Kür] E6 Format-Pilot · E7 eigener L2-Korpus

MODELLE       14 = 7 multimodal + 4 ASR-Baseline + 3 text-only
              (3 Märkte: USA/China/EU)

KORPORA       Pflicht: Tone Perfect (native, 9.840) + LATIC + OMPAL (L2, ~4.350)
              Vergleich: AISHELL-1 (CER) · Kür: eigener dt. L2-Korpus (~8–12 Sprecher)

METRIKEN      TER + PFER (Kernbeitrag) + CER (Literaturanschluss)

KOSTEN        Pflicht 15–30 € · alles < 50 € · Annotation nur für Kür-Korpus

ZEIT          ~6 Wochen Pflicht-Kern + Puffer/Kür in Wochen 9–12
```
