# Design changelog

Append-only, newest first. One entry per committed copperhead run.

## 2026-11-06 — Docs only: bring the design record in step with board commit 4698973, which the person chose to make with a KiCad pcbnew script and a kicad-cli zone refill because copperhead do cannot refill zones (the earlier request to turn the terminals was refused for that reason). Read the commit message of 4698973 and the board for exact coordinates. Record: (1) docs/LAYOUT.md placement rows: J1 now (105.6, 110.42) 270° with pin 1 VIN_RAW on top and pin 2 GND at y 115.5; J2 now (154.4, 109.08) 90° with pin 2 GND on top at y 104 and pin 1 +5V at y 109.08; TP4 now (147.3, 101.8) with its 0.5 mm GND trace along y 101.8 and a 45 into J2.2; both terminals' wire openings face outward over their board edges, clamp faces about 0.9 mm inside the edge so the pin-1 silk triangles stay on the board; (2) the Routed bullets for VIN_RAW (J1.1 straight to (109.895, 110.42), one 45 into F1.1, 1.5 mm), +5V (x 156.8 trunk stops at y 111.48, one 45 into J2.1, 2.0 mm) and TP4 to J2.2; (3) the Silkscreen section: +VIN/-GND at (114, 110.42)/(114, 115.5), J2 -GND at (146.6, 104) and +5V at (147, 109.08), TP4 reference at (144.8, 101.8), FB1 reference at (141.8, 101.9), J1 reference (112.1, 118.5), J2 reference (148, 106.54), and that TP4's own GND name text was removed for lack of room; (4) docs/PINOUT.md if it states the pin positions or order of J1/J2; (5) DECISIONS: why (in rev A to C both terminals faced inward, J2 into C5; docs/BRIEF.md Physical now requires the wire openings to face outward, 2026-11-06), that the change was made outside copperhead do at the person's request and why, that it postdates rev C (0abd2dd) so it belongs to rev D, and that outputs/ is stale until the next export; (6) CHANGELOG entry; (7) an openspec requirement that each screw terminal's wire openings face outward over its board edge with nothing in front of the clamps, citing docs/BRIEF.md Physical, verified from the footprint rotation (TerminalBlock_Phoenix horizontal wire entry is footprint local +y: J1 at 270° on the left edge, J2 at 90° on the right edge) and courtyard position. KiCad DRC with zone refill and schematic parity, copperhead check and sync --dry-run were clean at 4698973. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean.

- Change: docs-sync-board-4698973
- Files: docs/LAYOUT.md, docs/CHANGELOG.md, docs/SPEC.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Docs synced to board commit 4698973 (J1/J2 wire openings turned outward; TP4 moved)

- Change: docs-sync-board-4698973
- Files: docs/LAYOUT.md (placement rows J1, J2, TP1..TP5; Routed VIN_RAW, +5V and TP4 → J2.2 bullets; Silkscreen), docs/SPEC.md §6, .copperhead/constraints.json (`mech.board_mm`), openspec/changes/docs-sync-board-4698973 (terminal-orientation requirement), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The person made board commit 4698973 with a KiCad pcbnew script and a kicad-cli zone refill, outside copperhead do. copperhead do cannot refill zones, and the earlier request to turn the terminals was refused for that reason. This run records that commit; it edits no KiCad file.
  - Source: coordinates read from the live .kicad_pcb. The change list follows the person's summary of the commit, because the workspace is not a git repository and the commit message could not be read here.
  - Why: docs/BRIEF.md Physical (2026-11-06) requires each terminal's wire openings to face outward over its board edge with nothing in front of the clamps. In rev A to C both faced inward, J2 into C5.
  - Placement:
    - J1 (106, 115.5) 90° → (105.6, 110.42) 270°. Pin 1 VIN_RAW on top at y 110.42, pin 2 GND at y 115.5.
    - J2 (154, 104) 270° → (154.4, 109.08) 90°. Pin 2 GND on top at y 104, pin 1 +5V at y 109.08.
    - TP4 (147.5, 109.08) → (147.3, 101.8).
    - Wire entry of the horizontal MKDS footprint is local +y: −x for J1 at 270°, +x for J2 at 90°. Clamp faces (local y 4.72) at x ≈ 100.88 and ≈ 159.12, about 0.9 mm inside the edges, so the pin-1 silk triangles stay on the board.
  - Routes:
    - VIN_RAW 1.5 mm: J1.1 straight east to (109.895, 110.42), one 45° into F1.1 (114.315, 106).
    - +5V 2.0 mm: the x 156.8 trunk stops at y 111.48, one 45° into J2.1 (154.4, 109.08).
    - TP4 → J2.2 0.5 mm GND: along y 101.8 to (152.2, 101.8), one 45° into J2.2 (154.4, 104).
  - Silkscreen:
    - `+VIN` (114, 110.42), `-GND` (114, 115.5); J2 `-GND` (146.6, 104), `+5V` (147, 109.08).
    - References: TP4 (144.8, 101.8), FB1 (141.8, 101.9), J1 (112.1, 118.5), J2 (148, 106.54).
    - TP4's own `GND` name text removed for lack of room (intentional).
  - docs/PINOUT.md checked: it gives J1/J2 pin-to-net only (unchanged), no positions, so it is not edited.
  - Person-reported: KiCad DRC with zone refill and schematic parity, `copperhead check` and `copperhead sync --dry-run` were clean at 4698973.
  - The commit postdates rev C (0abd2dd), so it belongs to rev D.
  - No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit. No net, value, width or footprint change.
