# QA Midterm — Mobile Apps: Multiplayer username length limit

Delivery index for the Quality Assurance project on
[gabrielhuav/PolitecnicoOpenWorld](https://github.com/gabrielhuav/PolitecnicoOpenWorld).
This document and the `docs/` folder live only on the academic branch `qa/entrega` of my fork;
they are not part of the game code or the Pull Request.

## 1. Team

| Member | GitHub user | Group |
|---|---|---|
| Diego Raymundo Hernández Alonso | [@DiegoRHA030427](https://github.com/DiegoRHA030427) | 7CV4 |

## 2. Goal

Limit the username typed in the Multiplayer *Connect to Server* dialog to **16 characters**,
so the label rendered above the player sprite no longer grows without bound and covers
other players' screens.

## 3. Scope

**In scope**

- *Username* field of the *Connect to Server* dialog (`MainMenuScreen.kt`, `shared` module).
- Input is capped at 16 characters while typing or pasting, and again when tapping *Connect*
  (covers a long name that was already saved).
- The `Jugador_XXXX` fallback name is kept when the field is left blank.

**Out of scope**

- Changes to the multiplayer server or the network message format.
- Validation of special characters or duplicate names.
- Other game modes (Free Roam, Story Mode, Titulación por Combate).

## 4. Traceability links

| Item | Link / value |
|---|---|
| Issue | [DiegoRHA030427/PolitecnicoOpenWorldExam1#1](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1/issues/1) |
| Pull Request | [gabrielhuav/PolitecnicoOpenWorld#167](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/167) |
| Change branch | `fix/limit-multiplayer-username` |
| Base SHA | [`7ed325393f82872c2be94ff2ada46948efa19152`](https://github.com/gabrielhuav/PolitecnicoOpenWorld/commit/7ed325393f82872c2be94ff2ada46948efa19152) |
| Final delivered SHA | _Pending — declared when QA is closed._ |
| App version | `1.0.0.18` |

## 5. Test matrix

_Pending — will be documented in `docs/pruebas.md`._

## 6. Evidence: before and after

### 6.1 *Connect to Server* dialog

| Before (base SHA `7ed32539`) | After (fix branch) |
|---|---|
| <img src="docs/evidencias/antes_dialogo.jpeg" width="260" alt="Before: the field accepts a name longer than 16 characters"> | <img src="docs/evidencias/despues_dialogo.jpeg" width="260" alt="After: the field stops at 16 characters"> |
| The field accepts a name with no limit (`NoTieneLimiteNoTieneLimiteNoTiene…`). | The field stops at 16 characters (`YaEstaLimitadoYa`). |

### 6.2 Name label on the map (seen by another player)

**Before:** the unlimited name is drawn as a strip across the whole screen,
on top of the controls and the HUD.

| Capture 1 | Capture 2 | Capture 3 |
|---|---|---|
| <img src="docs/evidencias/antes_mapa_1.jpeg" width="300" alt="Before: long label across the screen, close view"> | <img src="docs/evidencias/antes_mapa_2.jpeg" width="300" alt="Before: long label across the screen, medium view"> | <img src="docs/evidencias/antes_mapa_3.jpeg" width="300" alt="Before: long label across the screen, far view"> |

**After:** the label takes a bounded space next to the player.

<img src="docs/evidencias/despues_mapa.jpeg" width="600" alt="After: bounded YaEstaLimitadoYa label">

## 7. Automated checks

_Pending._

## 8. Peer review

_Pending._

## 9. Conclusions

_Pending._

## 10. References

- [Upstream repository: gabrielhuav/PolitecnicoOpenWorld](https://github.com/gabrielhuav/PolitecnicoOpenWorld)
- [Working fork: DiegoRHA030427/PolitecnicoOpenWorldExam1](https://github.com/DiegoRHA030427/PolitecnicoOpenWorldExam1)
