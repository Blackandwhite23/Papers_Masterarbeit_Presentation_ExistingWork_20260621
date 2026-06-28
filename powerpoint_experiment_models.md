# PowerPoint — Modellauswahl & Kostenkalkulation (Experiment)
## Masterarbeit Stefan Dosch — *Assessing Multimodal Foundation Models for Phonetic and Tonal Transcription of Mandarin Speech*

> **Zweck:** Folien zur Frage „Welche Modelle testen wir, was können sie, und was kostet das?" — mit **kritischer Prüfung** des DeepSeek-Plans und **pessimistischer** Kostenschätzung.
>
> **Status der Prüfung:** Claude-Preise gegen offizielle Referenz verifiziert (Stand 2026-06-04). Drittanbieter-Preise (GPT-5.5, Gemini 3.1, Grok 4.2 etc.) **nicht offiziell verifizierbar** — als Schätzung gekennzeichnet.

---

# TEIL 0: PRÜF-ERGEBNIS (für Stefan, nicht zum Vortragen)

## Was am DeepSeek-Plan stimmt
- ✅ **Kategorisierung korrekt:** Echte multimodale LLMs vs. reine STT/ASR (Hanzi-Output) vs. Text-only ist sauber getrennt
- ✅ **Claude raus = richtig:** Claude Opus 4.8 / Sonnet 4.6 haben **keinen Audio-Input** (nur Text + Bild) — korrekt nur für RQ1 (Text→Pinyin) eingeplant
- ✅ **Claude-Preise korrekt:** Opus 4.8 = $5/$25 pro 1M Token (in/out), Sonnet 4.6 = $3/$15 — **offiziell bestätigt**. Die $0,49 / $0,30 für RQ1 stimmen rechnerisch
- ✅ **Doubao als „nicht testbar"** korrekt (keine öffentliche API aus Europa)
- ✅ **Whisper / FireRedASR2S = Hanzi-Output, G2P nötig** — wichtiger methodischer Punkt richtig erkannt

## Wo der Plan zu optimistisch ist (4 Fehler)
1. **❌ Audio-Input-Preis bei GPT-5.5 ~10× zu niedrig.** Der Plan rechnet mit $0,006/Min (Whisper-**Transkriptions**rate). Audio in ein *multimodales LLM* zu füttern kostet als Audio-Token ein Vielfaches (~$0,04–0,06/Min äquiv.). → GPT-5.5 Audio ≈ **$20–25**, nicht $2,45
2. **❌ Wiederholter Prompt-Overhead ignoriert.** Jeder der ~11.840 Audio-Calls schickt System+User-Prompt erneut (~150 Token Input). Das sind **~1,8 Mio. Input-Token pro Modell**, die in „139.000 Output-Token" nicht auftauchen
3. **❌ „Lokal = $0" ignoriert GPU-Kosten.** Qwen-Omni, Step-Audio, Kimi, GLM, FireRed sind Open-Source — aber 5 Modelle × ~12.000 Inferenzen brauchen eine GPU. Ohne eigene Hardware: Cloud-GPU-Miete ~**$20–35**
4. **❌ Gemini-Audio ≠ $0.** Gemini zählt Audio als Token (~$2–5 realistisch, nicht $0,70 gesamt)

## Inkonsistenzen im Plan
- DeepSeek V4: Tabelle sagt „nativ multimodal inkl. Audio", finales Set sagt „KEIN AUDIO" → **widersprüchlich**, muss verifiziert werden (Stand Wissen: DeepSeek ist Text/Vision, **kein** Audio → nur RQ1)
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
(Audio→Pinyin Grok 4.2 (xAI)        GLM-4-Voice (Zhipu)
 direkt)                            Step-Audio 2 (StepFun)
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

## Folie X3 — Was kostet das wirklich? (pessimistisch)

**Titel:** Kostenschätzung — bewusst konservativ gerechnet

