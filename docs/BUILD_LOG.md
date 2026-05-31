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
