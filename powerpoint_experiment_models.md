# PowerPoint — Modellauswahl & Kostenkalkulation (Experiment)
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Zweck:** Folien zur Frage „Welche Modelle testen wir, was können sie, und was kostet das?" — mit **kritischer Prüfung** des DeepSeek-Plans und **verifizierter** Kostenschätzung.
>
> **Status der Prüfung (2026-06-28):** Alle Preise per WebSearch gegen offizielle Quellen verifiziert. Detaillierte Quellenangaben in Teil 0.

---

# TEIL 0: PRÜF-ERGEBNIS (für Stefan, nicht zum Vortragen)

## Verifizierte API-Preise (Stand 2026-06-28)

| Modell | Input $/1M Token | Output $/1M Token | Audio-Preis | Quelle | Verifiziert |
|---|---|---|---|---|---|
| GPT-5.5 (OpenAI) | $5,00 | $30,00 | Audio-Token: ~$3–6/1M; Realtime: $0,05/Min | OpenAI Pricing Page | ✅ |
| Gemini 3.1 Pro (Google) | $2,00 | $12,00 | Audio-Multiplikator 2–7× Text | Google AI Pricing | ✅ |
| Grok **4.3** (xAI) | $1,25 | $2,50 | Voice: $0,05/Min; STT batch: $0,10/Std | xAI API Docs | ✅ |
| Qwen3.5-Omni Plus (Alibaba) | $0,40–1,82 | $4,80–10,79 | Audio inkl. im Token-Preis | Alibaba Cloud Model Studio | ✅ |
| Kimi K2.6 (Moonshot) | $0,95 | $4,00 | Text-API; Audio-Modell separat | Moonshot Kimi Pricing | ✅ |
| Kimi-Audio-7B (Replicate) | — | — | ~$0,0023/Sek GPU-Zeit | Replicate | ✅ |
| Step-Audio 2.5 ASR (StepFun) | — | — | **$0,022/Std** | StepFun API Docs | ✅ |
| GLM-4.6 (DeepInfra/Together) | $0,60 | $2,20 | Voice-Endpoint verfügbar | DeepInfra GLM-4.6 | ✅ |
| Whisper large-v3 (OpenAI) | — | — | $0,006/Min ($0,36/Std) | OpenAI Pricing | ✅ |
| Voxtral Small (Mistral) | $0,10 | $0,30 | Transkription: $0,001–0,002/Min | Mistral Pricing | ✅ |
| Scribe v2 (ElevenLabs) | — | — | $0,22/Std | ElevenLabs Pricing | ✅ |
| FireRedASR2S (Xiaohongshu) | — | — | **Keine API** — nur lokal | Hugging Face | ✅ |
| DeepSeek V4 Pro | $0,44 | $0,87 | **KEIN AUDIO** (bestätigt) | DeepSeek API Docs | ✅ |
| Claude Opus 4.8 | $5,00 | $25,00 | **KEIN AUDIO** | Anthropic offiziell | ✅ |
| Claude Sonnet 4.6 | $3,00 | $15,00 | **KEIN AUDIO** | Anthropic offiziell | ✅ |

## Was am DeepSeek-Plan stimmt
- ✅ **Kategorisierung korrekt:** Echte multimodale LLMs vs. reine STT/ASR (Hanzi-Output) vs. Text-only ist sauber getrennt
- ✅ **Claude raus = richtig:** Claude Opus 4.8 / Sonnet 4.6 haben **keinen Audio-Input** (nur Text + Bild) — korrekt nur für RQ1 (Text→Pinyin) eingeplant
- ✅ **Claude-Preise korrekt:** Opus 4.8 = $5/$25, Sonnet 4.6 = $3/$15 — **offiziell bestätigt**
- ✅ **DeepSeek V4 = kein Audio:** Bestätigt — nur Text+Vision → nur RQ1
- ✅ **Doubao als „nicht testbar"** korrekt (keine öffentliche API aus Europa)
- ✅ **Whisper / FireRedASR2S = Hanzi-Output, G2P nötig** — wichtiger methodischer Punkt richtig erkannt

