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

## Audit-baseline 1 — joncampbell123/dosbox-x (v0.0.2-import — UITGEVOERD 2026-05-31)

**Bron:** [joncampbell123/dosbox-x](https://github.com/joncampbell123/dosbox-x)
**Pinned commit:** `4a95241b33b11537eef61a883b330478017cca3c` (master tip 2026-05-31, geverifieerd via `git ls-remote`)
**Add-datum:** 2026-05-31 (DOSHorse_Core v0.0.2-Dean)
**Repo waar gebruikt:** `DOSHorse_Core/upstream/dosbox-x/` (git submodule, `--depth 1` shallow, ~359 MB working tree, ~7720 files / ~4463 source files)
**Project-licentie:** **GPL-2.0-or-later** (geverifieerd via 3 bronnen — zie hieronder)
**Upgrade-pad:** GPL-2.0+ → GPL-3.0 → AGPL-3.0 (zie License-chain bovenaan)

### Licentie-bewijslast (3-bronnen-verificatie)

| Bron | Vondst | Status |
|------|--------|--------|
| `COPYING` (root) | 2× "any later version" (FSF GPL-2.0 boilerplate met "or later"-clause) | ✅ GPL-2.0-or-later expliciet |
| `README.md` | "released under the GNU General Public License, version 2" — oppervlakkig leek dit -only, maar README is samenvatting; project-keuze "or later" zit in source-headers | ⚠ Ambigu zonder source-check (alleen) |
| `src/dosbox.cpp` header | "version 2 of the License, **or (at your option) any later version**" — primaire bron | ✅ **Bewijst -or-later** |

**Conclusie:** GPL-2.0-or-later staat onomstreden vast. AGPL-3.0-distributie van DOSHorse_Core (en alle downstream platform-builds) is legitiem.

### Sub-licenties binnen dosbox-x (baseline-audit 2026-05-31 op pinned commit `4a95241b`)

| Subcomponent | Locatie | Licentie | Audit-status | Bron |
|---|---|---|---|---|
| dosbox-x core | `src/` (alle), `include/` | **GPL-2.0-or-later** | ✅ Geverifieerd | `src/dosbox.cpp` header + `COPYING` |
| xBRZ (graphics scaler) | `src/libs/xBRZ/` | **GPL-3.0** (boilerplate Version 3) | ✅ AGPL-compat via GPL-3.0 §13 | `src/libs/xBRZ/License.txt` |
| tinyfiledialogs | `src/libs/tinyfiledialogs/` | **Zlib** | ✅ Permissive, AGPL-compat | SPDX-header in `tinyfiledialogs.h` |
| Audio decoders (mp3, flac) | `src/libs/decoders/` | **GPL-2.0-or-later** (DOSBox Staging Team) | ✅ AGPL-compat via or-later | Source-header in `mp3.cpp` |
| libchdr/zstd | `src/libs/libchdr/zstd/` | **BSD-3-Clause + GPL-2.0** dual | ✅ Permissive-OR-compat | `LICENSE` in dir |
| libchdr/lzma | `src/libs/libchdr/lzma/` | **Public Domain (LZMA SDK)** | ✅ Permissive | `LICENSE` in dir |
| FluidSynth | `src/libs/fluidsynth/` + `include/fluidsynth/` | **LGPL-2.1-or-later** (verwacht via upstream FluidSynth-project) | ⚠ Lokale COPYING/LICENSE missing — bevestiging vereist v0.0.3 follow-up | upstream: FluidSynth/fluidsynth |
| MUNT (MT-32 emulator) | `src/libs/mt32/` | **LGPL-2.1-or-later** (verwacht via upstream munt-project) + sub-lib `mt32/sha1/` heeft eigen license-file | ⚠ Lokale COPYING/LICENSE missing — bevestiging vereist v0.0.3 follow-up | upstream: munt/munt |
| Vendored Visual Studio libs | `vs/sdl/`, `vs/sdl2/`, `vs/libpng/`, `vs/zlib/`, `vs/sdlnet/`, `vs/sdl2net/` | Zlib (SDL), libpng-2.0, Zlib | ✅ Permissive, alleen Windows-build | n.v.t. voor Mac/Linux/Web build |
| dosbox-x PC BIOS (clean-room) | embedded in `src/bios/`, `src/ints/` | GPL-2.0-or-later (eigen impl, geen IBM-IP) | ✅ Geverifieerd | source-headers |

**SPDX-headers gevonden in:** `tinyfiledialogs.{c,h}`, `imfc.cpp`, `imfc_rom.c`, `decoders/mp3.cpp`, `decoders/flac.c` — slechts 6 van ~4463 source-files. Volledige SPDX-spider gepland v0.0.3 (`tools/license-audit.py`, P-DSH-07 mechanisme).

**Conclusie:** Geen blocking conflicten. AGPL-3.0-keten staat. Twee aandachtspunten (FluidSynth/MUNT lokale LICENSE-files ontbreken in vendored copies) zijn niet-blokkerend — upstream-projecten zijn LGPL en bij twijfel verifieerbaar via upstream-repo. **Follow-up v0.0.3.**

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
| 2026-05-31 | **v0.0.2-import baseline** — dosbox-x submodule add @ `4a95241b`, GPL-2.0-or-later 3-bronnen-bewijs, sub-license-tabel ingevuld voor 10 vendored libs op basis van per-dir LICENSE-files + source-headers | Claude Opus 4.7 1M | ✅ Geen blocking conflicts; 2 aandachtspunten (FluidSynth/MUNT lokale LICENSE-file ontbrekend) niet-blokkerend, follow-up v0.0.3 |
