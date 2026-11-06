# Pinout: TPS5430 12 V to 5 V industrial buck converter

Generated from `schematic.intent.json` (stage 4, schematic capture). Pin numbers are the REAL pins of the installed KiCad symbols, as listed in the BOM.md lib_id column. There is no MCU, so there is no strapping table. U1 is the only IC.

## U1 TPS5430DDAR (Regulator_Switching:TPS5430DDA, HSOP-8 PowerPAD)

| Pin | Name | Net | Rationale |
|---|---|---|---|
| 1 | BOOT | BOOT | C4 10 nF bootstrap to SW, per datasheet |
| 2 | NC | (no connect) | Not connected internally. Marked no-connect on purpose |
| 3 | NC | (no connect) | Not connected internally. Marked no-connect on purpose |
| 4 | VSENSE | FB | R2 10.0 kΩ from +5V (at C5), R3 3.24 kΩ to GND, TP6. Sets 4.99 V |
| 5 | EN | EN | Internal pull-up only, so the default is ON. J3/TP7 to GND disables. C9 1 nF EN-to-GND noise filter is fitted (change fit-c9-en-filter, 2026-11-06). NO external pull-up (ENA abs max ~7 V, `protection.ena_pin_max_V`) |
| 6 | GND | GND | Power ground |
| 7 | VIN | VIN_BUCK | C1, C2 10 µF/50 V + C3 100 nF at the pin |
| 8 | PH | SW | L1, D3 cathode, C4, TP5 |
| 9 | GNDPAD | GND | Exposed pad to the GND pour with thermal vias (`thermal.tj_max_C`) |

## Discrete pin assignments

| Ref | Pin | Net | Note |
|---|---|---|---|
| J1 | 1 / 2 | VIN_RAW / GND | Input terminal |
| F1 | 1 / 2 | VIN_RAW / VIN_FUSED | 4 A slow-blow 0154004.DRT (SPEC §9 Q1 decided 2026-11-05) |
| Q1 | 1 G / 2 D / 3 S | Q1_GATE / VIN_FUSED / VIN_PROT | Reverse-polarity P-FET. Drain on the supply side, source on the protected side |
| D2 | 1 K / 2 A | VIN_PROT / Q1_GATE | 12 V gate clamp, source to gate |
| R1 | 1 / 2 | Q1_GATE / GND | 100 kΩ gate pull-down |
| D1 | 1 K / 2 A | VIN_PROT / GND | SMBJ18A unidirectional TVS, drawn as D_Zener to keep polarity |
| C7 | 1 / 2 | VIN_PROT / GND | DNP π-filter input shunt |
| FB1 | 1 / 2 | VIN_PROT / VIN_BUCK | 0 Ω bridge (rev A) |
| C8 | 1 / 2 | VIN_BUCK / GND | DNP π-filter output shunt |
| C1, C2, C3 | 1 / 2 | VIN_BUCK / GND | Input caps |
| C4 | 1 / 2 | BOOT / SW | Bootstrap |
| D3 | 1 K / 2 A | SW / GND | B560C catch diode |
| L1 | 1 / 2 | SW / +5V | 15 µH |
| C5 | 1 + / 2 − | +5V / GND | 220 µF hybrid |
| C6 | 1 / 2 | +5V / GND | 22 µF X7R |
| R2 | 1 / 2 | +5V / FB | Feedback top |
| R3 | 1 / 2 | FB / GND | Feedback bottom |
| J2 | 1 / 2 | +5V / GND | Output terminal |
| J3 | 1 / 2 | EN / GND | EN pads (DNP header): open = run, short = off |
| C9 | 1 / 2 | EN / GND | 1 nF EN-to-GND noise filter, fitted (CL10B102KB8NNNC, change fit-c9-en-filter 2026-11-06) |
| R4 | 1 / 2 | +5V / LED_A | LED series 1.5 kΩ |
| D4 | 1 K / 2 A | GND / LED_A | Green power-present LED |
| TP1..TP7 | 1 | VIN_PROT, +5V, GND, GND, SW, FB, EN | Test pads |

## Nets added beyond SUBSYSTEMS.md planned list
- **LED_A:** the R4 to D4 anode node. It is a two-pin internal node that needs a name for the IR.

## Intentionally unconnected / absent
- U1.2 and U1.3 (NC) are explicit no-connects.
- There is no EN pull-up resistor (R_en). This is intentional; see BOM.md "Intentionally absent".
- H1 and H2 are M3 mounting holes (Mechanical:MountingHole, MountingHole:MountingHole_3.2mm_M3, non-plated). They have 0 pins and no net. They are unplated and unbonded on purpose, so the mount is not a GND or chassis path; chassis bonding is the installation's choice (SELV, shared ground).
