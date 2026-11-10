# Bridges V2 ZMK firmware

Independent ZMK board support for the Good Great Grand Wonderful Bridges V2
split keyboard. Bridges V2 and Crosses V2 share their electronics; this fork
uses Bridges naming because the physical keyboard it targets is a Bridges V2.

This repository is derived from
[`Good-Great-Grand-Wonderful/crosses-v2-zmk-firmware`](https://github.com/Good-Great-Grand-Wonderful/crosses-v2-zmk-firmware)
and retains its history, copyright notices, and MIT license.

## Scope

The long-term goal of this repository is to describe hardware rather than a
particular user's pointing or keymap policy:

- nRF52840 left and right board definitions
- key matrix and physical layouts
- PAW3222 sensor wiring
- split pointing-device transport

Reusable pointing processors and runtime tuning belong in
[`measlesbagel/zmk-pointing-tools`](https://github.com/measlesbagel/zmk-pointing-tools).
Personal keymaps, tuned values, and firmware builds belong in
[`measlesbagel/zmk-config`](https://github.com/measlesbagel/zmk-config).

ZMK Studio is not enabled by the board.

## Boards and centrals

The board doesn't decide which device is the split central. The same board
code supports three setups:

| Setup | Central | Left half | Right half |
| --- | --- | --- | --- |
| Dongle | `xiao_ble//zmk` + `bridges_v2_dongle` shield | `bridges_v2_left` | `bridges_v2_right` |
| Left central | `bridges_v2_left` + `bridges_v2_central` snippet | (central) | `bridges_v2_right` |
| Right central | `bridges_v2_right` + `bridges_v2_central` snippet | `bridges_v2_left` | (central) |

A half built without the snippet is a peripheral, and its firmware is the
same whatever the central is. Flash `settings_reset` to every device when
switching centrals, because the split pairing is stored in flash.

The dongle shield targets the [ZMK Prospector](https://github.com/carrefinho/prospector)
(Seeed XIAO nRF52840). Add its `prospector_adapter` shield after
`bridges_v2_dongle` for the display.

### Devicetree for configs

- Sensors: `trackball_left` on the left half, `trackball_right` on the right
  half. Each half defines `BRIDGES_V2_LEFT_TRACKBALL` or
  `BRIDGES_V2_RIGHT_TRACKBALL`, so a config can guard settings that name
  the local sensor.
- Split channels: `trackball_left_split` (reg 0) and `trackball_right_split`
  (reg 1). Each half sends its ball on its own channel.
- Listeners: `trackball_left_listener` and `trackball_right_listener`. They
  are enabled only on the central, where they read the local sensor or the
  split channel as needed. Attach input processors to these.
- `config_acm`: a USB serial port, enabled only on the central.

### Example `build.yaml`

```yaml
include:
  # Dongle setup
  - board: xiao_ble//zmk
    shield: bridges_v2_dongle prospector_adapter
  - board: bridges_v2_left
  - board: bridges_v2_right
  # Left half as central, used with the same bridges_v2_right build
  - board: bridges_v2_left
    snippet: bridges_v2_central
```
