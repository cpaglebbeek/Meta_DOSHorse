---
date: 2026-05-31
repo: Meta_DOSHorse
status: open
resume: "verder met DOSHorse v0.0.2-import — dosbox-x als submodule in DOSHorse_Core + eerste X86-build (Bradley → +0.0.1)"
session: newp DOS Emulator
agent: Claude Opus 4.7 (1M context)
---

# Newp-sessie DOS Emulator → DOSHorse (2026-05-31)

## Trigger-prompt

> `newp "DOS Emulator". Web,X86,Android. source code: https://github.com/joncampbell123/dosbox-x`

## Verloop

### Fase 0 — /verifyrules
Skill `/verifyrules` aangeroepen vóór WhatIf. Naleefmatrix opgesteld:
- Meta_Master pull ✓ ("Already up to date")
- 48 feedback-memory files geïnventariseerd
- Hooks gecheckt: SessionStart + Stop + UserPromptSubmit + PostToolUse actief
- Score: 4/13 nageleefd, 8/13 in flight, 1/13 aandachtspunt (multi-session bij repo-aanmaak), 0 schendingen

### Fase 1 — WhatIf beslispunten (één-voor-één §6b)

| # | Punt | Keuze | Toelichting |
|---|------|-------|-------------|
| 1 | Scope | A — hard fork dosbox-x + ports voor Web/X86/Android | Geen wrapper, geen submodule-only |
| 2 | Ecosysteem | 1 — sub-master onder Gaming | Zus van AmigaHorse + SteamDeckMSX |
| 3 | Repo-namen + thema | DOSEmulator_* + IBM-PC hardware-pioniers | (later herzien) |
| 4 | Licentie | 1 — PUBLIC GPL-2.0-or-later, README-attributie, geen courtesy-notice | (later herzien naar AGPL-3.0) |
| 5 | Deploy | 3 — HorseCloud55 DDNS | `horsecloud55.ddns.net/DOSHorse/` |

### Fase 1.5 — Meta-impact STOP

Tijdens repo-skeleton-aanmaak ontdekt: **AmigaHorse** bestaat al in Gaming-ecosysteem als gelijksoortig project (multi-platform emulator op GPL-2.0+ upstream). Sessie gestopt om eerdere beslissingen te herzien tegen het AmigaHorse-precedent. Gebruiker akkoord met "jouw voorstellen" → optie X (volledig AmigaHorse-conform).

**Herziene beslissingen:**
- Naam: `DOSEmulator` → **`DOSHorse`** (iCt-Horse-merkstijl, consistent met AmigaHorse)
- Repo-structuur: 4 repos → **6 repos** (incl. `_Core` shared submodule + `_SteamDeck` 4e variant)
- Licentie: GPL-2.0-or-later → **AGPL-3.0** (via or-later upgrade-keten, consistent met Gaming-ecosysteem)

### Fase 2-4 — Plan + Impact + Akkoord
Plan voorgelegd met expliciete impactanalyse (Meta_Master 6 nieuwe repo-entries, GitHub 6 PUBLIC repos, HC55 nginx fase-2-impact, MEMORY.md grootte als aandachtspunt). Akkoord gegeven.

### Fase 5 — Skeleton-uitvoering

Stappen voltooid:
1. 6 lokale dirs aangemaakt in `/Users/christian/Documents/Gemini_Projects/`
2. LICENSE (AGPL-3.0) gekopieerd vanuit Meta_AmigaHorse naar alle 6
3. VERSION-files met v0.0.1-codenames (Estridge/Eggebrecht/Bradley/Faggin/Hoff/Lowe)
4. README + CLAUDE + ARCHITECTURE + CHANGELOG per repo
5. Meta_DOSHorse `prompts/2026-05-31_newp_dos_emulator.md` (deze sessie)
6. Meta_Master updates: PROJECTS.json + ECOSYSTEMS.md + STATUS.md + claude_memory/
7. Claude memory: `project_doshorse_ecosystem.md` + MEMORY.md pointer
8. GitHub: 6 PUBLIC repos aangemaakt + initial push
9. Skill `/sanitycheck` (fase N+1)

## Beslissingen-samenvatting (definitief)

| Aspect | Waarde |
|--------|--------|
| Project-naam | DOSHorse |
| Ecosysteem | Gaming (zus van AmigaHorse + SteamDeckMSX) |
| Sub-master | Meta_DOSHorse |
| Platform-repos | DOSHorse_Core, DOSHorse_X86, DOSHorse_Web, DOSHorse_Android, DOSHorse_SteamDeck |
| Licentie | AGPL-3.0 |
| Visibility | PUBLIC |
| Upstream | joncampbell123/dosbox-x (GPL-2.0-or-later) |
| Codename-thema | IBM-PC hardware-pioniers |
| Deploy-target | horsecloud55.ddns.net/DOSHorse/ |
| Ontwikkelvolgorde | X86 → Core → Web → Android → SteamDeck |

## Open beslispunten (volgende sessie)
Zie `ARCHITECTURE.md` §Open beslispunten (O1..O6).
