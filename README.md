# Corne OLED Wireless — ZMK Config

Personal ZMK firmware configuration for a **42-key Corne** (nice_nano_v2) wireless split keyboard.

Architecture based on [urob/zmk-config](https://github.com/urob/zmk-config).

## Features

- **Colemak-DH** base layout
- **"Timeless" homerow mods** — balanced flavor with positional hold-tap and `require-prior-idle-ms`
- **Combos** for symbols instead of a dedicated symbol layer
- **Smart-num** — tap: num-word, double-tap: sticky num, hold: num layer
- **Smart-shift** — tap: sticky-shift, double-tap: caps-word, hold: shift
- **Smart-mouse** — auto-toggle mouse layer (requires zmk-tri-state)
- **Alt+Tab swapper** via tri-state
- **Mod-morphs** — comma→semicolon, dot→colon, qmark→excl on shift
- **Nav cluster** — arrows double as home/end/doc-start/doc-end on long-press
- **Space morph** — shift+space produces `. ` + sticky-shift
- **Conditional Sys layer** — FN + NUM activates System (BT, bootloader, reset)
- **Mouse emulation** with pointer speed tuning
- **OLED display** + **ZMK Studio** support

## Layout

```
╭──────────────────────────╮ ╭──────────────────────────╮
│ TAB   Q  W  F  P  B     │ │     J  L  U  Y  '   ESC │
│ CTRL  A  R  S  T  G     │ │     M  N  E  I  O   '   │
│ SHFT  Z  X  C  D  V     │ │     K  H  ,  .  ?!  RPT │
╰─────────╮ GUI SPC  RET  │ │  NUM  SFT  MSE ╭────────╯
          ╰────────────────╯ ╰────────────────╯
```

Inner 5 columns use HRMs (home-row mods): GACS on left, SCAG on right.

## File Structure

```
config/
├── base.keymap    # Shared 5-column keymap (layers, behaviors, macros)
├── corne.keymap   # 42-key Corne wrapper (outer columns + key-labels)
├── corne.conf     # Board-specific Kconfig (OLED, Studio, BLE, sleep)
├── combos.dtsi    # Symbol combos (vertical + horizontal)
├── mouse.dtsi     # Mouse emulation settings
└── west.yml       # West manifest (ZMK + modules)
```

## ZMK Modules

| Module | Purpose |
|--------|---------|
| [zmk-helpers](https://github.com/urob/zmk-helpers) | Helper macros for cleaner DTS syntax |
| [zmk-auto-layer](https://github.com/urob/zmk-auto-layer) | Num-word (auto-deactivate number layer) |
| [zmk-tri-state](https://github.com/urob/zmk-tri-state) | Smart-mouse, Alt+Tab swapper |

## Building

Firmware builds automatically via GitHub Actions on push. Download the `.uf2` files from the Actions tab.

To build locally:
```bash
west init -l config
west update
west zephyr-export
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD=corne_left -DZMK_CONFIG="$(pwd)/config"
```

## Keymap Editor

https://nickcoutsos.github.io/keymap-editor/