- Open: outputs/ (gerbers, drill, STEP, renders, BOM.csv) predates 4698973 and stays stale until the next export (or /release-board).
- Verification: ERC clean. DRC clean on the untouched board (copperhead run_drc, no zone refill or parity; the refilled and parity-checked result is the person's KiCad run at 4698973). Drift clean. Docs only; no KiCad file was edited.

## 2026-11-06 — Docs only: bring docs/LAYOUT.md, docs/DECISIONS.md and docs/CHANGELOG.md in step with board commit 956cc1e, which the person chose to make with a KiCad pcbnew script and a kicad-cli zone refill because copperhead do cannot refill zones (the earlier request fb-route-straighten-r3-tp6 was refused for stale GND fill). Read the commit message of 956cc1e and the board for exact coordinates. Record: (1) placement table: R3 now at (119.5, 129.405) and TP6 at (117.0, 129.405), with the FB silkscreen label at (115.2, 129.405); (2) the FB route: it no longer runs along a y 128.2 channel; it leaves U1.4 straight at y 128.405 to (119.675, 128.405), takes one 45 into R3.1 at (118.675, 129.405) and runs straight at y 129.405 into TP6, and R2.2 tees straight down from (120.325, 127.2) to the y 128.405 run; this removes the 0.205 mm jog at U1.4 and the 45 plus stub into TP6; (3) why R3 moved with the run (at y 128.405 the track would be 0.195 mm from R3.2 GND, under the 0.2 mm clearance; it now keeps about 0.4 mm) and why TP6 sits at x 117.0 rather than 116.3 (keeps 1.20 mm edge to edge from the GND via at (115, 130) under the 1 mm GND-via-to-FB rule of gnd-stitching-vias-density; 116.3 would be 0.5 mm). Note that KiCad DRC with zone refill and schematic parity, copperhead check and sync --dry-run were clean at 956cc1e. In DECISIONS also note that this change was made outside copperhead do at the person's request, and why, and that it postdates the rev B release (47b172a), so it belongs to the next revision; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean.

- Change: docs-sync-board-956cc1e
- Files: docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Docs synced to board commit 956cc1e (FB route straightened; R3 and TP6 moved)

- Change: docs-sync-board-956cc1e
- Files: docs/LAYOUT.md (placement table R2/R3/TP6 row, Routed FB legs bullet, 9af7427 tidy-up FB bullet marked superseded, new 956cc1e bullet), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The person made board commit 956cc1e with a KiCad pcbnew script and a kicad-cli zone refill, outside copperhead do. copperhead do cannot refill zones, and the earlier request fb-route-straighten-r3-tp6 was refused for stale GND fill. This run records that commit; it edits no KiCad file.
  - Source: coordinates read from the live .kicad_pcb. The change list is the person's summary of the commit, because the workspace is not a git repository and the commit message could not be read here.
  - Placement:
    - R3 moved from (119.5, 129.2) to (119.5, 129.405).
    - TP6 moved from (116.3, 128.2) to (117.0, 129.405).
    - The TP6 `FB` silkscreen label is at (115.2, 129.405).
    - R2 did not move.
  - FB route (0.25 mm F.Cu):
    - U1.4 (123.3, 128.405) runs straight west at y 128.405 to (119.675, 128.405).
    - It takes one 45° into R3.1 at (118.675, 129.405), then runs straight at y 129.405 into TP6.
    - R2.2 tees straight down from (120.325, 127.2).
    - The y 128.2 channel, the 0.205 mm jog at U1.4 and the 45° plus stub into TP6 are gone.
  - Why R3 moved with the run: with the run at y 128.405 and R3 left at y 129.2, R3.2 (GND) would be 0.195 mm from the track, under 0.2 mm (`pcb.min_clearance_mm`). The gap is now ≈ 0.40 mm.
  - Why TP6 is at x 117.0: its pad keeps 1.20 mm edge to edge from the GND via at (115, 130), meeting the ≥ 1 mm GND-via-to-FB rule of gnd-stitching-vias-density. At x 116.3 the gap would be 0.5 mm.
  - Both clearances were re-checked by hand from the pad geometry in this run.
  - Person-reported: KiCad DRC with zone refill and schematic parity, `copperhead check` and `copperhead sync --dry-run` were clean at 956cc1e.
  - This commit postdates the rev B release (47b172a), so it belongs to the next revision.
  - No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit. No net, value, width or footprint change.
- Open: outputs/ (gerbers, drill, STEP, renders, BOM.csv) predates 956cc1e and stays stale until the next export (or /release-board).
- Verification: ERC clean. DRC clean on the untouched board (copperhead run_drc, no zone refill or parity; the refilled and parity-checked result is the person's KiCad run at 956cc1e). Drift clean. Docs only; no KiCad file was edited.

## 2026-11-06 — Re-export the fabrication package into outputs/ with export_outputs from the current commit. Since the last export (2873a78) the board changed: TP4 moved (708b948), C9 fitted (833bb5d), GND stitching vias and the trace tidy-up with TP7 moved to (120, 135) (9af7427), the C3 B.Cu link (5d7e534), the fab note on User.Comments (4a9f21c), and a KiCad zone refill (c4297c8). Regenerate gerbers with job file, drill, DXF outline, STEP, the SVG renders, and outputs/BOM.csv (C9 is fitted, 1 nF CL10B102KB8NNNC, LCSC C1588). If export_outputs or another copperhead tool can also write PNG renders of the top and bottom of the board into outputs/renders/, do that; if not, say so and leave PNGs out. Keep outputs/jlcpcb-bom.csv as written by copperhead export bom (commit 64a6da4, includes C9). Update outputs/README.md: list what was regenerated and from which commit; KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected, 0 parity issues; all BOM figures are datasheet-verified; the remaining checks before ordering are 2 oz copper selected on the JLC order form, through-hole J1/J2 assembly, finished hole size of at least 1.3 mm for the J1/J2 square pins (also in the fab note), tented vias kept tented, the FB1 and C5 land patterns slightly smaller than the makers' recommendation (accepted for rev A), and that the pick-and-place file comes from /release-board. Note that these changes postdate the frozen rev A release in releases/rev-a/, so this package belongs to the next revision. Do not modify the board or schematic. Every export must succeed.

- Change: outputs-reexport-post-9af7427
- Files: outputs/, outputs/BOM.csv, outputs/README.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC not required, DRC clean

## 2026-11-06 — Fabrication package re-exported after board commits 708b948–c4297c8 (next revision after rev A)

- Change: outputs-reexport-post-9af7427
- Files: outputs/gerbers/* (with gbrjob), outputs/*.drl, outputs/outline.dxf, outputs/board.step, outputs/board.svg, outputs/schematic.svg, outputs/BOM.csv (C9 row), outputs/README.md, .copperhead/renders/, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Pre-export `run_drc` clean. `export_outputs` re-ran from the current tree (after c4297c8, KiCad zone refill, per the person; the workspace is not a git repository, so the hash could not be read here). All six artifacts succeeded: gerbers with job file, drill, outline.dxf, board.step, board.svg, schematic.svg. `export_svg` sch and pcb renders regenerated.
  - The package now carries every board change since the 2873a78 export: TP4 move (708b948), C9 fitted (833bb5d), 59-via GND lattice, trace tidy-up and TP7 at (120, 135) (9af7427), straightened C3 B.Cu link (5d7e534), User.Comments fab note (4a9f21c), KiCad refill (c4297c8).
  - outputs/BOM.csv: C9 now fitted, CL10B102KB8NNNC, Qty 1, 1nF, LCSC C1588, VERIFIED (Samsung spec sheet). 22 fitted placements. C7, C8, J3, TP1–TP7, H1, H2 stay at Qty 0 with an empty LCSC cell on purpose.
  - outputs/jlcpcb-bom.csv untouched, as written by `copperhead export bom` (commit 64a6da4, includes C9).
  - **PNG renders not produced:** no available copperhead tool writes PNGs (`export_svg` is SVG only, `export_outputs` makes none), so outputs/renders/ was not created.
  - outputs/README.md: regenerated artifacts and source commit; KiCad DRC with zone refill and schematic parity 0 violations, 0 unconnected, 0 parity issues (person-reported); BOM fully datasheet-verified; pre-order checks: 2 oz on the JLC form, through-hole J1/J2, finished hole ≥ 1.3 mm for J1/J2 (also in the fab note), tented vias kept tented, FB1/C5 lands slightly small (accepted for rev A), CPL from /release-board. Stale STEP note (old 4.8 mm L1 model) corrected.
  - These changes postdate the frozen releases/rev-a/ release, so this package belongs to the next revision. releases/rev-a/ not edited.
  - No board, schematic, intent, net, value or footprint change.
- Open: generate the CPL with /release-board and confirm it lists C9 and omits C7, C8, J3, TP1–TP7. Render top/bottom PNGs with `kicad-cli pcb render` outside copperhead if wanted.
- Verification: DRC clean before export. Drift clean. ERC not re-run: no schematic edit.

## 2026-11-06 — Docs only: clear two marks that research/parts/datasheet-verification.md now settles (AGENTS.md rule 5). (1) C9: research/parts/datasheet-verification.md now has a C9 row confirming CL10B102KB8NNNC as 1.0 nF, ±10 %, 50 V DC, X7R (−55 to +125 °C), 0603 from Samsung Electro-Mechanics' product page and spec sheet read 2026-11-06 (commit fc60842). In docs/BOM.md remove the UNVERIFIED mark from the C9 row and cite that note and source, and correct the C9 leakage line in the shutdown-current table if it still says UNVERIFIED for the part figures (X7R leakage stays a typical-behaviour estimate, so word it as such rather than as verified). This matters because copperhead export bom leaves UNVERIFIED rows out of outputs/jlcpcb-bom.csv, so C9 would not be assembled. (2) U1 maximum current limit: the U1 row now records IHS(OC) 4.0 / 5.0 / 6.0 A min / typ / max from TI SLVS632L rev L, p. 5 (commit b70d55a). Wherever docs/SPEC.md, docs/SUBSYSTEMS.md or docs/BOM.md marks the 6.0 A maximum current limit 'confirm on the datasheet' or similar, replace that with the citation. Keep each BOM table cell free of pipe characters and keep Refdes, Value, Footprint first and LCSC last. Add DECISIONS and CHANGELOG entries. No schematic, board or outputs/ change. Drift must stay clean.

- Change: docs-clear-c9-u1-ilim-marks
- Files: docs/BOM.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC not required

## 2026-11-06 — C9 and TPS5430 current-limit marks cleared from datasheet verification

- Change: docs-clear-c9-u1-ilim-marks
- Files: docs/BOM.md (U1, L1 and C9 rows; C9 shutdown roll-up line), docs/SPEC.md §2 (inductor-peak row), docs/SUBSYSTEMS.md §3 (U1 and L1 bullets), .copperhead/constraints.json (power.inductor_peak_A text and source), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - **C9 CL10B102KB8NNNC:** the UNVERIFIED sentence in the BOM C9 row is replaced by a VERIFIED citation. The source is research/parts/datasheet-verification.md (C9 row, commit fc60842), from the Samsung Electro-Mechanics product page and spec sheet for CL10B102KB8NNN (generated 2026-11-06, p. 1): 1.0 nF, ±10 %, 50 V DC, X7R (−55 to +125 °C), 0603. The trailing C is the packaging code.
  - **C9 leakage:** in both the C9 row and the shutdown roll-up line it is now worded as a typical-behaviour estimate for X7R (nA class). It is not a datasheet figure, so it is not called verified. Shutdown stays ≈ 0.121 mA worst case (`power.iin_shutdown_mA`).
  - **TPS5430 current limit:** IHS(OC) 4.0 / 5.0 / 6.0 A min / typ / max (TI SLVS632L rev L, p. 5; commit b70d55a) is now cited in the BOM U1 and L1 rows, SPEC §2, SUBSYSTEMS §3 and `power.inductor_peak_A`. Every 'confirm on the datasheet' or 'not yet listed' clause on the 6.0 A maximum is gone.
  - The criterion still holds: the ≈ 3.25 A peak is below the 4.0 A minimum, and L1 Isat 12.5 A exceeds the 6.0 A maximum.
  - BOM cells hold no pipe characters. Refdes, Value, Footprint stay first and LCSC last.
  - No value, part, net, footprint, budget, schematic, board or outputs/ change. Past CHANGELOG and DECISIONS entries and the DEVPLAN §2 historical table keep their original wording.
- Open: re-run `copperhead export bom` so outputs/jlcpcb-bom.csv includes C9 (it was left out while the row was UNVERIFIED). The rest of outputs/ is unaffected by this docs change.
- Verification: drift clean. ERC and DRC not required (docs only).

## 2026-11-06 — Documentation and fab-note fixes from the design review; no schematic or copper change. (1) L.F2: add a fabrication note on the board's User.Comments layer (outside the outline is fine) and in outputs/README.md stating: J1 and J2 plated holes must finish at 1.3 mm minimum for the 0.9 mm square terminal pins; 2 oz outer copper; vias marked tented must be tented. (2) R.F1 and P.F2, accepted for this revision: record in docs/LAYOUT.md Draft quality and docs/DECISIONS.md that C3 (100 nF HF bypass) returns to GND through its via and the B.Cu link to U1's exposed-pad copper because the F.Cu channel between the VIN_BUCK copper and the D3 SW pad is only 0.65 mm; the primary input loop is closed by C1 (10 uF, about 1.6 mm from U1 pin 7) with its 1.5 mm F.Cu GND run to the D3 anode. C2 stays about 12 mm from U1 and acts as bulk input capacitance only; correct docs/SUBSYSTEMS.md where it says C2 shares the input ripple equally. Note both as candidates for a rev B layout change (move D3 or C1 to open the channel). (3) P.F1: fix docs/SPEC.md if it states the inductor-peak criterion backwards: the inductor peak at 3 A must stay below the TPS5430 minimum current limit of 4.0 A, and L1 Isat (12.5 A, SRP1265A datasheet) must exceed the 6.0 A maximum current limit. (4) B.F1: in the Vout tolerance stack use the datasheet Vref range 1.196-1.245 V over temperature (research/parts/datasheet-verification.md) with 0.1 % feedback resistors, about 4.88 to 5.09 V, and state whether that meets the SPEC tolerance. (5) B.F3: replace any remaining pre-verification figures in docs/SPEC.md, docs/SUBSYSTEMS.md or docs/DECISIONS.md that docs/BOM.md has corrected (U1 quiescent 2 mA typ 4.4 mA max, D2 Vz 11.4-12.7 V, L1 body 13.5 x 12.5 mm). ERC, DRC and drift must stay clean.

- Change: review-docs-fab-notes
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, outputs/README.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/LAYOUT.md, docs/BOM.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Design review doc and fab-note fixes (L.F2, R.F1, P.F2, P.F1, B.F1, B.F3)

- Change: review-docs-fab-notes
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (one Cmts.User gr_text only), outputs/README.md, docs/LAYOUT.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/BOM.md (C2 and L1 rows), docs/DECISIONS.md, docs/CHANGELOG.md, .copperhead/constraints.json (power.inductor_peak_A, power.vout_V)
- Summary:
  - **L.F2 fab note:** a User.Comments (`Cmts.User`) text at (100, 143), below the outline. It says J1/J2 plated holes finish at 1.3 mm minimum for the 0.9 mm square pins, outer copper is 2 oz, and vias marked tented must be tented. The same notes are in a new outputs/README.md section. No copper, mask or silk change.
  - **R.F1 / P.F2 accepted for rev A:** C3 returns to GND through its via and the B.Cu link to U1 pad 9, because the F.Cu channel between VIN_BUCK and the D3 SW pad is 0.65 mm. C1 (≈ 1.6 mm from U1.7, 1.5 mm F.Cu GND run to the D3 anode) closes the primary input loop. C2 (≈ 12 mm away) is bulk only. SUBSYSTEMS §3 and the BOM C2 row are corrected. Rev B candidate: move D3 or C1.
  - **P.F1:** inductor-peak criterion corrected. The 3.25 A peak stays below the 4.0 A minimum limit, and L1 Isat 12.5 A exceeds the 6.0 A maximum limit. The 6.0 A maximum is to be confirmed on the datasheet.
  - **B.F1:** Vout stack with datasheet Vref 1.196–1.245 V and 0.1 % R2/R3 gives ≈ 4.880–5.095 V, which meets 4.85–5.15 V with ≈ 30 mV margin low.
  - **B.F3:** pre-verification figures replaced in SPEC and SUBSYSTEMS. Three DECISIONS entries get bracketed corrections (ENA 7 V, L1 13.5 × 12.5 mm, D2 12.7 V); history is kept.
  - No schematic, net, value, footprint, track, via, zone or zone-fill change. The frozen releases/rev-a/ copy was not edited.
- Open:
  - Confirm the TPS5430 6.0 A maximum current limit on TI SLVS632L and add it to research/parts/datasheet-verification.md.
  - Re-export outputs/ (or /release-board) so the User.Comments note appears in the package. The existing gerbers stay valid, because the note is on a non-fab layer.
  - docs/DEVPLAN.md §2 keeps its historical 'what was checked' table with pre-verification wording; it is marked as a record, not current figures.
  - The outputs/README.md STEP row still describes the old 4.8 mm L1 model; it is stale until re-export.
- Verification: DRC clean. ERC clean. Drift clean.

## 2026-11-06 — Docs only: bring docs/LAYOUT.md, docs/DECISIONS.md and docs/CHANGELOG.md in step with board commit 9af7427, which the person chose to make with a KiCad pcbnew script because copperhead do cannot refill zones (its DRC fails on stale fill when copper moves inside a GND pour, and an empty fill fails the unrouted check). Read the commit message of 9af7427 for the full list and read the board for exact coordinates. Record: (1) the trace tidy-up: R1.2 to the via at (136.5, 110); SW from U1.8 leaving horizontally into the y 124 run; FB along the y 128.2 channel with a 45 into R3.1 and a tee from R2.2; C1.2 to D3.2 as 45 plus horizontal; the F.Cu strap between the D3 vias at y 135.5; VIN_RAW true 45 into F1.1; the C2.2 and C3.2 via stubs at 45; the orthogonal VIN_BUCK and LED_A runs; and the 14 mitred corners. (2) TP7 moved from (104, 132.5) to (120, 135) on the EN trace 4 mm from J3, with the 20 mm EN run to the board edge removed: update the placement table (it still says TP7 at (108, 133)) and the EN route description (it now ends at TP7 (120, 135)). (3) GND stitching: the 34 ad hoc vias from 877f152 were replaced by 59 tented vias on a lattice aligned with the edge fence (27 on 5 mm nodes, 20 on 2.5 mm, 12 on 1.25 mm), with the seven pad-return vias unchanged and a largest distance to a GND via of 4.5 mm inside both pours; replace the old via-site list with this. Note that zones were refilled with KiCad, and that KiCad DRC with schematic parity, copperhead check and sync --dry-run were clean at 9af7427. Also note in DECISIONS that the via lattice and trace tidy-up were made outside copperhead do at the person's request, and why. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json or any other file; outputs/ stays stale until the next export. Drift must stay clean.

- Change: docs-sync-board-9af7427
- Files: docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Docs synced to board commit 9af7427 (pcbnew-script tidy-up, TP7 move, GND via lattice)

- Change: docs-sync-board-9af7427
- Files: docs/LAYOUT.md (placement table J3/C9/TP7 row, Fine list TP7 line, Routed: EN route and new trace tidy-up bullet, GND links R1.2 bullet, stitching via-density section), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Board commit 9af7427 was made by the person with a KiCad pcbnew script outside copperhead do. copperhead do cannot refill zones: its DRC fails on stale fill when copper moves inside a GND pour, and an empty fill fails the unrouted check. This run records that commit; it edits no KiCad file.
  - Source: coordinates read from the live .kicad_pcb. The change list is the person's summary of the commit, because the workspace is not a git repository and the commit message could not be read here.
  - Trace tidy-up (geometry only, no net, width class, footprint or pad change):
    - R1.2 now runs into the D1.2 GND via at (136.5, 110).
    - SW leaves U1.8 horizontally and turns 45° into the y 124 run.
    - FB runs along the y 128.2 channel, with a 45° into R3.1 and a tee from R2.2.
    - C1.2 to D3.2 is a 45° plus a horizontal; a 1.0 mm F.Cu strap joins the D3 vias at y 135.5.
    - VIN_RAW enters F1.1 at a true 45°; the C2.2 and C3.2 via stubs run at 45°.
    - VIN_BUCK and LED_A runs are orthogonal; 14 corners mitred (person's count).
  - TP7 moved from (104, 132.5) to (120, 135), on the EN trace 4 mm west of J3.1 (124, 135). The ~20 mm EN run to the left board edge is removed, so EN ends at TP7. Still no EN pull-up (`protection.ena_pin_max_V`).
  - GND stitching: the 34 ad hoc interior vias of gnd-stitching-vias-density were replaced by 59 tented vias (0.6/0.3 mm) on a lattice aligned with the edge fence: 27 on 5 mm nodes, 20 on 2.5 mm, 12 on 1.25 mm. All 59 sites are listed in LAYOUT.md. The seven pad-return vias (5717c400-000d) with their stubs and the 34 edge-fence and power-stage vias (5717c400-000b) are unchanged. Largest distance to a GND via inside both pours: 4.5 mm (person-reported).
  - Zones were refilled with KiCad. KiCad DRC with schematic parity, `copperhead check` and `copperhead sync --dry-run` were clean at 9af7427 (person-reported).
  - No .kicad_pcb, .kicad_sch, schematic.intent.json or other file edit. The frozen releases/rev-a/ copy was not edited.
- Open: outputs/ (gerbers, drill, STEP, renders) predates 9af7427 and stays stale until the next export (or /release-board).
- Verification: DRC clean on the untouched board. ERC clean. Drift clean. Docs only; no KiCad file was edited.

## 2026-11-06 — Straighten the B.Cu GND link from the C3 pad 2 via to U1's exposed pad. The 0.5 mm B.Cu GND segment (uuid c0ffee00-0009-4000-8000-000000000023) runs from the via at (131.45, 125.3) to (127.2, 125.9) and leans about 8 degrees, because commit b9fab6c (change vias-out-of-paste-c3-d1) moved the via from (132.07, 125.9) to (131.45, 125.3) but left the U1 end at y 125.9. Move only the U1 end to (127.2, 125.3) so the segment runs horizontally at y 125.3. That point is still inside U1 pad 9's B.Cu copper (2.95 x 4.5 mm centred on U1 at (126, 126.5), so about y 124.25 to 128.75), so the C3.2 to exposed-pad GND path is unchanged. Keep width 0.5 mm, layer B.Cu, net GND. No footprint, via, other track or zone change; do not edit zone fills (this is GND on the GND pour, so the B.Cu fill does not conflict). Cosmetic layout tidy-up after the rev A release, requested by the person (docs/LAYOUT.md). Record it in docs/LAYOUT.md where the C3.2 via link is described, plus DECISIONS and CHANGELOG. ERC and DRC must be clean with 0 unconnected items.

- Change: c3-via-bcu-link-straighten
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — C3.2 via B.Cu GND link to U1 pad 9 straightened

- Change: c3-via-bcu-link-straighten
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (end point of GND segment c0ffee00-0009-4000-8000-000000000023 only), docs/LAYOUT.md (GND links: C3.2 bullet), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - This is a cosmetic tidy-up after the rev A release, requested by the person. The 0.5 mm B.Cu GND link from the C3.2 via to U1 pad 9's B.Cu copper leaned about 8°. Change vias-out-of-paste-c3-d1 (commit b9fab6c) had moved the via from (132.075, 125.9) to (131.45, 125.3) and left the U1 end at (127.2, 125.9).
  - The U1 end moved to (127.2, 125.3), so the segment now runs horizontally at y 125.3. The start (131.45, 125.3), width 0.5 mm, layer B.Cu, net GND and uuid are unchanged.
  - The new end is still inside pad 9's 2.95 × 4.5 mm B.Cu copper (about y 124.25–128.75), so the C3.2 to exposed-pad GND path is unchanged (`thermal.tj_max_C`).
  - No footprint, via, other track, zone or zone-fill change. No schematic or net change. The frozen releases/rev-a/ copy was not edited.
- Open:
  - Refill the zones with KiCad (`kicad-cli pcb drc --refill-zones --save-board`).
  - Re-export outputs/ (or /release-board), because the B.Cu gerber predates this change.
- Verification: DRC clean (0 violations, 0 unconnected). ERC clean. Drift clean.

## 2026-11-06 — Add GND stitching vias, requested by the person after a via-density review. The board has 39 GND vias but only 14 inside the edge fence, all placed ad hoc, and the power-stage ground returns are a few mm from the nearest via: C1.2 (131.4, 130.475) 2.2 mm, C2.2 (135.475, 117.2) 1.9 mm, C5.2 (148.75, 116.8) 3.6 mm, C6.2 (152.8, 117.025) 4.6 mm, D3.2 (136.4, 132.9) 2.6 mm. Interior spots far from any GND via or GND through-hole pad: (118, 114) 12.5 mm, (114, 121) 12 mm, (126, 113) 10.9 mm, (113, 129) 9.7 mm, (145, 129) 8.9 mm, (146, 112) 8.4 mm. The front GND pour is cut up by power traces, so these returns go through long, narrow F.Cu paths before reaching the solid B.Cu pour. (1) Add one or two GND vias within about 1 mm of the pad edge of each of C1.2, C2.2, C5.2, C6.2 and D3.2, on the side away from the pad's other terminal. Each via sits in the F.Cu GND pour or has a short GND trace to the pad. (2) Add GND vias on a roughly 5 mm grid in the interior wherever both GND pours are present, so no interior point inside the pours is more than about 4 mm from a GND via. Use the same via as the existing stitching vias (0.6 mm diameter, net GND, F.Cu-B.Cu), tented on both sides like the vias in commit eacf079. Keep every via outside every pad and solder-paste opening (design review finding layout-lint.via-in-paste), outside the SW_copper zones on both layers, outside the H1/H2 keepouts, at least 1 mm from the FB and EN traces and the R2.1 Kelvin feed, and clear of all other nets by the board's clearance rules. Do not move or change any footprint, track or existing via. Do not edit zone fills; GND vias inside the GND pours do not conflict with the existing fill, and the zones are refilled afterwards with KiCad. The frozen releases/rev-a/ copy is not edited. Record the via sites in docs/LAYOUT.md, and add DECISIONS and CHANGELOG entries. ERC and DRC must be clean with 0 unrouted connections, and drift clean.

- Change: gnd-stitching-vias-density
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — GND stitching-via density pass (41 vias, 5 stubs)

- Change: gnd-stitching-vias-density
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (41 new via blocks, 5 new GND segments; nothing else), docs/LAYOUT.md (via-density pass under the stitching-via item), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Why: a via-density review asked for this. The board had 39 GND vias, only 14 of them inside the edge fence. The power-stage returns were 1.9–4.6 mm from the nearest via (C1.2, C2.2, C5.2, C6.2, D3.2), and interior spots were up to 12.5 mm from any GND via or GND through-hole pad. The F.Cu pour is cut up by the power traces, so these returns ran through long, narrow F.Cu paths before reaching the solid B.Cu pour.
  - Via type: every new via is 0.6 mm pad, 0.3 mm drill, F.Cu–B.Cu, net GND. Each carries an explicit `(tenting (front yes) (back yes))` like tent-gnd-vias-c3-d1 (commit eacf079), with `covering`, `plugging`, `capping` and `filling` set to `no`. Via uuids use prefix `5717c400-000d`, stub uuids `5717c400-000e`.
  - Return vias, each within about 1 mm of the pad edge on the side away from the pad's other terminal:
    - C1.2: (132, 131.55).
    - C2.2: (136.55, 118.95), with a 0.5 mm stub. It sits at the south-east corner because the B.Cu Kelvin and R4 feeds run under C2.
    - C5.2/C6.2: a strap via at (150.975, 116.8) on a 0.5 mm F.Cu strap between the two GND pads, plus (152.3/153.4, 115.65) above C6.2.
    - D3.2: (135.2/137.6, 134.75), each with a 0.5 mm stub.
  - Interior grid: 34 vias on a roughly 5 mm grid wherever both GND pours are present. That includes two added after the first DRC to close gaps of about 4.2–4.7 mm, at (140.5, 112.5) and (142.5, 135.5). The sites are listed in docs/LAYOUT.md.
  - Rules held: no via is in a pad or paste opening (layout-lint.via-in-paste), in either SW_copper zone, or in the H1/H2 keepouts. All clear other nets by the board rules.
  - Spacing to FB, EN and the R2.1 Kelvin feed is at least 1 mm edge to edge, by hand calculation; DRC does not check this. The tightest sites are about 1.1 mm from the C9.1 EN pad and about 1.15 mm from the EN trace at y 135.
  - Coverage of about 4 mm is by hand calculation, not tool-verified. It excludes the U1/SW area, where the B.Cu SW zone displaces the GND pour.
  - No footprint, track, existing via, zone or zone-fill change. No schematic or net change. The frozen releases/rev-a/ copy was not edited.
- Open:
  - Refill with `kicad-cli pcb drc --refill-zones --save-board` and re-run DRC.
  - Check that every new via lands in GND fill on both layers; DRC does not flag a via that touches pour on only one layer.
  - Re-export outputs/ (or /release-board), because the copper, drill and mask files predate this change.
- Verification: DRC clean (0 violations, 0 unconnected). ERC clean. Drift clean.

## 2026-11-06 — Re-apply the board half of change fit-c9-en-filter (commit 59a5f79), which was lost: a concurrent copperhead run (commit 708b948, TP4 trace) rewrote the .kicad_pcb from its own copy and overwrote it. The schematic and docs/BOM.md already show C9 as fitted, 1nF, CL10B102KB8NNNC, LCSC C1588, but the C9 board footprint still has Value 'DNP' and (attr smd exclude_from_pos_files), so C9 would be left out of the pick-and-place file and fails schematic parity. On the C9 footprint only: set the Value property to 1nF and the attr to (attr smd). Keep its footprint, position (131, 132.5), rotation, pads, nets (EN, GND), copper and text positions. Leave TP4 at (147.5, 109.08) with its Reference at local (-2.4, 0) as committed in 708b948. No other footprint, track, via or zone change; do not edit zone fills. The docs already describe C9 as fitted, so only add DECISIONS and CHANGELOG entries saying the board edit was re-applied. ERC and DRC must be clean and drift clean.

- Change: reapply-c9-fitted-board
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — C9 board footprint re-applied as fitted (board half of fit-c9-en-filter)

- Change: reapply-c9-fitted-board
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (C9 Value and `(attr …)` only), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The board half of change fit-c9-en-filter (commit 59a5f79) was lost. The concurrent run tp4-gnd-trace-straighten (commit 708b948) rewrote the .kicad_pcb from its own copy, so C9 was back to Value `DNP` and `(attr smd exclude_from_pos_files)`. The schematic and docs/BOM.md already showed C9 fitted (1nF, CL10B102KB8NNNC, LCSC C1588).
  - Re-applied on the C9 footprint only (path /2024000f-9ed6-5dea-8bf3-77783667258d): Value `DNP` → `1nF` (same uuid, position, F.Fab layer and effects), and `(attr smd exclude_from_pos_files)` → `(attr smd)`, so C9 is placed and appears in the CPL.
  - Footprint, position (131, 132.5), rotation, pads, nets (C9.1 EN, C9.2 GND), copper and text positions are unchanged.
  - TP4 was checked and left as committed in 708b948: at (147.5, 109.08), Reference at local (−2.4, 0).
  - No other footprint, track, via, zone or zone-fill change. No schematic or other doc change; SUBSYSTEMS, BOM, PINOUT, LAYOUT and DEVPLAN already describe C9 as fitted. The frozen releases/rev-a/ copy was not edited.
  - Constraints unchanged from fit-c9-en-filter: `protection.ena_pin_max_V` (a capacitor cannot pull EN high), `power.iin_shutdown_mA` (≈ 0.121 mA worst case). `assembly.qty` is restored (C9 back in the CPL).
- Open: re-run `kicad-cli pcb drc --schematic-parity` in KiCad to confirm the C9 Value parity (copperhead run_drc does not run it). Re-export outputs/ (or /release-board) so C9 appears in the CPL and assembly BOM. Serialise board-editing runs so concurrent runs cannot overwrite each other's .kicad_pcb.
- Verification: DRC clean. ERC clean. Drift clean.

## 2026-11-06 — Fit C9 as the EN noise filter (docs/BRIEF.md Assumptions, 2026-11-06; design review finding P.F3/S.F1): change C9 from DNP to 1 nF, MPN CL10B102KB8NNNC (Samsung, 1 nF +/-10 % 50 V X7R 0603), LCSC C1588 (research/parts/jlcpcb-sourcing.md), in the schematic value and in docs/BOM.md, and mark it fitted. Remove exclude_from_pos_files from the C9 board footprint so it is placed, and keep its footprint, position and nets (EN to GND). Update DECISIONS and SUBSYSTEMS where they say C9 is DNP. ERC and DRC must be clean.

- Change: fit-c9-en-filter
- Files: schematic.intent.json, docs/BOM.md, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/PINOUT.md, docs/SUBSYSTEMS.md, docs/LAYOUT.md, docs/DEVPLAN.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Straighten the GND trace from TP4 to J2 pin 2. TP4 (TestPoint_Pad_D1.5mm) is at (147.50, 110.00) and J2 pin 2 is at (154.00, 109.08), so the single 0.5 mm F.Cu GND segment between their centres (uuid c0ffee00-0009-4000-8000-000000000027) leans about 8 degrees, which looks like a routing error. Move TP4 up 0.92 mm to (147.50, 109.08), keeping its rotation, and make that segment run horizontally from TP4's centre to J2 pin 2's centre at y = 109.08 with the same width, layer and net. This moves TP4 further from the 2 mm +5V trace at y = 113.4. Moving TP4 up puts its Reference text (offset 0, -1.648) on top of J2's '-GND' polarity label at (146, 107), so move TP4's Reference and, if needed, Value text (the (at ...) inside the properties, not the footprint) to readable spots near TP4 clear of all pads, the new trace, and other silkscreen. Do not move J2's text. No other footprint, track, via or zone changes; the GND zones are filled and are refilled afterwards with KiCad, so do not edit the fills. The frozen releases/rev-a/ copy is not edited. Cosmetic layout tidy-up after the rev A release (docs/LAYOUT.md). ERC and DRC must be clean with 0 unrouted connections and no silkscreen warnings.

- Change: tp4-gnd-trace-straighten
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — C9 fitted as the EN noise filter (1 nF CL10B102KB8NNNC)

- Change: fit-c9-en-filter
- Files: schematic.intent.json, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch (re-drafted), tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (C9 attr and Value; TP4 Reference `(at …)`), docs/BOM.md (C9 row, header notes, roll-up), docs/SUBSYSTEMS.md §5, docs/PINOUT.md, docs/LAYOUT.md, docs/DEVPLAN.md §3, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Per docs/BRIEF.md Assumptions (decided 2026-11-06, design review finding P.F3/S.F1), C9 changes from DNP to 1 nF Samsung CL10B102KB8NNNC (±10 %, 50 V, X7R, 0603), LCSC C1588 (research/parts/jlcpcb-sourcing.md). Its figures are UNVERIFIED until the manufacturer datasheet is checked in research/parts/datasheet-verification.md.
  - Schematic: value set through schematic.intent.json and a re-draft (the sheet is engine-drafted). Nets unchanged: C9.1 EN, C9.2 GND.
  - Board: C9 `(attr smd exclude_from_pos_files)` → `(attr smd)`, so it is placed and appears in the CPL. Value text `1nF`. Footprint, position (131, 132.5), pads and copper unchanged. Anchored on C9's path UUID /2024000f-….
  - Constraint check: a capacitor cannot pull EN high (`protection.ena_pin_max_V`) and is not the resistor to GND the datasheet forbids; X7R leakage is nA, so `power.iin_shutdown_mA` still holds (≈ 0.121 mA worst case); 50 V on a < 7 V pin meets `components.voltage_derating`; 0603 meets `assembly.qty`. Enable delay ≈ 1.5 ms (1.5 MΩ internal pull-up × 1 nF), harmless.
  - Outside the C9 scope, needed for clean DRC: DRC reported silk_overlap between the TP4 Reference and J2's `-GND` text at (146, 107). Only the TP4 Reference `(at …)` moved, from local (0, −1.648) to (−2.4, 0), left of the pad. No pad, copper or position change.
  - C7, C8 and J3 stay DNP with `exclude_from_pos_files`. The frozen releases/rev-a/ copy was not edited.
- Open: verify CL10B102KB8NNNC against the Samsung datasheet. Re-export outputs/ (BOM.csv, jlcpcb-bom.csv via `copperhead export bom`, CPL via /release-board) so C9 appears in the assembly BOM and CPL; the current outputs list C9 as DNP. Re-run `kicad-cli pcb drc --schematic-parity` in KiCad (Value changed on both sides).
- Verification: ERC clean. DRC clean. Legibility 0 errors (1 low-utilization advisory, A3 kept). Drift clean.

## 2026-11-06 — TP4 to J2 pin 2 GND trace straightened

- Change: tp4-gnd-trace-straighten
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (TP4 footprint position and Reference offset, GND segment c0ffee00-0009-4000-8000-000000000027 start point only), docs/LAYOUT.md (TP4 placement row, GND links TP4 bullet), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - This is a cosmetic tidy-up after the rev A release. The 0.5 mm F.Cu GND segment from TP4 to J2 pin 2 leaned about 8°, which read as a routing error.
  - TP4 moved with move_footprint from (147.5, 110) to (147.5, 109.08). Its rotation is unchanged, at 0°. TP4 is now further from the 2 mm +5V trace at y 113.4.
  - The segment now runs horizontally from (147.5, 109.08) to J2 pin 2 at (154, 109.08). Width (0.5 mm), layer (F.Cu) and net (GND) are unchanged.
  - TP4's F.SilkS Reference offset changed from (0, −1.648) to (−2.4, 0), left of the pad. The default offset would have sat on J2's `-GND` label at (146, 107). move_footprint set this offset itself and did not report it. I kept it because it clears FB1 pad 2, TP4's silk circle, J2's `-GND` label and the new trace.
  - TP4's Value and `${REFERENCE}` texts are on F.Fab and were left unchanged. J2's text was not moved.
  - No other footprint, track, via, zone or zone-fill change. No net, value or schematic change. The frozen releases/rev-a/ copy was not edited.
- Open:
  - Refill the zones with KiCad (`kicad-cli pcb drc --refill-zones --save-board`), because the pour around the moved pad predates this change.
  - Re-export outputs/ (or /release-board), because the copper and silkscreen gerbers predate this change.
- Verification: DRC clean (0 violations, 0 unconnected, no silkscreen findings). ERC clean. Drift clean.

## 2026-11-06 — Tent the three GND vias that were just moved out of paste openings (commit b9fab6c): the via at (131.45, 125.3) beside C3 pad 2, and the two vias at (136.50, 110.0) and (137.50, 110.0) below D1 pad 2. The C3 via edge is about 0.01 mm from the pad corner, so there is no solder-mask web between them and an open via can still pull solder from the joint. Cover these vias with solder mask on both F.Mask and B.Mask (tented), using the via's own tenting property. No copper, net or position change; do not edit zone fills. ERC and DRC must be clean with 0 unrouted connections.

- Change: tent-gnd-vias-c3-d1
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — GND vias beside C3 pad 2 and below D1 pad 2 tented on both sides

- Change: tent-gnd-vias-c3-d1
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (three via `(covering …)` blocks only), docs/LAYOUT.md (GND links: C3.2 and D1.2 bullets), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Follow-up to vias-out-of-paste-c3-d1 (commit b9fab6c). The C3.2 via at (131.45, 125.3) has its edge about 0.01 mm from the pad corner, so there is no solder-mask web between them. The D1.2 vias at (136.5, 110.0) and (137.5, 110.0) sit 0.2 mm below the pad. Untented, the open barrels could still pull solder from those joints.
  - The three vias (uuids c0ffee00-0009-4000-8000-000000000022, -29, -2a) now carry their own explicit tenting property, `(tenting (front yes) (back yes))`, so they are covered by F.Mask and B.Mask whatever the board-level default says.
  - Correction made within this run: a first edit set the IPC-4761 `(covering (front yes) (back yes))` block instead. `covering` is a separate Type II cover process, not solder-mask tenting, so it was reverted to `(front no) (back no)` on all three vias before DRC was re-run. `plugging`, `capping` and `filling` stay `no`.
  - Finding: before this change the three vias had no per-via `tenting` token and inherited the board `(setup)` default `(tenting (front yes) (back yes))`, so they were probably already tented in the existing mask gerbers. The explicit per-via token stops their tenting from depending on that global setting.
  - No position, size, drill, layer, net, track, zone or zone-fill change. No other via changed. The frozen releases/rev-a/design/ copy was not edited.
- Open: re-export outputs/ (or /release-board), because the F.Mask and B.Mask gerbers predate this change. In the KiCad 3D viewer or a mask gerber, check that the three vias show no mask opening. Confirm at order time that JLCPCB honours per-via tenting from the gerbers (no global "untented vias" option selected).
- Verification: DRC clean (0 violations, 0 unconnected). ERC clean. Drift clean.

## 2026-11-06 — Move vias out of solder-paste openings (design review findings layout-lint.via-in-paste): the GND via at (132.07, 125.90) sits inside C3 pad 2, and the two GND vias at (136.50, 108.60) and (137.50, 108.60) sit inside D1 pad 2. Unfilled vias in a paste opening wick solder away and starve the joint. Move each via just outside its pad, keeping it on GND with a short trace from the pad and the same connection to the B.Cu GND copper (C3.2 currently reaches U1's exposed-pad copper through its via; keep that path), clear of other nets. The GND zones are filled; do not fill or edit the fills, they are refilled afterwards with KiCad. ERC and DRC must be clean with 0 unrouted connections.

- Change: vias-out-of-paste-c3-d1
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Vias moved out of the C3 and D1 solder-paste openings

- Change: vias-out-of-paste-c3-d1
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (3 vias, their B.Cu links, 3 new F.Cu stubs, C3 pad 2 zone_connect), docs/LAYOUT.md (GND links), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Design review finding layout-lint.via-in-paste: unfilled GND vias inside paste openings would wick solder and starve the joints.
  - C3.2 via: (132.075, 125.9) → (131.45, 125.3), just outside the pad's upper-left corner. A 0.3 mm F.Cu stub joins it to the pad. The 0.5 mm B.Cu link to U1 pad 9 copper now starts at the new via, so the C3.2 return to the exposed-pad copper is kept (`thermal.tj_max_C`). Clearance is about 0.25 mm to the SW run and about 0.26 mm to C3 pad 1 (≥ 0.2 mm, `pcb.min_clearance_mm`).
  - With the in-pad via gone, DRC reported starved_thermal on C3 pad 2 (1 spoke, minimum 2). C3 pad 2 now has a pad-level `(zone_connect 2)` (solid) override, the same fix as R1 pad 2. The board-wide spoke minimum is unchanged.
  - D1.2 vias: (136.5/137.5, 108.6) → (136.5/137.5, 110.0), 0.2 mm below the pad edge. They are joined to the pad by 1.0 mm F.Cu stubs, and the 1.0 mm B.Cu strap between them is kept. The R1.2 GND trace still ends on D1 pad 2 copper at (136.5, 108.6), so its path to the B.Cu pour now runs through the stub and the via.
  - Same uuids for the moved vias and links. No net, value, footprint position or schematic change. Zone fills were not edited.
- Open: refill with `kicad-cli pcb drc --refill-zones --save-board` and re-run DRC. Re-export outputs/ (or /release-board), because the gerbers predate this change.
- Verification: DRC clean (0 violations, 0 unconnected). ERC clean. Drift clean.

## 2026-11-06 — Take the not-fitted parts C7, C8, C9 and J3 out of the pick-and-place file: set exclude_from_pos_files on their board footprints. They are DNP in docs/BOM.md for rev A (C7/C8 input-filter capacitors bridged by FB1, C9 EN filter, J3 EN header) and have no LCSC number, but kicad-cli pcb export pos currently lists them, so an assembler could place them (design review finding assembly-parity.dnp-placed). Leave exclude_from_bom unchanged so KiCad's schematic parity check stays clean. No pad, net, copper or position change. ERC and DRC must stay clean.

- Change: dnp-exclude-from-pos
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/BOM.md, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — DNP parts C7, C8, C9 and J3 excluded from the pick-and-place file

- Change: dnp-exclude-from-pos
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (C7, C8, C9, J3 `(attr …)` only), docs/BOM.md (header note), docs/LAYOUT.md (Test-point footprint attributes section), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Design review finding assembly-parity.dnp-placed: `kicad-cli pcb export pos` listed the rev A DNP parts C7, C8, C9 and J3, so an assembler could have placed them.
  - C7, C8 and C9 now read `(attr smd exclude_from_pos_files)`, and J3 reads `(attr through_hole exclude_from_pos_files)`. Each edit was anchored on the footprint's own path UUID.
  - `exclude_from_bom` is left unset on purpose, so the footprints still match the schematic symbols (`in_bom yes`) and KiCad's schematic-parity check stays clean. They stay out of the JLCPCB assembly BOM because it is generated from docs/BOM.md, where they have no LCSC number (`assembly.qty`).
  - No pad, net, copper, zone, position or schematic change.
- Open: re-run `kicad-cli pcb export pos` (or /release-board) to confirm the CPL no longer lists C7, C8, C9 or J3; the copperhead tools did not regenerate the CPL. Re-run `kicad-cli pcb drc --schematic-parity` in KiCad to confirm parity (copperhead run_drc does not run it).
- Verification: DRC clean. ERC clean. Drift clean.

## 2026-11-06 — Fix the missing 3D models on three footprints (visual and STEP only; lib/3d/README.md). U1: its model path points to ${KICAD10_3DMODEL_DIR}/Package_SO.3dshapes/HTSOP-8-1EP_3.9x4.9mm_Pitch1.27mm.step, which does not exist; point it to ${KICAD10_3DMODEL_DIR}/Package_SO.3dshapes/TI_SO-PowerPAD-8.step. L1: replace its model with ${KIPRJMOD}/lib/3d/l1/easyeda_l1.3dshapes/IND-SMD_L12.8-W13.5-H6.5.step. F1: replace its model with ${KIPRJMOD}/lib/3d/f1/easyeda_f1.3dshapes/FUSE-SMD_L9.7-W5.0-H3.8.step. Offset 0, rotation 0, scale 1 for all three (alignment checked by rendering a scratch copy with kicad-cli pcb render). Edit only the (model ...) entries of these three footprints; no pad, net, copper or position change. ERC and DRC must stay clean.

- Change: fix-3d-models-u1-l1-f1
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/BOM.md, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — 3D models fixed on U1, L1 and F1

- Change: fix-3d-models-u1-l1-f1
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (U1, L1 and F1 `(model …)` path strings only), docs/BOM.md (L1 row), docs/LAYOUT.md (item 10), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - U1: `${KICAD10_3DMODEL_DIR}/Package_SO.3dshapes/TI_SO-PowerPAD-8.step`. The old HTSOP-8-1EP_3.9x4.9mm_Pitch1.27mm.step path did not exist, so U1 had no body in renders or STEP.
  - L1: `${KIPRJMOD}/lib/3d/l1/easyeda_l1.3dshapes/IND-SMD_L12.8-W13.5-H6.5.step` (EasyEDA/LCSC C189912), replacing the stock 4.8 mm SRP1245A body. The model is 6.5 mm tall; the datasheet's 6.2 mm stays the reference for enclosure clearance.
  - F1: `${KIPRJMOD}/lib/3d/f1/easyeda_f1.3dshapes/FUSE-SMD_L9.7-W5.0-H3.8.step` (EasyEDA/LCSC C206915).
  - Offset 0, scale 1, rotation 0 for all three (already set; unchanged), per lib/3d/README.md. Alignment was checked by the person with `kicad-cli pcb render` on a scratch copy; this run did not re-render.
  - No pad, net, copper, zone, position or schematic change. The frozen releases/rev-a/design/ copy was not edited.
- Open: outputs/board.step and the SVG renders predate this change. Re-export (or release with /release-board) to get the new bodies in the STEP.
- Verification: DRC clean (no lib_footprint_mismatch). ERC clean. Drift clean.

## 2026-11-06 — Re-export the fabrication package into outputs/ with export_outputs from the current commit: docs/BOM.md is now verified against manufacturer datasheets (research/parts/datasheet-verification.md, no UNVERIFIED marks left) and FB1's reference label moved. Regenerate gerbers, drill, DXF outline, STEP, SVG renders and outputs/BOM.csv. Keep outputs/jlcpcb-bom.csv as written by copperhead export bom. Update outputs/README.md: KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected, 0 parity issues at all severities; BOM figures are datasheet-verified; remaining checks before ordering are 2 oz copper selected on the JLC order form, through-hole J1/J2 assembly, finished hole size of at least 1.3 mm for the J1/J2 square pins, the FB1 and C5 land patterns being slightly smaller than the makers' recommendation (accepted for rev A), and that the pick-and-place file comes from /release-board. Do not modify the board or schematic. Every export must succeed.

- Change: outputs-reexport-verified-bom
- Files: outputs/, outputs/BOM.csv, outputs/README.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC not required, DRC clean

## 2026-11-06 — Fabrication package re-exported after BOM datasheet verification and the FB1 label move

- Change: outputs-reexport-verified-bom
- Files: outputs/gerbers/*, outputs/*.drl, outputs/outline.dxf, outputs/board.step, outputs/board.svg, outputs/schematic.svg, outputs/BOM.csv, outputs/README.md, .copperhead/renders/, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - `export_outputs` re-ran from the current commit. All six artifacts succeeded: gerbers, drill, outline.dxf, board.step, board.svg and schematic.svg. The export_svg sch and pcb renders were also regenerated. The silkscreen gerbers now carry the moved FB1 Reference label.
  - outputs/BOM.csv was regenerated from the datasheet-verified docs/BOM.md. Every UNVERIFIED note is replaced by the verified figure and its datasheet citation. The LCSC column stays last. DNP and no-part rows stay at Qty 0 with an empty LCSC cell.
  - outputs/jlcpcb-bom.csv is untouched, as written by `copperhead export bom`.
  - outputs/README.md:
    - DRC: KiCad DRC with zone refill and schematic parity at all severities reports 0 violations, 0 unconnected and 0 parity issues (person-reported for the current commit). This run's run_drc agrees but does not run parity.
    - FB1 silk and UNVERIFIED items are closed.
    - Remaining pre-order checks: 2 oz copper on the JLC form; through-hole J1/J2; finished hole ≥ 1.3 mm for the square J1/J2 pins; FB1/C5 land patterns slightly small (accepted for rev A); CPL from /release-board.
  - No board, schematic, net, value or footprint change.
- Verification: DRC clean before export. Drift clean. ERC not re-run: no schematic edit.

## 2026-11-06 — Update docs/BOM.md from research/parts/datasheet-verification.md (every fitted part checked against its manufacturer datasheet on 2026-11-06). For every figure that note confirms, remove its UNVERIFIED mark and cite the datasheet revision from the note. Apply the corrections: U1 quiescent current 2 mA typ, 4.4 mA max (not ~3 mA), and update any no-load or shutdown budget roll-up in BOM.md or docs/SPEC.md that used ~3 mA; D2 Vz 11.4-12.7 V (still under Q1's +/-20 V Vgs); D4 VF specified at 20 mA (2.0 V typ, 2.4 V max), about 1.9-2.0 V at 2 mA from the typical curve; L1 body 13.5 x 12.5 mm; F1 cold resistance 18.6 mOhm, melting I2t 34.4 A2s, about 2.94 A continuous at 50 C after derating; J1/J2 IEC 17.5 A, 250 V (III/3), UL 300 V 10-15 A, 1.3 mm drill matches Phoenix's recommended hole; C1/C2 about 7.4 uF at 18 V; C6 about 17.5-18 uF at 5 V. Record as accepted for rev A that the KiCad land patterns for FB1 (R_2512_6332Metric) and C5 (CP_Elec_8x10) are slightly smaller than the makers' recommended lands, with terminations still fully on copper. Remove the BOM header line saying every MPN is UNVERIFIED. Keep each table cell free of pipe characters, and keep Refdes, Value, Footprint as the first three columns and LCSC last. Docs only; no schematic or board change; drift must stay clean.

- Change: bom-datasheet-verification
- Files: docs/BOM.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/DEVPLAN.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC not required

## 2026-11-06 — BOM figures verified against manufacturer datasheets

- Change: bom-datasheet-verification
- Files: docs/BOM.md, docs/SPEC.md §2, docs/SUBSYSTEMS.md §1/§3/§4/§6/§9, docs/DEVPLAN.md §2, .copperhead/constraints.json (power.iin_noload_mA, protection.pfet_vgs_max_V, protection.fuse_rating_A), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Source: research/parts/datasheet-verification.md (every fitted part checked 2026-11-06). Each confirmed BOM figure is now VERIFIED with the datasheet revision and page. The header line saying every MPN is UNVERIFIED is removed.
  - Corrections:
    - U1 Iq 2 mA typ, 4.4 mA max (was ~3 mA). No-load roll-up ≈ 7–11 mA worst case (≤ 20 mA).
    - D2 Vz 11.4–12.7 V. Q1 gate-source voltage magnitude ≤ 12.7 V (≤ 15 V target, 20 V abs). Clamp current ≤ 66 µA at 18 V; shutdown roll-up ≈ 0.121 mA (≤ 1 mA).
    - D4 VF specified at 20 mA (2.0 V typ, 2.4 V max); ≈ 1.9–2.0 V at 2 mA from the typical curve only. LED current ≈ 1.9–2.2 mA (1–3 mA).
    - L1 body 13.5 × 12.5 mm. FB1 body 6.40 × 3.20 mm.
    - F1 cold R 18.6 mΩ, melting I²t 34.4 A²s, ≈ 2.94 A continuous at 50 °C (≥ 1.96 A worst case).
    - J1/J2 IEC 17.5 A, 250 V (III/3); UL 300 V, 10–15 A; 1.3 mm drill matches Phoenix's recommended hole.
    - C1/C2 ≈ 7.4 µF at 18 V; C6 ≈ 17.5–18 µF at 5 V, so the LC pole is ≈ 2.7 kHz (`power.output_filter_compensation` still satisfied).
  - Accepted for rev A: FB1 (R_2512_6332Metric) and C5 (CP_Elec_8x10) land patterns are slightly smaller than the makers' recommended lands; terminations land fully on copper.
  - No cell contains a pipe character; Refdes, Value, Footprint stay first and LCSC last. No schematic, board, net, value or footprint change.
- Open: re-export outputs/BOM.csv with `copperhead export bom`. Bench checks (Q1 marking, L1 at 50 °C, loop stability) stay in DEVPLAN §4.
- Verification: drift check run. ERC and DRC not required (docs only).

## 2026-11-06 — Clear the last DRC warning, silk_over_copper on FB1: FB1's Reference text overlaps its pad 1 (VIN_PROT) and is clipped by solder mask. Move the FB1 Reference field (its (at ...) inside the property, not the footprint) to a readable spot clear of all pads, vias and other silkscreen, near FB1. No footprint, pad, track or zone change. ERC and DRC must be clean with no warnings left; kicad-cli pcb drc --severity-all should report only items already accepted.

- Change: fb1-reference-silk-clear
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md, docs/DEVPLAN.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — FB1 Reference label moved clear of pad 1 (last DRC finding)

- Change: fb1-reference-silk-clear
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (FB1 Reference property `(at …)` only), docs/LAYOUT.md (Fine list, item 9 note), docs/DEVPLAN.md (R6), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - DRC reported silk_over_copper on FB1. The Reference text was at local (−4.8, 0), drawn vertical, and ran into pad 1 (VIN_PROT), where the mask opening clipped it.
  - Earlier entries called this finding library-internal. That was wrong: the label placement caused it, not the stock R_2512_6332Metric footprint.
  - The Reference property moved to local (−4.6, −0.7, 0°). It is now horizontal at about board (142.5, 101.9), above pad 1 and between the GND stitching vias at (140, 101.5) and (145, 101.5).
  - No footprint, pad, track, via, zone, net or schematic change.
- Open:
  - Re-run `kicad-cli pcb drc --severity-all --refill-zones` in KiCad to confirm it matches the copperhead run_drc result. The copperhead DRC does not run the schematic-parity check.
  - outputs/ predates this change, so the silkscreen gerber still has the old label. Re-export (or release with /release-board) before ordering.
- Verification: DRC clean with no findings at all. ERC clean. Drift clean.

## 2026-11-06 — Make TP1-TP7 pass KiCad's schematic parity check (kicad-cli pcb drc --schematic-parity reports footprint_symbol_mismatch: 'Exclude from bill of materials' differs). The schematic drafting engine always writes (in_bom yes) on symbols and cannot change it, so make the board side agree: clear the exclude_from_bom attribute on the TP1-TP7 footprints, and KEEP exclude_from_pos_files set so the bare test pads stay out of the pick-and-place file. The JLCPCB BOM is generated from docs/BOM.md, where TP1-TP7 have no LCSC number and are not fitted, so they stay out of the assembly BOM. No pad, net, copper or position change. ERC, DRC and drift must stay clean.

- Change: tp-in-bom-schematic-parity
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/BOM.md, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — TP1–TP7 footprints in the KiCad BOM for schematic parity

- Change: tp-in-bom-schematic-parity
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (TP1–TP7 attr only), docs/BOM.md, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - `kicad-cli pcb drc --schematic-parity` reported footprint_symbol_mismatch ('Exclude from bill of materials' differs) on TP1–TP7. The drafting engine always writes the symbols as `(in_bom yes)` and cannot change that, so the board now agrees.
  - The TP1–TP7 footprints change from `(attr exclude_from_pos_files exclude_from_bom)` to `(attr exclude_from_pos_files)`. No other footprint carried `exclude_from_bom`.
  - `exclude_from_pos_files` is kept, so the bare pads stay out of the CPL. They stay out of the JLCPCB assembly BOM, which is generated from docs/BOM.md, where TP1–TP7 have no LCSC number (`assembly.qty`). KiCad's own BOM export will list them; that list is not the assembly BOM.
  - No pad, net, copper, position or schematic change.
- Open: re-run `kicad-cli pcb drc --schematic-parity` in KiCad to confirm the seven findings are gone (the copperhead run_drc does not run the parity check).
- Verification: DRC clean (only the pre-existing FB1 library-internal silk). ERC clean. Drift clean.

## 2026-11-06 — Write docs/DEVPLAN.md, the development plan for rev A, which the create pipeline's devplan stage never produced because the firmware stage correctly refused: the board has no microcontroller or programmable part, and docs/BRIEF.md Must not rules out digital control, telemetry and remote sensing. State plainly that rev A has no firmware and that firmware/ intentionally holds only its README. Then plan the path from here: design review of the layout (docs/LAYOUT.md Draft quality), verifying the UNVERIFIED datasheet figures in docs/BOM.md, ordering (JLCPCB PCBA with 2 oz copper, through-hole J1 and J2, release with /release-board which adds the pick-and-place file), and bring-up and validation: current-limited power-up, 5 V setpoint and ripple, load steps to 3 A, efficiency at 9, 12 and 18 V, temperatures of U1, D3, L1, F1 and the terminals at 3 A and 50 C ambient (docs/SPEC.md budgets), reverse-polarity and EN-disable tests. Cite the SPEC requirement each test checks. Docs only; drift must stay clean.

- Change: devplan-rev-a
- Files: docs/DEVPLAN.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC not required

## 2026-11-06 — Development plan for rev A (docs/DEVPLAN.md)

- Change: devplan-rev-a
- Files: docs/DEVPLAN.md (new), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - New docs/DEVPLAN.md. It states that rev A has no firmware: there is no MCU or programmable part, and BRIEF Must not / SPEC §8 rule out digital control, telemetry and remote sensing (`safety.selv_only`). firmware/ intentionally holds only its README.md.
  - §1 plans a layout design review against docs/LAYOUT.md Draft quality (U1 pad 9 flood, two-layer via landing, C8.2, SW area for D3, FB1 silk, L1 6.2 mm height, silkscreen). It also lists the stale LAYOUT.md text to reconcile.
  - §2 lists every UNVERIFIED BOM figure, with the constraint each one protects.
  - §3 covers ordering: re-export the BOM (outputs/BOM.csv predates the D2 fix), release with /release-board for the CPL, select 2 oz copper by hand on the JLCPCB form, and order through-hole assembly for J1/J2 or hand-solder them.
  - §4 lists bring-up and validation tests T0–T12. Each test cites the SPEC section and constraint key it checks. They cover current-limited power-up, the 5 V setpoint, ripple, load steps to 3 A, efficiency at 9/12/18 V, temperatures of U1, D3, L1, F1, J1 and J2 at 3 A and 50 °C, reverse polarity to −18 V, and EN disable. The load-step test has no invented budget, because SPEC sets none.
  - No schematic, board, net, value, footprint or constraint change.
- Verification: drift check run. ERC and DRC not required (docs only).

## 2026-11-06 — In docs/BOM.md, the D2 row's rationale contains escaped pipes ('\|Vgs\|'), which Markdown table parsers, including copperhead export bom, read as extra columns, so D2's LCSC cell ends up empty and its LCSC number is exported as 'Vgs\'. Rewrite that phrase without any pipe character (for example 'the gate-source voltage magnitude stays at or below 12 V') and make sure D2's LCSC column holds C124196 (research/parts/jlcpcb-sourcing.md). Check that no other BOM row contains a pipe character inside a cell. Docs only; no schematic or board change; drift must stay clean.

- Change: bom-d2-remove-escaped-pipes
- Files: docs/BOM.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC not required

## 2026-11-06 — BOM D2 rationale: escaped pipes removed

- Change: bom-d2-remove-escaped-pipes
- Files: docs/BOM.md (D2 row), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The D2 rationale held escaped pipes around Vgs. Markdown table parsers, including `copperhead export bom`, split on them, so D2's LCSC number was exported as 'Vgs\' and its LCSC cell was empty.
  - The phrase now reads 'the gate-source voltage magnitude stays at or below 12 V', with no pipe character. D2's last (LCSC) cell is C124196 (research/parts/jlcpcb-sourcing.md).
  - Every other BOM row was scanned: no escaped pipes and no row with more than the seven separators of the six-column table.
  - No schematic, board, net, value, footprint or constraint change. outputs/BOM.csv was not regenerated in this run; re-export it with `copperhead export bom` before ordering.
- Verification: drift clean. ERC and DRC not required (docs only).

## 2026-11-06 — Re-export the fabrication package into outputs/ with export_outputs, replacing the stale files: the board was refilled with KiCad after the last export (U1 exposed pad now solid to GND, commit 70ca14b) and docs/BOM.md gained LCSC numbers and part substitutions. Regenerate gerbers and drill, DXF outline, STEP, SVG renders, and outputs/BOM.csv from docs/BOM.md including the LCSC column. Update outputs/README.md to the current state: close the 'U1 pad 9 solid connection' and 'zone fill' items (both done and verified with kicad-cli DRC after refill: 0 unconnected, only the FB1 library silkscreen warning), note that the JLCPCB BOM comes from copperhead export bom and the pick-and-place file from /release-board, and keep the remaining honest checks (order 2 oz copper on the JLC form, UNVERIFIED listing figures, through-hole J1/J2). Do not modify the board or schematic. Every export must succeed.

- Change: outputs-reexport-post-refill
- Files: outputs/, outputs/BOM.csv, outputs/README.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC not required, DRC clean

## 2026-11-06 — Fabrication package re-exported after the KiCad refill

- Change: outputs-reexport-post-refill
- Files: outputs/gerbers/*, outputs/*.drl, outputs/outline.dxf, outputs/board.step, outputs/board.svg, outputs/schematic.svg, outputs/BOM.csv, outputs/README.md, .copperhead/renders/, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - `export_outputs` re-ran on the refilled board. The person reports the refill is in commit 70ca14b, with the U1 exposed pad solid to GND. All six artifacts succeeded: gerbers, drill, outline.dxf, board.step, board.svg and schematic.svg. The export_svg sch and pcb renders were also regenerated.
  - outputs/BOM.csv was regenerated from docs/BOM.md with a new last column, LCSC. It is filled for every fitted part. The LCSC numbers moved out of the Note cells. DNP parts and no-part items stay at Qty 0 with an empty LCSC cell on purpose.
  - outputs/README.md:
    - The zone-fill and U1 pad 9 items are closed (`thermal.tj_max_C`).
    - It states that the JLCPCB BOM comes from `copperhead export bom` and the CPL from /release-board.
    - Open checks kept: order 2 oz on the JLC form, plot profile unconfirmed, FB1 silk, UNVERIFIED listing figures, through-hole J1/J2 and the OMNI-BLOK F1, and bench validation.
  - No board, schematic, net, value or footprint change.
- Verification: DRC clean before export (only the pre-existing FB1 library-internal silk). Drift clean. ERC not re-run: no schematic edit.

## 2026-11-06 — Update docs/BOM.md for JLCPCB assembly from research/parts/jlcpcb-sourcing.md (docs/BRIEF.md Physical: fab-assembled PCBA at JLCPCB). Append an LCSC column at the end of the parts table (keep Refdes, Value, Footprint as the first three columns, in order) and fill it for every fitted part: U1 C9864, Q1 C154901, F1 C206915, D1 C135055, D2 C124196, D3 C85100, D4 C6848499, L1 C189912, FB1 C346503, C1 and C2 C77102, C3 C14663, C4 C57112, C5 C341198, C6 C21397, R1 C25803, R2 C95204, R3 C728589, R4 C22843, J1 and J2 C480516. Leave it empty for C7, C8, C9, J3 (not fitted), TP1-TP7 and H1, H2. Change these MPNs, with values and footprints unchanged: FB1 RCL12250000Z0EG to Stackpole HCJ2512ZT0R00 (63.2 A 2512 jumper; the RCL1225 is a wide-terminal 1225 part that does not fit R_2512), C3 to CC0603KRX7R9BB104, C4 to 0603B103K500NT, C5 EEH-ZA1A221P to EEH-ZA1E221P (same 220 uF ZA series, 25 V, 27 mOhm ESR, 8 x 10.2 mm; EEH-ZA1A221P is not on LCSC), C6 GRM32ER71A226KE20L to GRM32ER71E226KE15L (22 uF X7R 1210, 25 V; original out of stock), R1 to 0603WAF1003T5E, R2 to RT0603BRD0710KL (0.1 %, 25 ppm), R3 RT0603DRE073K24L to RT0603BRD073K24L (0.1 %, 25 ppm; original out of stock), R4 to 0603WAF1501T5E. Cite research/parts/jlcpcb-sourcing.md in each changed rationale. Keep UNVERIFIED on any figure that comes only from a distributor listing. Carry the MPN changes into the schematic fields if the schematic holds MPNs. Check that C5's ESR change keeps power.output_filter_compensation satisfied. No footprint, net or board geometry change; ERC, DRC and drift must stay clean.

- Change: bom-jlcpcb-lcsc-sourcing
- Files: docs/BOM.md, docs/SUBSYSTEMS.md, docs/SPEC.md, outputs/BOM.csv, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — BOM LCSC column and JLCPCB MPN substitutions

- Change: bom-jlcpcb-lcsc-sourcing
- Files: docs/BOM.md, docs/SUBSYSTEMS.md (§1 D1 leakage, §2 FB1, §4 C5/C6 and ESR zero, §9 shutdown roll-up), docs/SPEC.md §3, outputs/BOM.csv, .copperhead/constraints.json (power.output_filter_compensation), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - docs/BOM.md has a new last column, LCSC, from research/parts/jlcpcb-sourcing.md. It is filled for every fitted part. It is empty on purpose for C7, C8, C9 and J3 (not fitted) and for TP1–TP7, H1 and H2 (not parts).
  - MPN changes, with values and footprints unchanged:
    - FB1: HCJ2512ZT0R00. The RCL1225 does not fit R_2512; the HCJ is rated 63.2 A.
    - C3: CC0603KRX7R9BB104.
    - C4: 0603B103K500NT.
    - C5: EEH-ZA1E221P (25 V, 27 mΩ per listing).
    - C6: GRM32ER71E226KE15L (25 V).
    - R1: 0603WAF1003T5E.
    - R2: RT0603BRD0710KL and R3: RT0603BRD073K24L (both 0.1 %, 25 ppm).
    - R4: 0603WAF1501T5E.
  - Compensation re-check (`power.output_filter_compensation`): the ESR zero moves from ~24 kHz to ~27 kHz, and the LC pole stays at ~2.6 kHz. Ripple is ≈ 13 mVpp. The constraint is satisfied; confirm on the datasheet and the bench.
  - Listing figures are kept UNVERIFIED:
    - D1 IR ≤ 5 µA, so shutdown is ≈ 0.115 mA (≤ 1 mA).
    - D2 is 370 mW.
    - L1 is Isat 12.5 A, Irms 9 A, DCR 28 mΩ.
  - The schematic, intent file and board hold no MPN fields, so no KiCad file changed. docs/BRIEF.md Parts still names the 10 V C5 and C6 parts; this deviation is recorded in DECISIONS.
- Open: recheck LCSC stock before ordering. J1/J2 need through-hole assembly or hand soldering.
- Verification: ERC clean. DRC clean (only the pre-existing FB1 library-internal silk). Drift clean.

## 2026-11-06 — Make U1's exposed pad 9 connect solidly to the GND pours (thermal.tj_max_C, docs/BRIEF.md Physical: large copper pours and thermal vias beneath the TPS5430 exposed pad). The GND zones use thermal-relief pad connection by default, and the .kicad_dru rule U1_pad9_solid_to_GND does not take effect (the same rule form failed for R1). Apply the pad-level solid zone connection override (zone_connect 2), the same way R1 pad 2 was fixed, to every U1 pad numbered 9: the exposed SMD pad and its thermal-via pads. Remove the ineffective .kicad_dru rule. Do not change the zones or their fills; they are refilled afterwards with KiCad. ERC and DRC must be clean with 0 unrouted connections, and update docs/LAYOUT.md to close the U1 pad 9 open item.

- Change: u1-pad9-solid-zone-connect
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_dru, docs/LAYOUT.md, docs/SPEC.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — U1 pad 9 solid GND connection (pad-level override)

- Change: u1-pad9-solid-zone-connect
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_dru, docs/LAYOUT.md (U1 pad 9 item closed), docs/SPEC.md §6, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - All 11 U1 pads numbered 9 now carry a pad-level `(zone_connect 2)` (solid) override: the 2.6 × 3.1 mm exposed SMD pad, the 2.95 × 4.5 mm F.Cu and B.Cu pad copper, and the 8 thermal-via pads (`thermal.tj_max_C`). This is the same fix used for R1 pad 2.
  - The ineffective `.kicad_dru` rule `U1_pad9_solid_to_GND` was removed and replaced by a comment. The via-drill rule is unchanged.
  - Zones and fills were not edited. The zone default stays thermal relief for every other pad. No net, value, position or schematic change.
- Open: refill with `kicad-cli pcb drc --refill-zones --save-board` and visually confirm that the pours flood U1 pad 9 with no spokes.
- Verification: DRC clean, 0 unconnected, no `lib_footprint_mismatch` (only the pre-existing FB1 library-internal silk). ERC clean. Drift clean.

## 2026-11-06 — create pipeline stage: outputs

- Change: outputs-package-stage6
- Files: outputs/, outputs/BOM.csv, outputs/README.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC not required, DRC clean

## 2026-11-06 — Stage 6 outputs package

- Change: outputs-package-stage6
- Files: outputs/ (gerbers/, .drl, outline.dxf, board.step, board.svg, schematic.svg, BOM.csv, README.md), docs/CHANGELOG.md, docs/DECISIONS.md
- Summary:
  - `export_outputs` produced gerbers plus a gbrjob, the Excellon drill, the DXF outline, STEP, and board and schematic SVGs. `export_svg` sch and pcb renders are also in .copperhead/renders/.
  - The gbrjob confirms 2 layers, 1.6 mm, 0.07 mm (2 oz) copper on both layers and 0.2 mm clearance.
  - The drill file lists all vias, 8 U1 thermal vias, the THT holes, and 3.2 mm NPTH at H1 and H2.
  - outputs/BOM.csv is generated from docs/BOM.md (Refdes, MPN, Qty_per_board, Value, Footprint, Fitted, Note). It has 21 fitted placements per board. DNP parts (C7, C8, C9, J3) and the no-part items (TP1–TP7, H1, H2) are listed at Qty 0 on purpose.
  - No schematic, board, net, value or footprint change.
- Open (see outputs/README.md):
  - The JLC plot profile is not confirmed. The output is JLC-compatible, but it includes extra User, Fab and Adhesive layers and a merged PTH/NPTH drill file.
  - Order 2 oz copper in the JLC order form.
  - Confirm the zones were refilled after the stitching vias were added, then re-export.
  - The U1 pad 9 solid check is still OPEN and blocks fab (`thermal.tj_max_C`).
  - No CPL or JLC-format BOM was produced.
  - Every MPN is UNVERIFIED.
- Verification: DRC clean (only the pre-existing FB1 library-internal silk), drift clean. ERC not re-run: no schematic edit.

## 2026-11-06 — The GND and SW zones are now filled with KiCad's filler and the board has 0 unconnected items. Two things remain. 1) Clear the DRC error starved_thermal on R1 pad 2 (GND, F.Cu pour gives 1 spoke, minimum 2). R1 pad 2 is already joined to GND by a trace. Either give the R1 footprint a solid zone connection override, or move the nearby trace so a second spoke fits; do not lower the zone's minimum spoke count for the whole board. 2) Add GND stitching vias (0.6 mm pad, 0.3 mm drill) at about 5 mm pitch along the board edges outside the H1 and H2 copper keepouts, and around the power stage (U1 exposed pad, C3 and D3 ground, C1 and C2 ground, C5 and C6 ground), placing each via only inside existing filled GND copper on both layers and clear of every other net, so the existing fill stays valid. Do not fill or edit the zone fills; they are refilled afterwards with KiCad. ERC and DRC must be clean, with 0 unrouted connections.

- Change: r1-solid-gnd-stitching-vias
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_dru, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — R1 pad 2 solid GND connection; 34 GND stitching vias

- Change: r1-solid-gnd-stitching-vias
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_dru (comment only), docs/LAYOUT.md (Check after the KiCad zone fill), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - **R1 pad 2 starved_thermal:** cleared with a pad-level `(zone_connect 2)` (solid) override on R1 pad 2 only. The zone minimum spoke count (2) is unchanged board-wide, and DRC shows no `lib_footprint_mismatch`. A `.kicad_dru` `zone_connection solid` rule scoped to R1 pad 2 was tried first, with and without the zone condition. It did not clear the error, so I removed it and left a comment in its place.
  - **U1 pad 9 (open check):** `U1_pad9_solid_to_GND` uses the same rule form, so after the refill confirm in KiCad that U1 pad 9 is really solid (`thermal.tj_max_C`). If not, give it the same pad-level override.
  - **Stitching vias:** 34 GND vias, 0.6 mm pad / 0.3 mm drill.
    - Edges at 5 mm pitch: top y 101.5 (x 110–145), bottom y 138.5 (x 105–150), left x 101.5 (y 120–135), right x ≈ 158.5 (y 116, 123, 128).
    - All edge vias are outside the H1/H2 copper keepouts.
    - Power stage: three below U1 pad 9, one on the C1.2→D3 GND link, two flanking the D3 anode via pair, one east of C2.2, two south of C5−/C6.2.
    - All are outside both SW zones and clear of every other net.
  - The zone fills were not edited. No net, value, footprint or schematic change.
- Open:
  - Refill with `kicad-cli pcb drc --refill-zones --save-board` and re-run DRC.
  - Confirm U1 pad 9 is solid.
  - DRC confirms no via is too close to another net or dangling, but a via that touches GND fill on only one layer would not be flagged. Check after the refill that each via lands in pour on both layers.
- Verification: DRC clean (only the pre-existing FB1 library-internal silk finding), 0 unconnected. ERC clean. Drift clean.

## 2026-11-06 — Fix two connection findings that KiCad's DRC reports once the GND zones are filled (kicad-cli pcb drc --refill-zones). 1) Q1 (SOT-223, footprint SOT-223-3_TabPin2) has two pads numbered 2 on net VIN_FUSED: the tab at board (125.85, 106) is connected by the 1.5 mm F.Cu trace from F1, but pin 2 at (132.15, 106) has no copper to it, so KiCad reports it unconnected. Add a short VIN_FUSED F.Cu trace joining Q1 pin 2 to the tab, at least 1.0 mm wide, clear of pins 1 and 3. 2) R1 pad 2 (GND) gets only one thermal spoke from the F.Cu GND pour (zone minimum is 2), because nearby traces block it. Add a short GND trace from R1 pad 2 to the nearest GND pad (for example D2, D1 pad 2 or C7 pad 2) so it does not depend on the pour. The GND zones stay intentionally unfilled; do not fill them. ERC and DRC must be clean and the unrouted count must not rise above 16.

- Change: q1-pin2-r1-gnd-links
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Q1 pin 2 to tab link; R1.2 GND link

- Change: q1-pin2-r1-gnd-links
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md (Routed, unrouted count), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - **VIN_FUSED:** a 1.5 mm F.Cu link along y 106 joins Q1 pin 2 (132.15, 106) to the Q1 tab (125.85, 106). With the zones filled, KiCad had reported the second pad 2 of SOT-223-3_TabPin2 as unconnected. The link has 0.8 mm clearance to pins 1 and 3 and carries ≥ 4 A (`protection.fuse_rating_A`).
  - **VIN_PROT spur:** the stretch under Q1 (x 129, y 103.7 → 108.5) moved from F.Cu to 1.0 mm B.Cu between two 0.8/0.4 mm vias, which frees y 106 for the link above.
  - **GND:** a 0.5 mm F.Cu trace runs from R1.2 (136.825, 112) to the D1.2 GND via at (136.5, 108.6). The F.Cu pour gave R1.2 only one thermal spoke (zone minimum 2).
  - No net, value, footprint or schematic change. The zones are still unfilled.
- Verification: DRC clean, unrouted 16 → 14 (all GND); only the pre-existing FB1 library-internal silk finding. ERC clean.

## 2026-11-06 — Route every remaining non-GND connection with traces, and add short GND traces or vias wherever a GND pad would otherwise be cut off from the GND pours once they are filled (for example U1 pin 6 to the exposed pad 9). The GND zones on F.Cu and B.Cu are outlined but intentionally unfilled; do not try to fill them. Then finish the silkscreen: add clear polarity and terminal labels at J1 (VIN +, GND -) and J2 (+5V, GND) and labels for the test points, as docs/BRIEF.md Physical requires; move TP4 (GND) next to J2 (Draft quality item 7); make sure reference labels are readable and not covered by part bodies (item 9). ERC and DRC must be clean, and the only connections left unrouted must be GND connections that the pours will make. Update docs/LAYOUT.md Draft quality to the board's real state.

- Change: route-remaining-signals-silkscreen
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Remaining signals routed, GND links added, silkscreen finished

- Change: route-remaining-signals-silkscreen
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md (placement TP4, Routed, Silkscreen, unrouted count, items 7 and 9), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - **EN:** U1.5 → C9.1 → J3.1 → TP7, 0.3 mm F.Cu below U1, away from SW. There is still no pull-up (`protection.ena_pin_max_V`).
  - **FB legs:** R2.2 → R3.1 → TP6, 0.25 mm, left of U1. The R2.1 Kelvin feed is unchanged.
  - **GND links:**
    - U1.6 → pad 9 (F.Cu 0.5 mm).
    - C3.2: in-pad via plus 0.5 mm B.Cu to pad 9.
    - D3 anode: drop to a strapped via pair at y 135.5, below the B.Cu SW zone.
    - D1.2 (TVS return): strapped via pair at y 108.6.
    - TP4 → J2.2.
  - **TP4** moved from (116,133) to (147.5,110), beside J2 pin 2 (closes LAYOUT item 7).
  - **Silkscreen:**
    - J1 `+VIN`/`-GND` and J2 `+5V`/`-GND`.
    - Test-point names for TP1–TP7.
    - The C5, L1, D3 and C6 reference labels moved out from under the part bodies (closes item 9).
  - No net, value, footprint or schematic change. The zones are still unfilled.
- Open:
  - Fill the zones with KiCad's filler and re-run DRC.
  - Check C8.2 (DNP) for a thin pour connection after the fill.
  - Measure the filled SW area.
  - Add stitching vias.
- Verification: DRC clean (only the pre-existing FB1 library-internal silk finding). Unrouted 24 → 16, all GND. ERC clean. Drift clean.

## 2026-11-06 — Route the high-current paths for 3 A continuous with 2 oz copper (power.iout_A, pcb.copper_oz), using wide traces or copper zones on their own nets: VIN_PROT (Q1 source to D1 to FB1 to TP1 and C7), VIN_BUCK (FB1 to C1, C2, C8 and U1 pin 7), SW (U1 pin 8 to D3 cathode and L1 pin 1, kept compact) and +5V (L1 pin 2 to C5, C6, J2 pin 1, TP2, R4). Tighten the VIN-C3-U1-D3 switching loop, moving C1, C2 and D3 closer to U1 if needed, and give D3 about 2 to 3 cm2 of SW copper (thermal.component_rise_C). Feed R2 pin 1 from the C5 terminal with a dedicated trace, not from the +5V copper near L1 or SW, and keep FB away from SW (docs/LAYOUT.md Draft quality items 2 to 6). The GND zones on F.Cu and B.Cu are outlined but intentionally unfilled; do not try to fill them, they are filled at the end with KiCad's filler. GND connections can be left to the pour. ERC and DRC must be clean and the unrouted count must drop below 44.

- Change: route-high-current-paths
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — High-current paths routed on 2 oz copper; hot loop tightened

- Change: route-high-current-paths
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md (placement rows, Routed, unrouted count, items 2–6), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - **VIN_PROT:** 2.0 mm F.Cu top row Q1.3 → D1.1 → FB1.1. D1 and FB1 were flipped from 90° to 270° to make this possible (closes LAYOUT item 3). A 1.0 mm spur feeds D2.1, TP1 (moved to (128, 114.5)) and C7.1.
  - **Q1_GATE:** Q1.1 → D2.2 → R1.1, 0.4 mm.
  - **VIN_BUCK:** 1.5 mm F.Cu trunk FB1.2 → C2 (and a 1.0 mm branch to C8). It crosses under the SW run on 2.0 mm B.Cu through 3 + 3 vias, then feeds C1.1 and C3.1. U1.7 → C3.1 widened from 0.5 to 0.8 mm.
  - **Hot loop:** C1 moved from (134, 120.6) to (131.4, 129) 270°, between U1's right-hand pins and D3, with a direct 1.5 mm GND link to the D3 anode. D3, C2, U1, C3, C4 and the FB divider did not move.
  - **SW:** U1.8 → D3.1 → L1.1 on 1.0–2.5 mm F.Cu. SW zone outlines on F.Cu and B.Cu (priority 1 above GND), stitched by four vias, ≈ 2.1 cm² of outline for D3 (`thermal.component_rise_C`). UNFILLED, like the GND pours.
  - **+5V:** L1.2 → C6 → TP2/J2.1 on 2.0–2.5 mm, with a 2.0 mm branch to C5+.
  - **R2.1 Kelvin:** a dedicated 0.25 mm trace from the C5 + pad, mostly on B.Cu along the board's left and top, away from SW (closes item 4). R4 has its own 0.3 mm feed from C5+.
  - Reference labels on D1, FB1, C1, C8 and C9 moved to clear silk.
  - No net, value, footprint or schematic change. GND connections are left to the pours.
- Open: fill the GND and SW zones with `kicad-cli pcb drc --refill-zones --save-board`, re-run DRC, and measure the filled SW area. Bench-check D3 rise. EN and stitching vias are still to do.
- Verification: DRC clean, unrouted 44 → 24; only the pre-existing FB1 library-internal silk_over_copper finding. ERC clean.

## 2026-11-05 — Add GND copper zone outlines on both F.Cu and B.Cu covering the board inside the 60 x 40 mm outline, respecting the H1 and H2 copper keepouts, with thermal-relief or solid connection to GND pads and solid connection to U1 exposed pad 9 (thermal.tj_max_C). Set the board stackup copper to 2 oz (70 um) on both layers, as docs/BRIEF.md Physical asks (pcb.copper_oz), and adjust design rules if needed. Do NOT add stitching vias in this change and do not try to fill the zones: your tools cannot fill zones, and the zones will be filled afterwards with KiCad's own filler (kicad-cli pcb drc --refill-zones --save-board); stitching vias come in a later change. Record in docs/LAYOUT.md that the zones are outlined and need a KiCad fill. ERC and DRC must be clean and the unrouted count must not rise above 44.

- Change: gnd-zones-2oz-stackup
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_dru, docs/LAYOUT.md, docs/SPEC.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-05 — GND zone outlines on F.Cu/B.Cu; 2 oz stackup

- Change: gnd-zones-2oz-stackup
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_dru, docs/LAYOUT.md (items 1 and 12), docs/SPEC.md §6, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Added GND_pour_F.Cu and GND_pour_B.Cu zone outlines, (100.5,100.5)–(159.5,139.5), inset 0.5 mm from Edge.Cuts. Net GND, thermal relief (gap 0.5 mm, spoke 0.5 mm), clearance 0.3 mm, min thickness 0.25 mm.
  - The existing H1_/H2_copper_keepout_r3.5 rule areas (copperpour not_allowed) keep the pours out of the mounting-hole keepouts.
  - U1 pad 9 connects solid to GND through the new rule U1_pad9_solid_to_GND in the project .kicad_dru (`thermal.tj_max_C`). The stock footprint is unmodified.
  - Added an explicit board stackup: F.Cu/B.Cu 0.07 mm (2 oz), 1.44 mm FR4 core, 1.6 mm overall (`pcb.copper_oz`). Other design rules unchanged.
  - The zones are NOT filled, and no stitching vias were added. Fill with `kicad-cli pcb drc --refill-zones --save-board`, then re-run DRC. Stitching vias come in a later change.
- Verification: ERC clean. DRC clean, 44 unrouted (unchanged); only the pre-existing FB1 library-internal silk_over_copper finding. Drift clean.

## 2026-11-05 — Add two M3 mounting holes H1 and H2 (Mechanical:MountingHole symbol, MountingHole:MountingHole_3.2mm_M3 non-plated footprint, no nets) to the schematic and BOM, and place them on the board at two diagonally opposite corners, with copper and component keepouts around them, as docs/BRIEF.md Physical requires. All four corners are currently occupied by J1, J2 and L1, so you may slide J1 and J2 along their own board edges (they must stay on opposite edges, left and right), move L1 and nearby parts, and if still needed grow the outline modestly (the brief says the board is targeted around 55 x 35 mm and that final dimensions and hole coordinates can be adjusted during layout; stay within about 60 x 40 mm). Keep C3, C4 and D3 adjacent to U1 and the feedback divider away from SW. ERC and DRC must stay clean and the unrouted count must not rise above 44.

- Change: add-m3-mounting-holes-h1-h2
- Files: schematic.intent.json, docs/BOM.md, docs/SUBSYSTEMS.md, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/PINOUT.md, docs/SPEC.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-05 — M3 mounting holes H1/H2 with keepouts; outline 60 × 40 mm

- Change: add-m3-mounting-holes-h1-h2
- Files: schematic.intent.json, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch (re-drafted), tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, .copperhead/constraints.json, docs/BOM.md, docs/SUBSYSTEMS.md (new §10 Mechanical), docs/LAYOUT.md, docs/PINOUT.md, docs/SPEC.md §6 and §9 Q2, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Added H1 and H2: Mechanical:MountingHole (0 pins, no net), footprint MountingHole:MountingHole_3.2mm_M3, NPTH. H1 is top-left at (103,103) and H2 is bottom-right at (157,137), so they are diagonally opposite.
  - H1 and H2 are the unmodified stock footprints. Their keepouts are board-level rule areas on F.Cu and B.Cu:
    - A full r ≥ 3.5 mm copper-keepout disc per hole (a 16-gon with 3.57 mm circumscribed radius). It forbids tracks, vias and copper pour.
    - A half-annulus pair per hole (≈ 3.48–3.8 mm) that forbids footprints. It sits just outside the hole's own 3.45 mm courtyard.
  - A first attempt embedded the keepout in the footprint. That produced two `lib_footprint_mismatch` DRC warnings, so it was replaced by the board-level zones.
  - The Edge.Cuts outline grew from 55 × 35 to 60 × 40 mm, (100,100)–(160,140), because L1 and the right column fill the old bottom-right corner. This is at the person's 60 × 40 mm limit. `mech.board_mm` max changed from 55 to 60.
  - J1 slid down the left edge to (106,115.5), and the VIN_RAW stub was re-anchored. J2 slid right to (154,104). TP3 moved to (106,121.5).
  - U1, C3, C4, D3, L1 and R2/R3/TP6 did not move, so the hot loop and the FB-away-from-SW placement are unchanged.
  - LAYOUT.md Draft quality item 8 and SPEC §9 Q2 are closed.
- Verification: ERC clean. DRC clean, 0 violations and 44 unrouted (unchanged). The only remaining finding is the pre-existing FB1 library-internal silk_over_copper. Legibility 0 errors (1 low-utilization advisory, A3 kept). Drift clean.

## 2026-11-05 — Record the copperhead_power symbols (+5V, GND, PWR_FLAG, VIN_BUCK) as an accepted engine limitation and close docs/LAYOUT.md Draft quality item 11. The schematic drafting engine can only place copperhead_power symbols, they are genuine KiCad power symbols in a project library, and the nets and netlist are identical to what power:* symbols would give; KiCad has no stock power:VIN_BUCK. The person accepted this on 2026-11-05. Record a decision; no schematic, net or board change.

- Change: accept-copperhead-power-symbols
- Files: docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC not required

## 2026-11-05 — copperhead_power symbols accepted as an engine limitation

- Change: accept-copperhead-power-symbols
- Files: docs/LAYOUT.md (Draft quality item 11 closed), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The schematic keeps copperhead_power:* symbols (+5V, GND, PWR_FLAG, VIN_BUCK). The drafting engine can only place these. They are genuine KiCad power symbols in a project library, so the nets and netlist match what power:* would give. KiCad has no stock power:VIN_BUCK.
  - The person accepted this on 2026-11-05. It supersedes the stage-4 instruction to retarget before netlist export.
  - The verify_symbols wrong-library finding on these four symbols is expected and needs no action.
  - No schematic, net, board or constraint change.
- Verification: docs-only; drift check run.

## 2026-11-05 — Mark the L1 footprint Inductor_SMD:L_Bourns_SRP1245A as verified for SRP1265A-150M: the Bourns SRP1265A and SRP1245A datasheets give the same recommended layout (14.2 mm overall, 8.0 mm gap, 5.0 mm pad width) and the KiCad footprint pads are 3.1 x 5 mm at +/-5.55 mm, which matches. Only the body height differs (6.2 mm vs 4.8 mm), which affects the 3D model, not the copper. Update the BOM note and close the LAYOUT.md Draft quality item about the L1 pad check. No footprint, net or board change.

- Change: l1-footprint-verified-srp1265a
- Files: docs/BOM.md, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC not required

## 2026-11-05 — L1 footprint verified for SRP1265A-150M

- Change: l1-footprint-verified-srp1265a
- Files: docs/BOM.md (L1 row), docs/LAYOUT.md (Draft quality item 10 closed), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Inductor_SMD:L_Bourns_SRP1245A is accepted for the SRP1265A-150M. Both Bourns datasheets give the same recommended layout (14.2 mm overall, 8.0 mm gap, 5.0 mm pad width), and the KiCad pads (3.1 × 5 mm at ±5.55 mm) match it.
  - Only the body height differs (6.2 mm vs 4.8 mm). This affects the 3D model and any height-clearance check, not the copper.
  - L1 electrical figures (Isat, Irms, DCR) stay UNVERIFIED; SPEC §9 Q3 stays open.
  - No footprint, net, schematic, board or constraint change.
- Verification: docs-only; drift check run.

## 2026-11-05 — Change the input fuse F1 from 0154003.DRT (3 A) to Littelfuse 0154004.DRT, 4 A slow-blow, in the same OMNI-BLOK holder and the same footprint Fuse:Fuseholder_Littelfuse_Nano2_154x. Reason: docs/BRIEF.md Parts now names 0154004.DRT (decided 2026-11-05, research/parts/input-fuse.md) for margin over the 1.96 A worst-case input current at 50 C. Close SPEC open question Q1 (3 A vs 4 A) as decided. Footprint and nets do not change; ERC, DRC and drift must stay clean.

- Change: fuse-f1-4a-0154004
- Files: schematic.intent.json, docs/BOM.md, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/PINOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-05 — F1 fuse 3 A → 4 A (0154004.DRT)

- Change: fuse-f1-4a-0154004
- Files: schematic.intent.json, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch (re-drafted), tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (F1 Value text only), .copperhead/constraints.json, docs/BOM.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/PINOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - F1 changes from Littelfuse 0154003.DRT (3 A) to 0154004.DRT (4 A slow-blow), per docs/BRIEF.md Parts and research/parts/input-fuse.md.
  - The holder, the footprint (Fuse:Fuseholder_Littelfuse_Nano2_154x) and the nets (VIN_RAW/VIN_FUSED) are unchanged.
  - At the 1.96 A worst-case input the fuse now runs at ~49 % of rating, where the 3 A part ran at ~87 %.
  - SPEC §9 Q1 is closed as decided.
  - `protection.fuse_rating_A` max changes from 3 to 4.
  - FB1 (and any later series filter inductor) now needs a ≥ 4 A rating instead of ≥ 3 A. The RCL1225 (~7 A class, UNVERIFIED) still passes.
  - The SUBSYSTEMS §0 block-diagram F1 line was briefly corrupted by a malformed edit, then restored.
- Open: confirm the 0154004.DRT 50 °C rerating and cold resistance on the datasheet. Size the input copper against 4 A when it is routed.
- Verification: ERC clean. DRC clean (44 unrouted, unchanged baseline; 1 pre-existing FB1 library-internal finding). Legibility 0 errors. Drift clean.

## 2026-11-05 — create pipeline stage: layout-draft

- Change: layout-draft-tps5430-buck
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-05 — create pipeline stage: layout-draft

- Change: layout-draft-tps5430-buck
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md (new), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - Edge.Cuts resized to 55 × 35 mm, (100,100)–(155,135).
  - All parts placed with move_footprint. J1 is on the left edge and J2 on the right edge. The input chain runs J1 → F1 → Q1 → D1 → FB1 across the top. The buck core (U1, C3, C4, D3) is in the bottom band, with L1 in the bottom-right corner.
  - Routed: BOOT, SW (C4/TP5 leg), FB, VIN_RAW, VIN_FUSED, VIN_BUCK (C3 bypass) and LED_A. Unrouted connections went from 52 to 44.
  - No footprint, pad or net was added, deleted or rewritten.
- Open items (see docs/LAYOUT.md Draft quality):
  - No GND pour yet.
  - The high-current paths (VIN_PROT, VIN_BUCK bulk, SW to D3/L1, +5V) are unrouted.
  - One finding inside the FB1 library footprint (R_2512 silk_over_copper), which cannot be fixed at placement.
  - No M3 mounting holes exist in the schematic or BOM.
  - TP4 is not placed at J2.
- Silkscreen: the 25 placement silk warnings were cleared by moving Reference labels. TP1 moved to (138.6, 113.8) and TP7 to (104, 132.5).
- Verification: DRC clean (44 unrouted vs 52 baseline; 1 library-internal finding on FB1). ERC clean. Drift clean.

## 2026-11-05 — create pipeline stage: schematic

- Change: schematic-capture-tps5430-buck
- Files: schematic.intent.json, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch, docs/PINOUT.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean

## 2026-11-05 — create pipeline stage: schematic

- Change: schematic-capture-tps5430-buck
- Files: schematic.intent.json (new), tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch (drafted), docs/PINOUT.md (new), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - All 34 BOM.md parts are captured with the BOM lib_ids, values and footprints, grouped by SUBSYSTEMS.md §1–§7 headings.
  - Nets follow the SUBSYSTEMS planned list. LED_A (R4 to D4 anode) is the only added net.
  - U1.2 and U1.3 (NC) are explicit no-connects. There is still no EN pull-up (`protection.ena_pin_max_V`).
  - D1 SMBJ18A stays Device:D_Zener to keep unidirectional polarity. The dossier's bidirectional Diode:SMAJ18A was rejected.
- Open item: verify_symbols flags the engine-generated power symbols (copperhead_power:+5V, GND, PWR_FLAG, VIN_BUCK) as wrong-library; it expects power:*. This cannot be set from the IR. Retarget them before netlist export to layout. All 13 BOM part symbols verified.
- Verification: ERC clean. Legibility 0 errors (1 advisory: low utilization on A3, kept). Drift clean. Score 75.7/100.
- Constraint revisits: the 12 reopened PCB-scope items are resolved as pending layout (no copper yet).

## 2026-11-05 — create pipeline stage: part-selection

- Change: part-selection-bom
- Files: docs/BOM.md, docs/SPEC.md, docs/DECISIONS.md, docs/SUBSYSTEMS.md, docs/CHANGELOG.md
- Verification: ERC not required

## 2026-11-05 — create pipeline stage: part-selection

- Change: part-selection-bom
- Files: docs/BOM.md (new), docs/SPEC.md §5, docs/SUBSYSTEMS.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - One BOM row per refdes: U1, Q1, F1, D1–D4, L1, FB1, C1–C9, R1–R4, J1–J3, TP1–TP7. Every MPN is flagged UNVERIFIED.
  - Every symbol was confirmed installed with symbol_pins, and every footprint resolved with check_footprints.
  - Leakage and quiescent roll-up: shutdown about 0.11 mA (budget 1 mA), no-load about 6–10 mA (budget 20 mA).
- Deviations:
  - J1/J2 change from Würth 691137710002 to Phoenix MKDS 1,5/2-5.08 (1715721), because no Würth footprint is installed.
  - L1 uses the SRP1245A footprint, because no SRP1265A footprint is installed. Compare the pads before layout.
  - D1 (SMBJ18A) is drawn with the Device:D_Zener symbol, because no unidirectional TVS symbol is installed.
  - R_en is still intentionally absent.
- Verification: no schematic exists yet, so drift is vacuously clean. ERC and DRC are not required (no schematic or board edits).

## 2026-11-05 — create pipeline stage: architecture

- Change: architecture-subsystems
- Files: docs/SUBSYSTEMS.md, docs/DECISIONS.md, docs/CHANGELOG.md, docs/SPEC.md
- Verification: ERC not required

## 2026-11-05 — create pipeline stage: architecture

- Change: architecture-subsystems
- Files: docs/SUBSYSTEMS.md (new), docs/SPEC.md, docs/DECISIONS.md, .copperhead/constraints.json
- Summary: prose block diagram and per-subsystem key values for input/protection (J1 → F1 → Q1 → D1 TVS on protected side, 12 V zener + 100 kΩ gate clamp), the bridged EMI filter, the buck stage, output/feedback/compensation, EN (internal pull-up only, no external pull-up fitted on purpose), the LED (1.5 kΩ, about 2 mA) and test points. MCU, connectivity and UI are explicitly not present. Budget roll-ups are all within SPEC limits; efficiency is marginal and needs a bench check.
- New constraints: protection.ena_pin_max_V, power.output_filter_compensation
- Deviation from brief: feedback resistors specified at 0.5 % instead of 1 % to hold the ±3 % Vout budget
- Verification: ERC/DRC not required (no schematic or board edits)

## 2026-11-05 — create pipeline stage: spec-seed

- Change: spec-seed-tps5430-buck
- Files: docs/SPEC.md, docs/DECISIONS.md
- Verification: ERC not required
