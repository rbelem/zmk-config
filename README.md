# zmk-config — Ferris Sweep (cradio)

ZMK firmware for a 34-key **Ferris Sweep** running on **nice_nano_v2**.

## Layout

13 layers, 34 keys, Brazilian ABNT2 mappings. Inspired by the [Mirrorboard concept](https://blog.xkcd.com/2007/08/14/mirrorboard-a-one-handed-keyboard-layout-for-the-lazy/) — the layout mirrors across halves so you can type with either hand alone. Home-row mods, leader-key sequences, combos (chords), tap-dance layer access, mouse keys, tri-state alt-tab.

## Build

Requires [devbox](https://www.jetify.com/devbox):

```sh
devbox run build all              # full rebuild
devbox run build cradio           # both halves
devbox run _build_single nice_nano_v2 cradio_left "" ""   # single target
```

Output in `firmware/` — `.uf2` for nice_nano_v2. Flash by copying to the mounted bootloader drive.

### Targeting

| `devbox run build <expr>` | Builds |
|---|---|
| `cradio` | left + right |
| `cradio_left` | left only |
| `cradio_right` | right only |
| `settings_reset` | clear BLE pairing |

## Key features

- **Leader key** on both inner thumbs — sequences for app launcher (`SPACE`), symbol layers (`F`/`D`/`J`/`K`), terminal (`T T`), terminal tab (`T G`), Ctrl+Alt+Del (`Q`/`P`)
- **Thumb Ctrl** — hold either inner thumb for Ctrl, tap for leader
- **Combos** — simultaneous keypress shortcuts (ESC on `W+E`, backspace on `X+C`, layer access via tap-dance combos, etc.)
- **Home-row mods** — `&mt` on `A`/`S`/`D`/`F`/`H`/`J`/`K`/`L` with 250ms tapping term
- **Mirror layers** — SYMBOL, BASE, and FUNCTION mirrored for either-hand access
- **Mouse layer** — pointing with acceleration tuning

## Structure

| Path | Role |
|---|---|
| `config/cradio.keymap` | Keymap + custom behaviors |
| `config/combos.dtsi` | Combo (chord) definitions |
| `config/leader.dtsi` | Leader sequences |
| `config/macros.dtsi` | Custom macros |
| `config/cradio.conf` | Kconfig overrides |
| `config/west.yml` | West manifest (urob modules, v0.3) |
| `build.yaml` | CI build matrix |
