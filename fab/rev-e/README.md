# Fabrication outputs, rev E

**Released 2026-11-07 from the design at `116202e` (release commit `c50e737`). This board has never been fabricated or powered, and neither has rev D. Until bring-up passes, it is unverified. If you send these files to a fab, that decision is yours.**

The copperhead logo and `tps5430/rev-e` label are on the front silkscreen in these files.

## What is here

| File | What |
| --- | --- |
| `tps5430-buck-gerbers.zip` | 10 files: copper, mask, paste and silkscreen for both sides, the outline, and the drill file |
| `gerbers/` | the same files unzipped, plus the `.gbrjob` job file |
| `tps5430-buck-bom-jlcpcb.csv` | BOM for JLCPCB: 23 parts with LCSC numbers |
| `tps5430-buck-cpl-jlcpcb.csv` | component placement, 23 parts, in KiCad's CSV format |
| `drc.json` | DRC report on this board (zones refilled, schematic parity, all severities) |

## Where they came from

Every gerber, the drill file, the job file, the BOM and the placement file are byte-for-byte copies of the frozen rev E release (`releases/rev-e/` in the design workspace). Only the BOM and placement file were renamed. Their SHA-256 sums match the ones recorded at release:

```text
48235fe3602e115e26a20bedab231becd9b52aa1e04a518e571c72dc102c5c89  tps5430-buck-bom-jlcpcb.csv
5be1576bd559afe07c81bb4144a3545bfdb896e79bd09657e6118fa82f2f90bf  tps5430-buck-cpl-jlcpcb.csv
261b7f9dcfc136aa3c6a69316fc0d0eb8aa6c5b15d02c3b486fe043f9ae5aa5c  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter.drl
87cca9bbdfd96442f3be86265f5b462db65b98048f42366b73f04ca780bd5787  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter-F_Cu.gtl
baed39554bda5a4c4117a4dc36674daa492e12325ba242bf2801a42123bc4c80  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter-B_Cu.gbl
18fe548cf917110e14c99674b5715be438959ff5ab8956f10628453e8c8d4d5d  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter-job.gbrjob
```

The BOM was made with `copperhead export bom --supplier jlcpcb`. The drill file has 194 holes: 178 vias and 16 drilled pads.

## Checks on this board

- KiCad DRC (`--refill-zones --schematic-parity --severity-all`): 0 violations, 0 unconnected items, 0 parity issues.
- KiCad ERC (`--severity-all`): 0 violations.
- copperhead check: ERC 0, DRC 0, 0 unrouted, no drift between the board and its design documents.
- The project's quiet-copper rule, which DRC cannot check: no GND via within 1 mm of FB, EN, EN_EXT or the output sense line, including their teardrops and fillets.
- The placement file and the BOM list the same 23 parts.
- Every fitted BOM figure was checked against its manufacturer datasheet. LCSC numbers and stock come from the JLCPCB parts API on 2026-11-06.

## On the order form

The fab notes are on User.Comments, which is not in these gerbers. Give them to JLCPCB on the order form or in its remark field:

- **2 oz outer copper.** The job file states 70 µm, but the gerbers do not set it.
- **J1/J2 holes at least 1.3 mm finished**, for the 0.9 mm square pins of the Phoenix 1715721.
- **Tented vias stay tented.**

## What has not been checked

- **Nothing in rev E has been built or measured.** The bench plan was written for rev D, which was never ordered.
- The gerbers in a viewer, and JLCPCB's DFM report. Do that before you pay for anything.
- **Placement rotations against JLCPCB's preview.** For J1 and J2 this decides the design: a 180° error turns the wire openings back into the board. In the preview, J1's openings must face the left edge and J2's the right edge. Check U1 pin 1, Q1's tab, D1 to D4 and C5's polarity too.
- Loop stability, EMC, and signal or power integrity, beyond TI's reference values for the TPS5430 and a calculated commutation-loop inductance (6.9 nH).
- Thermal performance at 3 A and 50 °C ambient. U1's estimated junction temperature at 9 V, 3 A, 50 °C is about 118 °C, 7 °C inside its limit.
- Stock at the moment of ordering.

## Known issues

- **Catch-diode short-circuit rating is open.** In a sustained output short, the TPS5430's 4 to 6 A current limit could put 2.6 to 4.5 W into the 5 A B560C (D3). A shorted-output bench test decides between a larger diode in a later revision and an external fuse on the 5 V wiring.
- **J1 and J2 are through-hole.** They need JLCPCB through-hole assembly or hand soldering. If hand-soldered, fit them with the wire openings toward the nearest board edge.
- **Not fitted:** C7 and C8 (input filter; FB1 bridges it), C10 (100 µF bulk input capacitor, footprint only) and J3 (the EN header). None is in the BOM or placement file. Confirm JLCPCB shows them as unmatched, not substituted.
- **Placement file format:** KiCad's CSV (Ref, Val, Package, PosX, PosY, Rot, Side). JLCPCB's upload maps these columns; otherwise rename them to Designator, Mid X, Mid Y, Rotation, Layer.
- **Land patterns slightly smaller than the makers' recommendation, accepted:** FB1 and C5. The terminations land fully on copper.
