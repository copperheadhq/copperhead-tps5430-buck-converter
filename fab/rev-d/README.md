# Fabrication outputs, rev D

**Released 2026-11-06 from the design at `e03bac1` (release commit `1393414`). This board has never been fabricated or powered. Until bring-up passes, it is unverified. If you send these files to a fab, that decision is yours.**

**No branding in these files.** The copperhead logo and `tps5430/rev-d` label in `hardware/` were added on the front silkscreen after this release. These gerbers are the frozen release and do not show them.

## What is here

| File | What |
| --- | --- |
| `tps5430-buck-gerbers.zip` | 10 files: copper, mask, paste and silkscreen for both sides, the outline, and the drill file |
| `gerbers/` | the same files unzipped, plus the `.gbrjob` job file |
| `tps5430-buck-bom-jlcpcb.csv` | BOM for JLCPCB: 25 parts with LCSC numbers; C7, C8 and J3 listed as DNP without one |
| `tps5430-buck-cpl-jlcpcb.csv` | component placement, 22 parts, in KiCad's CSV format |
| `drc.json` | DRC report on this board (zones refilled, schematic parity, all severities) |

## Where they came from

Every gerber, the drill file, the job file, the BOM and the placement file are byte-for-byte copies of the frozen rev D release (`releases/rev-d/` in the design workspace). Only the BOM and placement file were renamed. Their SHA-256 sums match the ones recorded at release:

```text
4c8355cd3eff898a9f32560872102f19657e958c55bcf7fb29be478ed65670b7  tps5430-buck-bom-jlcpcb.csv
ce255db08a9874ec8eff1272b937b679d778ace97c9e0f4b883b3145a1ac91dc  tps5430-buck-cpl-jlcpcb.csv
6737530125bf899f310b2e98f1d4ebfe4c0e70155c41d181711cbdd27c5c2643  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter.drl
69dd713fc041b37f0dd55b39a0f6f1be9dd6268b1660b0a990912b6484dd9680  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter-F_Cu.gtl
de28c86a19a5228129aefae0c92e1ada003b7e5db2074b3786c7645ecb78d933  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter-B_Cu.gbl
8c70f69763791b6ac151f034df56dcb845e9b7cdb9f034320a137af8b9f24027  gerbers/tps5430-12-v-to-5-v-industrial-buck-converter-job.gbrjob
```

The BOM was made with `copperhead export bom --supplier jlcpcb`. The drill file has 137 holes: 121 vias and 16 drilled pads.

## Checks on this board

- KiCad DRC (`--refill-zones --schematic-parity --severity-all`): 0 violations, 0 unconnected items, 0 parity issues.
- KiCad ERC (`--severity-all`): 0 violations.
- Terminal orientation, from the footprint rotation and 3D renders: J1's wire openings face the left edge and J2's the right edge, with nothing in front of either clamp.
- Every fitted BOM figure was checked against its manufacturer datasheet. LCSC numbers and stock come from the JLCPCB parts API on 2026-11-06.

## On the order form

The fab notes are on User.Comments, which is not in these gerbers. Give them to JLCPCB on the order form or in its remark field:

- **2 oz outer copper.** The job file states 70 µm, but the gerbers do not set it.
- **J1/J2 holes at least 1.3 mm finished**, for the 0.9 mm square pins of the Phoenix 1715721.
- **Tented vias stay tented.**

## What has not been checked

- The gerbers in a viewer, and JLCPCB's DFM report. Do that before you pay for anything.
- **Placement rotations against JLCPCB's preview.** KiCad and JLCPCB can disagree on a part's zero rotation; the SOT-223, the SMB/SMC diodes, the electrolytic and the fuse holder are the usual ones. **For J1 and J2 this decides the design:** a 180° error turns the wire openings back into the board. In the preview, J1's openings must face the left edge (beside H1 and F1) and J2's the right edge (beside TP2 and C6).
- Loop stability, EMC, and signal or power integrity, beyond following TI's reference values for the TPS5430.
- Thermal performance at 3 A and 50 °C ambient, especially D3 (1.2 to 1.5 W) on its SW copper.
- Stock at the moment of ordering.

## Known issues

- **J1 and J2 are through-hole.** They need JLCPCB through-hole assembly or hand soldering. If hand-soldered, fit them with the wire openings toward the nearest board edge.
- **Not fitted:** C7 and C8 (input filter; FB1 bridges it) and J3 (the EN header). Confirm JLCPCB shows them as unmatched, not substituted.
- **Placement file format:** KiCad's CSV (Ref, Val, Package, PosX, PosY, Rot, Side). JLCPCB's upload maps these columns; otherwise rename them to Designator, Mid X, Mid Y, Rotation, Layer.
- **Land patterns slightly smaller than the makers' recommendation, accepted:** FB1 and C5. The terminations land fully on copper.
- **Deferred to a later revision:** C3 has no top-layer GND return (the F.Cu channel is only 0.65 mm), and C2 sits about 12 mm from U1, so it acts as bulk capacitance only.
- **SW copper on both layers under D3/L1** (about 2.1 cm², for D3's heat) removes some B.Cu ground under the power stage. Check D3's temperature and the noise on SW at bring-up.
- **The schematic title block says "Rev: A".** The schematic is unchanged since rev A.