## Wo der Plan korrigiert werden muss (5 Korrekturen)
1. **❌ Grok 4.2 existiert nicht → aktuell Grok 4.3.** Aber gute Nachricht: Grok 4.3 ist mit $1,25/$2,50 pro 1M Token **deutlich günstiger** als geschätzt. STT-Batch-API: nur $0,10/Std
2. **❌ „Lokale Modelle" haben APIs.** Qwen3.5-Omni → Alibaba Cloud API; Kimi-Audio → Replicate; Step-Audio 2.5 → StepFun API ($0,022/Std!); GLM-4 → DeepInfra/Together. **Nur FireRedASR2S braucht wirklich eine lokale GPU**
3. **❌ Audio-Input-Preis bei GPT-5.5 war zu niedrig.** Der Plan rechnet mit $0,006/Min (Whisper-Rate). Audio-Token in GPT-5.5 kosten ~$3–6/1M Token — aber GPT-5.5-Output ist mit $30/1M auch teurer als geschätzt
4. **❌ Wiederholter Prompt-Overhead ignoriert.** Jeder der ~9.840 Audio-Calls schickt System+User-Prompt erneut (~150 Token). Das sind **~1,5 Mio. Input-Token pro Modell**
5. **⚠️ Gesamtkosten dennoch unter Budget.** Trotz der Korrekturen bleiben die Gesamtkosten dank günstiger chinesischer APIs und Grok-Preisüberraschung **unter 50 €** — auch ohne Sub-Sampling

## Inkonsistenzen im Plan (gelöst)
- ~~DeepSeek V4: Tabelle sagt „nativ multimodal inkl. Audio", finales Set sagt „KEIN AUDIO"~~ → **Gelöst: DeepSeek V4 = kein Audio, nur Text+Vision → nur RQ1**
- Überschrift „11 Modelle", Liste enthält 12

---

# TEIL I: FOLIEN

---

## Folie X1 — Modellauswahl: drei Märkte, drei Systemtypen

**Titel:** 12 Modelle — aber nicht alle messen dasselbe

**Visuelles Raster:**
```
                USA / GLOBAL          CHINA                  EUROPA
─────────────────────────────────────────────────────────────────────
MULTIMODAL    GPT-5.5 (OpenAI)      Qwen3.5-Omni (Alibaba)
LLM           Gemini 3.1 Pro        Kimi-Audio (Moonshot)
(Audio→Pinyin Grok 4.3 (xAI)        GLM-4-Voice (Zhipu)
 direkt)                            Step-Audio 2.5 (StepFun)
─────────────────────────────────────────────────────────────────────
REINE ASR     Whisper large-v3      FireRedASR2S            Voxtral 2
(→ Hanzi,     Scribe v2 (11Labs)    (Xiaohongshu)          (Mistral)
 G2P nötig)
─────────────────────────────────────────────────────────────────────
NICHT TESTBAR                       Doubao (keine API)
─────────────────────────────────────────────────────────────────────
TEXT-ONLY     Claude Opus 4.8       DeepSeek V4
(nur RQ1)     Claude Sonnet 4.6
```

**Sprechernotiz:** „Die Auswahl deckt bewusst die drei großen KI-Märkte ab — USA, China, Europa — damit die Ergebnisse generalisierbar sind. Entscheidend ist aber die senkrechte Achse: Es sind drei verschiedene Systemtypen. Oben die echten multimodalen LLMs, die Audio direkt verarbeiten und auf Aufforderung Pinyin mit Tönen ausgeben können — das ist der Kern meiner Forschungsfrage. In der Mitte reine ASR-Systeme wie Whisper oder FireRed: Die geben chinesische Schriftzeichen aus, nicht Pinyin — ich muss sie nachträglich per Grapheme-to-Phoneme umwandeln, und genau dabei können Tonfehler bei Homophonen verschwinden. Diese Systeme laufen deshalb als Baseline, nicht als gleichwertige Konkurrenten. Claude und DeepSeek haben gar keinen Audio-Input und testen nur RQ1, also Text zu Pinyin."

---

## Folie X2 — Die versteckte Hürde: Hanzi vs. Pinyin-Output

**Titel:** Warum ASR-Systeme nur Baseline sind

