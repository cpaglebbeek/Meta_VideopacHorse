---
date: 2026-07-27
repo: Meta_VideopacHorse
status: open
resume: "verder met VideopacHorse netplay: BIOS+ROM-uitwisseling host→gast + deterministische lockstep (analyse staat in dit document)"
---

# Sessie 2026-07-27 — v0.2.0 t/m v0.4.0: joystick-app, GAMES, samen spelen, controller-koppeling

Vervolg op `2026-07-26_newp_videopachorse_skeleton.md`. Alle releases lock-step over 6 repo's.

## Opgeleverd (chronologisch)

| Versie | Inhoud |
|---|---|
| v0.1.1-Palmer | BUG-001: char-renderer naar absolute-Y-adressering (MAME-gedragsfeit) — "SELECT GAME" leesbaar |
| v0.1.2-Boris | S4 toetsenbord-matrix (36/36 QUIRKS groen), game-select werkt |
| v0.2.0-Gust | Android BLE-joystick (6e repo), GAMES-catalogus 65 titels, BIOS-knop archive.org |
| v0.3.0-Guttenbrunner | 🎭 Samen spelen: code-pairing (PHP+SQLite) + WebRTC-stream + DataChannel-input |
| v0.3.1-Harrison | Sessie = auto power-cycle; speler 2 exclusief voor de gast; BUG-007 SQLite-locks |
| v0.4.0-Rusch | Telefoon koppelt via **sessiecode** (BLE eruit), max 2 joysticks server-side; 13 review-findings; BUG-009/010/011 |

## Bugs met RCA (alle in `VideopacHorse_Web/docs/BUGLIST.md` resp. `VideopacHorse_Core/docs/BUGLIST.md`)

- **BUG-001** char-renderer (Geel) · **BUG-002** proxy-cache → `?v=`-busters
- **BUG-003/003b/004/005/006** pairing/WebRTC: GC wiste gast-signalen, gast-token niet herkend,
  offerer/answerer omgedraaid, gast zag eigen canvas, vroege ICE-kandidaten weggegooid
- **BUG-007** SQLite write-locks (throttled GC, BEGIN IMMEDIATE, retry-helper)
- **BUG-009** poll nam onnodig schrijfslot → 503 midden in pairing
- **BUG-010/011** (melding gebruiker: zwart beeld gast + sessie weg): (a) `disconnected` werd als
  definitief einde behandeld terwijl het bij een CPU-piek routinematig voorkomt → herstelperiode
  8 s + ICE-restart; (b) **niet-uitgelezen PDO-cursor blokkeerde de eigen schrijfactie** — dát was
  de echte oorzaak van de 503's, niet contentie. E2e daarna 3/3 groen.

## OPEN — netplay (volgende sessie, akkoord gebruiker al gegeven)

Doel: **geen videostream meer**, maar beeld+geluid bij de gast opbouwen uit de emulatie zelf,
met BIOS+ROM-uitwisseling host→gast tijdens het koppelen ("klassieke netplay").

**Haalbaarheidsanalyse (getoetst aan de code, 2026-07-27):**
- `vdc8244_write` is de enige registeringang; `vdc8244_begin_line`/`vdc8244_render_line` maken een
  render-only pad mogelijk. Geen floats in `cpu8048.c`/`sys.c` → deterministisch.
- Audio ontstaat in dezelfde VDC (2 samples per scanlijn) en komt dus gratis mee.
- Twee routes:
  1. **Deterministische lockstep** (door gebruiker gekozen): beide kanten draaien de volledige
     emulatie, alleen frame-genummerde input over de lijn (~50 B/s). Vereist identieke BIOS+ROM
     (host stuurt ze over het DataChannel, met CRC-verificatie tegen `games.json`) + identieke
     core-versie (handshake op `G7K_VERSION_STRING`) + desync-detectie (savestate-hash) met
     savestate-resync als herstel. Input-delay 3-5 frames.
  2. Alternatief (niet gekozen, wel gedocumenteerd): per-scanlijn registerlog streamen — gast
     heeft dan géén ROM nodig, ~10-30 kB/s, geen desync-risico.
- Verificatie-idee: framebuffer-hash per frame moet host==gast (`tools/g7k_run` heeft al FNV-hash).
- Aandachtspunt: ROM-overdracht host→gast is peer-to-peer distributie van copyrighted materiaal —
  juridisch zwaarder dan de huidige stream; alternatief is de gast dezelfde ROM uit `games.json`
  laten halen (zelfde CRC) i.p.v. hem te versturen. **Beslispunt voor de volgende sessie.**

## Overige open punten

- Drive-upload-config ontbreekt op deze Mac (`~/.config/fi/drive-config.json`) — APK's staan wel op HorseAPK
- 5× architectuur-viewer (sanitycheck-P2), PRINCIPLES/DEPENDENCIES-split (P3)
- Android/SteamDeck-frontends van de emulator zelf nog placeholders
- VP+-fase (EF9340/41) — `games.json` heeft al een geverifieerde `bios_g7400`-bron
