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

The inherited pointing pipeline remains temporarily while it is extracted into
`zmk-pointing-tools`. ZMK Studio is not enabled by the board.

## Boards

- `bridges_v2_left`
- `bridges_v2_right` (split central)