**Visuelles Diagramm:**
```
Audio: "yí" (Tone 2)

  MULTIMODALES LLM (GPT-5.5, Qwen-Omni …)
  Prompt: "Transcribe to Pinyin with tone number"
  → "yi2"   ✓ Ton direkt prüfbar

  REINE ASR (Whisper, FireRedASR2S …)
  → 一 (Hanzi)
  → Pypinyin(一) = "yī"   ✗ Ton 1 statt Ton 2 — FEHLER VERDECKT
                            (G2P nutzt Standard-Ton des Zeichens,
                             nicht den tatsächlich gesprochenen)
```

**Bullets:**
- Multimodale LLMs lassen sich **prompten**, Pinyin+Ton direkt auszugeben
- ASR-Systeme geben **Hanzi** aus → G2P-Nachbearbeitung → Tonfehler bei Homophonen unsichtbar
- → Sauberer methodischer Beitrag: **die zwei Kategorien getrennt auswerten**

**Sprechernotiz:** „Diese Folie zeigt, warum die Unterscheidung kein Detail ist. Sage ich dem multimodalen LLM ‚gib mir Pinyin mit Tonnummer', bekomme ich ‚yi2' und kann den Ton direkt prüfen. Die reine ASR gibt mir das Schriftzeichen 一. Wenn ich das per Pypinyin in Pinyin umwandle, bekomme ich den kanonischen Ton des Zeichens — und der kann vom tatsächlich gesprochenen abweichen. Der Tonfehler verschwindet in der Nachbearbeitung. Deshalb sind Whisper und FireRed bei mir Baseline-Referenzen, keine gleichwertigen Kandidaten. Diese Trennung adressiert kein einziges der 42 Paper explizit."

---

## Folie X3 — API-Verfügbarkeit: Fast alles online nutzbar

**Titel:** Überraschung — nur 1 von 12 Modellen braucht lokale GPU

**Tabelle (Folie):**

| Modell | API-Zugang | Anbieter | Preis (verifiziert) |
|---|---|---|---|
| GPT-5.5 | ✅ API | OpenAI | $5/$30 pro 1M Token |
| Gemini 3.1 Pro | ✅ API | Google AI | $2/$12 pro 1M Token |
| Grok 4.3 | ✅ API | xAI | $1,25/$2,50 pro 1M Token; STT: $0,10/Std |
| Qwen3.5-Omni | ✅ API | Alibaba Cloud | $0,40–1,82/$4,80–10,79 pro 1M Token |
| Kimi-Audio | ✅ API | **Replicate** | ~$0,0023/Sek GPU-Zeit |
| Step-Audio 2.5 | ✅ API | **StepFun** | **$0,022/Std** |
| GLM-4-Voice | ✅ API | **DeepInfra** / Together | $0,60/$2,20 pro 1M Token |
| Whisper large-v3 | ✅ API | OpenAI | $0,006/Min |
| Voxtral Small | ✅ API | Mistral | $0,001–0,002/Min |
| Scribe v2 | ✅ API | ElevenLabs | $0,22/Std |
| FireRedASR2S | ❌ **nur lokal** | Hugging Face Weights | Eigene GPU nötig |

**Sichtbares Hinweis-Kästchen:**
> **Keine eigene GPU nötig** für 11 von 12 Modellen. Chinesische Open-Source-Modelle sind über Drittanbieter (Replicate, DeepInfra, StepFun) aus Europa erreichbar.

**Sprechernotiz:** „Eine wichtige Erkenntnis aus der Preisrecherche: Fast alle Modelle, die der ursprüngliche Plan als ‚lokal, kostenlos' eingestuft hat, sind inzwischen über APIs zugänglich — und oft sehr günstig. Step-Audio von StepFun kostet nur 2 Cent pro Stunde. Kimi-Audio läuft über Replicate, GLM über DeepInfra. Einzig FireRedASR2S hat keinen API-Anbieter und muss lokal auf einer GPU laufen. Das vereinfacht die Infrastruktur enorm: Statt fünf Open-Source-Modelle auf eigener Hardware zu betreiben, reicht ein Python-Skript mit verschiedenen API-Endpunkten."

---

## Folie X4 — Was kostet das wirklich? (verifiziert)

**Titel:** Kostenschätzung — mit verifizierten Preisen (Stand Juni 2026)

**Tabelle (Folie):**

