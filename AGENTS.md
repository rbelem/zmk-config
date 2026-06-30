# zmk-config — Ferris Sweep (cradio)

ZMK firmware config for a 34-key Ferris Sweep running on nice_nano_v2.

## Key structure

```
config/           # West-managed config dir (west.yml pins to urob v0.3, not ZMK main)
  cradio.keymap   # Main keymap — 13 layers
  cradio.conf     # Kconfig overrides
  combos.dtsi     # Combo (chord) definitions
  leader.dtsi     # Leader-key sequences
  macros.dtsi     # Custom behaviors (layer_macro, clear_sl)
  west.yml        # West manifest + urob ZMK modules
boards/shields/   # Empty — cradio lives upstream in ZMK
build.yaml        # CI build matrix
```

## Dev environment

Uses **devbox** (Nix). The init hook runs `west init -l config && west update && west zephyr-export`.

```sh
# Enter devbox shell with all tooling
devbox shell

# Build firmware for targets matching an expression
devbox run build <expr>          # e.g. "cradio" or "all" or "cradio_left"
```

Build matrix entries in `build.yaml` define `board,shield,snippet,artifact-name`.
Output goes to `firmware/` — `.uf2` for nice_nano_v2, `.bin` otherwise.

## Build targets

Defined in `build.yaml`:

- `nice_nano_v2` + `cradio_left`
- `nice_nano_v2` + `cradio_right`
- `nice_nano_v2` + `settings_reset` — clear pairing data on keyboard

## CI

GitHub Actions — `.github/workflows/build.yml` delegates to `zmkfirmware/zmk/.github/workflows/build-user-config.yml@main`.

## Design rules

- **One-handed operation**: Every feature must be usable with either hand alone. The layout mirrors across halves — left and right have mirrored leader sequences, layer access, and modifier combos. Never add a feature that requires both hands.

## Keymap quirks

- **Brazilian ABNT2**: `BR_*` defines map ABNT2 keys to US positions (BR_ACUTE=`LBKT`, BR_TILDE=`QUOT`, BR_CEDIL=`SEMI`, etc.)
- **Auto-shift macro**: `AS(keycode)` = `&as LS(keycode) keycode`
- **Layer tap dance pattern**: `MO_TOG(layer)` / `KP_SL_TO(key, layer)` convenience macros. Most layer accesses use tap-dance → momentary-on-tap, toggle-on-double-tap
- **34-key layout** — no number row; thumb keys at positions 30-33
- **Home row mods**: `&mt` tapping-term-ms=250
- **Custom behaviors**: `swap` (tri-state alt+tab), `td_*` (multi-function tap dances), `h_kp_sl` / `kp_csl` (hold-tap combos)

## ZMK module stack (urob, pinned at v0.3)

- `zmk-adaptive-key`, `zmk-auto-layer`, `zmk-helpers`, `zmk-leader-key`, `zmk-tri-state`, `zmk-unicode`

## Config notes

- `CONFIG_ZMK_POINTING=y` — mouse layer enabled
- `CONFIG_ZMK_BLE_EXPERIMENTAL_CONN=y` — up to 9 BLE connections, 8dB TX power
- `CONFIG_ZMK_SLEEP=y`, idle timeout 30 min — keyboard auto-sleeps
- `.west/` is initialized locally — don't run `west init` again without understanding the state

## Common tasks

```sh
# Full rebuild (from devbox shell)
devbox run build all

# Build single shield variant
devbox run _build_single nice_nano_v2 cradio_left "" ""

# Override west args
devbox run build cradio -DCONFIG_ZMK_STUDIO=y
```

## Files to edit for common changes

- **Key layout / layers** → `config/cradio.keymap`
- **Chords (simultaneous key combos)** → `config/combos.dtsi`
- **Leader sequences** → `config/leader.dtsi`
- **Kconfig options** → `config/cradio.conf`
- **Build targets** → `build.yaml`
- **Dependency versions / modules** → `config/west.yml`