**Tabelle (Folie):**

| Modell | Plan sagt | Realistisch (pessimistisch) | Warum teurer |
|---|---|---|---|
| GPT-5.5 (OpenAI) | $4,54 | **$25–36** | Audio-Input ~10× höher + Prompt-Overhead |
| Grok 4.2 (xAI) | $2,83 | **$20–35** | Audio-API unbestätigt + Overhead |
| Gemini 3.1 Pro | $0,70 | **$3–6** | Audio ist nicht gratis |
| Kimi / GLM / Voxtral / Scribe | je $0,2–1,8 | **je $2–6** | Prompt-Overhead |
| Open-Source „lokal" (5 Modelle) | $0 | **$0 (eigene GPU) / $20–35 (Cloud-GPU)** | GPU-Miete falls keine Hardware |
| Claude (RQ1, Text) | $0,79 | **$0,79 ✓** | offiziell verifiziert |
| **GESAMT** | **~$24** | **~$30 (mit Sub-Sampling) bis $100+ (alles voll)** | |

**Sichtbares Hinweis-Kästchen:**
> **Budget-Grenze: 50 €.** Erreichbar — **aber nur mit zwei Maßnahmen** (nächste Folie).

**Sprechernotiz:** „Hier wird es ehrlich. Der DeepSeek-Plan landet bei knapp 24 Dollar, aber das ist zu optimistisch. Vier Dinge wurden unterschätzt: Erstens der Audio-Input-Preis bei GPT — der Plan rechnet mit der billigen Whisper-Transkriptionsrate, aber Audio in ein multimodales LLM zu geben kostet das Zehnfache. Zweitens: Jeder der fast 12.000 Audio-Aufrufe schickt den Prompt erneut mit, das summiert sich auf knapp zwei Millionen Input-Token pro Modell, die im Plan fehlen. Drittens: ‚Lokal gleich kostenlos' stimmt nur, wenn ich eine eigene GPU habe — sonst kommt Cloud-GPU-Miete dazu. Viertens ist auch Geminis Audio nicht gratis. Realistisch und pessimistisch gerechnet: Mit Sub-Sampling lande ich bei rund 30 Dollar, also unter den 50 Euro. Wenn ich aber alle Modelle auf allen 9.840 Samples laufen lasse und Cloud-GPUs miete, können es über 100 werden. Die nächste Folie zeigt, wie ich sicher unter dem Budget bleibe."

---

## Folie X4 — Wie wir unter 50 € bleiben

**Titel:** Zwei Maßnahmen halten das Budget

**Bullets:**
- **Maßnahme 1 — Premium-APIs nur auf Stichprobe.** GPT-5.5 und Grok 4.2 **nicht** auf allen 9.840 Samples, sondern auf einer **stratifizierten Teilstichprobe** (z. B. 600–800: alle 410 Silben × wenige Töne). → senkt GPT/Grok von je ~$30 auf je **~$3**
- **Maßnahme 2 — Open-Source-Modelle auf eigener Hardware.** Qwen-Omni, Step-Audio, Kimi, GLM, FireRed lokal auf vorhandener GPU laufen lassen → **$0** statt Cloud-Miete
- **Reihenfolge:** erst gratis/lokal (Whisper, Qwen, Step-Audio, FireRed), dann günstige APIs (Gemini, Voxtral), zuletzt teure (GPT, Grok) — Budget jederzeit im Blick
- **Pilot zuerst:** 400-Sample-Pilot bestätigt Pipeline, bevor das volle Geld fließt

**Sichtbares Hinweis-Kästchen:**
> Mit Sub-Sampling + eigener GPU: **geschätzt 15–30 € Gesamt** — sicher unter Budget.

