# ARCHITECTURE — DOSHorse ecosysteem

> Eén functionele kern (DOS-emulatie via dosbox-x fork), vijf deployment-targets, één gedeelde Core (DOSHorse_Core) als git-submodule onder vier platform-repos.

## Functionele kern (alle varianten)

**Doel:** Klassieke DOS-software (MS-DOS, PC-DOS, FreeDOS, applicaties + games uit 1981-2000) speelbaar maken op moderne hardware, met gamepad-/touch-/keyboard-input afhankelijk van platform.

**Functionele componenten (geërfd uit dosbox-x):**
- **CPU** — 8086/8088, 80286, 80386, 80486, Pentium-klasse emulatie (real mode + protected mode + V86)
- **Chipset** — IBM PC/XT/AT, programmable interval timer (8253), PIC (8259), DMA (8237), keyboard controller (8042)
- **Video** — MDA, CGA, EGA, VGA, SVGA (S3 Trio64, ET4000, etc.) — dosbox-x heeft uitgebreide chipset-emulatie
- **Audio** — PC speaker, AdLib, Sound Blaster 1/2/Pro/16, Gravis Ultrasound, MT-32, General MIDI (mu80/munt), Tandy 3-voice, Disney Sound Source
- **Storage** — diskette images (.img, .ima), CD-ROM images (.iso, .cue/.bin), harddisk images (.img/.vhd), gemounte host-directories
- **Input** — IBM PC keyboard, PS/2 muis, gameport (analog), seriële muis
- **DOS-shell** — interne DOS-emulatie (file-API, EMS/XMS, DPMI) of bootbare echte DOS

**Functionele componenten (DOSHorse-specifiek, bovenop dosbox-x):**
- **Save-states** — quicksave/quickload, slot-based, **portable formaat** als API-contract tussen alle 4 varianten (te ontwerpen, vergelijk P-AMH-04 in AmigaHorse)
- **ROM-library** — game/applicatie-image-beheer (lokaal of cloud), tags, last-played
- **Input-mapping** — gamepad → keyboard/joystick-translaatie (met defaults per game-type), on-screen-keyboard, touch-overlay
- **OSD** — pauze, menu, slot-selectie, mount/unmount tijdens spel

## Componenten-overzicht (Y-pattern, AmigaHorse-conform)

```
          ┌───────────────────────────────────────────────┐
          │                  DOSHorse_Core                │
          │  (dosbox-x fork als git-submodule             │
          │   + DOSHorse-abstractie-laag in C/C++)        │
          │                                               │
          │  ┌─────────────────┐  ┌─────────────────────┐ │
          │  │  Emulator-core  │  │  Public API         │ │
          │  │  CPU, Chipset,  │──┤  (C/C++ headers)    │ │
          │  │  Video, Audio,  │  │  + WASM-binding     │ │
          │  │  Storage, DOS   │  │  + JNI-binding      │ │
          │  └─────────────────┘  │  + SDL2-binding     │ │
          │                       │  + Flatpak-binding  │ │
          │                       └─────────────────────┘ │
          └─────────────┬─────────────────────────────────┘
                        │ (git submodule)
       ┌────────────────┼────────────────┬────────────────┐
       │                │                │                │
       ▼                ▼                ▼                ▼
┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐
│ X86        │   │ Web        │   │ Android    │   │ SteamDeck  │
│ (Linux/    │   │ (WASM)     │   │ (NDK +     │   │ (Flatpak + │
│  Win/Mac)  │   │ + JS UI    │   │  Compose)  │   │  Steam     │
│ + SDL2 of  │   │ + canvas/  │   │ + gamepad+ │   │  Input)    │
│  Qt6/GTK4  │   │  WebGL     │   │  touch     │   │            │
│            │   │            │   │            │   │            │
│ prio 1     │   │ prio 2     │   │ prio 3     │   │ prio 4     │
└────────────┘   └────────────┘   └────────────┘   └────────────┘
```

## Variant 1 — X86 (prio 1, direct na Core-import)

**Strategie:** dosbox-x compileert al natively op Linux/Win/macOS. Eerste runnable = onze fork builden met onze branding + DOSHorse_Core abstractie-laag erbovenop.

