# Klipper Fork for FlashForge Adventurer 5M

## Overview

This project is a fork of [Klipper](https://www.klipper3d.org) modified for the FlashForge Adventurer 5M.  

I initially tried the [Klipper mod](https://github.com/xblax/flashforge_ad5m_klipper_mod/), but encountered stability issues. The stock version, although more stable, is outdated, limited, and includes some strange decisions. The goal of the project is to enhance and improve it while still maintaining compatibility with the stock firmware.  
Unused components will be removed to keep the setup lightweight. Nevertheless, this is a more correct fork compared to the official Flashforge version.  
**Root access and SSH are required**

## Changes

Refer to [this commit](https://github.com/DeZepTup/klipper/commit/9fa5d8a270b7d7417bc0381fc5ad3734d213ad56) for the original FlashForge changes.

- Pending: Python 3.11
- Pending: Backporting

## Additions

The project includes custom configurations, macros and service GCODE files for the functions not available via screen.

- Pending: Description of custom macros
- Pending: GCODE calibration templates (PID, shaper MZV, mesh)

## Tuning

- TMC: Currently using microsteps: 32 and interpolate: True, as recommended in the [TMC Autotune README](https://github.com/andrewmcgr/klipper_tmc_autotune). Subject to further testing.

## Installation

- Klipper should be placed in /opt/klipper/
- Configuration files should be placed in /opt/config/
- Pending: Python 3.11 setup
- Pending: Root script
- Pending: Installation script

## Slicing

#### Make sure to disable **Arc fitting** and set **Z-hop type** to **Normal**  

* Start GCODE
```gcode
; Heat with normal GCODE to make sure FF function correctly
M190 S[bed_temperature_initial_layer_single]
M104 S[nozzle_temperature_initial_layer]

; Leveling heat
M300 S116 P350 ; notify for nozzle cleanup
M300 S61 P1000
M106 S255 ; turn on fan
M104 S180 ; lower to 180
TEMPERATURE_WAIT SENSOR=extruder MAXIMUM=180
M107 ; turn off fan

; Leveling
BED_MESH_CALIBRATE PROFILE=MESH_DATA

; Reheat nozzle
M104 S[nozzle_temperature_initial_layer]
TEMPERATURE_WAIT SENSOR=extruder MINIMUM=[nozzle_temperature_initial_layer]

; Wipe nozzle
G90
M83
G1 Z5 F6000
G1 E-0.2 F800
G1 X110 Y-110 F6000
G1 E2 F800
G1 Y-110 X55 Z0.25 F4800
G1 X-55 E8 F2400
G1 Y-109.6 F2400
G1 X55 E5 F2400
G1 Y-110 X55 Z0.45 F4800
G1 X-55 E8 F2400
G1 Y-109.6 F2400
G1 X55 E5 F2400
G92 E0
```
- Pending: End GCODE
- Pending: Notes on PA
