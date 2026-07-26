---
date: 2026-07-26
repo: Meta_VideopacHorse
status: open
resume: "verder met VideopacHorse: S4 keyboard-tabel / game-smoke met o2rom.bin / Android+Deck-promotie (web LIVE /videopac/)"
---

# Sessie 2026-07-26 — newp VideopacHorse (G7000-emulator) + O2EM-deep-dive + start bouw

## Prompt(s)

1. "newp g7000 videopac emulator: kijk naar https://github.com/wagesreiterb/g7000 en bouw een
   emulator engine die ik kan verwerken in een android app, webapp of steamdeck app. gebruik ultracode"
2. "maak placeholders voor elke implementatie: web, android, steamdeck" (+ akkoord)
3. "doe nu een deep dive van de zip file in downloads dat een emulator is. daarnaast staan 3 game
   rom files .bin. ultracode"
4. "bouw nu webversie op horsecloud55.ddns.net/videopac" (+ akkoord op WhatIf fases 1-5)

## Besluiten (via one-by-one WhatIf)

- **B1:** C11-core met platform-agnostische API (`g7000.h`); JNI/WASM/SDL2-routes.
- **B2:** naam **VideopacHorse**, 5-repo Gaming-patroon (à la AmigaHorse/DOSHorse), PUBLIC, AGPL-3.0.
- **B3:** codenaam-thema **Videopac/Odyssey²-pioniers**, start v0.0.1-Baer, lock-step.
- **B4:** scope herzien door prompt 4: engine + web-port + LIVE op HC55 `/videopac/` (v0.1.0-Averett).

## Uitgevoerd (deze sessie, fase 1)

- Referentie-analyse wagesreiterb/g7000 (Java, ~6.2k regels, incompleet, geen licentie → alleen studie).
- **Ultracode deep-dive O2EM 1.21 + 3 ROMs** (5 agents): zie `VideopacHorse_Core/docs/O2EM_DEEPDIVE.md`.
  Kern: ROMs hash-geïdentificeerd (vp_01pl=Race+Spin-out+Cryptogram VP+/4×2K-banking, vp_14=Gunfighter,
  vp_24=Thunderball); cart-mapping-formule; quirk-canon → `docs/QUIRKS.md`; CAL-licentie-brandmuur.
- Skeleton 5 repos v0.0.1-Baer: Core (API + Makefile + harness groen 2/2 + stub), Web/Android/SteamDeck
  placeholders (SteamDeck compileert native), Meta (deze repo). PROJECTS.json/ECOSYSTEMS/STATUS bijgewerkt.

## Open

- Fase 2: engine-bouw via ultracode-workflow in VideopacHorse_Core.
- Fase 3-4: web-port + HC55-deploy `/videopac/` (snippet-patroon, backup, all-blocks-verify).
- BIOS `o2rom.bin` (md5 562d5ebf…) nog niet op de Mac — nodig voor echte game-smoke; site werkt met
  gebruikers-upload (IndexedDB, niets naar server).
- Android: promotie naar bouwbaar Gradle-project (eigen release).
- Homebrew-test-ROMs Manopac: toestemming vragen vóór eventuele publieke herdistributie.

## Slot (zelfde dag, fases 2-5)

- **Engine-workflow** (9 agents, 1,26M tokens, 0 fouten): architect (scheduler + kloktopologie exact rationaal) → 3 bouwers (cpu8048/vdc8244/cart+state, elk met per-quirk-tests) → integrator (73→84 tests, ASan groen, `tools/g7k_run`, QUIRKS-vinkjes) → 3 adversarial reviewers (**18 findings**, o.a. ontbrekende P16-gate op extram-writes, geïnverteerde strobe-bit, te smalle collision/charset-tests) → fixer (alle 18 verwerkt).
- **Eigen herverificatie:** `make test` 84/84 groen mét `G7K_ROMDIR` (echte vp_01pl-vectoren), wasm-build 44KB, node-smoke "0.1.0-Averett 320x240".
- **v0.1.0-Averett** lock-step ×5 gecommit + gepusht.
- **LIVE:** https://horsecloud55.ddns.net/videopac/ — snippet-patroon, vhost-backup, includes 23→24, buurprojecten 200, headless-browser-verificatie op de live pagina (core-badge + BIOS-flow). Geen ROMs op de server.
- Sanitycheck: 0×P1; DUP-001..003 geregistreerd; viewers/PRINCIPLES-split → ACTIONS.md.

## Kruisverwijzingen

Zusterrepo-sessies: `VideopacHorse_Core/prompts/2026-07-26_skeleton.md` (+ korte MDs in _Web/_Android/_SteamDeck).
