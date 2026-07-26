# DUPLICATES.md — VideopacHorse-familie (geregistreerde bewuste duplicaties)

Conform sanitycheck-regel "DRY-met-akkoord". Familie-breed register (sub-master).

## DUP-001 — version.json lock-step-spiegel ×5

| Veld | Waarde |
|---|---|
| id | DUP-001 |
| bestanden | `{Meta_VideopacHorse,VideopacHorse_Core,_Web,_Android,_SteamDeck}/version.json` (integraal) |
| regels | 8 per kopie |
| similarity | 100% |
| reden | lock-step-versionering is een familie-ontwerpprincipe (zelfde patroon als AmigaHorse/DOSHorse); elke repo moet zelfstandig zijn versie kennen zonder cross-repo read |
| alternatief-overwogen | centrale versie in Meta + symlink/submodule — verworpen: breekt zelfstandige clones en GitHub-weergave |
| akkoord-door | cpaglebbeek |
| akkoord-datum | 2026-07-26 ("ga verder" op sanitycheck-P2-voorstel) |

## DUP-002 — port-sessie-MDs 2026-07-26 ×3

| Veld | Waarde |
|---|---|
| id | DUP-002 |
| bestanden | `{_Web,_Android,_SteamDeck}/prompts/2026-07-26_skeleton.md` (identiek op repo-naam na) |
| regels | 13 per kopie |
| similarity | ~95% |
| reden | protocol "één sessie-MD per geraakte repo met kruisverwijzing" (Meta_Master CLAUDE.md); inhoud is bewust een pointer naar de hoofd-MD in Meta |
| alternatief-overwogen | alleen hoofd-MD — verworpen: dan verschijnen de port-repos niet in prompts-audits |
| akkoord-door | cpaglebbeek |
| akkoord-datum | 2026-07-26 |

## DUP-003 — .gitignore-basisblok ×4 + ROM-blokkade ×5

| Veld | Waarde |
|---|---|
| id | DUP-003 |
| bestanden | `.gitignore` in alle 5 repos (Core uitgebreider) |
| regels | 4-15 |
| similarity | 80-100% |
| reden | de ROM/BIOS-blokkade (`*.bin`,`*.rom`) is een juridische verdedigingslinie die in ELKE repo zelfstandig moet gelden, ook bij losse clone |
| alternatief-overwogen | global gitignore — verworpen: beschermt andere machines/collaborators niet |
| akkoord-door | cpaglebbeek |
| akkoord-datum | 2026-07-26 |

*(LICENSE ×5 valt onder het ignore-patroon licentie-headers/boilerplate.)*
