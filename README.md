# zmk-config — Ferris Sweep (cradio)

ZMK firmware for a 34-key Ferris Sweep on nice_nano_v2.
Brazilian ABNT2 | 11 layers | One-handed mirrorboard | Leader key | Combos

> [!WARNING]
> Accented characters require the OS compose key set to **Shift+Right Alt**.
> See [OS Setup](#os-setup) before using accent leader sequences.

## Overview

Mirrorboard-inspired design — every feature works with either hand alone.
The layout mirrors across halves: leader sequences, combos, and layers
are all positionally mirrored (Q↔P, W↔O, E↔I, R↔U, T↔Y, A↔~, S↔L, …).

- **Leader key** — 61 sequences mapped to single and double-tap patterns
- **Combos** — simultaneous keypresses for ESC, TAB, BSPC, DEL, layers, etc.
- **Home-row mods** — `&mt` on A/S/D/F/H/J/K/L (250ms tapping term)
- **Tap-dance layers** — tap = sticky, double-tap = toggle
- **Thumb Ctrl** — hold inner thumb for Ctrl, tap for leader
- **Compose-key accents** — leader + letter sequences using OS compose system
- **Tri-state Alt-Tab** — hold Alt, tap Tab, release Alt
- **Mouse layer** — 5 buttons, scroll, configurable acceleration

## Hardware & Prerequisites

- 1× Ferris Sweep (cradio) PCB set
- 2× nice_nano_v2 controllers
- [Devbox](https://www.jetify.com/devbox) for building

## OS Setup — Compose Key

The keyboard sends **Shift+Right Alt** to trigger OS compose key. Without
this OS setting, accent sequences will print two separate characters.

| OS | How to set |
|---|---|
| Linux (Wayland) | Settings → Keyboard → Compose Key → Right Alt |
| Linux (X11) | `setxkbmap -option compose:ralt` |
| Windows | Install [WinCompose](https://github.com/samhocevar/wincompose) |

**Verify:** leader → `A` `A` → should produce `á`

## Quick Start

```sh
devbox shell              # enter devbox (auto west init/update)
devbox run build cradio   # build both halves
```

Output: `firmware/<shield>-nice_nano_v2.uf2`

| Command | Builds |
|---|---|
| `devbox run build cradio` | Left + right |
| `devbox run build cradio_left` | Left half only |
| `devbox run build cradio_right` | Right half only |
| `devbox run _build_single nice_nano_v2 settings_reset "" ""` | Clear BLE pairing |

**Flash:** Double-tap RESET → UF2 drive mounts → copy `.uf2` → repeat for other half.

## Layout

### Key Positions

```text
╭──────────────╮ ╭──────────────╮
│ 0  1  2  3  4│ │ 5  6  7  8  9│
│10 11 12 13 14│ │15 16 17 18 19│
│20 21 22 23 24│ │25 26 27 28 29│
╰────────╮30 31│ │32 33╭────────╯
         ╰─────╯ ╰─────╯         */
```

### Mirrorboard Map

Same column, same row, opposite hand:

```text
Q(0)─P(9)  W(1)─O(8)  E(2)─I(7)  R(3)─U(6)  T(4)─Y(5)
A(10)─~(19) S(11)─L(18) D(12)─K(17) F(13)─J(16) G(14)─H(15)
Z(20)─;(29) X(21)─.(28) C(22)─,(27) V(23)─M(26) B(24)─N(25)
```

### Base Layer

```text
╭──────────────╮ ╭──────────────╮
│ Q  W  E  R  T│ │ Y  U  I  O  P│
│ A  S  D  F  G│ │ H  J  K  L  ~│
│ Z  X  C  V  B│ │ N  M  ,  .  ;│
╰────────╮CTL SPC│ │SPC CTL╭────────╯
         ╰LDR SPC╯ ╰SPC LDR╯
```

Inner thumbs (30, 33): hold = Ctrl, tap = leader.
Outer thumbs (31, 32): hold = Shift, tap = Space.

## Features

### Leader Key (61 sequences)

Thumb tap enters leader mode. Then press a sequence:

| Group | Sequence | Action |
|---|---|---|
| **App** | `Space` | KRunner/Spotlight (`Alt+Space`) |
| **Layers** | `F` / `J` | SYMBOL (sticky) |
| | `D` / `K` | SYMBOL_MIRROR (sticky) |
| **Terminal** | `T T` / `Y Y` | New terminal (`Ctrl+Alt+T`) |
| | `T G` / `Y H` | Terminal tab (`Ctrl+Shift+T`) |
| **Ctrl+Alt+Del** | `Q` / `P` | System lock |
| **F-keys** | `V` + letter (left) / `M` + letter (right) | F1-F12 |
| **Alt+F4** | `A R` (left) / `~ U` (right) | Close window |
| **Accents** | `A A` / `~ ~` (á), `E E` / `I I` (é), `S S` / `L L` (í), `W W` / `O O` (ó), `R R` / `U U` (ú), `C C` / `, ,` (ç), … | 24 sequences |

Accent pattern: same key = acute, +G/+H = grave, +T/+Y = tilde,
+C/+, = circumflex.

### Combos (chords)

Press two keys simultaneously (within ~50ms):

| Combo | Keys | Action |
|---|---|---|
| ESC | 1+2 / 7+8 | Escape |
| TAB | 2+3 / 6+7 | Tab |
| BSPC | 21+22 / 27+28 | Backspace |
| DEL | 22+23 / 26+27 | Delete |
| RET | 11+12 / 17+18 | Enter |
| BR_CEDIL | 10+11 / 18+19 | ç |
| Alt-Tab | 2+12 / 7+17 | Task switcher |
| GUI | 0+30 / 9+33 | Sticky Super |
| ALT | 10+30 / 19+33 | Sticky Alt |
| Caps Word | 13+14 / 15+16 | Caps until Space |
| Lock Screen | 23+24 / 25+26 | Lock session |
| BASE toggle | 14+24 / 15+25 | Return to base layer |
| RESET | 20+24 / 25+29 | Reboot MCU |
| Bootloader | 0+10+20 / 9+19+29 | Flash mode |

Layer access combos use tap-dance — tap once for sticky layer,
tap twice to toggle. See [Layer Summary](#layer-summary).

### Home-Row Mods

`A`/`S`/`D`/`F` on left, `H`/`J`/`K`/`L` on right:
tap = letter, hold = Ctrl/Shift/Alt/GUI with 250ms tapping term.

### Mouse Layer

Access via combo (4+14 / 5+15) tap-dance. Movement on home row
(wasd-style arrows), scroll on top row, buttons left/right thumb.

```dts
// Acceleration tuning
&mmv { acceleration-exponent = <1>; time-to-max-speed-ms = <500>; }
&msc { acceleration-exponent = <1>; time-to-max-speed-ms = <40>; }
```

## Layers

| Layer | Access | Contents |
|---|---|---|
| BASE (0) | Default | QWERTY |
| BASE_MIRROR (1) | Combo 12+13 / 16+17 | Positional mirror of BASE |
| NUMBER (2) | Combo 24+30 / 25+33 (tap) | Numbers 0-9 |
| NUMPAD (3) | Combo 24+30 / 25+33 (tap×2) | Numpad layout |
| SYMBOL (4) | Leader → F / J | Brackets, operators, dead keys |
| SYMBOL_MIRROR (5) | Leader → D / K | Mirrored symbols |
| NAVIGATION (6) | Combo 3+13 / 6+16 (tap) | Arrows, Home/End, PgUp/PgDn |
| MULTIMEDIA (7) | Combo 4+24 / 5+25 (tap×2) | Brightness, volume, prev/play/next |
| FUNCTION (8) | Combo 14+30 / 15+33 (tap) | F1-F12 |
| BLUETOOTH (9) | Combo 0+4 / 5+9 | BLE profile 0-7, USB/BLE toggle |
| MOUSE (10) | Combo 4+14 / 5+15 (tap) | Mouse movement, scroll, clicks |

## Dependencies

Pinned to [urob ZMK modules](https://github.com/urob) v0.3:

- `zmk-adaptive-key`, `zmk-auto-layer`, `zmk-helpers`, `zmk-leader-key`,
  `zmk-tri-state`, `zmk-unicode`

## Bluetooth & Power

- 9 BLE connections (experimental), 8dB TX power
- `CONFIG_ZMK_SLEEP=y`, idle timeout 30 min
- Pairing reset via `settings_reset` build target

## Project Structure

| Path | Role |
|---|---|
| `config/cradio.keymap` | Keymap + custom behaviors |
| `config/combos.dtsi` | Combo definitions |
| `config/leader.dtsi` | Leader sequences |
| `config/macros.dtsi` | Compose-key macros |
| `config/cradio.conf` | Kconfig overrides |
| `config/west.yml` | West manifest (urob v0.3) |
| `build.yaml` | CI build matrix |
| `devbox.json` | Devbox environment + build scripts |

CI: GitHub Actions via `zmkfirmware/zmk/.github/workflows/build-user-config.yml`

## Credits

- [dxmh/zmk-config](https://github.com/dxmh/zmk-config) — combo design
- [urob/zmk-config](https://github.com/urob/zmk-config) — ZMK module stack
- [xkcd Mirrorboard](https://blog.xkcd.com/2007/08/14/mirrorboard-a-one-handed-keyboard-layout-for-the-lazy/) — one-handed concept

## License

MIT — see keymap header.
