---
date: 2026-07-27
repo: Meta_VideopacHorse
status: open
resume: "VideopacHorse v0.5.1-Kerstens staat LIVE: netplay IS /videopac/, streamversie gearchiveerd op /videopac/stream/. Open: telefoon-joystick op echt toestel testen met de nieuwe joystickcodes, VP+-fase, architectuur-viewers"
---

# Sessie 2026-07-27 (avond) — v0.5.0-Veiga: drie codes + netplay op /videopac/2/

Vervolg op `2026-07-27_v040_controller_pairing_netplay_open.md`. Lock-step over 6 repo's.

## Opdracht

1. "verander logica: als sessie wordt gestart: toon 3 codes: joystick player 1 (host),
   joystick player 2 (gast), gast sessie code."
2. "bouw in /videopac/2 versie waarbij beeld host en gast tegelijk vanuit emulatie
   gerenderd worden ipv beeld over de lijn"

Beslispunt vooraf voorgelegd en beantwoord: gast en telefoon op speler 2 **sluiten elkaar
niet uit maar tellen op** (OR), zoals toetsenbord en gamepad dat al deden.

## Deel 1 — drie codes (`/videopac/`)

`pair-create` levert nu `code` (gast), `ctrl_code_p1`, `ctrl_code_p2`. De rol zit in de
code; de server wijst niets meer toe.

- **Wat daarmee vervalt:** de slot-toewijzing in `ctrl-join` ("laagste vrije slot"), de
  kruisvalidatie in `pair-join` (de BUG-009-fix), `CTRL_MAX` als teller én
  `guestOwnsPlayer2()` + de dempingstak in `pushJoy`. Netto minder code, geen 409's meer
  tussen rollen die niets met elkaar te maken hebben.
- **Wat erbij komt:** idempotente DB-migratie (`PRAGMA table_info` + `ALTER TABLE`),
  UNIQUE-index per codekolom en op `(session_token, slot)`, en overname van een plek
  waarvan de telefoon >60 s stil is (anders moet je op de GC wachten precies wanneer je
  snel terug wilt).
- Android-app blijft protocol-compatibel (server leidt het slot af uit de code); alleen
  teksten aangepast + melding "je deelt speler 2 met de gast".

## Deel 2 — netplay (`/videopac/2/`)

Beide kanten draaien dezelfde emulatie; alleen invoer gaat over de lijn.

- **Geen kopie van de app:** `/videopac/2/` hergebruikt `../app.js`, `../style.css` en
  dezelfde wasm via `window.VPH_BASE`/`VPH_API`. De CSS is daarvoor uit `index.html`
  gehaald naar `web/style.css`.
- **Assets:** host stuurt CRC's; de gast haalt BIOS en cartridge **zelf** op (IndexedDB of
  dezelfde archive.org-bron als de GAMES-lijst). Alleen als dat onmogelijk is, komt het
  bestand peer-to-peer van de host — met melding in beeld. Er gaan geen ROM-bytes via HC55.
- **Lockstep:** invoer `delay` frames vooruit gepland (start 4, volgt de RTT, gaat alleen
  omhoog). Onbetrouwbaar DataChannel voor invoer (met 10 frames redundantie), betrouwbaar
  kanaal voor handshake/savestates/hashes. Speler 1 = host-bijdrage, speler 2 = host |
  gast — dus een telefoon op de P2-code werkt bij de gast net zo goed door.
- **Desync:** elke 60 frames een FNV-hash over de volledige savestate; bij verschil zet de
  host de gast bij met een savestate.
- **Onderbrekingen worden uitgelegd:** een tabblad dat niet zichtbaar is bevriest rAF, dus
  de lockstep laat de ander wachten. Nu meldt de pagina "je medespeler heeft het tabblad
  weggeklikt" resp. "wachten op je medespeler…" in plaats van stil te bevriezen.

## Gates (alles groen, ook tegen de LIVE-server)

| Gate | Uitkomst |
|---|---|
| `VideopacHorse_Core` — `make test` | **86/86** (voor het eerst zónder skip, zie hieronder) |
| `VideopacHorse_Core` — `make netcheck` (nieuw) | determinisme + desync-detectie + savestate-herstel; 9 varianten (4 seeds × PAL/NTSC + zonder cart) groen |
| `tests/api_test.sh` | 20/20 — rollen, één telefoon per plek, 6 gelijktijdige joins → precies 1 winnaar |
| `tests/pairplay_e2e.py` | 9/9 — gast joint mét 2 telefoons; host ziet stand **24 = 8 \| 16** op speler 2 |
| `tests/netplay_e2e.py` | 20/20 — state-hash gelijk op hetzelfde framenummer, telefoon-P2 werkt door tot bij de gast, onderbrekingen gemeld |
| Live op HC55 | pairplay 9/9 + netplay 20/20 tegen `https://horsecloud55.ddns.net/videopac/` |

