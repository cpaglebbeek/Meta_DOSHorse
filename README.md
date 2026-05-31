# Meta_DOSHorse

Master/orchestratie-repo voor het **DOSHorse** ecosysteem: een open-source DOS-emulator op vijf targets, gebaseerd op de [dosbox-x](https://github.com/joncampbell123/dosbox-x) codebase (Jonathan Campbell e.a.) als hard fork.

> **Status:** v0.0.1-Estridge skeleton (newp 2026-05-31). Geen runnable code in deze fase — alleen architectuur, principes, dependencies en sessie-vastlegging.

## Targets

In ontwikkelvolgorde:

1. **X86** — Native Linux + Windows + macOS desktop (eerste landingsplek voor dosbox-x fork-import)
2. **Web** — WebAssembly via Emscripten, browser-based, geen install
3. **Android** — NDK-build, Compose-UI, gamepad+touch
4. **SteamDeck** — SteamOS Flatpak, Steam Input preset

(Volgorde verschilt bewust van AmigaHorse omdat dosbox-x al een werkende X86-codebase heeft; Web-port is écht nieuw werk.)

## Ecosysteem (6 repos, allemaal PUBLIC, AGPL-3.0, branch `main`)

| Repo | Rol | Codenaam v0.0.1 |
|------|-----|------------------|
| **Meta_DOSHorse** | Master/docs (deze repo) | Estridge |
| [DOSHorse_Core](https://github.com/cpaglebbeek/DOSHorse_Core) | Gedeelde emulator-core (dosbox-x fork + abstractie-laag, submodule in 4 varianten) | Eggebrecht |
| [DOSHorse_X86](https://github.com/cpaglebbeek/DOSHorse_X86) | Native desktop Linux/Win/macOS | Bradley |
| [DOSHorse_Web](https://github.com/cpaglebbeek/DOSHorse_Web) | WASM-variant (Emscripten + web-UI) | Faggin |
| [DOSHorse_Android](https://github.com/cpaglebbeek/DOSHorse_Android) | Android NDK + Compose | Hoff |
| [DOSHorse_SteamDeck](https://github.com/cpaglebbeek/DOSHorse_SteamDeck) | SteamOS Flatpak | Lowe |

## Codenaam-thema

**IBM-PC hardware-pioniers** — mensen die het IBM-PC platform en de x86-architectuur hebben vormgegeven. Single source of truth voor de pool: zie `CLAUDE.md` in deze repo (drift-preventie tegen dupliceren in sub-repos).

## Upstream

[joncampbell123/dosbox-x](https://github.com/joncampbell123/dosbox-x) — GPL-2.0-or-later. Onze fork distribueert onder **AGPL-3.0** via de "or-later" upgrade-route (GPL-2.0+ → GPL-3.0 → AGPL-3.0 via §13 compatibility), consistent met de rest van het Gaming-ecosysteem (SteamDeckMSX, AmigaHorse).

## Status

| Repo | Versie | Skeleton | Code-import | Eerste runnable |
|------|--------|----------|--------------|------------------|
| Meta_DOSHorse | 0.0.1-Estridge | ✓ | n.v.t. | n.v.t. |
| DOSHorse_Core | 0.0.1-Eggebrecht | ✓ | open | open |
| DOSHorse_X86 | 0.0.1-Bradley | ✓ | open | open |
| DOSHorse_Web | 0.0.1-Faggin | ✓ | open | open |
| DOSHorse_Android | 0.0.1-Hoff | ✓ | open | open |
| DOSHorse_SteamDeck | 0.0.1-Lowe | ✓ | open | open |

## Deploy

Hosting op `horsecloud55.ddns.net/DOSHorse/{web,downloads/x86,downloads/android,downloads/steamdeck}/`. Nginx-subpad en poorten worden geclaimd in `SHARED_INFRA_PATCH.md` zodra een variant runnable wordt — niet bij skeleton-aanmaak.

## Verwijzingen

- `ARCHITECTURE.md` — cross-platform component-diagram + relaties + Core-as-submodule patroon
- `CLAUDE.md` — codename-pool + ecosysteem-regels
- `prompts/2026-05-31_newp_dos_emulator.md` — newp-sessie integraal
- `Meta_Master/PROJECTS.json` — bron van waarheid voor repo-registratie
- `Meta_Master/ECOSYSTEMS.md` — Gaming-ecosysteem-tabel
