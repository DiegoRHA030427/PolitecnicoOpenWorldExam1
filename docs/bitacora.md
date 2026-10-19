# Individual log — Diego Raymundo Hernández Alonso (@DiegoRHA030427)

[← Back to index](../README.md)

## My commits

### Code — Pull Request [#167](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/167) (branch `fix/limit-multiplayer-username`)

| SHA | Date | Version | Message | Files |
|---|---|---|---|---|
| [`fbcd55cd`](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/commit/fbcd55cdb6d571279c404f0cde0631a904b93785) | 2026-10-01 | v1 | fix: limit username to 16 characters | `MainMenuScreen.kt` |
| [`35bb1aef`](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/commit/35bb1aef) | 2026-10-19 | v2 | Merge upstream/main into fix/limit-multiplayer-username | — (sync, no conflicts) |
| [`8bc78e5a`](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/commit/8bc78e5a) | 2026-10-19 | v2 | feat: add PlayerNameRules for multiplayer username validation | `PlayerNameRules.kt`, `PlayerNameRulesTest.kt` |
| [`6116d70d`](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/commit/6116d70d) | 2026-10-19 | v2 | feat: validate username in the Connect to Server dialog | `MainMenuScreen.kt`, `strings.xml` (es / en) |
| [`3207af6b`](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/commit/3207af6bb68f80d850191d7369765edc891c012c) | 2026-10-19 | v2 | fix: cap remote player names before drawing them on the map **(final SHA)** | `WorldMapMultiplayer.kt` |

### Documentation — academic branch `qa/entrega`

| SHA | Date | Message |
|---|---|---|
| `d1c2cad6` | 2026-10-01 | docs: add before screenshots |
| `f45a0f4a` | 2026-10-01 | docs: add delivery index README |
| `ea77f455` | 2026-10-01 | docs: add test matrix, individual log and complete README |
| `80883101` | 2026-10-01 | docs: add screenshots for CP-01, CP-03, CP-04 and CP-05 |
| `75e528b3` | 2026-10-19 | docs: update README for PR v2 (username validation, new evidence) |
| `d702c147` | 2026-10-19 | docs: keep only completed checks in README |
| `9296ed5f` | 2026-10-19 | docs: add local Gradle run result to README |
| `d5e298fe` | 2026-10-19 | docs: remove server change from README, final SHA 3207af6b |
| _this commit_ | 2026-10-19 | docs: complete test matrix (v2), individual log and CI status |

## Test cases I executed

All 12 cases of the [test matrix](pruebas.md) were executed by me:

| Cases | Version | Date | SHA | Device |
|---|---|---|---|---|
| CP-01 to CP-06 | v1 | 2026-10-01 | `fbcd55cd` | Emulator Medium Phone (API 37) + physical Android phone |
| CP-07 to CP-11 | v2 | 2026-10-19 | `3207af6b` | Emulator Medium Phone (API 37) |
| CP-12 (unit tests + Gradle) | v2 | 2026-10-19 | `3207af6b` | JVM / Windows Gradle wrapper |

## Peer review

| Role | Link | Notes |
|---|---|---|
| Review I gave | [PR #156 — accessibility text clipping at large font sizes (by @Julio-Olascoaga)](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/156#pullrequestreview-5385457073) | Approved on 2026-10-01. PR #156 was later merged into `main`. |
| Review I received | [QA review by @Julio-Olascoaga on PR #167](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/167) | Minor finding: a long name saved before the fix was shown untruncated when the dialog opened. Fixed in `6116d70d` (the saved name is normalized and capped at 16 when the dialog is prefilled). |
| Review I received | [Approval by @MiguelCruz10 on PR #167](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/167) | Approved (*QA Complete*). |
