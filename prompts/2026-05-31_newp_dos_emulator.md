---
date: 2026-05-31
repo: Meta_DOSHorse
status: open
resume: "verder met DOSHorse v0.0.5 — DOSHorse_Core Public API headers (include/doshorse/) + DOSHorse_X86 source-branding patches (--version-string)"
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

---

## Vervolg dezelfde sessie — P2-documentatie + v0.0.2-import

Na skeleton + sanitycheck doorgegaan met "beide" (P2 + B) op verzoek gebruiker.

### P2 — Documentatie (Meta_DOSHorse v0.0.1+, geen version-bump)

- `docs/PRINCIPLES.md` — 8 principes P-DSH-01..08 (upstream-first, Y-pattern via Core-submodule, AGPL or-later-keten, save-state portabiliteit, codename single-source, multi-input first, per-file license-audit, geen ROM/OS in git met FreeDOS-fallback)
- `docs/UPSTREAM_AUDIT.md` — License-chain bewijslast + audit-baseline-template voor v0.0.2
- `docs/DEPENDENCIES.md` — Cross-repo Y-pattern + upstream-tabel + per-target deps + impact-matrix

Sanitycheck-conformiteit: ~88% → ~95%.

### v0.0.2-import (DOSHorse_Core 0.0.1-Eggebrecht → 0.0.2-Dean)

**Beslispunt O1 beantwoord:** git submodule (optie 1), past bij P-DSH-01 + P-DSH-07 + AmigaHorse-precedent.

**Acties:**
- `git submodule add --depth 1 https://github.com/joncampbell123/dosbox-x.git upstream/dosbox-x` (in DOSHorse_Core)
- Pinned commit: `4a95241b33b11537eef61a883b330478017cca3c` (master tip 2026-05-31, ~359 MB working tree, ~7720 files, ~4463 source)
- **License-audit baseline:** GPL-2.0-or-later geverifieerd via 3 bronnen (COPYING + README + `src/dosbox.cpp`). 10 vendored sub-libs geïnventariseerd. **Geen blocking conflicts.** 2 follow-ups (FluidSynth/MUNT lokale LICENSE-files) naar v0.0.3.
- `UPSTREAM_AUDIT.md` + `DEPENDENCIES.md` bijgewerkt met audit-baseline + pinned commit
- `Meta_DOSHorse/CLAUDE.md` codename-pool: Dean (Mark Dean, ISA-bus co-inventor) toegewezen aan Core v0.0.2

**Niet uitgevoerd (gepland v0.0.3):**
- CMake smoke-test build op Mac arm64
- Public API headers in `include/doshorse/` (v0.0.4)
- Per-file SPDX-spider tool (`tools/license-audit.py`)
- FluidSynth + MUNT lokale LICENSE-file follow-up

### Resume-trigger gewijzigd

Oud: "verder met DOSHorse v0.0.2-import — dosbox-x als submodule in DOSHorse_Core + eerste X86-build"
Nieuw: "verder met DOSHorse v0.0.3 — CMake smoke-test build op Mac (arm64) + eerste DOSHorse_X86 native binary"

---

## Vervolg dezelfde sessie — v0.0.3 build smoke-test (2026-06-01, "verder")

Datum is omgegaan — newp + v0.0.2 op 2026-05-31, build-werk op 2026-06-01 in dezelfde Claude-sessie.

### v0.0.3 — Build smoke-test op host-Mac

**WhatIf-akkoord:** `ja` op brew install + build (Intel Mac, x86_64, macOS 26.3).

**Uitgevoerd:**
- `brew install` voor 8 ontbrekende dosbox-x deps (automake, nasm, glfw, fluid-synth, libslirp, libpcap, pkg-config, sdl2_net)
- `./build-macos-sdl2` in upstream/dosbox-x — **GESLAAGD in ~9.5 min** (00:16:37 → 00:25:55)
- Binary: `upstream/dosbox-x/src/dosbox-x`, 22 MB, x86_64-only
- Smoke-run: `./src/dosbox-x --version` → "DOSBox-X version 2026.05.02 SDL2" ✅
- BUILD_LOG.md aangemaakt in `Meta_DOSHorse/docs/` met host-specs, build-keten, waarschuwingen, architectuur-noot

