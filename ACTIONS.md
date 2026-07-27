# ACTIONS — Meta_VideopacHorse

- [x] **ARCHITECTUUR: aparte koppelcode voor joysticks** — AFGEROND 27-07 (v0.5.0-Veiga). Drie codes in plaats van twee: gastcode + joystickcode per speler. De rol volgt uit de code, dus de server wijst niets meer toe; gast en telefoon op speler 2 tellen bij elkaar op in plaats van elkaar uit te sluiten. Zie `prompts/2026-07-27_v050_drie_codes_en_netplay.md`.
- [x] **NETPLAY** — AFGEROND 27-07 (v0.5.0-Veiga). `/videopac/2/`: beide kanten draaien dezelfde emulatie (deterministische lockstep), de gast haalt BIOS+cartridge zélf op, desync-detectie via state-hash met savestate-resync. Gate: `make netcheck` in de Core. Beslispunt ROM-overdracht opgelost: publieke bron eerst, peer-to-peer alleen als laatste redmiddel en met melding.
- [ ] **Telefoon-joystick op echt toestel testen** — APK **v0.5.0-Veiga** via HorseAPK; koppelen met een **joystickcode** (P1 of P2), één telefoon per plek (27-07)
- [x] **S4 toetsenbord-matrixtabel** — `g7k_key_from_char` afleiden + testen tegen echte BIOS-scanroutine; daarna console-toetsenbord in de web-UI activeren (26-07) [bron: QUIRKS.md S4 + noot 1] — AFGEROND 27-07 (v0.1.2-Boris resp. archive.org-BIOS)
- [x] **Game-smoke met echte BIOS** — Christian levert `o2rom.bin` (md5 562d5ebf9e030a40d6fabfc2f33139fd) aan; dan `tools/g7k_run --bios ... --cart vp_14.bin` + browser-test met Gunfighter/Thunderball/vp_01pl (26-07) — AFGEROND 27-07 (v0.1.2-Boris resp. archive.org-BIOS)
- [ ] **Architectuur-viewers ×5** — `architectuur/<repo>_viewer.html` per repo (sanitycheck-P2, geparkeerd t.b.v. web-livegang) (26-07)
- [ ] **VideopacHorse_Android promotie** — volledig Gradle-project + APK via HorseAPK-flow (eigen Oranje release) (26-07)
- [ ] **VideopacHorse_SteamDeck promotie** — Flatpak-bundle bouwen + /Deploy2SteamDeck e2e (26-07)
- [ ] **Manopac-toestemming** — Mark Guttenbrunner (videopac.nl) vragen of de hb-tools-ROMs in een publieke testsuite mogen (26-07) [bron: O2EM_DEEPDIVE.md §5]
- [ ] **VDC-interpretaties herzien** — 8 gedocumenteerde v0.1-canonkeuzes toetsen zodra echte-ijzer/BIOS-metingen beschikbaar zijn (26-07) [bron: QUIRKS.md noten 4-6]
- [ ] **docs/PRINCIPLES.md + DEPENDENCIES.md** als aparte bestanden per repo (sanitycheck-P3) (26-07)
- [ ] **Netplay-verfijning** — gast kan zelf nog geen telefoon-joystick koppelen (`ctrl-poll` is host-only); regio wisselen kan alleen buiten een sessie (27-07)
- [ ] **Core-test M7 onderscheidt "verkeerde ROM" niet van "emulator kapot"** — beide geven FAIL; een verkeerd bestand met de juiste naam hoort een duidelijke melding te geven (27-07)