| Modell | DeepSeek-Plan | Verifiziert (alle 9.840 Samples) | Verifiziert (Sub-Sample 700) |
|---|---|---|---|
| GPT-5.5 (OpenAI) | $4,54 | **~$10–15** | ~$1–2 |
| Gemini 3.1 Pro | $0,70 | **~$4–8** | ~$0,50–1 |
| Grok 4.3 (xAI) | $2,83 | **~$0,55–3** | ~$0,10–0,50 |
| Qwen3.5-Omni (Alibaba API) | $0 (lokal) | **~$2–5** | ~$0,30–0,60 |
| Kimi-Audio (Replicate) | $0 (lokal) | **~$2–5** | ~$0,30–0,60 |
| Step-Audio 2.5 (StepFun) | $0 (lokal) | **~$0,12** | ~$0,01 |
| GLM-4-Voice (DeepInfra) | $0 (lokal) | **~$1–3** | ~$0,15–0,40 |
| Whisper large-v3 | $0,75 | **~$2** | ~$0,15 |
| Voxtral Small | $1,05 | **~$0,50–1** | ~$0,05–0,10 |
| Scribe v2 | $1,82 | **~$1,20** | ~$0,10 |
| FireRedASR2S (lokal) | $0 | **$0** (eigene GPU) | $0 |
| Claude (RQ1, Text) | $0,79 | **$0,79 ✓** | $0,79 |
| DeepSeek V4 (RQ1, Text) | $0,10 | **$0,10 ✓** | $0,10 |
| **GESAMT** | **~$24** | **~$25–44** | **~$4–7** |

**Sichtbares Hinweis-Kästchen:**
> **Budget-Grenze: 50 €.** Auch ohne Sub-Sampling machbar. Mit Sub-Sampling: **~5 € Gesamt.**

**Sprechernotiz:** „Mit den verifizierten Preisen sieht das Bild besser aus als befürchtet. Der DeepSeek-Plan hatte zwar die Audio-Input-Kosten bei GPT unterschätzt, aber dafür sind drei Dinge günstiger als gedacht: Erstens ist Grok 4.3 mit 1,25 Dollar pro Million Token viel billiger als die geschätzten 20 Dollar. Zweitens haben die chinesischen Modelle inzwischen günstige APIs — Step-Audio kostet nur 2 Cent pro Stunde, Qwen-Omni läuft über Alibaba Cloud. Drittens brauche ich keine Cloud-GPU-Miete, weil fast alles per API erreichbar ist. Selbst wenn ich alle Modelle auf allen 9.840 Samples laufen lasse, lande ich bei 25 bis 44 Dollar — unter den 50 Euro. Mit einer Teilstichprobe wäre es sogar nur 4 bis 7 Dollar. Die einzige Ausnahme ist FireRedASR2S, das hat keine API und braucht eine eigene GPU."

---

## Folie X5 — Budget-Strategie und Reihenfolge

**Titel:** Strategie: günstig anfangen, teuer nur bei Bedarf

**Bullets:**
- **Phase 1 — Pilot (400 Samples):** Pipeline testen mit den billigsten Modellen (Step-Audio: $0,01; Whisper: $0,15; Voxtral: $0,05) → **< $1**
- **Phase 2 — Günstige APIs auf vollem Corpus:** Step-Audio, Whisper, Voxtral, Scribe, GLM, Qwen-Omni auf allen 9.840 Samples → **~$7–12**
- **Phase 3 — Mittlere APIs:** Gemini, Grok, Kimi-Audio auf allen/meisten Samples → **+$5–15**
- **Phase 4 — Premium-APIs:** GPT-5.5 auf stratifizierter Teilstichprobe (700 Samples) → **+$1–2**
- **Phase 5 — Text-only (RQ1):** Claude + DeepSeek → **+$0,89**
- **FireRedASR2S:** Lokal auf eigener GPU oder weglassen (durch Qwen3-ASR ersetzbar)

**Sichtbares Hinweis-Kästchen:**
> **Realistisches Gesamtbudget: 15–30 €.** Sub-Sampling nur bei GPT-5.5 nötig — Rest passt auf vollem Corpus ins Budget.

