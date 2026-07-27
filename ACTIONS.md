# ACTIONS — Meta_VideopacHorse

- [ ] **NETPLAY (eerst volgende sessie)** — BIOS+ROM-uitwisseling host→gast + deterministische lockstep i.p.v. videostream; analyse + beslispunt (ROM versturen vs. gast laadt zelf uit games.json) staat in `prompts/2026-07-27_v040_controller_pairing_netplay_open.md` (27-07)
- [ ] **Telefoon-joystick op echt toestel testen** — APK v0.4.0-Rusch via HorseAPK; koppelen met sessiecode, max 2 (27-07)
- [x] **S4 toetsenbord-matrixtabel** — `g7k_key_from_char` afleiden + testen tegen echte BIOS-scanroutine; daarna console-toetsenbord in de web-UI activeren (26-07) [bron: QUIRKS.md S4 + noot 1] — AFGEROND 27-07 (v0.1.2-Boris resp. archive.org-BIOS)
- [x] **Game-smoke met echte BIOS** — Christian levert `o2rom.bin` (md5 562d5ebf9e030a40d6fabfc2f33139fd) aan; dan `tools/g7k_run --bios ... --cart vp_14.bin` + browser-test met Gunfighter/Thunderball/vp_01pl (26-07) — AFGEROND 27-07 (v0.1.2-Boris resp. archive.org-BIOS)
- [ ] **Architectuur-viewers ×5** — `architectuur/<repo>_viewer.html` per repo (sanitycheck-P2, geparkeerd t.b.v. web-livegang) (26-07)
- [ ] **VideopacHorse_Android promotie** — volledig Gradle-project + APK via HorseAPK-flow (eigen Oranje release) (26-07)
- [ ] **VideopacHorse_SteamDeck promotie** — Flatpak-bundle bouwen + /Deploy2SteamDeck e2e (26-07)
- [ ] **Manopac-toestemming** — Mark Guttenbrunner (videopac.nl) vragen of de hb-tools-ROMs in een publieke testsuite mogen (26-07) [bron: O2EM_DEEPDIVE.md §5]
- [ ] **VDC-interpretaties herzien** — 8 gedocumenteerde v0.1-canonkeuzes toetsen zodra echte-ijzer/BIOS-metingen beschikbaar zijn (26-07) [bron: QUIRKS.md noten 4-6]
- [ ] **docs/PRINCIPLES.md + DEPENDENCIES.md** als aparte bestanden per repo (sanitycheck-P3) (26-07)
