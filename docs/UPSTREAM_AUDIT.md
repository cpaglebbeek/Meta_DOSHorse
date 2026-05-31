# Upstream Audit — DOSHorse

> Verplicht per **P-DSH-07** (per-file licentie-audit bij upstream-bumps). Bijgehouden bij elke `git submodule update` of `git pull upstream/master`. Doel: bewijs dat elke vendored regel code AGPL-3.0-compat is.

## License-chain (P-DSH-03 bewijslast)

**Geen vendored code in v0.0.1-Estridge skeleton.** Onderstaande audit-baseline geldt voor v0.0.2-import (dosbox-x submodule-add).

```
dosbox-x (GPL-2.0-or-later)
     │
     │  "or later" clause → toegestaan upgrade
     ▼
GPL-3.0
     │
     │  §13 van GPL-3.0 expliciet AGPL-3.0-combinatie
     ▼
AGPL-3.0  ← DOSHorse distributie-licentie
```

**Bron-citaten:**
- dosbox-x' LICENSE-file: "either version 2 of the License, or (at your option) any later version"
- GNU GPL-3.0 §14 "Revised Versions of this License": permitteert opwaarts vervangen
- GNU GPL-3.0 §13: "Notwithstanding any other provision of this License, you have permission to link or combine any covered work with a work licensed under version 3 of the GNU Affero General Public License"

---

## Audit-baseline 1 — joncampbell123/dosbox-x (gepland v0.0.2-import)

**Bron:** [joncampbell123/dosbox-x](https://github.com/joncampbell123/dosbox-x)
**Pinned commit:** **TBD bij v0.0.2-import** (vermelding hier verplicht zodra submodule-add gedaan is)
**Add-datum:** TBD (v0.0.2)
**Repo waar gebruikt:** `DOSHorse_Core/upstream/dosbox-x/` (submodule)
**Project-licentie (LICENSE):** GPL-2.0-or-later (geverifieerd via README van upstream — formele LICENSE-file-check bij submodule-add)
**Upgrade-pad:** GPL-2.0+ → GPL-3.0 → AGPL-3.0 (zie License-chain hierboven)

### Sub-licenties binnen dosbox-x (te verifieren bij v0.0.2-import)

dosbox-x vendort meerdere subcomponenten. Hieronder de **verwachte** sub-licenties (op basis van publieke documentatie van upstream); definitieve verificatie volgt bij eerste submodule-add.

| Subcomponent | Verwachte locatie | Verwachte licentie | Audit-status |
|---|---|---|---|
| dosbox-x core (CPU/Chipset/DOS-shell) | `src/`, `include/` | GPL-2.0-or-later | TBD v0.0.2 |
| SDL2 | `vs/sdl/` (Windows-build) | Zlib | TBD — AGPL-compat (permissive) |
| FluidSynth | `vs/fluidsynth/` | LGPL-2.1-or-later | TBD — AGPL-compat (LGPL combineerbaar) |
| libpng | `vs/libpng/` | libpng-2.0 (BSD-stijl) | TBD — AGPL-compat (permissive) |
| zlib | `vs/zlib/` | Zlib | TBD — AGPL-compat (permissive) |
| FFmpeg headers (optioneel) | `vs/ffmpeg/` | LGPL-2.1+ (headers) of GPL-2+ (codecs) | TBD — codec-build-flag check |
| MUNT (MT-32 emulator) | `src/hardware/mt32/` | LGPL-2.1-or-later | TBD — AGPL-compat |
| dosbox-x BIOS implementaties | `src/bios/` | GPL-2.0-or-later (clean-room) | TBD v0.0.2 |
| Phoenix-stijl PC BIOS | embedded in core | clean-room re-implementation | TBD — geen IBM-IP |

**Conclusie (voorlopig):** Geen verwachte license-blockers; alle subcomponenten lijken AGPL-3.0-compat. **Definitief bewijs vereist v0.0.2-import-audit.**

### Te volgen bij v0.0.2-import (eerste audit)

1. `git clone --recursive https://github.com/joncampbell123/dosbox-x` lokaal in `DOSHorse_Core/upstream/dosbox-x/`
2. Pin commit (HEAD van master op import-moment)
3. Audit-script (`tools/license-audit.py`, te maken) loopt over alle `.cpp/.h/.c/.hpp` files:
   - Zoek SPDX-header (`SPDX-License-Identifier:`)
   - Zoek `License`/`Copyright` keywords
   - Match tegen whitelist: `{GPL-2.0+, GPL-3.0+, LGPL-2.1+, LGPL-3.0+, MIT, BSD-2-Clause, BSD-3-Clause, Zlib, MPL-2.0, ISC}`
   - Output: per-file mapping in `DOSHorse_Core/upstream/dosbox-x/LICENSE_AUDIT.csv`
4. Bij conflicten (GPL-2.0-only zonder "or later", proprietary, onbekend): **blokkeer merge**, document in deze file met file-paths + remediation-plan
5. Pin de audited commit in deze file + DEPENDENCIES.md

### Te volgen bij elke volgende `git submodule update`

1. Diff `git log <old>..<new> --stat` op `upstream/dosbox-x` lopen na bump
2. Nieuwe files met onbekende license-header → per-file-check
3. Bij gevonden conflict → blokkeer merge, raadpleeg `Meta_DOSHorse/CLAUDE.md` per-file-review-clausule
4. Update deze tabel met `audit-datum / commit-range / bevindingen`

### Bekende risico's

- **MS-DOS-compatibility-mode**: dosbox-x heeft hardcoded DOS-call-tabellen die clean-room zijn (eigen implementatie, geen MS-IP). Verificatie verplicht bij audit.
- **MIDI-soundfonts**: optionele `.sf2` files (FluidSynth-soundbanks). Niet vendored in dosbox-x master, maar als wij ooit bundelen → eigen audit per soundfont-licentie.
- **Windows-build vendors externe libs** (`vs/`); Linux/Mac builds gebruiken system-libs. Onze cross-compile voor Web/Android moet eigen vendoring kiezen (zie DEPENDENCIES.md).

---

## Audit-baseline 2..N — toekomstige upstreams

Plek voor:
- `js-dos` / `em-dosbox` (indien bootstrap-keuze in DOSHorse_Web O3 → A optie)
- `DOSBox Pure` libretro (indien Android-engine-keuze O4 → wisselend)
- FreeDOS-image (GPL-2.0, bundled per P-DSH-08) — audit bij eerste image-bundle

---

## Audit-history

| Datum | Actie | Auditor | Resultaat |
|-------|-------|---------|-----------|
| 2026-05-31 | Skeleton-baseline (license-chain bewijs, sub-license-tabel verwacht) | Claude Opus 4.7 1M | ✅ Geen vendored code, AGPL-chain documentair bewezen, audit-plan voor v0.0.2 vastgelegd |
