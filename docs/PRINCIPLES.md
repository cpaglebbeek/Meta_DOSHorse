# Principes — DOSHorse

Ontwerpprincipes die voor alle 6 repo's gelden. Dragon1-structuur: **statement → rationale → mechanisme → implicaties**.

P-DSH-01 t/m 08 vastgesteld v0.0.1-Estridge (2026-05-31). Verdere principes worden toegevoegd bij majeure beslispunten.

---

## P-DSH-01 — Upstream first

**Statement:** Bug-fixes en cross-platform-verbeteringen die niet DOSHorse-specifiek zijn, gaan eerst als PR naar **upstream [joncampbell123/dosbox-x](https://github.com/joncampbell123/dosbox-x)**. Pas als upstream afwijst of niet reageert binnen 30 dagen, houden we een private patch in `DOSHorse_Core/patches/`.

**Rationale:** dosbox-x is een 10+ jaar volwassen project met actieve maintainer (Jonathan Campbell). Forks die uit elkaar groeien zonder upstream-synchronisatie sterven binnen 2 jaar (zie historie van vele DOSBox-forks: DOSBox SVN, DOSBox ECE, DOSBox Staging, DOSBox Pure — allemaal divergeren). Door upstream-first blijft onze platform-laag dun en hervatbaar bij upstream-updates.

**Mechanisme:** Per commit aan vendored dosbox-x-files evalueren: "kan dit naar upstream?". Zo ja → PR + wachten. Zo nee → patch in `DOSHorse_Core/patches/<feature>.patch` + comment waarom upstream het niet wil (link naar PR-discussie of "DOSHorse-specifiek").

**Implicaties:**
- Platform-specifieke UI / WASM-bindings / Android-input / Steam-Input zaken komen in **onze laag**, niet in upstream
- Geen breaking-changes aan dosbox-x' Public API; alles backwards-compatible
- Bij `newp`-beslispunt 4 (geen courtesy-notice) bewust gekozen om geen upfront-contact te leggen — wel actief PR-en bij echte bug-fix

---

## P-DSH-02 — Y-pattern via Core-submodule

**Statement:** `DOSHorse_Core` is **de enige plek** waar dosbox-x-code (+ patches) leeft. De 4 platform-repos consumeren Core via git-submodule (tag-gebaseerd, geen floating main).

**Rationale:** Drie hard forks per platform = duplicatie + drift. Een gedeelde Core voorkomt dat een fix in Web nooit Android bereikt. Tag-gebaseerd voorkomt mid-flight Core-API-breekstoten.

**Mechanisme:** Elke platform-repo heeft `core/` als submodule met `git submodule update --recommend-shallow --depth=1` voor compacte clone. Bump-discipline: per platform-repo eigen `core/` commit-pin in `git submodule status`. PR-merge in `_Core` triggert tag → 4 platform-repos kunnen submodule-bump-PRs maken.

**Implicaties:**
- Geen dosbox-x-bestanden duplicaten buiten `_Core`
- Geen "hot fix in Web" zonder eerst Core-fix + tag-bump
- Submodule-add gebeurt in v0.0.2-import (vóór eerste platform-build)

---

## P-DSH-03 — AGPL-3.0 via or-later upgrade-keten

**Statement:** Alle 6 repo's distribueren onder **AGPL-3.0** — gerechtvaardigd via de "or-later"-clausule in dosbox-x' GPL-2.0-or-later-LICENSE.

**Rationale:** Consistentie met Gaming-ecosysteem (SteamDeckMSX en AmigaHorse: beide AGPL-3.0). AGPL voegt §13 toe (netwerk-distributie source-availability) die voor `DOSHorse_Web` deploy-target essentieel is: een browser-gebruiker is functioneel een gebruiker over het netwerk → AGPL beschermt sourcebeschikbaarheid bij SaaS-misbruik.

**Mechanisme:** Upgrade-keten bewezen: **GPL-2.0-or-later → GPL-3.0** (toegestaan via "or later"-clausule in GPL-2.0-LICENSE) **→ AGPL-3.0** (toegestaan via §13 van GPL-3.0 die expliciet AGPL-3.0-combinatie permitteert). Bewijslast: zie `UPSTREAM_AUDIT.md` §License-chain.

**Implicaties:**
- README + LICENSE in elke repo bevatten correcte attributie naar joncampbell123/dosbox-x als upstream
- Geen permissive sub-deps (MIT/BSD/Zlib) zijn problematisch (combined work mag, mits header behouden)
- GPL-2.0-**only** deps (zonder "or later") zouden blokkerend zijn — checken bij elke upstream-bump per `UPSTREAM_AUDIT.md`

---

## P-DSH-04 — Save-state cross-platform portabiliteit

**Statement:** Een save-state gemaakt in DOSHorse_Web moet open kunnen worden in DOSHorse_X86 en vice versa. Het portable formaat (`.dhs`) is **API-contract** tussen alle 4 platform-repos.

**Rationale:** Een gebruiker speelt onderweg op Android, savet, gaat naar X86-desktop, verwacht door te kunnen spelen. Zonder cross-platform-savegame is het ecosysteem 4 losse emulators in plaats van één DOSHorse.

**Mechanisme:** `.dhs` is een ZIP-container (of CBOR — open beslispunt C3) met:
1. dosbox-x' interne raw-state (binary, versie-gepind)
2. DOSHorse-metadata: machine-config (CPU-type, video-mode, audio-cards), DOS-versie, gemounte volumes
3. Optioneel: thumbnail van laatste frame (PNG, 320×200 max)

Versie-veld in metadata; bij format-bump migration-path documenteren in `Meta_DOSHorse/docs/SAVESTATE_FORMAT.md` (te maken v0.0.2).

**Implicaties:**
- Save-state-API moet in `DOSHorse_Core/include/doshorse/savestate.h` — geen platform-specifieke serialisatie
- Web kan IndexedDB voor `.dhs`-blobs; X86 native filesystem; Android SAF; SteamDeck Flatpak-data-dir
- Backwards-compat van `.dhs` blijft bewaard tussen minor-versies; major-version-bump = migration-tool

---

## P-DSH-05 — Codenaam-pool single source of truth

**Statement:** De codenaam-pool (IBM-PC hardware-pioniers) leeft **uitsluitend** in `Meta_DOSHorse/CLAUDE.md`. Sub-repos refereren, dupliceren niet.

**Rationale:** AmigaHorse-precedent: bij twee bronnen ontstaat drift (Web release v0.0.2 met codenaam X, Core release v0.0.5 met codenaam X = conflict). Eén bron = nul drift.

**Mechanisme:** Sub-repo CLAUDE.md bevat alleen "v0.0.x = <naam>" voor eigen versies — niet de hele pool. Bij volgende codenaam-toewijzing: schuif uit pool in `Meta_DOSHorse/CLAUDE.md`, voeg toe aan "Toegewezen"-tabel, commit.

**Implicaties:**
- Code-reviews bij sub-repo CHANGELOG-update kijken niet naar uniciteit — alleen Meta_DOSHorse is autoriteit
- Bij grote pool-uitputting: nieuwe IBM-PC-pionier-namen toevoegen aan pool in Meta (niet ad-hoc in sub-repo)

---

## P-DSH-06 — Multi-input first (toetsenbord + muis + gamepad + touch)

**Statement:** Elke DOSHorse-variant ondersteunt **alle vier** input-modaliteiten: toetsenbord, muis, gamepad, touch. UI is bedienbaar via elk daarvan op zichzelf.

**Rationale:** DOS-applicaties zijn extreem divers. Tekstverwerker = toetsenbord. Doom = WASD+muis. Adventure-game = muis. Pinball Fantasies op SteamDeck = gamepad. Mobile gebruiker = touch-overlay met virtuele toetsenbord. Een UI die één van deze uitsluit (zoals "alleen gamepad" of "alleen muis") faalt op een hele klasse software.

**Mechanisme:** Input-abstractie in `DOSHorse_Core` met **alle 4** als first-class. Platform-implementaties bepalen welke beschikbaar zijn, niet welke verplicht. UI-controls hebben focus-navigatie (D-pad/Tab/click/tap allemaal werkend). On-screen-keyboard verplicht op Web-mobile en Android.

**Implicaties:**
- Geen modale "kies je input"-popup
- Geen menus die ≥7 items per niveau hebben zonder D-pad-pageering
- Touch-overlay configureerbaar per game (preset of custom — Android beslispunt A3)

---

## P-DSH-07 — Per-file licentie-audit bij upstream-bumps

**Statement:** Bij elke `git submodule update` of `git pull upstream/master` op vendored dosbox-x: per-file-licentie-audit op nieuwe/gewijzigde files. Bewijslast in `UPSTREAM_AUDIT.md`.

**Rationale:** dosbox-x vendort vele sub-components (SDL2, FluidSynth, libpng, MUNT, etc.) met verschillende licenties. Een upstream-PR die een GPL-2.0-only file toevoegt zou onze AGPL-3.0-keten breken (zie P-DSH-03).

**Mechanisme:** Audit-script (te maken v0.0.2) loopt over diff:
1. Voor elke nieuwe `.cpp/.h` zoek SPDX-header of `License`/`Copyright` regels
2. Match tegen whitelist (GPL-2.0-or-later, GPL-3.0-or-later, MIT, BSD-2/3-Clause, Zlib, MPL-2.0, LGPL-2.1-or-later)
3. Bij onbekende licentie of GPL-2.0-only: **blokkeer merge**, mark in `UPSTREAM_AUDIT.md`

**Implicaties:**
- Geen "blind merge" van upstream — zelfs voor kleine updates
- Baseline-audit van huidige dosbox-x master in v0.0.2-import is **eerste verplichting**

---

## P-DSH-08 — Geen ROM/OS-binaries in git

**Statement:** Géén MS-DOS-binaries, geen IBM PC BIOS-ROM, geen commerciële game-images in enige DOSHorse-repo. **FreeDOS** als bundled fallback voor "ik wil iets draaien zonder eigen DOS".

**Rationale:** MS-DOS is closed-source eigendom van Microsoft (zelfs MS-DOS 1.25/2.0 zijn alleen voor "personal/non-commercial educational use" gereleased onder MIT-stijl licentie sinds 2018, maar redistribute binnen onze AGPL-app is twijfelachtig). IBM PC BIOS = Phoenix Technologies clean-room. Game-images = commerciële IP. Bundle = lawsuit-risico.

**Mechanisme:** FreeDOS-image (GPL-2.0, gratis redistributable) gebundeld als `DOSHorse_Core/assets/freedos.img`. Per-platform UI-flow:
- Web: File API om eigen DOS-image te uploaden (IndexedDB-cache)
- X86: filesystem-picker
- Android: SAF (Storage Access Framework)
- SteamDeck: Flatpak data-dir

Geen download-link naar commerciële DOS in app. Help-link naar FreeDOS-website voor users die meer willen.

**Implicaties:**
- `freedos.img` als build-time-asset, niet als runtime-download
- IBM-PC BIOS: dosbox-x heeft eigen Phoenix-stijl clean-room BIOS-implementatie — die mag wel
- DOSHorse-marketing: positionering als "BYOD" (Bring Your Own DOS), niet als "pirate-emulator"
