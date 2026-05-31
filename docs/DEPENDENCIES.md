# Dependencies — DOSHorse ecosysteem

## Cross-repo (intern, Y-pattern)

```
Meta_DOSHorse  ──► (geen runtime-dep; docs/orchestratie only)
                   │
                   │ verwijst naar
                   ▼
DOSHorse_Core  ◄───┬──── DOSHorse_X86       (SDL2-binding, prio 1)
                   ├──── DOSHorse_Web       (WASM/Emscripten-binding)
                   ├──── DOSHorse_Android   (JNI-binding)
                   └──── DOSHorse_SteamDeck (X86-binary herverpakt → Flatpak)
```

**Submodule-strategie (P-DSH-02):** `DOSHorse_Core` is git-submodule in elke platform-repo. **Tag-gebaseerd** (geen floating main); platform-repos pinnen specifieke Core-tag voor reproduceerbare builds.

**Bump-flow:**
1. PR-merge in `DOSHorse_Core` (bv. Core v0.0.3-Bradley → v0.0.4-Dean)
2. Tag `v0.0.4-Dean` in `_Core`
3. Per platform-repo: PR die `git submodule update --remote core && git add core` doet + CHANGELOG-entry "Core bump v0.0.3 → v0.0.4"

## Upstream (extern)

| Upstream | Licentie | Rol | Repo |
|---|---|---|---|
| **joncampbell123/dosbox-x** | **GPL-2.0-or-later** (geverifieerd 3-bronnen 2026-05-31, zie UPSTREAM_AUDIT.md §License-bewijslast) | **DOSHorse_Core basis** — submodule `upstream/dosbox-x/` toegevoegd 2026-05-31 (Core v0.0.2-Dean), pinned `4a95241b33b11537eef61a883b330478017cca3c` | [joncampbell123/dosbox-x](https://github.com/joncampbell123/dosbox-x) |
| SDL2 | Zlib | Cross-platform input/audio/video-abstractie (dosbox-x vendort, wij ook bij Web/Android-build) | [libsdl-org/SDL](https://github.com/libsdl-org/SDL) |
| FluidSynth | LGPL-2.1-or-later | General MIDI synth voor DOS-MIDI-output (Sound Canvas SC-55-stijl) | [FluidSynth/fluidsynth](https://github.com/FluidSynth/fluidsynth) |
| MUNT | LGPL-2.1-or-later | MT-32 emulator (Roland MT-32 voor klassieke DOS-games als Sierra adventures) | dosbox-x vendored |
| libpng + zlib | libpng-2.0 + Zlib | Screenshot / save-state-thumbnail | dosbox-x vendored |
| FreeDOS-image | GPL-2.0 | Bundled fallback-OS (P-DSH-08: BYOD) | [freedos.org](https://www.freedos.org/) |
| **js-dos** of **em-dosbox** | GPL-2.0+ | (Conditioneel, indien bootstrap-keuze O3 → A) — bestaande WASM-port voor snelle DOSHorse_Web kickstart | [js-dos](https://github.com/caiiiycuk/js-dos) of [emscripten-ports/em-dosbox](https://github.com/dreamlayers/em-dosbox) |
| **DOSBox Pure** | GPL-2.0+ | (Conditioneel, indien Android-engine-keuze O4 → libretro-route) — alternatieve Core voor Android via libretro | [schellingb/dosbox-pure](https://github.com/schellingb/dosbox-pure) |

## Per-target deps (verwacht v0.0.2+)

### DOSHorse_X86 (prio 1)
- C++17 compiler (gcc ≥9, clang ≥13, MSVC 2022)
- CMake ≥3.16
- SDL2 ≥2.0.20 (vendored of system-lib)
- FluidSynth ≥2.2 (optioneel, voor MIDI)
- FFmpeg ≥4.4 (optioneel, voor video-capture in dosbox-x)
- macOS: Xcode CLT + universal binary toolchain (lipo)
- Windows: WiX 3 of NSIS voor installer
- Linux: AppImage-tool + Flatpak-builder

### DOSHorse_Core
- C++17 (zelfde als X86)
- CMake ≥3.16
- Optioneel: clang-format + clang-tidy voor code-hygiene

### DOSHorse_Web
- Emscripten ≥3.1.x (zelfde toolchain als AmigaHorse_Web — emsdk 5.0.7 mogelijk hergebruikbaar in `~/Documents/Gemini_Projects/emsdk`)
- IndexedDB (browser-native)
- Web Audio API + AudioWorklet (browser-native)
- WebGL 1.0 + Canvas2D fallback (browser-native)
- Gamepad API (browser-native)
- TypeScript ≥5 (framework-keuze open beslispunt W2)
- Geen Node-runtime-dependency in productie

### DOSHorse_Android
- Android Studio Hedgehog+ / NDK r26+
- Kotlin 2.0+
- Jetpack Compose 1.7+
- OpenSL ES of AAudio (audio)
- Android InputManager (gamepad+touch)
- Gradle 8.x

### DOSHorse_SteamDeck
- Flatpak-builder ≥1.4
- SteamOS Holo runtime (`org.freedesktop.Platform//22.08`)
- Steam Input controller-layout (`.vdf` formaat)
- Geen eigen C++-toolchain — herverpakt X86 Linux-build

## Impact-matrix (oorzaak → gevolg)

| Wijziging in... | Raakt direct... | Raakt indirect... |
|-----------------|-----------------|-------------------|
| `DOSHorse_Core` Public API | Alle 4 platform-repos (submodule-bump nodig) | UI-laag indien API-handtekening wijzigt |
| `DOSHorse_Core/upstream/dosbox-x` bump | Core-tag → 4 platform-bumps; license-audit verplicht (P-DSH-07) | Save-state-formaat indien dosbox-x internal-state-layout wijzigt |
| `DOSHorse_X86` UI-stack-keuze (X1) | Geen impact op andere platforms | Hint voor X86-codename + CHANGELOG |
| `DOSHorse_Web` bootstrap-keuze (W1) | Geen impact op andere platforms | Web-codename + roadmap |
| `Meta_DOSHorse/CLAUDE.md` codename-pool | Volgende release in elke sub-repo (lookup-bron) | Geen runtime-impact |
| Save-state-formaat-bump (`.dhs`) | Alle 4 platforms (P-DSH-04 cross-compat) | Migration-tool nodig bij major-bump |
| `LICENSE` of fork-attributie wijziging | Alle 6 repos identiek bij te werken | GitHub-repo-description + README |
| Deploy `horsecloud55.ddns.net/DOSHorse/` subpad | DOSHorse_Web (rsync-target), Android downloads-pagina | HC55 nginx + `Meta_Master/SHARED_INFRASTRUCTURE.md` (P-TECH-04) |

## Geen runtime-dependencies tussen platform-repos

Web ⊥ X86 ⊥ Android ⊥ SteamDeck — geen onderlinge build- of runtime-coupling, alleen via Core. Save-states zijn data-contract (P-DSH-04), niet code-contract.
