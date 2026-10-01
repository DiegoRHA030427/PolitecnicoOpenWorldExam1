# Test matrix — Multiplayer username limit (16 characters)

[← Back to index](../README.md)

## Acceptance criteria

| ID | Criterion |
|---|---|
| AC1 | Happy path: typing `Diego` and tapping *Connect* joins the map as `Diego`. |
| AC2 | Boundary: after 16 characters, additional typed characters are ignored. |

## Risks

| ID | Risk | Impact | Covered by |
|---|---|---|---|
| R1 | The cap alters short, valid names | High — breaks the most common flow | CP-01 |
| R2 | The cap does not hold at the boundary (16 → 17) | Medium — AC2 not met | CP-02 |
| R3 | The blank-name fallback `Jugador_XXXX` stops working | Medium — player joins with no name | CP-03 |
| R4 | A 16-character name does not fit with large font | Low — visual / accessibility issue | CP-05 |

## Common data

| Field | Value |
|---|---|
| Author | Diego Raymundo Hernández Alonso ([@DiegoRHA030427](https://github.com/DiegoRHA030427)) |
| Date | 2026-10-01 |
| Tested SHA | `fbcd55cdb6d571279c404f0cde0631a904b93785` (baseline: `7ed325393f82872c2be94ff2ada46948efa19152`) |
| App version | `1.0.0.18` (debug build from branch `fix/limit-multiplayer-username`) |
| Devices | Emulator *Medium Phone*, Android 17 (API 37), x86_64 · Physical Android phone (second multiplayer client) |
| Test data | Fictitious usernames only (`Diego`, `YaEstaLimitadoYa`, `DiegoHernandezAlonso`) |

## Test cases

### CP-01 — Happy path (AC1, R1)

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

### CP-02 — Boundary (AC2, R2)

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

### CP-03 — Regression: blank-name fallback (R3)

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

### CP-04 — Navigation and state

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

### CP-05 — Accessibility: large font (R4)

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

### CP-06 — Compatibility: emulator vs physical device

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

## Summary

| Case | Type | Status |
|---|---|---|
| CP-01 | Happy path | ✅ Passed |
| CP-02 | Boundary | ✅ Passed |
| CP-03 | Regression | ✅ Passed |
| CP-04 | Navigation / state | ✅ Passed |
| CP-05 | Accessibility | ✅ Passed |
| CP-06 | Compatibility | ✅ Passed |

**6 / 6 passed.** No defects found in the scope of the acceptance criteria.
