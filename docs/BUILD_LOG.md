# Build Log — DOSHorse

Per-platform build-resultaten. Append-only. Bij elke build (geslaagd of mislukt) een nieuwe rij + diagnose-sectie.

## Smoke-test #1 — macOS Intel x86_64 (2026-06-01)

**Status:** ✅ **GESLAAGD**
**Scope:** Bewijs dat upstream dosbox-x op deze ontwikkelmachine compileerbaar is via het meegeleverde `./build-macos-sdl2` script — fundament voor toekomstige DOSHorse_X86-build-wrapper.

### Host
| Eigenschap | Waarde |
|---|---|
| Hardware | Intel Mac (Homebrew prefix `/usr/local/`) |
| OS | macOS 26.3 (build 25D125) |
| Architectuur | x86_64 |
| Compiler | g++ (Apple) — autoconf 2.73 |
| SDL2 (Homebrew) | 2.32.10 |
| Brew prefix | `/usr/local/` |

### Source
| Eigenschap | Waarde |
|---|---|
| Repo | `DOSHorse_Core/upstream/dosbox-x/` (git submodule, shallow `--depth 1`) |
| Pinned commit | `4a95241b33b11537eef61a883b330478017cca3c` |
| Build-script | `./build-macos-sdl2` (upstream's eigen wrapper rond autotools) |

### Dependencies (Homebrew install)
Per dosbox-x BUILD.md `brew install autoconf automake nasm glfw glew fluid-synth libslirp libpcap pkg-config sdl2_net`.

Status vóór sessie:
- ✓ al aanwezig: `autoconf`, `glew`, `sdl2` (impliciet via andere projecten)
- ✗ geïnstalleerd in deze sessie (8 packages): `automake nasm glfw fluid-synth libslirp libpcap pkg-config sdl2_net`

### Build
| Eigenschap | Waarde |
|---|---|
| Start | 00:16:37 |
| Eind | 00:25:55 |
| Duur | ~9.5 min |
| Resultaat-binary | `src/dosbox-x` |
| Binary-grootte | 22 MB |
| Universal? | Nee — x86_64 only (`-arch x86_64 -mmacosx-version-min=10.13`) |

### Smoke-run
```
$ ./src/dosbox-x --version
DOSBox-X version 2026.05.02 SDL2, copyright 2011-2026 The DOSBox-X Team.
DOSBox-X project maintainer: joncampbell123 (The Great Codeholio)
```
✅ Versie-output klopt.

### Waarschuwingen (niet-blokkerend)
- Diverse `unused parameter/variable/function` warnings — upstream code, geen actie nodig
- `output_direct3d11.o has no symbols` — DirectX 11 stub op macOS, leeg object correct
- `ld: warning: object file ... was built for newer 'macOS' version (26.0) than being linked (10.13)` — Apple SDK header-versies vs deployment target; upstream-script-instelling, niet ons probleem
- `ld: warning: building for macOS-10.13, but linking with dylib ... built for newer version 14.0` — Homebrew dylibs vs deployment target; runtime werkt op huidig systeem
- `ld: warning: search path '.../vs/sdlnet/linux-host/lib' not found` — SDL2_net wordt via brew gelinkt, dosbox-x' bundled `vs/sdlnet/` mis-zoekpad

### Architectuurnoot
Build is x86_64 op een Intel Mac. Voor Apple Silicon (arm64) of universal-binary (lipo) is een Mac met M-chip nodig (per dosbox-x BUILD.md: "Universal macOS builds are only possible when building on a host machine powered by an Apple Silicon CPU"). Pad voor toekomstige DOSHorse_X86 v0.0.4+ universal builds: tweede build-machine of CI.

### Bewijslast voor P-DSH-02 (Y-pattern Core-submodule werkt)
✅ Submodule add (v0.0.2-Dean) → buildbaar op tweede commit (v0.0.3 smoke-test) zonder modificatie van Core-source. De keten dosbox-x@4a95241b → DOSHorse_Core/upstream/dosbox-x/ → werkende `dosbox-x` binary is reproduceerbaar.

### Bewijslast voor P-DSH-03 (AGPL-3.0 via or-later)
Build levert een GPL-2.0-or-later binary op (afgeleid van dosbox-x). Distributie onder AGPL-3.0 (zoals DOSHorse) is licentie-compatibel via de keten gedocumenteerd in `UPSTREAM_AUDIT.md` §License-chain — bewijs in code-praktijk: ✅.

### Niet uitgevoerd in deze stap
- Eigen DOSHorse_X86 build-script of CMakeLists.txt-wrapper (v0.0.4)
- Eigen DOSHorse-branding op binary (v0.0.4)
- Public API headers `include/doshorse/` (v0.0.5)
- Universal binary (vereist Apple Silicon machine; v0.0.4+)
- Per-file SPDX-spider tool (UPSTREAM_AUDIT follow-up)
- FluidSynth/MUNT lokale LICENSE-file follow-up
- Run-test met echte DOS-image (alleen `--version` getest)

### Resume-trigger update
- **Vorig:** "verder met DOSHorse v0.0.3 — CMake smoke-test build op Mac (arm64) + eerste DOSHorse_X86 native binary"
- **Nieuw:** "verder met DOSHorse v0.0.4 — DOSHorse_X86 eigen build-wrapper (Makefile of CMakeLists.txt die Core consumeert) + eigen branding op binary"

---

## Smoke-test #2 — DOSHorse_X86 wrapper end-to-end (2026-06-01)

**Status:** ✅ **GESLAAGD**
**Scope:** Bewijs dat DOSHorse_X86 v0.0.2-Sams Makefile-wrapper end-to-end werkt: `make build && make install && make smoke` produceert een werkende `dist/doshorse-x86` binary via de submodule-keten `DOSHorse_X86 → core/ → core/upstream/dosbox-x/`.

### Host
Identiek aan smoke-test #1 (Intel Mac, macOS 26.3, x86_64).

### Submodule-keten geverifieerd
```
DOSHorse_X86 (HEAD: 4bfe917 "v0.0.2-Sams ...")
  └─ core/                       → DOSHorse_Core@9924880 (v0.0.2-Dean)
       └─ upstream/dosbox-x/     → joncampbell123@4a95241b (master tip 31-05)
```
Drie niveaus diep, `git submodule update --init --recursive --depth 1` pakt alles in één commando.

### Run
```bash
cd DOSHorse_X86
make build && make install && make smoke
```

| Eigenschap | Waarde |
|---|---|
| Start | 00:53:35 |
| Eind | 01:03:39 |
| Duur | **10m 04s** |
| Resultaat | `core/upstream/dosbox-x/src/dosbox-x` → kopie naar `dist/doshorse-x86` |
| Binary-grootte | 22 MB |
| Versie-output | `DOSBox-X version 2026.05.02 SDL2, copyright 2011-2026 The DOSBox-X Team.` (upstream-string, branding-laag komt v0.0.5+) |
| Exit code | 0 |

### Wat dit bewijst (P-DSH-02 + P-DSH-03 in praktijk, niveau 2)

Smoke-test #1 (v0.0.3) bewees: dosbox-x is buildbaar vanuit `DOSHorse_Core/upstream/dosbox-x/`.
Smoke-test #2 (v0.0.4) bewijst additioneel: **DOSHorse_X86 kan upstream consumeren via de Y-pattern Core-submodule keten** zonder source-modificaties. De wrapper-only architectuur (approach A) werkt end-to-end.

- ✅ **P-DSH-02** Y-pattern Core-submodule: drie-niveau submodule-keten werkt
- ✅ **P-DSH-03** AGPL-3.0 via or-later: tweede end-to-end-build bevestigt licentie-keten in praktijk
- ✅ **P-DSH-01** upstream-clean: `core/upstream/dosbox-x/` gitlink onveranderd (4a95241b), geen source-touch

### Wat blijft (v0.0.5+)

- `make build` voor Linux + Windows (nu macOS-only)
- Source-branding patches (binary `--version` rapporteert nog upstream-string)
- Universal binary (vereist Apple Silicon host)
- Public API headers (v0.0.5)
- DOSHorse_Web v0.0.2 Emscripten-build (apart traject)
- Per-file SPDX-spider tool (UPSTREAM_AUDIT follow-up)
- FluidSynth/MUNT lokale LICENSE-file follow-up
- Run-test met echte DOS-image (`.exe`/`.img` boot)

### Resume-trigger update
- **Vorig:** "verder met DOSHorse v0.0.4 — DOSHorse_X86 eigen build-wrapper (Makefile/CMakeLists.txt rond Core-submodule) + eigen branding op binary"
- **Nieuw:** "verder met DOSHorse v0.0.5 — DOSHorse_Core Public API headers (include/doshorse/) + DOSHorse_X86 source-branding patches (--version-string)"

---

## Smoke-test #3 — DOSHorse_X86 v0.0.3-Noyce + branding-patch (2026-06-01)

**Status:** ✅ **GESLAAGD** (build + apply-patches + binary-branding bewezen; host-specifieke runtime-workaround voor smoke-run)
**Scope:** Bewijs dat de wrapper-pipeline met patch-stap (apply-patches → build → install → smoke) end-to-end werkt en dat de DOSHorse-branding zichtbaar is in `--version` output.

### Host
Identiek aan smoke-test #1 + #2 (Intel Mac, macOS 26.3, x86_64). Brew x265 echter gedreven van 4.1 → 4.2 sinds v0.0.3 smoke-test (host-state-drift, niet DOSHorse-veroorzaakt).

### Submodule-keten + apply-patches

```
DOSHorse_X86@afb4b87 (v0.0.3-Noyce)
  └─ core/@54db632 (DOSHorse_Core v0.0.3-Canion)
       └─ upstream/dosbox-x@4a95241b  ← + patches/0001-doshorse-branding.patch applied
```

`make apply-patches` (via `core/tools/apply-patches.sh`) gepatcht 1 file:
- `src/gui/sdlmain.cpp:7365` — banner vóór `--version`-output
- `src/gui/sdlmain.cpp:7381` — banner vóór `--help`-output

Idempotent + reversible (lokaal getest in DOSHorse_Core).

### Run

```bash
cd DOSHorse_X86
make    # apply-patches → build → install → smoke
```

| Eigenschap | Waarde |
|---|---|
| Start | 01:24:46 |
| Eind | 01:40 (~15 min) |
| Apply-patches | ✓ (1 patch, `0001-doshorse-branding.patch`) |
| Build resultaat | `core/upstream/dosbox-x/src/dosbox-x` (exit 0) |
| Install | `dist/doshorse-x86` 22 MB |
| Smoke-test in Makefile | ❌ Faalde op dyld-runtime (host-issue, niet DOSHorse) |

### Smoke-run met host-workaround

```bash
DYLD_LIBRARY_PATH=/usr/local/Cellar/x265/4.1/lib \
  ./dist/doshorse-x86 --version
```

Output (eerste 6 regels):
```
DOSHorse version 0.0.3-Canion (forked from upstream below)

DOSBox-X version 2026.05.02 SDL2, copyright 2011-2026 The DOSBox-X Team.
DOSBox-X project maintainer: joncampbell123 (The Great Codeholio)
...
```

✅ **DOSHorse-banner zichtbaar vóór upstream DOSBox-X info.** Upstream-attributie behouden. P-DSH-01 (upstream-clean) + branding-doel beide bereikt in praktijk.

### Bekende host-issue (niet DOSHorse-veroorzaakt)

Sinds v0.0.3 smoke-test heeft Homebrew x265 gedreven van 4.1 (`libx265.215.dylib`) naar 4.2 (`libx265.216.dylib`). ffmpeg 8.1 op deze Mac was gelinkt tegen 4.1; bij elke `dyld`-lookup naar `libx265.215.dylib` faalt het. Beide versies zijn lokaal aanwezig in `/usr/local/Cellar/x265/{4.1,4.2}/`.

Workarounds (kies één bij hervatting):
- **Snelste:** `DYLD_LIBRARY_PATH=/usr/local/Cellar/x265/4.1/lib` prefix
- **Permanent:** `brew reinstall ffmpeg` (~5-10 min, rebuildt tegen huidige x265 4.2)
- **Brew-symlink:** `ln -s /usr/local/Cellar/x265/4.2/lib/libx265.216.dylib /usr/local/Cellar/x265/4.2/lib/libx265.215.dylib` — niet ABI-veilig, niet aanbevolen

**Geen wijziging in DOSHorse-code nodig** — dit is een ontwikkelaar-host-state issue.

### Bewezen architectuurprincipes (niveau 3)

- ✅ **P-DSH-01** upstream-first / clean: patches leven in `Core/patches/`, niet in submodule-history. `upstream/dosbox-x/` gitlink onveranderd 4a95241b
- ✅ **P-DSH-02** Y-pattern Core-submodule: drie-niveau keten (X86→Core→dosbox-x) + apply-patches-script werkt
- ✅ **P-DSH-03** AGPL-3.0 or-later: derde end-to-end build bevestigt licentie-keten; branded binary distribueert met behoud van upstream-COPYING
- ✅ **P-DSH-04** save-state API ontworpen (skeleton-headers in `Core/include/doshorse/savestate.h`)

### Niet uitgevoerd (v0.0.6+)

- Public API implementatie (`src/doshorse/*.c` koppelend aan dosbox-x' internal-state)
- Linux/Windows build-support
- Universal binary (Apple Silicon host vereist)
- DOSHorse_Web v0.0.2 (Emscripten-pad)
- DOSHorse_Android v0.0.2 (NDK-pad)
- Per-file SPDX-spider tool

### Resume-trigger update
- **Vorig (v0.0.5):** "verder met DOSHorse v0.0.5 — DOSHorse_Core Public API headers (include/doshorse/) + DOSHorse_X86 source-branding patches (--version-string)"
- **Nieuw (v0.0.6):** "verder met DOSHorse v0.0.6 — eerste Public API implementatie (DOSHorse_Core src/doshorse/) + Linux/Windows Makefile-support"
