# CLAUDE.md — Meta_VideopacHorse

Sub-master van de **VideopacHorse**-familie (Gaming-ecosysteem): emulatie van de
Philips Videopac G7000 / Magnavox Odyssey² met één portable C11-core en drie frontends.

## Familie (lock-step versies, thema "Videopac/Odyssey²-pioniers")

| Repo | Rol | Pad |
|---|---|---|
| Meta_VideopacHorse | regie, ecosysteem-docs, versie-orchestratie | `~/Documents/Gemini_Projects/Meta_VideopacHorse` |
| VideopacHorse_Core | C11 emulator-engine (`include/g7000.h` = API) | `.../VideopacHorse_Core` |
| VideopacHorse_Web | WASM-frontend — LIVE-doel `horsecloud55.ddns.net/videopac/` | `.../VideopacHorse_Web` |
| VideopacHorse_Android | Kotlin + NDK/JNI-frontend (placeholder) | `.../VideopacHorse_Android` |
| VideopacHorse_SteamDeck | SDL2 + Flatpak-frontend (placeholder, `/Deploy2SteamDeck`) | `.../VideopacHorse_SteamDeck` |

## Regels

1. **Lock-step:** elke release bumpt `version.json` in ALLE vijf repos naar dezelfde versie+codenaam (Baer → Averett → Palmer → …). Groen +0.0.1 / Oranje +0.1.0 / Rood +1.0.0 + release-protocol (buglijst).
2. **API-wijzigingen** aan `g7000.h` beginnen hier: impact-check op alle drie de ports vóór de Core-commit.
3. **GEEN ROMs/BIOS in enige repo** — zie VideopacHorse_Core/CLAUDE.md regel 3-4 (copyright + licentie-brandmuur t.o.v. O2EM/MAME).
4. Meta_Master-protocollen gelden onverkort (WhatIf, prompts/, statusblok, OEU, ZSH-safety).
5. HC55-deploy van de Web-port raakt gedeelde infra: altijd `Meta_Master/SHARED_INFRASTRUCTURE.md` volgen (snippet-patroon, backup, all-blocks-verify).

## Referenties

- Kennisbasis: `VideopacHorse_Core/docs/O2EM_DEEPDIVE.md` + `docs/QUIRKS.md`
- Zusterpatroon: AmigaHorse / DOSHorse (zelfde 5-repo-structuur)
