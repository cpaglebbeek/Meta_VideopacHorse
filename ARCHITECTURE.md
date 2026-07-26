# ARCHITECTURE.md — VideopacHorse-ecosysteem

## Overzicht

```
                    ┌─────────────────────┐
                    │  Meta_VideopacHorse │  regie / lock-step / docs
                    └─────────┬───────────┘
                              │ orkestreert
                    ┌─────────▼───────────┐
                    │  VideopacHorse_Core │  C11-engine, g7000.h
                    └───┬──────┬──────┬───┘
              WASM/emcc │  JNI │      │ SDL2/Flatpak
        ┌───────────────▼┐ ┌───▼────────┐ ┌─▼──────────────┐
        │ _Web           │ │ _Android   │ │ _SteamDeck     │
        │ HC55 /videopac │ │ APK        │ │ Deploy2SteamDeck│
        └────────────────┘ └────────────┘ └────────────────┘
```

## Componenten & relaties

- **Core → ports:** ports consumeren uitsluitend `include/g7000.h`; de core kent de ports niet. Framebuffer RGBA8888 + mono s16-audio + joystick/keymatrix-input als volledige contractset.
- **Build-koppeling:** _Web draait `make wasm` in de Core-checkout (zusterpad); _SteamDeck en _Android linken `libg7000.a`/bronbestanden via relatieve zusterpaden. Geen submodules — lock-step-versies + zusterpad-conventie (zelfde patroon als AmigaHorse/DOSHorse).
- **ROM-stroom:** BIOS + carts komen ALTIJD van de eindgebruiker (file-picker/bestandspad); geen ROM verlaat het apparaat van de gebruiker (web: IndexedDB, lokaal).

## Oorzaak/gevolg

| Wijziging | Impact |
|---|---|
| `g7000.h` | alle drie ports hercompileren; Oranje versie-bump alle 5 repos |
| Core-gedrag (quirks) | QUIRKS.md-status + testsuite in Core; ports ongewijzigd |
| Web-UI | alleen _Web + HC55-deploy |
| Release | version.json ×5 + STATUS.md (Meta_Master) + release-notes hier |

## Fasering

1. **v0.0.1-Baer** — skeleton ×5, API bevroren, testharness groen (stub).
2. **v0.1.0-Averett** — werkende engine + Web-port LIVE op HC55 `/videopac/`.
3. Later: Android-APK (HorseAPK-flow), Steam Deck-Flatpak (Deploy2SteamDeck), VP+/Voice-modules.
