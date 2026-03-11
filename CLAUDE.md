# CLAUDE.md – Project Instructions & Workflow

## Project Overview

This repo is a research collection for the **Slovenian parliamentary elections on 22 March 2026**. It contains:
- Official party program documents (PDF or scraped MD)
- Objective, impartial summaries of each program
- Thematic analyses (women's rights, polling data, SDS voter support)
- An executive summary across all parties

**Core principle:** All summaries and analyses must be **completely impartial and objective**. The purpose is to inform a voter, not to influence their decision. Never editorialize, compare parties favorably/unfavorably, or express value judgments.

---

## Agent Usage Pattern

This project established a clear pattern for agent tasks:

- **Haiku agent** → web scraping, downloading files, data gathering, file operations
- **Sonnet/Opus agent** → writing summaries, analyses, and any text that requires judgment and quality

Always use subagents to keep the main context window clear. Run independent tasks in parallel (multiple agents in a single message).

---

## Directory & File Conventions

```
programi/[ABBREVIATION]/
    program.pdf      # if PDF was available
    program.md       # if web-scraped (no PDF) or wiki content
    povzetek.md      # objective summary, written in Slovenian, 400-600 words
```

**Root-level analysis files:**
- `povzetek_volitve_2026.md` — executive summary of all parties
- `ankete_projekcije_2026.md` — polls and seat projections
- `zenske_pravice_2026.md` — women's rights / reproductive rights / sexual violence by party
- `sds_podpora_analiza_2026.md` — why voters support SDS/Janša

**povzetek.md format:**
```markdown
# [Full Party Name] – Povzetek volilnega programa 2026

## O stranki
## Ključna področja in cilji
### [Področje]
- [cilj/ukrep]
## Glavne vrednote in prioritete

---
*Vir: [source]*
*Povzetek pripravljen na podlagi volilnega programa stranke.*
```

---

## Parties Covered

| Folder | Stranka | Program format | Notes |
|---|---|---|---|
| `GS` | Gibanje Svoboda | MD | No 2026 PDF; web scrape of 2024 program |
| `SDS` | Slovenska demokratska stranka | PDF | Feb 2026 |
| `SD` | Socialni demokrati | PDF | Feb 2026 |
| `NSi` | Nova Slovenija | PDF | Joint list NSi+SLS+Fokus, Feb 2026 |
| `SLS` | Slovenska ljudska stranka | PDF | **Same PDF as NSi** – joint list |
| `Levica` | Levica | PDF | Feb 2026, joint list with Vesna; file is `program_2026.pdf` not `program.pdf` |
| `Resnica` | Resni.ca | PDF | Mar 2026 |
| `Pirati` | Piratska stranka | MD | Scraped from wiki, 27 sections combined |
| `SNS` | Slovenska nacionalna stranka | MD | Scraped from sns.si |
| `GOD` | Glas za otroke in družine | MD | Scraped from glaszaotrokeindruzine.si |
| `MiSocialisti` | Mi, socialisti! | MD | Scraped from misocialisti.si |
| `Vesna` | Vesna – zelena stranka | MD | **Website blocks automated access (HTTP 403)** – file contains only a note |

**Defunct/no program parties** (folders removed): NPS (dormant since 2016), Solidarnost (domain parked), Sloga (unreachable)

---

## Key Sources

- **Ženski lobi Slovenije (ŽLS):** https://www.zenskilobi.si — published party analysis on 10.3.2026
  - Full article: `/novice/zenski-lobi-slovenije-je-pripravil-analizo-vpogled-v-kandidatne-liste-in-programe-parlamentarnih-strank-z-vidika-enakosti-spolov-volitve-2026.html`
- **Institut 8. marca:** Published manifest "Ne boste nam vzeli prihodnosti" on 8.3.2026 — sent to 9 parties
- **Volilna napoved:** https://volilna-napoved.si — seat projections, updated frequently
- **Wikipedia SL polls:** https://sl.wikipedia.org/wiki/Javnomnenjske_raziskave_za_državnozborske_volitve_v_Sloveniji_2026
- **RTV Slovenija:** https://www.rtvslo.si — most reliable general news source
- **Pollsters:** Mediana (most reliable), Ninamedia, Valicon, RC IJEK, Parsifal (note: Parsifal/Nova24TV skews higher for SDS)

---

## Workflow for Adding a New Party

1. Find official website and program document
2. If PDF: `wget`/`curl` to `programi/[ABBR]/program.pdf`
3. If web only: scrape full text to `programi/[ABBR]/program.md`, include source URL at bottom
4. Write `povzetek.md` using **Sonnet/Opus agent** — read the program, write 400-600 word summary in Slovenian
5. Update `povzetek_volitve_2026.md` — add party to appropriate group and all thematic tables
6. Update `zenske_pravice_2026.md` — search program for relevant terms, add party section and table row
7. Commit and push

---

## Workflow for Adding a New Analysis Topic

1. Use **Haiku agent** to search party programs for relevant keywords
2. Use **Haiku agent** for web research (recent sources only, confirm dates)
3. Use **Sonnet/Opus agent** to write the analysis file — objective, sourced, no value judgments
4. Save to root as `[topic]_2026.md`
5. Update README.md main files table
6. Commit and push

---

## Things to Watch For in Future Sessions

- **Vesna program:** The website (vesna.si) has been blocking automated access. Worth retrying periodically — they are an active party and their program should exist.
- **GS program:** Only a 2024 document was available. If Gibanje Svoboda publishes a proper 2026 election PDF, replace `programi/GS/program.md` with it.
- **Post-election updates:** After 22.3.2026, this repo could be extended with actual results, coalition negotiations, and government formation.
- **Polling data expires quickly** — `ankete_projekcije_2026.md` reflects state as of ~11 March 2026. Update if more recent polls are published before election day.
- **NSi and SLS share a PDF** — any analysis should note this. Their joint list is called "Skupaj v akcijo", led by Jernej Vrtovec (NSi).
- **Levica's program file** is named `program_2026.pdf` (not `program.pdf`) — keep consistent if updating.

---

## Electoral System Notes

- 88 seats elected by proportional representation (D'Hondt formula across 8 electoral units)
- 2 seats reserved for ethnic minorities (Italian, Hungarian)
- Electoral threshold: **4%** of total votes
- Majority needed to form government: **46 seats**
- Constitutional majority (for constitutional amendments): **60 seats**
