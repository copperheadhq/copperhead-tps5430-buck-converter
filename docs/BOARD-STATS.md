# Board statistics: tps5430-12-v-to-5-v-industrial-buck-converter

Rev E, release commit **c50e737** (design at 116202e); board `hardware/tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb`. KiCad 10.0.6.
Counted with the KiCad `pcbnew` Python API on 2026-11-07, from the board as committed (zones filled). DRC run with zones refilled on a copy; nothing in `hardware/` is written.

## Board

| Field | Value |
| --- | --- |
| Outline | 60 mm x 40 mm |
| Thickness | 1.6 mm |
| Copper layers | 2: F.Cu, B.Cu, 2 oz (70 µm) |
| Copper pours | GND: B.Cu 2135.0 mm², F.Cu 1655.5 mm². No switch-node pour (removed in rev E) |
| Teardrops | 48 (KiCad teardrop zones on pads and vias, curved) |
| Drawn fillets | 23 (curved copper fillets at T-junctions and track steps) |
| Keep-out areas | 6 |
| Mounting holes | 2, M3 (3.2 mm, non-plated) |

## Components

| Type | Front | Back | Total |
| --- | --- | --- | --- |
| smd | 24 | 0 | 24 |
| tht | 3 | 0 | 3 |
| unspecified (test pads, mounting holes) | 10 | 0 | 10 |
| total | 37 | 0 | 37 |

| Field | Value |
| --- | --- |
| By reference prefix | C 10, D 4, F 1, FB 1, H 2, J 3, L 1, Q 1, R 5, TP 8, U 1 |
| Unique footprints | 18 |
| JLCPCB BOM (`fab/rev-e/`) | 23 parts with LCSC numbers |
| Placement file | 23 parts |
| Left out of the BOM and placement file | C7, C8, C10, J3 (not fitted), TP1 to TP8, H1, H2 |

## Assembly and drilling

| Pads | Count |
| --- | --- |
| smd | 67 |
| through_hole | 14 |
| npth | 2 |

| Holes | Size | Plated | From |
| --- | --- | --- | --- |
| 168 | 0.3 mm (0.6 mm pad) | yes | Via |
| 10 | 0.4 mm (0.8 mm pad) | yes | Via |
| 8 | 0.3 mm | yes | Pad (U1 thermal pad) |
| 4 | 1.3 mm | yes | Pad (J1, J2) |
| 2 | 1.0 mm | yes | Pad (J3) |
| 2 | 3.2 mm | no | Pad (H1, H2) |

194 holes in all, as in the drill file.

## Routing

| Field | Value |
| --- | --- |
| Nets (on pads) | 13 |
| Vias | 178, all through |
| Track segments | 98 (F.Cu 90, B.Cu 8) |
| Track length | F.Cu 217.4 mm, B.Cu 44.4 mm |
| Default net class | 0.25 mm track, 0.2 mm clearance |
| Custom rule (`.kicad_dru`) | via drill at least 0.3 mm |

| Track width (mm) | Segments | Length (mm) |
| --- | --- | --- |
| 0.25 | 6 | 9.8 |
| 0.3 | 11 | 21.9 |
| 0.4 | 11 | 45.2 |
| 0.5 | 9 | 16.8 |
| 0.6 | 2 | 3.6 |
| 0.8 | 3 | 3.6 |
| 1 | 26 | 49.4 |
| 1.5 | 14 | 46.0 |
| 2 | 14 | 53.4 |
| 2.5 | 2 | 12.1 |

| Net | Min width (mm) | Max width (mm) | Track length (mm) |
| --- | --- | --- | --- |
| VIN_RAW | 1.5 | 1.5 | 10.5 |
| VIN_FUSED | 1.5 | 1.5 | 11.5 |
| VIN_PROT | 1 | 2 | 35.2 |
| VIN_BUCK | 0.8 | 1.5 | 41.0 |
| SW | 0.5 | 2 | 18.8 |
| +5V | 0.3 | 2.5 | 78.6 |
| GND | 0.5 | 2.5 | 29.1 |
| /BOOT | 0.4 | 0.4 | 4.0 |
| /Q1_GATE | 0.4 | 0.4 | 5.8 |
| EN | 0.3 | 0.3 | 9.8 |
| /EN_EXT | 0.3 | 0.3 | 7.7 |
| FB | 0.25 | 0.25 | 7.9 |
| /LED_A | 0.25 | 0.25 | 1.9 |

The 0.4 mm section of +5V is the sense line from the output capacitor C5 to the top of the feedback divider (R2), not the output path.

## Checks

| Check | Result |
| --- | --- |
| DRC (zones refilled, schematic parity, all severities) | 0 violations |
| Unconnected items | 0 |
| Schematic parity | 0 issues |
| ERC (all severities) | 0 violations |

The full DRC report is [`fab/rev-e/drc.json`](../fab/rev-e/drc.json).