**Sprechernotiz:** „Zwei einfache Maßnahmen halten das Budget. Erstens: Die teuren APIs — GPT und Grok — laufen nicht auf allen knapp 10.000 Samples, sondern auf einer durchdachten Teilstichprobe von vielleicht 700 Samples, die alle Silben und alle vier Töne abdeckt. Statistisch reicht das locker für einen Modellvergleich, und es senkt die Kosten pro Premium-Modell von rund 30 auf etwa 3 Dollar. Zweitens: Die Open-Source-Modelle laufen lokal auf eigener Hardware statt auf gemieteten Cloud-GPUs. Wichtig ist die Reihenfolge: Ich fange mit den kostenlosen und lokalen Modellen an, gehe dann zu den günstigen APIs und zünde die teuren erst zum Schluss, wenn ich das Budget genau im Blick habe. Davor steht ein Pilot mit 400 Samples, der die ganze Pipeline absichert, bevor das eigentliche Geld fließt. Damit lande ich realistisch bei 15 bis 30 Euro — sicher unter der 50-Euro-Grenze."

---

## Folie X5 — Offene Punkte vor dem Start (Verifikation)

**Titel:** Vor dem ersten Euro: 4 Dinge prüfen

**Tabelle (Folie):**

| Zu prüfen | Risiko | Aktion |
|---|---|---|
| **Grok 4.2 Audio-API** | Existiert ein Audio-Endpoint? | Vorher 1 Test-Call; sonst Step-Audio 2 als Ersatz |
| **DeepSeek V4 Audio?** | Plan widersprüchlich | Verifizieren — Stand Wissen: nur Text → nur RQ1 |
| **API-Zugang aus DE** | GLM/Kimi evtl. CN-only | Je 1 Test-Call vor Einplanung |
| **Eigene GPU verfügbar?** | sonst Cloud-Kosten | VRAM prüfen (Qwen-Omni groß) |

**Sprechernotiz:** „Bevor der erste Euro fließt, vier konkrete Checks. Erstens: Hat Grok 4.2 überhaupt eine Audio-API? Der Plan ist da selbst unsicher — ein einziger Test-Call klärt das, und falls nein, springt Step-Audio 2 ein. Zweitens: DeepSeek V4 — der Plan widerspricht sich, ob es Audio kann. Nach meinem Stand ist DeepSeek reines Text und Vision, also nur RQ1. Drittens: Bei GLM und Kimi muss ich den API-Zugang aus Deutschland mit je einem Test-Call prüfen, manche chinesischen Dienste sind China-only. Viertens: Habe ich eine GPU mit genug Speicher? Qwen-Omni ist groß. Diese vier Checks kosten nichts und verhindern böse Überraschungen mitten im Experiment."

---

# TEIL II: Anmerkungen für Stefan

- **Folien X1–X5** sind die vortragbaren Folien; **Teil 0** ist nur deine interne Prüf-Checkliste.
- **Wichtigste Botschaft an Tim:** Die Modellauswahl ist methodisch stark (drei Märkte, klare Kategorien), die **Kostenschätzung des Plans war aber zu optimistisch** — du hast das erkannt und mit Sub-Sampling + eigener GPU einen belastbaren Plan unter 50 €.
- **Verifizierte Fakten (Q&A-fest):**
  - Claude Opus 4.8 = $5/$25 pro 1M Token, Sonnet 4.6 = $3/$15 — **offiziell bestätigt**
  - Claude/DeepSeek = **kein Audio-Input** → nur RQ1
  - Whisper/FireRed/Scribe/Voxtral = **Hanzi-Output** → G2P → Baseline
- **Nicht verifizierbar (ehrlich kennzeichnen):** Alle 2026er Drittanbieter-Preise (GPT-5.5, Gemini 3.1, Grok 4.2, Qwen3.5-Omni etc.) — als Schätzung deklarieren, vor Start mit einem Test-Call + aktueller Preisliste gegenchecken.
- **Kürzungsoption:** X3 und X4 zu einer Folie zusammenziehen, wenn die Zeit knapp ist.