Gemeten snelheid live, twee losse browsers: **44-49 van de 50 PAL-frames/s**, RTT 26-28 ms,
0 desyncs.

## Bevindingen onderweg

- **`test_M7_real_rom_vp01pl` stond sinds de start op SKIP.** De suite meldde 85 pass +
  1 skip en zag er groen uit; met de juiste ROM (Videopac nr 1 "plus", 8 KB, CRC EE3EE642)
  draait hij wél. Aandachtspunt: de test geeft FAIL zowel bij een kapotte emulator als bij
  een verkeerd bestand met dezelfde naam.
- **Versie-drift in de joystick-app** (BUG-013): `build.gradle.kts` bouwde 0.4.0-Rusch
  terwijl de repo 0.4.2 was. Leest nu `version.json`.
- **Twee testvalkuilen** die eerst vals groen gaven: twee lege canvassen zijn ook "gelijk",
  en twee canvassen op hetzelfde *klokmoment* vergelijken deugt niet — de machines mogen
  frames uit elkaar lopen. De test vergelijkt nu de state-hash bij hetzelfde *framenummer*.
- **fps-teller telde rAF-ticks**, niet emulatieframes; tijdens netplay toont hij nu de
  gemeten emulatiesnelheid.

## Deploy

`rsync web/ → /var/www/videopac/`. Geen nginx-wijziging nodig: `/videopac/2/` valt onder de
bestaande `location /videopac/`. Backups vóór de deploy: `/root/videopac-www.backup-voor-v050-*`,
`/root/videopac-pairing.db.backup-voor-v050-*`, `/root/videopac-locations.conf.bak-voor-v050-*`.
DB-migratie geverifieerd op de productiedatabase (kolommen + indexen aanwezig).

## Open

- Telefoon-joystick op een **echt toestel** testen met de nieuwe joystickcodes (APK
  v0.5.0-Veiga staat klaar).
- Netplay-verfijning: gast kan zelf nog geen telefoon-joystick koppelen (ctrl-poll is
  host-only); regio wisselen kan alleen buiten een sessie.
- Ongewijzigd open: architectuur-viewers ×5, Android/SteamDeck-frontends, VP+-fase
  (EF9340/41), PRINCIPLES/DEPENDENCIES-split, Manopac-toestemming.

## Naschrift — v0.5.1-Kerstens: netplay is de gewone versie geworden

Opdracht aansluitend: "archiveer de schermstream-versie op /videopac en vervang door de
simulatieversie van /videopac/2".

- `/videopac/` = netplay (was `/videopac/2/`); `/videopac/stream/` = de gearchiveerde
  streamversie; `/videopac/2/` blijft bestaan als doorverwijzing, want die URL staat in
  documentatie en bladwijzers — een dode link is een slechtere afloop.
- In de repo: `web/index.html` + `web/netplay.js` zijn de hoofdpagina, `web/stream/`
  bevat het archief (met `VPH_BASE`/`VPH_API` naar boven), `web/2/index.html` is de
  redirect. `build.sh` bustert alle drie de pagina's.
- Deploy met `rsync --delete` na een dry-run: alleen de twee verhuisde bestanden
  verdwenen. Backup: `/root/videopac-www.backup-voor-v051-*`.
- Gates opnieuw: lokaal 49/49, live 29/29 (netplay 20/20 op `/videopac/`, archief 9/9 op
  `/videopac/stream/`), `make netcheck` groen, core 86 tests groen.

**Geprobeerd en teruggedraaid** (staat in `docs/BUGLIST.md`): de host een savestate laten
sturen als hij lang wacht. Aanname was "de ander loopt achter" — precies verkeerd: wie
stalt is zélf de kant die niet verder kan. Gemeten gevolg: host bleef op frame 15 steken
met 8 resyncs in 20 s. Zonder dat mechanisme herstelt een onderbreking van 2,5 s én van
6 s in **0,2-0,3 s**, want de wachtende kant is zelf ook gestopt en er is hooguit `delay`
frames in te halen.
