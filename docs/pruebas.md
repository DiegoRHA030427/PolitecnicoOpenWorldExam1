# Test matrix — Multiplayer username validation (v1: 16-character limit · v2: full validation)

[← Back to index](../README.md)

## v1 — 16-character limit

### Acceptance criteria (v1)

| ID | Criterion |
|---|---|
| AC1 | Happy path: typing `Diego` and tapping *Connect* joins the map as `Diego`. |
| AC2 | Boundary: after 16 characters, additional typed characters are ignored. |

### Risks (v1)

| ID | Risk | Impact | Covered by |
|---|---|---|---|
| R1 | The cap alters short, valid names | High — breaks the most common flow | CP-01 |
| R2 | The cap does not hold at the boundary (16 → 17) | Medium — AC2 not met | CP-02 |
| R3 | The blank-name fallback `Jugador_XXXX` stops working | Medium — player joins with no name | CP-03 |
| R4 | A 16-character name does not fit with large font | Low — visual / accessibility issue | CP-05 |

### Common data (v1)

| Field | Value |
|---|---|
| Author | Diego Raymundo Hernández Alonso ([@DiegoRHA030427](https://github.com/DiegoRHA030427)) |
| Date | 2026-10-01 |
| Tested SHA | `fbcd55cdb6d571279c404f0cde0631a904b93785` (baseline: `7ed325393f82872c2be94ff2ada46948efa19152`) |
| App version | `1.0.0.18` (debug build from branch `fix/limit-multiplayer-username`) |
| Devices | Emulator *Medium Phone*, Android 17 (API 37), x86_64 · Physical Android phone (second multiplayer client) |
| Test data | Fictitious usernames only (`Diego`, `YaEstaLimitadoYa`, `DiegoHernandezAlonso`) |

### Test cases (v1)

#### CP-01 — Happy path (AC1, R1)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, portrait, default font |
| Preconditions | App installed from the fix branch; main menu open |
| Steps | 1. Tap **MULTIPLAYER**. 2. Wait for the server to wake up. 3. Type `Diego`. 4. Tap **Connect**. |
| Expected | The field shows `Diego` unchanged and the map loads. |
| Actual | The field showed `Diego`; the map loaded. |
| Status | ✅ Passed |
| Evidence | Observed during the emulator run · [Map](evidencias/cp01_mapa_diego.png) |
| Defect | — |
| Decision | Accept |

#### CP-02 — Boundary (AC2, R2)

| Field | Value |
|---|---|
| Configuration | Emulator API 37 + physical phone as second client |
| Preconditions | Same as CP-01 |
| Steps | 1. Open the *Connect to Server* dialog. 2. Type a name longer than 16 characters. 3. Tap **Connect**. 4. Look at the label from the other device. |
| Expected | Input stops at 16 characters and the map label stays short. |
| Actual | Input stopped at `YaEstaLimitadoYa` (16); the label is bounded. Baseline accepted an unlimited name that crossed the whole screen. |
| Status | ✅ Passed |
| Evidence | Before: [dialog](evidencias/antes_dialogo.jpeg), [map 1](evidencias/antes_mapa_1.jpeg), [map 2](evidencias/antes_mapa_2.jpeg), [map 3](evidencias/antes_mapa_3.jpeg) · After: [dialog](evidencias/despues_dialogo.jpeg), [map](evidencias/despues_mapa.jpeg) |
| Defect | — |
| Decision | Accept |

#### CP-03 — Regression: blank-name fallback (R3)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, landscape |
| Preconditions | Dialog open |
| Steps | 1. Delete all text in the field. 2. Tap **Connect**. 3. Go back to the menu and reopen **MULTIPLAYER**. |
| Expected | The map loads and a `Jugador_XXXX` name is generated and saved. |
| Actual | The map loaded; the dialog then showed `Jugador_5237`. |
| Status | ✅ Passed |
| Evidence | Observed during the emulator run · [Empty field](evidencias/cp03_campo_vacio.png), [Map](evidencias/cp03_mapa_fallback.png), [Saved name](evidencias/cp03_nombre_guardado.png) |
| Defect | — |
| Decision | Accept |

#### CP-04 — Navigation and state

| Field | Value |
|---|---|
| Configuration | Emulator API 37, rotated to landscape |
| Preconditions | Connected to the map as `Diego` |
| Steps | 1. Press **Back**. 2. Tap **Back to Menu**. 3. Rotate to landscape. 4. Reopen **MULTIPLAYER**. |
| Expected | The dialog shows the saved name `Diego`. |
| Actual | The dialog showed `Diego` in landscape. |
| Status | ✅ Passed |
| Evidence | Observed during the emulator run · [Dialog](evidencias/cp04_dialogo_landscape.png) |
| Defect | — |
| Decision | Accept |

#### CP-05 — Accessibility: large font (R4)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, landscape, font size set to maximum |
| Preconditions | Dialog open |
| Steps | 1. Set the font size to maximum. 2. Open the dialog. 3. Type 20 characters. |
| Expected | The field stops at 16 and the full name is visible. |
| Actual | The field showed `Jugador_5237asdf` (16) completely; the dialog did not break. |
| Status | ✅ Passed |
| Evidence | Observed during the emulator run · [Empty field](evidencias/cp05_fuente_grande_vacio.png), [16 chars](evidencias/cp05_fuente_grande_16.png) |
| Defect | — |
| Decision | Accept |

#### CP-06 — Compatibility: emulator vs physical device

| Field | Value |
|---|---|
| Configuration | Emulator API 37 and physical Android phone |
| Preconditions | Same build installed on both |
| Steps | 1. Repeat CP-02 on the emulator. 2. Check the result from the physical phone. |
| Expected | Same behavior on both devices. |
| Actual | The 16-character limit holds on the emulator and the label renders correctly on the phone. |
| Status | ✅ Passed |
| Evidence | [Emulator dialog](evidencias/despues_dialogo.jpeg), [phone map](evidencias/despues_mapa.jpeg) |
| Defect | — |
| Decision | Accept |

---

## v2 — Username validation (after the professor's feedback)

### Acceptance criteria (v2)

| ID | Criterion |
|---|---|
| AC3 | The field shows a live `x/16` character counter. |
| AC4 | A name with fewer than 3 characters shows *At least 3 characters* and disables **Connect**. |
| AC5 | A name with characters other than letters (accents included), digits, space and `_ - .` shows *Only letters, numbers, spaces and _ - .* and disables **Connect**. |
| AC6 | A blank name still allows **Connect** and generates the `Jugador_XXXX` fallback (regression of AC/R3). |

### Risks (v2)

| ID | Risk | Impact | Covered by |
|---|---|---|---|
| R5 | The new validation blocks or breaks the multiplayer connection | High — players cannot join | CP-10, CP-11 |
| R6 | Valid names are rejected (accents, digits, `_ - .`, exactly 3 or 16 characters) | Medium — legitimate players blocked | CP-08, CP-12 |
| R7 | Pasted text with line breaks/tabs, or long names sent by older clients, still produce an oversized label | Medium — label covers the HUD again | CP-12 |

### Common data (v2)

| Field | Value |
|---|---|
| Author | Diego Raymundo Hernández Alonso ([@DiegoRHA030427](https://github.com/DiegoRHA030427)) |
| Date | 2026-10-19 |
| Tested SHA | `3207af6bb68f80d850191d7369765edc891c012c` (final) · base: `f34b7ae4aa1e0bad170dafa76be35f0652b6f0cb` (upstream `main` merged into the branch) |
| App version | `1.0.0.18` (debug build from branch `fix/limit-multiplayer-username`, run from Android Studio) |
| Device | Emulator *Medium Phone*, Android 17 (API 37), x86_64, landscape, default font, app language English |
| Test data | Fictitious usernames only (`Jugador_3756`, `Jug`, `Ju`, `Ju😀`, empty, generated `Jugador_6765`) |

### CP-07 — Happy path: counter with a valid name (AC1, AC3)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, landscape, default font |
| Preconditions | App installed from SHA `3207af6b`; a valid name (`Jugador_3756`) saved from a previous session |
| Steps | 1. Tap **MULTIPLAYER**. 2. Wait for the server warm-up. 3. Observe the prefilled field. |
| Expected | The saved name is shown unchanged with the counter `12/16`; no error; **Connect** enabled. |
| Actual | `Jugador_3756` shown with `12/16`; no error; **Connect** enabled. |
| Status | ✅ Passed |
| Evidence | [v2_01_contador.png](evidencias/v2_01_contador.png) |
| Defect | — |
| Decision | Accept |

### CP-08 — Boundary: minimum length (AC4, R6)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, landscape, default font |
| Preconditions | Dialog open with `Jugador_3756` |
| Steps | 1. Delete characters with the keyboard until the field shows `Jug`. 2. Check the counter and the **Connect** button. 3. Delete one more character (`Ju`). |
| Expected | `Jug` (3) → `3/16`, no error, **Connect** enabled. `Ju` (2) → red field, *At least 3 characters*, **Connect** disabled. |
| Actual | `Jug` showed `3/16` with no error. `Ju` turned the field red with *At least 3 characters* and **Connect** greyed out. |
| Status | ✅ Passed |
| Evidence | [v2_02_muy_corto.png](evidencias/v2_02_muy_corto.png) |
| Defect | — |
| Decision | Accept |

### CP-09 — Alternate condition: invalid character (AC5)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, landscape, default font |
| Preconditions | Dialog open with `Ju` |
| Steps | 1. Open the emoji panel of the keyboard. 2. Insert 😀. 3. Close the emoji panel. |
| Expected | Red field with *Only letters, numbers, spaces and _ - .*; **Connect** disabled. |
| Actual | Field `Ju😀` in red with *Only letters, numbers, spaces and _ - .*; **Connect** greyed out. |
| Status | ✅ Passed |
| Evidence | [v2_03_caracter_invalido.png](evidencias/v2_03_caracter_invalido.png) |
| Defect | — |
| Decision | Accept |

### CP-10 — Regression: blank-name fallback (AC6, R3, R5)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, landscape, default font |
| Preconditions | Dialog open with `Ju😀` |
| Steps | 1. Delete all text. 2. Check the counter and **Connect**. 3. Tap **Connect**. 4. Press **Back** → **Back to Menu**. 5. Tap **MULTIPLAYER** again. |
| Expected | Empty field shows `0/16` with **Connect** enabled; a `Jugador_XXXX` name is generated, saved and prefilled the next time. |
| Actual | `0/16` with **Connect** enabled; after connecting, the dialog was prefilled with `Jugador_6765` (`12/16`). |
| Status | ✅ Passed |
| Evidence | [v2_04_vacio_respaldo.png](evidencias/v2_04_vacio_respaldo.png), [v2_05_respaldo_guardado.png](evidencias/v2_05_respaldo_guardado.png) |
| Defect | — |
| Decision | Accept |

### CP-11 — Regression: multiplayer session still connects (R5)

| Field | Value |
|---|---|
| Configuration | Emulator API 37, Android Studio Logcat filtered by `package:mine` |
| Preconditions | Valid name `Jugador_6765` in the dialog |
| Steps | 1. Tap **Connect**. 2. Read Logcat while the map loads. |
| Expected | The client joins the multiplayer session. |
| Actual | Logcat shows `Multiplayer: Mi rol en esta zona ahora es Host: true`. The map stayed on *Loading streets 25%* because `overpass-api.de` timed out (`Socket timeout`), an external map service unrelated to the username. |
| Status | ✅ Passed |
| Evidence | [v2_06_logcat_conectado.jpg](evidencias/v2_06_logcat_conectado.jpg) |
| Defect | — (the Overpass timeout is external and was not introduced by this change) |
| Decision | Accept |

### CP-12 — Automated: `PlayerNameRulesTest` (R1, R2, R6, R7)

| Field | Value |
|---|---|
| Configuration | JVM (JUnit 4) and Windows Gradle wrapper |
| Preconditions | Branch `fix/limit-multiplayer-username` at `3207af6b` |
| Steps | 1. Run `PlayerNameRulesTest` (20 cases). 2. Run `.\gradlew.bat :shared:testAndroidHostTest`. 3. Run `.\gradlew.bat :app:assembleDebug :app:testDebugUnitTest`. |
| Expected | All tests pass and the app builds. |
| Actual | 20 / 20 passed; both Gradle runs ended in `BUILD SUCCESSFUL`. Cases covered: names kept as typed, exact cut at 16, pasted line breaks/tabs, leading and double spaces, control characters, empty name and fallback, minimum of 3, accents/digits/`_ - .` accepted, symbols and emoji rejected, remote names cut to 16 with `…`. |
| Status | ✅ Passed |
| Evidence | [`PlayerNameRulesTest.kt`](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/blob/3207af6bb68f80d850191d7369765edc891c012c/PolitecnicoOpenWorld/shared/src/commonTest/kotlin/ovh/gabrielhuav/pow/domain/multiplayer/PlayerNameRulesTest.kt) · README section 8 |
| Defect | — |
| Decision | Accept |

## Summary

| Case | Type | Version | Status |
|---|---|---|---|
| CP-01 | Happy path | v1 | ✅ Passed |
| CP-02 | Boundary | v1 | ✅ Passed |
| CP-03 | Regression | v1 | ✅ Passed |
| CP-04 | Navigation / state | v1 | ✅ Passed |
| CP-05 | Accessibility | v1 | ✅ Passed |
| CP-06 | Compatibility | v1 | ✅ Passed |
| CP-07 | Happy path (counter) | v2 | ✅ Passed |
| CP-08 | Boundary (minimum length) | v2 | ✅ Passed |
| CP-09 | Alternate (invalid character) | v2 | ✅ Passed |
| CP-10 | Regression (blank-name fallback) | v2 | ✅ Passed |
| CP-11 | Regression (multiplayer connection) | v2 | ✅ Passed |
| CP-12 | Automated unit tests | v2 | ✅ Passed |

**12 / 12 passed.** No defects found in the scope of the acceptance criteria.