**Bewijslast voor principes:**
- ✅ P-DSH-02 (Y-pattern Core-submodule) — submodule add → buildbaar zonder Core-source-modificatie
- ✅ P-DSH-03 (AGPL-3.0 via or-later) — werkende GPL-2.0+ binary onder onze AGPL-3.0-keten is licentie-compatibel in praktijk

**Niet gecommit (terecht):** Build-output binnen submodule (binary + `.o`-files in `vs/sdl/linux-host/`). Core-repo's submodule-pointer onveranderd (still `4a95241b`).

**Niet uitgevoerd (v0.0.4+):**
- Eigen DOSHorse_X86 build-wrapper + branding
- Universal binary (vereist Apple Silicon)
- Public API headers (v0.0.5)
- Per-file SPDX-spider tool
- Run-test met echte DOS-image (alleen `--version` getest)

### Resume-trigger opnieuw bijgewerkt

Oud (v0.0.3 trigger): "verder met DOSHorse v0.0.3 — CMake smoke-test build op Mac (arm64) + eerste DOSHorse_X86 native binary"
Nieuw (v0.0.4 trigger): "verder met DOSHorse v0.0.4 — DOSHorse_X86 eigen build-wrapper (Makefile/CMakeLists.txt rond Core-submodule) + eigen branding op binary"

---

## Vervolg dezelfde sessie — v0.0.4 DOSHorse_X86 wrapper (2026-06-01, "verder")

### v0.0.4 — Eerste werkende build-wrapper (A: wrapper-only)

**WhatIf-akkoord:** `j` op A+Makefile.

**Uitgevoerd:**
- `git submodule add --depth 1` DOSHorse_Core → `core/` in DOSHorse_X86
- `git submodule update --init --recursive --depth 1` (drie-niveau keten: X86 → Core → dosbox-x@4a95241b)
- `DOSHorse_X86/Makefile` (75 regels) met targets: `deps-check`, `build`, `install`, `smoke`, `clean`, `clean-build`, `version`, `help`
- `DOSHorse_X86/BUILD.md` met quick-start + platform-status-tabel + branding-noot + architectuur-noot
- `.gitignore` uitgebreid voor `dist/`
- Lightweight Makefile-targets getest: `help`, `version`, `deps-check` (10 deps ✓)
- **Full smoke-test #2 end-to-end**: `make build && install && smoke` ≈ 10m 04s, binary 22 MB werkend, `--version` output OK ✅
- `BUILD_LOG.md` v0.0.4-sectie + Meta_DOSHorse/CLAUDE.md codename-pool (Sams toegewezen aan X86 v0.0.2)
- Meta_Master/PROJECTS.json: DOSHorse_X86 0.0.1-Bradley → 0.0.2-Sams

**Commits:**
- DOSHorse_X86 `4bfe917` — Makefile + BUILD.md + submodule-keten + CHANGELOG/VERSION-bump
- Meta_DOSHorse `5d85a41` — codename Sams toegewezen
- Meta_Master `2ce298f` — PROJECTS.json X86-bump

**Niet uitgevoerd (v0.0.5+):**
- Eigen source-branding patches (`--version` rapporteert nog upstream-string)
- Linux/Windows build-support in Makefile
- Public API headers `include/doshorse/`
- Universal binary (vereist Apple Silicon host)
- Run-test met echte DOS-image

### Resume-trigger weer bijgewerkt

Oud (v0.0.4 trigger): "verder met DOSHorse v0.0.4 — DOSHorse_X86 eigen build-wrapper (Makefile/CMakeLists.txt rond Core-submodule) + eigen branding op binary"
Nieuw (v0.0.5 trigger): "verder met DOSHorse v0.0.5 — DOSHorse_Core Public API headers (include/doshorse/) + DOSHorse_X86 source-branding patches (--version-string)"
