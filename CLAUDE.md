# CLAUDE.md — Meta_DOSHorse

> Master/orchestratie-repo voor het DOSHorse-ecosysteem (6 repo's). Verwijst naar `Meta_Master/CLAUDE.md` voor alle globale regels (WhatIf, ZSH, sessie-protocol, over-en-uit).

## Rol van deze repo

Deze repo bevat **geen runnable code**. Alleen:
- Ecosysteem-overstijgende architectuur (`ARCHITECTURE.md`)
- Principes (`docs/PRINCIPLES.md`) — P-DSH-01..N (op te bouwen vanaf v0.0.2)
- Cross-repo afhankelijkheden (`docs/DEPENDENCIES.md`) — op te bouwen vanaf v0.0.2
- Sessie-MD's (`prompts/`)

Wijzigingen aan code-componenten gaan ALTIJD in de sub-repo (`DOSHorse_Core`, `DOSHorse_X86`, `DOSHorse_Web`, `DOSHorse_Android`, `DOSHorse_SteamDeck`).

## Ontwikkelprioriteit (door gebruiker bepaald 2026-05-31)

1. **DOSHorse_X86** — prio 1 (dosbox-x compileert al native; eerste runnable build is "import dosbox-x in Core, build voor X86")
2. **DOSHorse_Core** — uitgesplitst zodra X86 runnable is en Web een gedeelde laag nodig heeft
3. **DOSHorse_Web** — Emscripten-port (echt port-werk, nieuwe build-config)
4. **DOSHorse_Android** — NDK + Compose UI (vergt nieuwe input-laag)
5. **DOSHorse_SteamDeck** — laatste (Flatpak van X86-build is laagdrempelig)

## Codenaam-thema: IBM-PC hardware-pioniers

> **Single source of truth voor de hele ecosysteem.** Sub-repos verwijzen hierheen — niet zelf de pool dupliceren (drift-risico, zie AmigaHorse-precedent).

Elke release krijgt een unieke naam van een IBM-PC- of x86-hardware-pionier als codenaam.

### Toegewezen (gebruikte namen)

| Repo | Versie | Codenaam | Pionier |
|------|--------|----------|---------|
| Meta_DOSHorse | v0.0.1 | Estridge | Don Estridge, vader IBM PC (Boca Raton lead) |
| DOSHorse_Core | v0.0.1 | Eggebrecht | Lewis Eggebrecht, chief architect IBM PC 5150 motherboard |
| DOSHorse_Core | v0.0.2 | Dean | Mark Dean, ISA-bus co-inventor (IBM PC AT) — Core verbindt componenten, ISA-bus verbindt hardware |
| DOSHorse_Core | v0.0.3 | Canion | Rod Canion, Compaq founder (1982) — pioneer PC-compatible clean-room fork-industrie, past bij Core's eerste eigen source-patches op upstream |
| DOSHorse_Core | v0.0.4 | Mazor | Stanley Mazor, Intel 4004 co-architect (instruction-set spec) — eerste C-impl van Public API = architecturale blueprint vastleggen |
| DOSHorse_X86 | v0.0.1 | Bradley | David Bradley, IBM PC BIOS-schrijver + Ctrl-Alt-Del |
| DOSHorse_X86 | v0.0.2 | Sams | Phil Sams (Phoenix Technologies), clean-room BIOS-implementatie — past bij wrapper-only build-approach (bouwen bovenop upstream zonder source-modificaties) |
| DOSHorse_X86 | v0.0.3 | Noyce | Robert Noyce (Intel co-founder, 1968) — co-uitvinder geïntegreerd circuit + Intel-founder, x86-fundament; past bij eerste echt-gebrand werkende DOSHorse-binary op x86 |
| DOSHorse_X86 | v0.0.4 | Felsenstein | Lee Felsenstein, Osborne 1-ontwerper (1981, eerste commerciële portable) — eerste cross-platform Makefile (macOS bewezen + Linux/Win-paden) |
| DOSHorse_Web | v0.0.1 | Faggin | Federico Faggin, 8080/Z80 chip-design pioneer |
| DOSHorse_Web | v0.0.2 | Allen | Paul Allen (Microsoft co-founder 1975) — pionier in software-porting naar nieuwe platforms (BASIC → Altair 8800); eerste WASM-port van DOSHorse-stack |
| DOSHorse_Android | v0.0.1 | Hoff | Marcian "Ted" Hoff, Intel 4004 designer |
| DOSHorse_SteamDeck | v0.0.1 | Lowe | Bill Lowe, pitched IBM PC project to IBM management |

### Vrije pool (IBM-PC/x86-pioniers, willekeurige volgorde)
 Moore (Gordon Moore), Grove (Andy Grove), Patterson (Tim Paterson, 86-DOS/QDOS), Gates (Bill Gates), Kildall (Gary Kildall, CP/M voorloper), Manning, Vaden, Bushnell (Nolan Bushnell, parallel pioneer), Brockenbrough, Sutherland (Ivan Sutherland, graphics), Roberts (Ed Roberts, Altair 8800 pioneer x86-voorloper), Kapor (Mitch Kapor, Lotus 1-2-3), Ratliff (Wayne Ratliff, dBase II), Bricklin (Dan Bricklin, VisiCalc), Frankston (Bob Frankston, VisiCalc), Bastian (Bruce Bastian, WordPerfect), Norton (Peter Norton, Norton Utilities/DOS-tools).

### Rotatie-regels

1. Bij toewijzen van een nieuwe codenaam: schuif uit "Vrije pool", plaats in "Toegewezen"-tabel met repo + versie + korte pionier-beschrijving.
2. Geen codenaam mag in meer dan één repo voorkomen.
3. Bij twijfel: kies een pionier wiens werk past bij de scope van de release (CPU-pionier voor CPU-werk, BIOS-pionier voor low-level, software-pionier voor frontend, etc.).

## Globale regels (verwijzing)

Alle globale regels (WhatIf, ZSH-safety, sessie-protocol, over-en-uit, post-response statusblok, één-vraag-per-response) gelden onverkort uit `Meta_Master/CLAUDE.md`.

## Upstream-relatie

Upstream is [joncampbell123/dosbox-x](https://github.com/joncampbell123/dosbox-x) (GPL-2.0-or-later). DOSHorse is een hard fork — geen courtesy-notice naar upstream verzonden (newp-besluit beslispunt 4). Wel: README en LICENSE bevatten correcte attributie.