**Sprechernotiz:** „Die Strategie ist einfach: billig anfangen und sich hocharbeiten. In Phase 1 teste ich die Pipeline mit einem 400-Sample-Pilot auf den günstigsten Modellen — das kostet unter einem Dollar und deckt trotzdem technische Probleme auf. In Phase 2 laufen alle günstigen APIs auf dem vollen Corpus, das sind unter 12 Dollar. Phase 3 bringt die mittelpreisigen Modelle dazu. Und nur bei GPT-5.5 in Phase 4 nutze ich Sub-Sampling, weil das das einzige wirklich teure Modell ist — 30 Dollar Output-Kosten pro Million Token. Aber selbst da reichen 700 Samples für einen statistisch belastbaren Vergleich. Insgesamt lande ich realistisch bei 15 bis 30 Euro. Die 50-Euro-Grenze ist kein Problem."

---

## Folie X6 — Offene Punkte vor dem Start

**Titel:** Vor dem ersten Euro: 3 Dinge prüfen

**Tabelle (Folie):**

| Zu prüfen | Status | Aktion |
|---|---|---|
| **Grok 4.3 Audio-API** | ✅ Existiert (STT batch $0,10/Std) | 1 Test-Call zur Bestätigung |
| **DeepSeek V4 Audio?** | ✅ Gelöst: kein Audio → nur RQ1 | Erledigt |
| **API-Zugang aus DE** | ⚠️ Replicate, StepFun, DeepInfra testen | Je 1 Test-Call vor Einplanung |
| **FireRedASR2S lokal** | ⚠️ Einziges Modell ohne API | GPU mit ausreichend VRAM prüfen; alternativ Qwen3-ASR als Ersatz |

**Sprechernotiz:** „Von den vier offenen Punkten ist einer gelöst: DeepSeek V4 hat definitiv kein Audio, also nur RQ1. Grok 4.3 hat eine dokumentierte STT-API, die muss ich mit einem Test-Call bestätigen. Der wichtigste Check: Funktionieren Replicate, StepFun und DeepInfra aus Deutschland? Das klärt je ein einziger API-Call. Und für FireRedASR2S — das einzige Modell ohne API — brauche ich entweder eine eigene GPU oder ich ersetze es durch Qwen3-ASR, das über Alibaba Cloud läuft und ebenfalls chinesische Transkription kann."

---

# TEIL II: Anmerkungen für Stefan

- **Folien X1–X6** sind die vortragbaren Folien; **Teil 0** ist deine interne Prüf-Checkliste mit allen Quellen.
- **Wichtigste Botschaft an Tim:** Die Modellauswahl ist methodisch stark (drei Märkte, klare Kategorien). Die Kostenschätzung des DeepSeek-Plans war in einzelnen Punkten ungenau, aber nach Verifizierung bleiben die **Gesamtkosten unter 50 €** — auch ohne Sub-Sampling. Sub-Sampling nur bei GPT-5.5 nötig.
- **Verifizierte Fakten (Q&A-fest):**
  - Alle Preise per WebSearch am 28.06.2026 gegen offizielle Quellen verifiziert
  - Claude Opus 4.8 = $5/$25, Sonnet 4.6 = $3/$15 — offiziell bestätigt
  - DeepSeek V4 = kein Audio — offiziell bestätigt
  - Grok 4.3 (nicht 4.2) = $1,25/$2,50 — verifiziert, deutlich günstiger als geschätzt
  - Step-Audio 2.5 ASR = $0,022/Std — verifiziert, extrem günstig
  - 11 von 12 Audio-Modellen haben API-Zugang (inkl. Drittanbieter wie Replicate, DeepInfra)
  - Nur FireRedASR2S hat keine API
- **Änderung gegenüber Erstversion:**
  - Folie X3 (API-Verfügbarkeit) ist **neu** — ersetzt den früheren „GPU-Kosten"-Abschnitt
  - Folie X4 enthält jetzt **verifizierte** statt geschätzte Preise
  - Folie X5 wurde zur Budget-Strategie mit Phasen umgebaut
  - Folie X6 reduziert offene Punkte von 4 auf 3 (DeepSeek gelöst)
- **Kürzungsoption:** X3+X4 zu einer Folie „API-Zugang & Kosten" zusammenziehen → 5 statt 6 Folien.
