# Board statistics: tps5430-12-v-to-5-v-industrial-buck-converter

Rev D, commit **1393414** (board unchanged since e03bac1); board `hardware/tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb`. KiCad 10.0.6.
Counted with the KiCad `pcbnew` Python API on 2026-11-06. DRC run with zones refilled on a copy; nothing in `hardware/` is written.

## Board

| Field | Value |
| --- | --- |
| Outline | 60 mm x 40 mm |
| Thickness | 1.6 mm |
| Copper layers | 2: F.Cu, B.Cu, 2 oz (70 µm) |
| Copper pours after refill | GND: F.Cu 1578.3 mm², B.Cu 1939.7 mm². SW: F.Cu 88.8 mm², B.Cu 123.2 mm² |
| Keep-out areas | 6 |
| Mounting holes | 2, M3 (3.2 mm, non-plated) |

## Components

| Type | Front | Back | Total |
| --- | --- | --- | --- |
| smd | 22 | 0 | 22 |
| tht | 3 | 0 | 3 |
| unspecified (test pads, mounting holes) | 9 | 0 | 9 |
| total | 34 | 0 | 34 |

| Field | Value |
| --- | --- |
| By reference prefix | C 9, D 4, F 1, FB 1, H 2, J 3, L 1, Q 1, R 4, TP 7, U 1 |
| Unique footprints | 18 |
| JLCPCB BOM (`fab/rev-d/`) | 25 parts; C7, C8 and J3 listed as DNP without an LCSC number |
| Placement file | 22 parts |
| Left out of the placement file | C7, C8, H1, H2, J3, TP1 to TP7 |

## Assembly and drilling

| Pads | Count |
| --- | --- |
| smd | 62 |
| through_hole | 14 |
| npth | 2 |

| Holes | Size | Plated | From |
| --- | --- | --- | --- |
| 105 | 0.3 mm (0.6 mm pad) | yes | Via |
| 16 | 0.4 mm (0.8 mm pad) | yes | Via |
| 8 | 0.3 mm | yes | Pad (U1 thermal pad) |
| 4 | 1.3 mm | yes | Pad (J1, J2) |
| 2 | 1.0 mm | yes | Pad (J3) |
| 2 | 3.2 mm | no | Pad (H1, H2) |

137 holes in all, as in the drill file.

## Routing

| Field | Value |
| --- | --- |
| Nets (on pads) | 14 |
| Vias | 121, all through |
| Track segments | 109 (F.Cu 91, B.Cu 18) |
| Track length | F.Cu 216.5 mm, B.Cu 102.0 mm |
| Default net class | 0.25 mm track, 0.2 mm clearance |
| Custom rule (`.kicad_dru`) | via drill at least 0.3 mm |

| Track width (mm) | Segments | Length (mm) |
| --- | --- | --- |
| 0.25 | 11 | 45.2 |
| 0.3 | 13 | 66.4 |
| 0.4 | 6 | 9.8 |
| 0.5 | 14 | 25.4 |
| 0.6 | 3 | 3.2 |
| 0.8 | 4 | 5.5 |
| 1.0 | 24 | 44.9 |
| 1.5 | 16 | 52.6 |
| 2.0 | 16 | 53.1 |
| 2.5 | 2 | 12.4 |

| Net | Min width (mm) | Max width (mm) | Track length (mm) |
| --- | --- | --- | --- |
| VIN_RAW | 1.5 | 1.5 | 10.5 |
| VIN_FUSED | 1.5 | 1.5 | 11.5 |
| VIN_PROT | 1.0 | 2.0 | 35.2 |
| VIN_BUCK | 0.8 | 2.0 | 43.1 |
| SW | 0.5 | 2.5 | 23.4 |
| +5V | 0.25 | 2.5 | 121.1 |
| GND | 0.3 | 1.5 | 35.3 |
| /BOOT | 0.4 | 0.4 | 4.0 |
| /Q1_GATE | 0.4 | 0.4 | 5.8 |
| EN | 0.3 | 0.3 | 18.6 |
| FB | 0.25 | 0.25 | 7.9 |
| /LED_A | 0.25 | 0.25 | 2.0 |

The 0.25 mm section of +5V is the sense line from the output to the top of the feedback divider (R2), not the output path.

## Checks

| Check | Result |
| --- | --- |
| DRC (zones refilled, schematic parity, all severities) | 0 violations |
| Unconnected items | 0 |
| Schematic parity | 0 issues |
| ERC (all severities) | 0 violations |

The full DRC report is [`fab/rev-d/drc.json`](../fab/rev-d/drc.json).