**Tech stack:** C++17, SDL2 (vendored door dosbox-x), CMake build-systeem, FFmpeg optioneel (video-capture).

**UI:** Voorlopig dosbox-x' eigen menu (SDL-based). Mogelijk later Qt6/GTK4/Dear ImGui — keuze open tot v0.0.3.

**Distributie:**
- Linux: AppImage + .deb + Flatpak
- Windows: portable .zip + .msi installer
- macOS: .dmg met universal binary (x86_64 + ARM64)

## Variant 2 — Web (prio 2)

**Strategie:** Emscripten-compile van DOSHorse_Core. dosbox-x heeft géén officiële Emscripten-port → echt port-werk nodig. Bestaande referenties: `js-dos` (v8 op DOSBox 0.74), `em-dosbox` (Emscripten op upstream DOSBox).

**Tech stack:** Emscripten → WASM + JS-glue, HTML5 canvas/WebGL voor video, Web Audio API voor sound, Gamepad API voor input, IndexedDB voor save-states + ROM-library.

**UI:** Eigen JS/HTML/CSS frontend (geen framework-lock in deze fase — vanilla TS overwogen).

**Routes (te kiezen v0.0.2, analoog aan AmigaHorse_Web):**
- **Quick mode** — drag-and-drop .exe of .img, auto-run
- **Full mode** — ROM-library, settings, compat-presets per game

## Variant 3 — Android (prio 3)

**Strategie:** NDK-build van DOSHorse_Core (CMake + Android.mk), Kotlin/Compose UI erbovenop. Bestaande referenties: Magic DOSBox (closed source), DOSBox Pure (libretro core).

**Tech stack:** NDK r26+, Kotlin 2.0+, Jetpack Compose, OpenSL ES / AAudio voor audio, Android InputManager voor gamepad+touch.

**UI:** Touch-overlay (virtuele toetsenbord + d-pad + muisemulatie via touch), gamepad-mapping per game.

## Variant 4 — SteamDeck (prio 4)

**Strategie:** Flatpak-bundeling van X86-build met Steam Input preset. dosbox-x werkt al op Deck via desktop-modus; onze winst is integratie + presets.

**Tech stack:** Flatpak manifest, Steam Input controller layouts, screen-rotation handling.

## Cross-cutting concerns

### Save-state portability (CRITISCH)
Een save-state gemaakt in Web moet open kunnen worden in X86 en vice versa. Vergt:
- Versie-gebonden DOSHorse-state-formaat (apart van dosbox-x' interne formaat)
- Embedded machine-config (CPU-type, video-mode, audio-cards)
- Export naar `.dhs` (DOSHorse-state) container met dosbox-x raw-state + DOSHorse-metadata

### ROM-library sync
Geen verplichte cloud-sync in v0.0.x. Lokaal per platform. Optioneel later: WebDAV/HC55-backend.

### Codename-pool drift-preventie
Eén bron in `Meta_DOSHorse/CLAUDE.md`. Sub-repos refereren ernaar, dupliceren niet. Vergeleken AmigaHorse: dezelfde regel.

## Open beslispunten (na skeleton, vóór v0.0.2)

| # | Vraag | Wanneer beslissen |
|---|-------|-------------------|
| O1 | Core-relatie: dosbox-x als git submodule binnen DOSHorse_Core, of fork-importeren in DOSHorse_Core zelf (single repo)? | v0.0.2-import-fase |
| O2 | UI-stack X86: dosbox-x' SDL-menu houden, of Qt6/GTK4/ImGui overzetten? | v0.0.3 |
| O3 | Web bestaande WASM (`js-dos`/`em-dosbox`) als shortcut, of pure eigen Emscripten van DOSHorse_Core? | v0.0.4 (Web-fase) |
| O4 | Android: pure dosbox-x-fork via NDK, of DOSBox Pure libretro als alternatieve engine? | v0.0.5 (Android-fase) |
| O5 | Save-state-formaat-versie en migratiebeleid | v0.0.2 (vóór eerste runnable) |
| O6 | Deploy CDN of pure nginx static op HC55? | v0.0.4 |
