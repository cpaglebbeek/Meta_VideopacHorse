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
- **ROM-stroom:** BIOS + carts komen ALTIJD van de eindgebruiker (file-picker/bestandspad) of rechtstreeks van een publieke bron naar diens browser; geen ROM verlaat het apparaat via ONZE server (web: IndexedDB, lokaal). Eén uitzondering, sinds v0.5.0 en alleen in netplay: kan de medespeler een cartridge nergens zelf krijgen, dan stuurt de host hem peer-to-peer, met een melding in beeld. Dat is verkeer tussen twee mensen die samen spelen — de server ziet die bytes niet.
- **Samen spelen = netplay (v0.5.1):** `/videopac/` laat beide kanten dezelfde emulatie draaien en wisselt alleen invoer uit (deterministische lockstep). De oudere variant, waarbij de host zijn beeld naar de gast streamde, staat gearchiveerd op `/videopac/stream/`; `/videopac/2/` verwijst door naar de hoofdpagina. Alle drie delen één `app.js`, één `style.css`, één wasm en dezelfde pairing-API — het archief zet alleen `window.VPH_BASE`/`VPH_API`. Determinisme is een eigenschap van de Core en wordt daar bewaakt: `make netcheck`.

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
