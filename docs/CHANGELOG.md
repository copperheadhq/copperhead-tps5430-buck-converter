# Design changelog

Append-only, newest first. One entry per committed copperhead run.

## 2026-11-07 — Board text only. In tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, the fab-note text on the User.Comments layer begins 'FAB NOTES (rev A):'. Change only that heading to 'FAB NOTES (rev E):'. Keep the three notes exactly as they are (1. J1 and J2 plated holes: finished hole 1.3 mm MINIMUM (0.9 mm square terminal pins). 2. Outer copper 2 oz (70 um) on F.Cu and B.Cu. 3. Vias marked tented must be tented (solder mask over via, both sides).), and keep the text's position, size and layer. Why: rev E is about to be released (person's request 2026-11-07) and the release freezes the design files; the heading still names rev A. No copper, net, footprint, zone, schematic or BOM change. Add docs/CHANGELOG.md and docs/DECISIONS.md entries. Do not touch releases/ or outputs/. DRC must stay at 0 violations; ERC, parity and drift must stay clean.

- Change: fab-note-heading-rev-e
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Board fab-note heading names rev E (board text only)

- Change: fab-note-heading-rev-e
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (heading of the Cmts.User fab-note gr_text only), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/fab-note-heading-rev-e
- Summary:
  - Rev E is about to be released (person's request, 2026-11-07), and the release freezes the design files. The User.Comments fab note added in change review-docs-fab-notes still began 'FAB NOTES (rev A):'.
  - The heading now reads 'FAB NOTES (rev E):'. The three notes (1.3 mm minimum finished J1/J2 holes, 2 oz outer copper, tented vias stay tented), the text's position (100, 143), size and layer are unchanged. Checked on the board after the edit.
  - No copper, net, footprint, zone, schematic or BOM change. releases/ and outputs/ untouched; outputs/ is stale until the next export (User.Comments is not a fab layer).
- Verification: DRC clean (0 violations). ERC clean. Drift clean. Schematic parity unaffected (no footprint or symbol change).

## 2026-11-07 — Docs only. Bring the design record in step with board commit 01c54ce, made with research/proposals/rev-e/teardrops/scripts/altium_teardrops.py and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07). It changes one rule of the Altium-style teardrops of 79270d0: T-junction fillets are now sized from the trunk's (wider track's) width, as Altium's dialog measures w on the trunk, instead of from the narrower track; still clamped to the copper along each arm and shrunk for 0.25 mm clearance and the 1 mm quiet rule. Effect: the C6-tee fillet (0.3 mm LED feed into the 2 mm +5V trunk) grows from about 0.3 mm to about 2 mm along the diagonal and down the feed; equal-width corners (FB, EN, EN_EXT, VIN_PROT, SW at C4) are unchanged; VIN_BUCK fillets at the 1.5 mm track are larger, one at (132.6, 119.8) at half size; the same 4 are skipped. Update docs/LAYOUT.md's teardrop section and the openspec requirement from change docs-sync-board-79270d0-altium-teardrops (a new change that MODIFIES it: T-junction fillets 100 % of the trunk width along the trunk and up the branch). Add docs/DECISIONS.md and docs/CHANGELOG.md entries (why: the person found the C6 fillet radius too small; Altium's diagram measures w on the trunk). The change postdates rev D and belongs to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-01c54ce-tjunction-trunk-width
- Files: docs/LAYOUT.md, openspec/changes/docs-sync-board-01c54ce-tjunction-trunk-width/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit 01c54ce: T-junction fillets sized from the trunk width (rev E)

- Change: docs-sync-board-01c54ce-tjunction-trunk-width
- Files: docs/LAYOUT.md (Teardrops and fillets current-state section), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-01c54ce-tjunction-trunk-width (MODIFIED 'Teardrops on pads and vias at track joints')
- Summary:
  - The person made board commit 01c54ce (2026-11-07) with research/proposals/rev-e/teardrops/scripts/altium_teardrops.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - One rule of the 79270d0 Altium-style teardrops changed: T-junction fillets are sized from the trunk's (wider track's) width, as Altium's dialog measures w on the trunk, instead of from the narrower track. Still clamped to the copper along each arm and shrunk for 0.25 mm clearance and the 1 mm quiet rule.
  - Why: the person found the C6-tee fillet radius too small.
  - Effect (person-reported): C6-tee fillet (0.3 mm LED feed into the 2 mm +5V trunk) about 0.3 → 2 mm along the diagonal and down the feed; equal-width corners (FB, EN, /EN_EXT, VIN_PROT, SW at C4) unchanged; VIN_BUCK fillets at the 1.5 mm track larger, the one at (132.6, 119.8) at half size; the same 4 joints skipped. Pad and via teardrops unchanged.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit; no net, value, part or footprint change.
- Open: outputs/ is stale until the next export. SW copper not re-measured.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit 79270d0, made with research/proposals/rev-e/teardrops/scripts/altium_teardrops.py and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07), because copperhead do cannot refill zones. Read its commit message and the board. The person asked for teardrops on every joint, via, track and pad in the style of Altium's Teardrops dialog: style Curved, adjust size on; via/TH pad 30 % of the diameter long and 70 % wide; SMD pad 100 % of the entering track's width long and 200 % wide; a track width change tapered over 100 % of the wider width; T-junction fillets 100 % of the narrower width along each track and up the branch. Record: (1) docs/LAYOUT.md teardrops section, replacing the current-state description (keep earlier steps 215da06, f862007, aa520d7, eb651e0 as history): pads and vias use KiCad teardrops set per item from the entering track's width (KiCad measures from the pad edge as a multiple of the pad's smaller dimension or the via diameter), curved edges, enabled on 56 pads and 15 vias, 48 generated by the refill and clipped to clearance, C5.1 included again because at these sizes it keeps the 1 mm quiet rule; tracks and T-junctions use 23 curved fillets drawn as filled F.Cu graphic polygons on the joint's net (concave quadratic-Bezier edges), clamped to the copper that exists along each arm and shrunk until they clear other nets by 0.25 mm and, on quiet nets, GND vias by 1 mm; 4 were skipped for lack of room (VIN_BUCK at the via hop at (133.35, 122.3) twice, VIN_BUCK at (127.03, 119.0), the generic SW corner at (130.5, 124.0)); GND joints are skipped because those tracks lie inside the solid GND pour; the SW step at U1 PH has a hand-drawn curved fillet; the straight C6 and SW fillets of aa520d7 and eb651e0 are replaced (the LED branch shift of eb651e0 stays). (2) docs/DECISIONS.md entry with the why (the person's request; mechanical and etch robustness for an industrial cabinet, docs/BRIEF.md Environment). (3) docs/CHANGELOG.md entry. (4) openspec: a new change that MODIFIES the earlier teardrop and T-joint fillet requirements (changes docs-sync-board-215da06-teardrops, docs-sync-board-f862007-tjoint-fillets, docs-sync-board-aa520d7-straight-fillets, docs-sync-board-eb651e0-c6-branch-shift) into one requirement citing docs/BRIEF.md Environment: pads, vias, track width changes and T-junctions SHALL carry curved teardrops or fillets at the Altium-dialog proportions above, reduced where needed to keep 0.2 mm clearance and the 1 mm quiet-copper rule, except GND joints inside the solid pour. Update the SW copper figure where LAYOUT, SPEC or SUBSYSTEMS give it only if it is stated as current (do not invent a value; say it changed slightly with the fillets if no value is measured). The change postdates rev D and belongs to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-79270d0-altium-teardrops
- Files: docs/LAYOUT.md, docs/SUBSYSTEMS.md, openspec/changes/docs-sync-board-79270d0-altium-teardrops/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit 79270d0: Altium-style curved teardrops and fillets (rev E)

- Change: docs-sync-board-79270d0-altium-teardrops
- Files: docs/LAYOUT.md (new Teardrops and fillets current-state section; 215da06, f862007, aa520d7, eb651e0 sections marked history; SW copper row and bullet), docs/SUBSYSTEMS.md §3 (SW copper note), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-79270d0-altium-teardrops (MODIFIED teardrop and T-joint fillet requirements, merged)
- Summary:
  - The person made board commit 79270d0 (2026-11-07) with research/proposals/rev-e/teardrops/scripts/altium_teardrops.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - Altium Teardrops dialog proportions, style Curved: via/TH pad 30 % of d long, 70 % wide; SMD pad 100 % of track width long, 200 % wide; width change tapered over 100 % of the wider width; T-junction 100 % of the narrower width along each track and up the branch.
  - Pads and vias: per-item KiCad teardrops from the entering track width, curved, enabled on 56 pads and 15 vias; the refill generated 48 teardrop zones, clipped to clearance. C5.1 included again (keeps the 1 mm quiet rule at these sizes).
  - Tracks and T-junctions: 23 curved filled F.Cu `gr_poly` fillets on the joint's net (quadratic-Bezier edges), clamped to existing copper and shrunk to clear other nets by 0.25 mm and, on quiet nets, GND vias by 1 mm. 4 skipped for room: VIN_BUCK at (133.35, 122.3) twice, VIN_BUCK at (127.03, 119.0), SW at (130.5, 124.0). GND joints skipped (inside the solid pour). Hand-drawn curved fillet at the SW step at U1 PH.
  - Replaced: the straight C6-tee (eb651e0) and SW (aa520d7) fillets. The eb651e0 LED branch shift stays.
  - Checked on the live board in this run: 71 enabled curved per-item teardrop blocks, 48 teardrop zones, 34 `gr_poly` (23 net-assigned F.Cu fillets + 11 lockup).
  - SW copper changed slightly with the fillets; not re-measured.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit; no net, value, part or footprint change.
- Open: outputs/ is stale until the next export. Re-measure SW copper if a figure is needed.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit eb651e0, made with research/proposals/rev-e/teardrops/scripts/c6_branch_shift.py and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07). Read the commit message and the board. (1) docs/LAYOUT.md placement rows: R4 (157.5, 123.5) -90 to (157.65, 123.5) -90 and D4 (157.5, 127.0) 90 to (157.65, 127.0) 90, keeping the old values as history. (2) docs/LAYOUT.md routing: the 0.3 mm +5V LED feed now runs (157.65, 118.22) to R4.1 (157.65, 122.675), its right edge flush with the 2 mm trunk's right edge x 157.8, starting level with the trunk's corner; /LED_A runs x 157.65 from R4.2 (124.325) to D4.2 (126.213). (3) docs/LAYOUT.md teardrops: the C6-tee +5V fillet of f862007/aa520d7 is replaced by a straight-edged triangular +5V fillet in the V between the feed and the trunk's diagonal, points (157.65, 118.75), (156.9, 119.45), (157.55, 119.75); the SW fillet at U1 PH is unchanged; tjoint_fillets.py now handles only the SW fillet. (4) docs/DECISIONS.md entry (why: the person asked to shift the trace and its downstream parts so the feed meets the trunk flush, with a fillet at the joint, instead of a large fillet bridging an off-centre joint). (5) docs/CHANGELOG.md entry. (6) Update docs/PINOUT.md or docs/BOM.md only if they give R4 or D4 coordinates. The placement file in outputs/ is stale until the next export. The change postdates rev D and belongs to rev E. Do not edit the .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-eb651e0-c6-branch-shift
- Files: docs/LAYOUT.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-eb651e0-c6-branch-shift/specs/board/spec.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit eb651e0: LED feed shifted flush with the +5V trunk, triangular C6-tee fillet (rev E)

- Change: docs-sync-board-eb651e0-c6-branch-shift
- Files: docs/LAYOUT.md (R4/D4 placement row, Rev E E4 bullet, Routed R4 feed and LED_A bullets, Teardrops cross-reference, T-joint copper fillets section), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-eb651e0-c6-branch-shift (MODIFIED 'copper fillets at narrow-into-wide T-joints')
- Summary:
  - The person made board commit eb651e0 (2026-11-07) with research/proposals/rev-e/teardrops/scripts/c6_branch_shift.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - R4 moved from (157.5, 123.5) −90° to (157.65, 123.5) −90°, and D4 from (157.5, 127.0) 90° to (157.65, 127.0) 90°.
  - The 0.3 mm +5V LED feed now runs (157.65, 118.22) → R4.1 (157.65, 122.675). It starts level with the trunk's corner, and its right edge is flush with the 2 mm trunk's right edge (x 157.8). /LED_A runs x 157.65 from R4.2 (124.325) to D4.2 (126.213).
  - The aa520d7 C6-tee +5V fillet (uuid 96dd1a03-…) is replaced by a straight-edged triangular +5V fillet in the V between the feed and the trunk's diagonal (uuid 2515ecc8-…): (157.65, 118.75), (156.9, 119.45), (157.55, 119.75). The SW fillet at U1 PH (307e574f-…) is unchanged. tjoint_fillets.py now handles only the SW fillet.
  - Checked on the live board in this run: R4 `(at 157.65 123.5 -90)`, D4 `(at 157.65 127 90)`, feed and /LED_A segment ends as above, the new polygon on F.Cu net +5V, and 96dd1a03 absent.
  - docs/PINOUT.md and docs/BOM.md give no R4/D4 coordinates, so they are unchanged.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit; no net, value, part or footprint change.
- Open: outputs/ is stale until the next export, including the placement file (R4/D4 positions).
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit aa520d7, made with research/proposals/rev-e/teardrops/scripts/tjoint_fillets.py and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07). It replaces the two rounded T-joint fillets of f862007 (convex hulls of two circles) with straight-sided tapered polygons, still filled F.Cu graphic polygons on the joint's net: +5V at the C6 tee, points (157.0, 118.3), (157.8, 118.22), (157.65, 119.9), (157.35, 119.9), (157.08, 119.35), the right edge continuing the trunk's x 157.8 edge down to the 0.3 mm LED feed; SW at U1 PH, points (129.1, 124.095), (130.5, 123.0), (130.9, 123.0), (130.9, 125.0), (130.5, 125.0), (129.1, 125.095), straight from the 1.0 mm track's edges to the corners of the 2.0 mm run. SW copper on F.Cu is now 48.3 mm2 (48.4 with the rounded fillet, 48.2 before any). Update docs/LAYOUT.md where it describes the f862007 fillet shapes and the SW copper figure, and docs/SUBSYSTEMS.md where it gives the SW copper figure; keep the rounded version as history. Add a docs/DECISIONS.md entry (why: the person asked for straight geometry) and a docs/CHANGELOG.md entry. If the openspec requirement from change docs-sync-board-f862007-tjoint-fillets says teardrop-shaped, add a change that MODIFIES it to say tapered copper fillet with straight edges. The change postdates rev D and belongs to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-aa520d7-straight-fillets
- Files: docs/LAYOUT.md, docs/SUBSYSTEMS.md, openspec/changes/docs-sync-board-aa520d7-straight-fillets/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit aa520d7: straight tapered T-joint fillets (rev E)

- Change: docs-sync-board-aa520d7-straight-fillets
- Files: docs/LAYOUT.md (option A SW copper row, Teardrops cross-reference, T-joint copper fillets section), docs/SUBSYSTEMS.md §3 (SW copper figure), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-aa520d7-straight-fillets (MODIFIED 'copper fillets at narrow-into-wide T-joints' and 'minimal switch-node copper')
- Summary:
  - The person made board commit aa520d7 (2026-11-07) with research/proposals/rev-e/teardrops/scripts/tjoint_fillets.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - The two rounded fillets of f862007 (convex hulls of two circles) are replaced by straight-sided tapered filled F.Cu `gr_poly` items on the joint's net:
    - +5V at the C6 tee (uuid 96dd1a03-…): (157.0, 118.3), (157.8, 118.22), (157.65, 119.9), (157.35, 119.9), (157.08, 119.35); the right edge continues the trunk's x 157.8 edge down to the 0.3 mm LED feed.
    - SW at U1 PH (uuid 307e574f-…): (129.1, 124.095), (130.5, 123.0), (130.9, 123.0), (130.9, 125.0), (130.5, 125.0), (129.1, 125.095), straight from the 1.0 mm track's edges to the corners of the 2.0 mm run.
  - Checked on the live board in this run: both polygons present with these points and nets; the f862007 uuids (1177ddbc-…, 97729a92-…) are gone.
  - SW copper on F.Cu 48.4 → 48.3 mm² (48.2 before any fillet; person-reported).
  - The rounded f862007 shapes are kept as history in LAYOUT. The f862007 openspec requirement said teardrop-shaped; it is MODIFIED to a tapered copper fillet with straight edges.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit; no net, value, part or footprint change.
- Open: outputs/ is stale until the next export.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit f862007, made with research/proposals/rev-e/teardrops/scripts/tjoint_fillets.py and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07), because copperhead do cannot refill zones. Read the commit message and the board. It adds two filled F.Cu graphic polygons with nets (teardrop-shaped copper fillets) at the two narrow-into-wide T-joints that KiCad's teardrop tool skips, because the narrow track ends inside another track rather than at its end: (a) +5V, the 0.3 mm LED feed ending at (157.5, 118.6) inside the corner of the 2 mm +5V trunk beside C6: hull of a 0.15 mm radius circle at (157.5, 119.9) and a 0.5 mm radius circle at (157.5, 118.6); (b) SW, the 1.0 mm PH track ending at (129.9, 124.6) inside the round end of the 2.0 mm SW run: hull of 0.5 mm radius at (129.1, 124.595) and 1.0 mm radius at (130.5, 124.0); SW copper on F.Cu 48.2 to 48.4 mm2. Record: (1) docs/LAYOUT.md teardrops section: the two fillets, why (T-joints are not handled by KiCad's teardrop tool, neither by the kicad-cli refill nor by Edit > Edit Teardrops in the editor, which the person tried on 2026-11-07), and that they are graphic copper shapes with a net, so they survive refills and DRC checks them; correct the earlier statement that the six track-to-track joints have no teardrops: of those, the C6 tee and the SW step now have fillets; the others end on pads (covered by pad teardrops) or are equal-width VIN_BUCK joints that need none. Update the SW copper figure where LAYOUT, SPEC or SUBSYSTEMS give it, and the minimal switch-node copper requirement only if its text lists the SW copper items (add the fillet). (2) docs/DECISIONS.md entry. (3) docs/CHANGELOG.md entry. (4) openspec: a new change with a requirement citing docs/BRIEF.md Environment: narrow-into-wide T-joints that KiCad's teardrop tool cannot reach SHALL carry a teardrop-shaped copper fillet on the joint's net. The change postdates rev D and belongs to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-f862007-tjoint-fillets
- Files: docs/LAYOUT.md, docs/SUBSYSTEMS.md, openspec/changes/docs-sync-board-f862007-tjoint-fillets/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit f862007: T-joint copper fillets (rev E)

- Change: docs-sync-board-f862007-tjoint-fillets
- Files: docs/LAYOUT.md (Teardrops section corrected, new T-joint copper fillets section, option A SW copper row), docs/SUBSYSTEMS.md §3 (SW copper figure), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-f862007-tjoint-fillets (ADDED T-joint fillet requirement, MODIFIED 'minimal switch-node copper')
- Summary:
  - The person made board commit f862007 (2026-11-07) with research/proposals/rev-e/teardrops/scripts/tjoint_fillets.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - Two filled F.Cu `gr_poly` copper fillets with nets, at the T-joints KiCad's teardrop tool skips (narrow track ending inside another track; neither the refill nor Edit > Edit Teardrops handles them, tried 2026-11-07):
    - +5V (uuid 1177ddbc-…): 0.3 mm LED feed into the 2 mm trunk corner beside C6; hull of r 0.15 mm at (157.5, 119.9) and r 0.5 mm at (157.5, 118.6).
    - SW (uuid 97729a92-…): 1.0 mm PH track into the round end of the 2.0 mm SW run; hull of r 0.5 mm at (129.1, 124.595) and r 1.0 mm at (130.5, 124.0).
  - Graphic copper with a net survives zone refills and is checked by DRC.
  - Correction to docs-sync-board-215da06-teardrops: of the six track-to-track junctions said to have no teardrop, the C6 tee and the SW step now have fillets; the others end on pads (pad teardrops) or are equal-width VIN_BUCK joints that need none.
  - SW copper on F.Cu 48.2 → 48.4 mm² (person-reported).
  - Checked on the live board in this run: both polygons present on F.Cu with nets +5V and SW and the points above.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit.
- Open: outputs/ is stale until the next export. The 215da06 open item asking for Edit > Edit Teardrops is closed by this change.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit 215da06 (teardrops), made with research/proposals/rev-e/teardrops/scripts/teardrops.py and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07), because copperhead do cannot refill zones. Read the commit message and the board. Record: (1) docs/LAYOUT.md, a teardrops bullet or section: the teardrop flag is set on every pad (82) and via (178) except C5.1; the refill generated 44 teardrop zones at track-to-pad and track-to-via joints, per net +5V 6, FB 5, /Q1_GATE 4, VIN_BUCK 4, GND 4, EN 3, /EN_EXT 3, VIN_PROT 3, VIN_FUSED 3, SW 3, /LED_A 2, /VIN_RAW 2, /BOOT 2; KiCad's default parameters from the project (87c1c40): length ratio 0.5 up to 1 mm, height ratio 1.0 up to 2 mm, only where the track is narrower than 0.9 x the pad, none on pads inside a zone. C5.1 has none, because its teardrop on the 0.4 mm Kelvin run came 0.68 mm from the GND via at (140.75, 118.75), inside the 1 mm quiet-copper rule; C5.1 is a 3.5 x 2.5 mm pad. Every GND via edge stays at least 1 mm from FB, EN, /EN_EXT, the Kelvin run and their teardrops. (2) The track-to-track option set in 87c1c40 (td_ontrackend true) generated nothing: kicad-cli's zone refill builds pad and via teardrops only, so the six track-to-track junctions have none (the 0.3 mm LED feed tee into the 2 mm +5V trunk at (157.5, 118.6), the SW width steps 0.5/1.0/2.0 mm at U1 PH, VIN_PROT 1.0 to 2.0 mm at Q1). They would need KiCad's Edit > Edit Teardrops in the editor. Say so in LAYOUT and in the DECISIONS entry, correcting the 87c1c40 DECISIONS entry's expectation without deleting it. (3) docs/DECISIONS.md entry (why: etch and mechanical robustness for a board in an industrial cabinet, docs/BRIEF.md Environment; JLCPCB does not require them). (4) docs/CHANGELOG.md entry. (5) openspec: a new change with a requirement citing docs/BRIEF.md Environment: pads and vias SHALL carry KiCad teardrops at track joints where the track is narrower than 0.9 x the pad, except where a teardrop would bring quiet copper within 1 mm of a GND via. The change postdates rev D and belongs to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-215da06-teardrops
- Files: docs/LAYOUT.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-215da06-teardrops/specs/board/spec.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit 215da06: teardrops on pads and vias (rev E)

- Change: docs-sync-board-215da06-teardrops
- Files: docs/LAYOUT.md (new Teardrops section), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-215da06-teardrops (requirement)
- Summary:
  - The person made board commit 215da06 (2026-11-07) with research/proposals/rev-e/teardrops/scripts/teardrops.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - Teardrop flag on every pad (82) except C5.1, and on every via (178). Parameters are KiCad defaults (87c1c40): length 0.5 up to 1 mm, height 1.0 up to 2 mm, filter 0.9, none on pads inside a zone.
  - 44 `padvia` teardrop zones were generated. Per net: +5V 6, FB 5, /Q1_GATE 4, VIN_BUCK 4, GND 4, EN 3, /EN_EXT 3, VIN_PROT 3, VIN_FUSED 3, SW 3, /LED_A 2, /VIN_RAW 2, /BOOT 2.
  - C5.1 has none: its teardrop on the 0.40 mm Kelvin run came 0.68 mm from the GND via at (140.75, 118.75), inside the 1 mm quiet-copper rule.
  - Checked on the live board in this run: 82 enabled pad teardrop blocks, none on C5.1, 44 `padvia` zones, no `track_end` zones.
  - Track-to-track teardrops (`td_ontrackend`, 87c1c40) generated nothing, because kicad-cli's refill builds pad and via teardrops only. The LED feed tee at (157.5, 118.6), the SW steps at U1 PH and VIN_PROT at Q1 have none. The 87c1c40 expectation is corrected in DECISIONS.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_pro, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit.
- Open:
  - outputs/ is stale until the next export.
  - Track-to-track teardrops need KiCad Edit > Edit Teardrops in the editor, if still wanted.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Project setting only. In tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pro, under board.design_settings, set the KiCad teardrop option 'td_ontrackend' (Board Setup > Design Rules > Teardrops > Track to track) to true, so a zone refill also generates track-to-track teardrops where a narrow track joins a wider one. Keep KiCad's defaults for everything else: td_onvia, td_onsmdpad and td_onpthpad true, td_onroundshapesonly false, and the three teardrop_parameters entries (td_round_shape, td_rect_shape, td_track_end) at length ratio 0.5, max length 1.0 mm, height ratio 1.0, max height 2.0 mm, width-to-size filter 0.9, two-track spans allowed, no teardrop on pads inside a zone. If the file has no teardrop_options or teardrop_parameters block yet, add them with these values. Why: the person asked for teardrops on 2026-11-07 (the 0.3 mm LED feed tee into the 2 mm +5V trunk at (157.5, 118.6), the SW width steps at U1 PH, VIN_PROT 1.0 to 2.0 mm at Q1, and ~45 track-to-pad/via joints) for mechanical and etch robustness on a board for an industrial cabinet (docs/BRIEF.md Environment). Pad and via teardrop flags and the zone refill that generates the teardrops follow as a separate person-made board commit, because copperhead do cannot refill zones. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Record a docs/DECISIONS.md entry and a docs/CHANGELOG.md entry. Drift must stay clean.

- Change: kicad-pro-teardrop-track-to-track
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pro, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Project setting: KiCad track-to-track teardrops enabled (rev E)

- Change: kicad-pro-teardrop-track-to-track
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pro (board.design_settings only), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/kicad-pro-teardrop-track-to-track
- Summary:
  - The person asked for teardrops on 2026-11-07 for mechanical and etch robustness in an industrial cabinet (docs/BRIEF.md Environment). Joints in question:
    - the 0.3 mm LED feed tee into the 2 mm +5V trunk at (157.5, 118.6)
    - the SW width steps at U1 PH
    - VIN_PROT stepping from 1.0 to 2.0 mm at Q1
    - about 45 track-to-pad/via joints
  - The project file had no teardrop block. Added `teardrop_options` with `td_ontrackend` true. The rest are KiCad defaults: `td_onvia`, `td_onsmdpad` and `td_onpthpad` true, `td_onroundshapesonly` false.
  - Added `teardrop_parameters` for `td_round_shape`, `td_rect_shape` and `td_track_end`, each at KiCad defaults:
    - length ratio 0.5, max length 1.0 mm
    - height ratio 1.0, max height 2.0 mm
    - width-to-size filter 0.9
    - two-track spans allowed, straight edges
    - no teardrop on pads inside a zone
  - No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit. No copper, net, value, part or footprint change. The board has no teardrops yet.
- Open: a separate person-made board commit sets the pad and via teardrop flags and refills the zones with KiCad, which generates the teardrops, because copperhead do cannot refill zones. Re-run KiCad DRC with zone refill and schematic parity after that commit. outputs/ stays stale until the next export.
- Verification: ERC clean. DRC clean. Drift clean.

## 2026-11-07 — Docs only. Bring the design record in step with board commit f05b01f, made with a pcbnew script (research/proposals/rev-e/sense-trace/scripts/kelvin_variants.py wide) and a kicad-cli zone refill outside copperhead do at the person's request (2026-11-07), because copperhead do cannot refill zones. The +5V R2.1 Kelvin feed (C5.1 -> R2.1) is widened from 0.25 to 0.40 mm on all five segments, same route: F.Cu C5.1 (141.0, 116.8) -> via (139.4, 116.8); B.Cu 21.85 mm along y 116.8 and 10.15 mm down x 117.3; via (117.3, 127.2) -> F.Cu to R2.1. Length unchanged, 32.4 mm on B.Cu. The B.Cu GND slot around it widens by 0.15 mm. No GND via moved; every GND via edge stays at least 1 mm from the run (rule of change gnd-stitching-vias-density). research/proposals/rev-e/ground-stitching/scripts/stitching.py now finds the Kelvin run by tracing narrow +5V copper from R2.1 rather than by its 0.25 mm width. Options compared (keep, shorten to 26.4 mm on a diagonal, widen) in research/proposals/rev-e/sense-trace/renders/sense-trace-options.png; the person chose widen because the run read as a hairline. Verify the widths against the board first. Record: docs/LAYOUT.md R2.1 Kelvin feed bullet (0.25 -> 0.40 mm, keep 0.25 as history), the quiet-copper stitching bullet and any other current-state mention of the '0.25 mm R2.1 Kelvin run' (refer to it as the R2.1 Kelvin run, 0.40 mm); docs/DECISIONS.md one entry (why: a 0.25 mm 32 mm run read as a hairline and is fragile to etch and handle; it carries only R2 divider current so width is not electrical; widen kept the route and the 1 mm quiet-copper margin, unlike the shorter diagonal); docs/CHANGELOG.md entry, and clear the open item in CHANGELOG that asks for this. If an openspec requirement names the run as 0.25 mm, update it in a new change citing docs/BRIEF.md. Postdates rev D; rev E; outputs/ stale until next export. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected, 0 parity issues.

- Change: docs-sync-board-f05b01f-kelvin-width
- Files: docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit f05b01f: R2.1 Kelvin run widened 0.25 → 0.40 mm (rev E)

- Change: docs-sync-board-f05b01f-kelvin-width
- Files: docs/LAYOUT.md (R2.1 Kelvin feed bullet, quiet-copper stitching bullet, item 4 history note), docs/DECISIONS.md, docs/CHANGELOG.md (this entry; the open item from docs-sync-board-tp8-spacing marked closed), openspec/changes/docs-sync-board-f05b01f-kelvin-width
- Summary:
  - The person made board commit f05b01f (2026-11-07) with research/proposals/rev-e/sense-trace/scripts/kelvin_variants.py `wide` and a kicad-cli zone refill, outside copperhead do.
  - The +5V R2.1 Kelvin run (C5.1 → R2.1) goes from 0.25 to 0.40 mm on all five segments, on the same route: F.Cu C5.1 (141, 116.8) → via (139.4, 116.8); B.Cu 21.85 mm along y 116.8, a 45°, and 10.15 mm down x 117.3; via (117.3, 127.2) → F.Cu to R2.1 (118.675, 127.2). Length is unchanged, 32.4 mm on B.Cu.
  - Checked on the live board in this run: every segment of the run reads `(width 0.4)`, on both F.Cu and B.Cu.
  - The B.Cu GND slot is 0.15 mm wider. No GND via moved, so the 1 mm quiet-copper margin of gnd-stitching-vias-density holds. stitching.py now finds the run by tracing narrow +5V copper from R2.1, not by its 0.25 mm width.
  - Options compared were keep, shorten (26.4 mm diagonal) and widen (research/proposals/rev-e/sense-trace/renders/sense-trace-options.png). The person chose widen.
  - No openspec requirement named the run as 0.25 mm, so none was modified.
  - The commit postdates rev D (1393414), so it belongs to rev E. No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit, and no net, value, part or footprint change.
- Open: outputs/ is stale until the next export.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD gives 0 violations, 0 unconnected and 0 parity issues. copperhead ERC, DRC and drift were run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commits cc082ae and 9493399 (9493399 restores the cc082ae TP8 position, which f05b01f had reverted by writing an older board copy), made with a pcbnew script outside copperhead do at the person's request (2026-11-07). TP8 (TP_GND3, GND probe pad, 1.0x1.0 mm) moved from (131.45, 121.6) to (131.85, 121.3), rotation 0, for more room between it and TP5 (SW, unchanged at (129.3, 122.0)). Centre spacing 2.2 -> 2.65 mm, pad-edge gap in x 1.15 -> 1.55 mm, courtyards no longer touch. Still within rev E E6's 2-3 mm (docs/BRIEF.md Interfaces: GND test pad within a few millimetres of TP5). The position is limited by the 1.5 mm VIN_BUCK track end at (132.6, 119.8), 0.28 mm clearance. TP8 keeps its solid GND zone connection; zone fill unchanged. TP8's Reference moved with it to (132.1, 119.95) (was 131.7, 120.25). Verify these against the board file first. Record: docs/LAYOUT.md placement row for C10/R5/TP8, the option A bullets that say TP8 at (131.45, 121.6) 2.2 mm from TP5, the TP8 reference position (keep old values as history); docs/DEVPLAN.md T5 if it cites the TP8 position or spacing; docs/DECISIONS.md one entry (why: the person asked for more spacing between the SW and GND probe pads so a probe tip and ground spring are less likely to bridge them; still inside E6); docs/CHANGELOG.md entry. Also note that the f05b01f +5V sense trace widening (0.25 -> 0.40 mm, C5.1 -> R2.1 Kelvin run) is a separate change not covered here unless already recorded. Postdates rev D; rev E; outputs/ stale until next export. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected, 0 parity issues.

- Change: docs-sync-board-tp8-spacing
- Files: docs/LAYOUT.md, docs/DEVPLAN.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commits cc082ae and 9493399: TP8 moved for more probe spacing (rev E)

- Change: docs-sync-board-tp8-spacing
- Files: docs/LAYOUT.md (C10/R5/TP8 placement row, option A placement bullet, both TP5/TP8 reference bullets), docs/DEVPLAN.md T5, docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-tp8-spacing
- Summary:
  - The person made both commits with a pcbnew script, outside copperhead do. 9493399 restores the cc082ae TP8 position, which f05b01f had reverted by writing an older board copy.
  - TP8 (TP_GND3, 1.0 × 1.0 mm GND probe pad) moved from (131.45, 121.6) to (131.85, 121.3), rotation 0. TP5 (SW) is unchanged at (129.3, 122.0).
  - Spacing: centre 2.2 → 2.65 mm, pad-edge gap in x 1.15 → 1.55 mm, and the courtyards no longer touch. This is still inside rev E E6's 2–3 mm (docs/BRIEF.md Interfaces).
  - The position is limited by the 1.5 mm VIN_BUCK track end at (132.6, 119.8), which is 0.28 mm from TP8's pad corner.
  - The TP8 reference moved with the pad to (132.1, 119.95), from (131.7, 120.25). The solid GND zone connection and the zone fill are unchanged.
  - Checked on the live board in this run: TP8 `(at 131.85 121.3)`, Reference local (0.25, −1.35), TP5 `(at 129.3 122)`, and the VIN_BUCK segment end at (132.6, 119.8).
  - **Not covered here:** f05b01f also widened the +5V Kelvin sense run (C5.1 → R2.1) from 0.25 to 0.40 mm. The live board shows 0.4 mm on both its F.Cu and B.Cu segments. That is a separate change, and no doc records it yet. *Recorded since by change docs-sync-board-f05b01f-kelvin-width (2026-11-07).*
  - The commits postdate rev D (1393414), so they belong to rev E. No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit.
- Open: outputs/ is stale until the next export. Record the f05b01f Kelvin-run widening in docs/LAYOUT.md (the R2.1 Kelvin feed bullet still says 0.25 mm) in a separate change. **Closed by change docs-sync-board-f05b01f-kelvin-width (2026-11-07).**
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift were run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with three board commits made with KiCad pcbnew scripts and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07), because copperhead do cannot refill zones: d4738e0 (branding position), bf2e241 (GND stitching redo) and e453b54 (fix to bf2e241). Plan and rules: research/proposals/rev-e/ground-stitching/stitching-plan.md; scripts: research/proposals/rev-e/ground-stitching/scripts/move_branding.py and stitching.py. Read those commit messages and the board for exact figures. Record:
(1) docs/LAYOUT.md silkscreen: the copperhead lockup and the tps5430/rev-e label moved together by (+0.5, -1.45) mm to x 102.0-116.4, y 134.15-138.0, 2.0 mm from both the left and the bottom board edge (was 1.5 mm and 0.55 mm), label still directly below the lockup (docs/BRIEF.md Physical); TP7's EN name moved from the left of TP7 to above-left of it (x 118.1-119.9, y 132.0-133.4) so it no longer reads as part of the lockup and stays clear of the J3 reference.
(2) docs/LAYOUT.md stitching: replace the via-site lists of r1-solid-gnd-stitching-vias and gnd-stitching-vias-density as current state (keep them as history) with the new rules: keep the 22 GND vias on a GND track end, inside D3's courtyard, or within 3.6 mm of D3.2, D1.2, C1.2, C2.2, C5.2 or C6.2; remove the other 82; add 148 tented 0.6/0.3 mm GND vias only where both GND fills overlap with 0.35 mm to spare: a ring within 1.6 mm of the SW copper (28), an edge fence at 2.5 mm pitch 1.4 mm in from the edges (55), and a 3.75 mm grid plus gap fill until every point that can take a via is within 3 mm of one (65). No new via in a part courtyard except L1, J1, J2, D3 and U1; none under the lockup and label (keep-out to the left and bottom edges, 2.6 mm towards TP7); every GND via edge at least 1 mm from FB, EN, /EN_EXT and the 0.25 mm R2.1 Kelvin run (the gnd-stitching-vias-density rule, kept). GND vias 104 -> 170; mean distance from the GND pours to the nearest GND via 2.12 -> 1.79 mm; the largest gap, 8.2 mm, is the strip under the label kept clear on purpose, elsewhere the worst is about 5.3 mm beside C5. Say the drill file gains 66 holes.
(3) docs/DECISIONS.md: one entry for the branding position and EN label (person's request, equal margins) and one for the stitching redo (why: ad hoc vias with uneven coverage and no edge fence; worst spacing about 5 mm, far below lambda/20 at 1 GHz in FR-4, about 7 mm). Note that bf2e241 broke the 1 mm quiet-copper rule for 8 vias, which KiCad DRC cannot check, and e453b54 fixed it.
(4) docs/CHANGELOG.md entry.
(5) openspec: a new change with requirements citing docs/BRIEF.md Physical ('Large copper pours'): GND stitching SHALL consist of the kept functional vias plus a ring along the SW copper, an edge fence at most 2.5 mm pitch, and a grid of at most 3.75 mm pitch with gap fill to 3 mm wherever both pours can take a via; no new GND via inside a small-part courtyard or under the lockup and label; every GND via edge at least 1 mm from FB, EN, /EN_EXT and the R2.1 Kelvin run. And a requirement that the lockup and label sit with equal margins of 2.0 mm from the left and bottom board edges.
The changes postdate rev D and belong to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-ground-stitching-branding
- Files: docs/LAYOUT.md, docs/SPEC.md, openspec/changes/docs-sync-board-ground-stitching-branding/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commits d4738e0 (branding position), bf2e241 (GND stitching redo) and e453b54 (fix to bf2e241) (rev E)

- Change: docs-sync-board-ground-stitching-branding
- Files: docs/LAYOUT.md (Silkscreen: TP7 EN name, lockup, label, clearance; new stitching current-state bullet, older via lists marked history), docs/SPEC.md §6, .copperhead/constraints.json (`pcb.copper_oz`), openspec/changes/docs-sync-board-ground-stitching-branding (two requirements), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The person made these commits with research/proposals/rev-e/ground-stitching/scripts/move_branding.py and stitching.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones. Plan: research/proposals/rev-e/ground-stitching/stitching-plan.md.
  - **Branding (d4738e0):** lockup and tps5430/rev-e label moved together by (+0.5, −1.45) mm to x 102.0–116.4, y 134.15–138.0. That is 2.0 mm from both the left and the bottom edge (was 1.5 and 0.55 mm). The label stays directly below the lockup. TP7's `EN` name moved from left of TP7 to above-left (x 118.1–119.9, y 132.0–133.4), so it no longer reads as part of the lockup and clears the J3 reference.
  - **Stitching (bf2e241, e453b54):**
    - 22 functional GND vias kept; 82 removed.
    - 148 tented 0.6/0.3 mm vias added where both GND fills overlap with 0.35 mm to spare: ring within 1.6 mm of the SW copper 28, edge fence at 2.5 mm pitch 1.4 mm in 55, 3.75 mm grid plus gap fill to 3 mm 65.
    - No new via in a part courtyard except L1, J1, J2, D3, U1; none under the lockup and label.
    - GND vias 104 → 170; the drill file gains 66 holes. Mean distance to the nearest GND via 2.12 → 1.79 mm. Largest gap 8.2 mm under the label, on purpose; elsewhere about 5.3 mm beside C5.
    - bf2e241 broke the 1 mm quiet-copper rule (FB, EN, EN_EXT, R2.1 Kelvin run) for 8 vias, which KiCad DRC cannot check; e453b54 fixed it.
  - Checked on the live board in this run: label at (102, 137.15), TP7 `EN` at (119, 132.7), 178 vias = 170 GND + 8 non-GND.
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit.
- Open: outputs/ is stale until the next export (the drill file there lacks the 66 extra holes).
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit fb7cc21, made with a KiCad pcbnew script and a kicad-cli zone refill outside copperhead do, at the person's request (2026-11-07), because they read as hairline traces under D3 and beside D1. That commit deletes three 0.5 mm F.Cu GND stubs: D3.2 anode to the via at (135.2, 134.75) along x 135.2 from y 132.4; D3.2 anode to the via at (137.6, 134.75) along x 137.6 from y 132.4; and R1.2 (136.825, 112) up x 136.825 to (136.825, 110.325) and a 45 degree into the D1.2 GND via at (136.5, 110). All three lay inside the solid F.Cu GND pour (D3.2, D1.2 and R1.2 have pad-level zone_connect solid), so they carried no current of their own; the vias and R1.2 stay on GND through the pour, and the 1.5 mm D3 anode trunk at x 136.4 and the D1.2 1.0 mm stubs and B.Cu strap are unchanged. The zone fill did not change. Record: (1) docs/LAYOUT.md: update the R1.2 GND bullets (lines that say R1.2 runs up x 136.825 into the D1.2 via) to say R1.2 reaches GND through its solid pour connection and the stub was removed in fb7cc21; update the D3 anode stub mention (the anode stubs to the vias at (135.2/137.6, 134.75)) and the E2 paragraph where it lists those vias, saying the vias now join the anode through the F.Cu pour, not 0.5 mm stubs. Keep history as history. (2) docs/DECISIONS.md: one entry with the why (redundant copper inside solid pour that read as hairline traces; person's request 2026-11-07). (3) docs/CHANGELOG.md entry. The change postdates rev D and belongs to rev E; outputs/ is stale until the next export. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-fb7cc21-gnd-stubs
- Files: docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit fb7cc21: three redundant GND stubs removed (rev E)

- Change: docs-sync-board-fb7cc21-gnd-stubs
- Files: docs/LAYOUT.md (Routed GND links R1.2 bullet, 9af7427 tidy-up R1.2 bullet, Rev E E2 paragraph, Option A GND list, via-density D3.2 return-via bullet), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-sync-board-fb7cc21-gnd-stubs
- Summary:
  - The person made board commit fb7cc21 (2026-11-07) with a KiCad pcbnew script and a kicad-cli zone refill, outside copperhead do.
  - Deleted: three 0.5 mm F.Cu GND stubs. Two ran from the D3.2 anode to the vias at (135.2, 134.75) and (137.6, 134.75), along x 135.2 and x 137.6 from y 132.4. The third ran from R1.2 (136.825, 112) up x 136.825 to (136.825, 110.325), then 45° into the D1.2 GND via at (136.5, 110).
  - Why: they lay inside the solid F.Cu GND pour (D3.2, D1.2 and R1.2 have pad-level `zone_connect 2`), carried no current of their own, and read as hairline traces under D3 and beside D1.
  - Unchanged: the vias, the 1.5 mm D3 anode trunk at x 136.4, the D1.2 1.0 mm stubs and B.Cu strap, and the zone fill. R1.2 and the vias stay on GND through the pour.
  - Checked on the live board in this run: both D3 vias present, no segment starting at (135.2/137.6, 132.4) or passing (136.825, 110.325).
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit; no net, value or footprint change.
- Open: outputs/ is stale until the next export.
- Verification: person-reported KiCad DRC with zone refill and schematic parity at HEAD: 0 violations, 0 unconnected, 0 parity issues. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Docs only. Bring the design record in step with board commit 55dc4db (option A power stage), made with a KiCad pcbnew script (research/proposals/rev-e/power-stage-loop/scripts/option_a_layout.py) and a kicad-cli zone refill outside copperhead do, at the person's request, because copperhead do cannot refill zones. Plan and reasons: research/proposals/rev-e/power-stage-loop/option-a-plan.md and power-stage-loop-analysis.md (sections 3, 4, 6, 9). Read the 55dc4db commit message and the board for exact coordinates. Record:
(1) docs/LAYOUT.md: placement rows for C1 (131.4, 127.4) -90, D3 (136.4, 128.0) -90, C3 (134.7, 121.0) 0, TP5 (129.3, 122.0), TP8 (131.45, 121.6); the routed bullets for VIN_BUCK (C2 -> C3.1 -> V1 via pair at (132.95/133.75, 122.3) -> 1.5 mm B.Cu hop -> V2 via pair at (133.35, 125.9/126.7) -> C1.1; pin 7 -> C1.1 on F.Cu; the B.Cu run at x 133.6 and its six vias are gone), SW (PH -> 2.0 mm straight at y 124.0 into D3.1 -> 2.0 mm to L1.1, F.Cu only), GND (pin 6 -> C1.2 between C1's pads; C1.2 -> 2.5 mm straight under D3's body to the anode; anode via moved to (135.3, 129.3); three tented GND vias at x 138.65, y 130.6/131.4/132.2; stitching via at (131.25, 121.25) removed; TP8 pad solid to GND), and the silkscreen moves (TP5/TP8 references 0.8 mm above the pads, SW label below TP5, C1 reference between its pads, C3 reference right of C3).
(2) docs/DECISIONS.md entries, each with its why: option A chosen by the person on 2026-11-07 over option B; C1 (10 uF) now closes the commutation loop at U1 pins 7/6 because U1's GND pin is not in the commutation loop, which partly reverses E1 (C3 moves upstream to the VIN hop); as-routed results from scripts/loop_inductance.py: commutation loop 46.1 -> 30.4 mm2 routed, 9.8 -> 6.9 nH with the B.Cu plane, diode leg 10.6 -> 8.1 nH, U1 local decoupling 2.0 -> about 2.7 nH (accepted); SW copper 47.1 -> 48.1 mm2, all F.Cu; B.Cu GND slot 10.5 x 2.6 mm -> about 5.2 x 1.6 mm (one hop; on two layers VIN must cross SW once); about 28-30 mm2 accepted as the floor with the B560C/SMC and a 1210 C1 (brief unchanged); the person chose to make the layout change before rev D bring-up, so the diode choice (E9) is still open and the D3 area is redone if the short-circuit test calls for a larger part. Say the change postdates rev D, belongs to rev E, and that outputs/ is stale until the next export.
(3) A docs/CHANGELOG.md entry.
(4) docs/SPEC.md section 6 'Tight switching loop' bullet: name C1 (10 uF) at U1 VIN/GND pins closing the commutation loop with D3 adjacent, and C3 (100 nF) at the VIN hop. Update docs/SUBSYSTEMS.md where it places C3 at the pins or describes the VIN_BUCK B.Cu run.
(5) openspec: a new change that MODIFIES the requirement 'HF input capacitor loop on F.Cu without vias' from openspec/changes/docs-sync-board-rev-e-e1-e4-e14 so it names C1: C1 SHALL have its VIN pad on U1 pin 7 copper and its GND pad on U1 pin 6 GND copper on F.Cu, with no via in the C1-U1-D3 commutation loop; scenario loop area about 30 mm2 routed. Also MODIFY 'minimal switch-node copper' only to drop the phrase that the B.Cu GND pour is continuous under the hot loop, replacing it with: interrupted only by the VIN_BUCK hop. ADD a requirement: VIN_BUCK SHALL cross under SW at most once, by one B.Cu hop with one via pair at each end. Each cites docs/BRIEF.md Physical ('Tight high-current switching loop, with the catch diode and input bypass capacitor adjacent to the regulator').
(6) docs/DEVPLAN.md T5: TP8 is now at (131.45, 121.6), 2.2 mm from TP5.
Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-55dc4db-option-a
- Files: docs/LAYOUT.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/DEVPLAN.md, openspec/changes/docs-sync-board-55dc4db-option-a/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Docs synced to board commit 55dc4db: option A power stage (rev E)

- Change: docs-sync-board-55dc4db-option-a
- Files: docs/LAYOUT.md (placement rows C1, D3, C3, TP1..TP5, C10/R5/TP8; superseded notes on the VIN_BUCK, SW, GND and E1 bullets; new Option A section; silkscreen), docs/SPEC.md §6, docs/SUBSYSTEMS.md §0/§3, docs/DEVPLAN.md T5, openspec/changes/docs-sync-board-55dc4db-option-a (two MODIFIED requirements, one ADDED), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - **How the commit was made.** The person made board commit 55dc4db with research/proposals/rev-e/power-stage-loop/scripts/option_a_layout.py and a kicad-cli zone refill, outside copperhead do, which cannot refill zones.
  - **Sources.** Coordinates are read from the live board and the script. The commit message could not be read, because the workspace is not a git repository. As-routed figures are person-reported from scripts/loop_inductance.py.
  - **Placement.** C1 (131.4, 127.4) −90°; D3 (136.4, 128.0) −90°; C3 (134.7, 121.0) 0° at the VIN hop; TP5 (129.3, 122.0); TP8 (131.45, 121.6).
  - **VIN_BUCK.** C2 → C3.1 → V1 vias (132.95/133.75, 122.3) → 1.5 mm B.Cu hop → V2 vias (133.35, 125.9/126.7) → C1.1. Pin 7 → C1.1 runs on F.Cu. The B.Cu run at x 133.6 and its six vias are gone.
  - **SW.** PH runs 2.0 mm straight at y 124.0 into D3.1, then 2.0 mm to L1.1, F.Cu only.
  - **GND.**
    - Pin 6 → C1.2 runs between C1's pads.
    - C1.2 → anode is a 2.5 mm run under D3's body.
    - The anode via moved to (135.3, 129.3).
    - Three tented GND vias were added at x 138.65, y 130.6 / 131.4 / 132.2.
    - The via at (131.25, 121.25) was removed.
    - TP8's pad is solid to GND.
  - **Silk.** TP5 and TP8 references sit above their pads; the SW label is below TP5; C1's reference is between its pads; C3's reference is to its right.
  - **Results.**
    - Loop 46.1 → 30.4 mm² routed.
    - 9.8 → 6.9 nH with the B.Cu plane.
    - Diode leg 10.6 → 8.1 nH.
    - U1 decoupling 2.0 → ≈ 2.7 nH, accepted.
    - SW 47.1 → 48.1 mm², all F.Cu.
    - B.Cu slot 10.5 × 2.6 → ≈ 5.2 × 1.6 mm.
  - **E1 and the floor.** E1 is partly reversed: C1, not C3, closes the loop at the pins. About 28–30 mm² is accepted as the floor; the brief is unchanged.
  - **Revision.** The commit postdates rev D (1393414), so it belongs to rev E.
  - **Not edited.** No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit. No value, part, net or footprint change.
- Open:
  - outputs/ is stale until the next export.
  - The diode choice (E9) is still open. If the rev D short-circuit test (DEVPLAN T13) calls for a larger part, the D3 area and the C1–D3 junction are redone.
  - Rev D bring-up tests the old layout; its thermal and short-circuit results still apply to the parts.
- Verification: person-reported KiCad DRC with zone refill and parity: 0 violations, 0 unconnected, 0 parity issues at HEAD. copperhead ERC, DRC and drift run in this change.

## 2026-11-07 — Replace the 4 mm copperhead mark on the front silkscreen with the copperhead lockup and the board name, per docs/BRIEF.md Physical (2026-11-06, commit f1a6f4b: the copperhead lockup with the board name and revision directly below it as tps5430/rev-<letter>; from rev E). (1) Delete the five F.Silkscreen graphics of the old mark added in a534656: the gr_circle centred at (104, 136.5) with radius 0.92 mm and the four gr_line ticks around it. (2) Insert verbatim the 11 gr_poly items from lib/branding/rev-e-lockup-board-graphics.kicad_sexp (filled, F.SilkS, already in board coordinates, spanning x 101.5 to 115.9 and y 135.6 to 137.46). Do not rescale or move them. (3) Add one gr_text on F.SilkS: 'tps5430/rev-e', 1.0 mm size, 0.15 mm thickness, left-justified at (101.5, 138.6), spanning about x 101.5 to 113.3 and y 137.75 to 139.45. Nothing else may change: no net, footprint, copper, zone, schematic, BOM or position-file change. Update docs/LAYOUT.md silkscreen, DECISIONS (the 4 mm mark from a534656 was replaced by the lockup and label at the person's request, 2026-11-06, to match the STM32 data logger board, and the same branding was put on the public GitHub copy of rev D as tps5430/rev-d) and CHANGELOG, and update the openspec requirement from copperhead-mark-front-silk (or add one) to cite docs/BRIEF.md Physical. Do not touch releases/ or outputs/. DRC must stay at 0 violations, including silk_overlap, silk_edge_clearance and silk_over_copper. ERC, parity and drift must stay clean.

- Change: rev-e-lockup-front-silk
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, openspec/changes/copperhead-mark-front-silk/proposal.md, openspec/changes/rev-e-lockup-front-silk/specs/board/spec.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-07 — Copperhead lockup and tps5430/rev-e label replace the 4 mm mark on the front silkscreen (rev E)

- Change: rev-e-lockup-front-silk
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (F.SilkS board graphics only), docs/LAYOUT.md (Silkscreen), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/rev-e-lockup-front-silk (requirement), openspec/changes/copperhead-mark-front-silk/proposal.md (superseded note)
- Summary:
  - Per docs/BRIEF.md Physical (2026-11-06, commit f1a6f4b): the copperhead lockup with the board name and revision directly below it as `tps5430/rev-<letter>`, from rev E.
  - Deleted: the five F.SilkS graphics of the old mark from a534656 (gr_circle (104, 136.5) r 0.92 mm and four gr_line ticks, uuids c0ffeeaa-0e00-4000-8000-00000000000[1-5]).
  - Inserted verbatim: the 11 filled F.SilkS gr_poly items from lib/branding/rev-e-lockup-board-graphics.kicad_sexp (board coordinates, x 101.5–115.9, y 135.6–137.46). They were not rescaled or moved.
  - Added: gr_text `tps5430/rev-e`, 1.0 mm size, 0.15 mm thickness, left-justified at (101.5, 138.6), about x 101.5–113.3, y 137.75–139.45.
  - No net, footprint, copper, zone, schematic, BOM or position-file change. releases/ and outputs/ untouched; outputs/ does not carry the lockup until the next export.
- Open: confirm schematic parity with `kicad-cli pcb drc --schematic-parity`. copperhead run_drc does not run parity, but no footprint or symbol changed, so parity is unaffected.
- Verification: DRC clean (0 violations; no silk_overlap, silk_edge_clearance or silk_over_copper). ERC clean. Drift clean.

## 2026-11-07 — Docs only. In docs/BOM.md, mark R5 (Uniroyal 0603WAF0000T5E, LCSC C21189) VERIFIED, citing research/parts/datasheet-verification.md: Uniroyal SMD-SP-001 version 9 (Jun. 21, 2025), section 7 Ratings, 0603 jumper < 50 mohm, rated 1 A, overload 2 A, -55 to +155 C, read 2026-11-06. Remove its UNVERIFIED mark, so copperhead export bom --supplier jlcpcb includes R5. R5 must be fitted: it is the only link from J3/TP7 (/EN_EXT) to U1 EN, and the exporter currently drops it as UNVERIFIED. In docs/DEVPLAN.md T10, set the reverse-current pass limit from the DMP6023LE IDSS of -1 uA max at VDS -60 V (DS37199 rev 3-2, now in datasheet-verification.md), allowing for leakage rising with temperature. In docs/SUBSYSTEMS.md, cite the B560C RthJT 10 C/W and RthJA 50 C/W from datasheet-verification.md, which closes the thermal half of verification finding B.F6. Add DECISIONS and CHANGELOG entries. Do not edit .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean.

- Change: docs-r5-verified-t10-idss-d3-thermal
- Files: docs/BOM.md, docs/SUBSYSTEMS.md, docs/DEVPLAN.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean

## 2026-11-07 — R5 verified, T10 IDSS pass limit, B560C thermal citation (docs only)

- Change: docs-r5-verified-t10-idss-d3-thermal
- Files: docs/BOM.md (R5 row), docs/DEVPLAN.md (T10), docs/SUBSYSTEMS.md (§1 reverse-input bullet, §3 D3 bullet, §5 R5 bullet, §9 component-rise row), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/docs-r5-verified-t10-idss-d3-thermal
- Summary:
  - R5 0603WAF0000T5E (LCSC C21189): UNVERIFIED mark removed in BOM and SUBSYSTEMS §5; now VERIFIED from research/parts/datasheet-verification.md (Uniroyal SMD-SP-001 version 9, Jun. 21 2025, section 7 Ratings, read 2026-11-06): < 50 mΩ, 1 A rated, 2 A overload, −55 to +155 °C. R5 is the only link from J3/TP7 (EN_EXT) to U1 EN, so it must be in the assembly BOM; `copperhead export bom --supplier jlcpcb` dropped it while UNVERIFIED (`assembly.qty`).
  - DEVPLAN T10: pass limit set from DMP6023LE IDSS −1 µA max at VDS −60 V, VGS 0 V, TJ 25 °C (DS37199 rev 3-2). Test at room temperature; reverse Iin ≤ 1 µA, with a stated temperature allowance (typical ~2× per 10 °C, not a datasheet limit) if the board is warm; > ~10 µA investigate, ~0.2 mA stop. Closes the IDSS open item of docs-rev-e-record-corrections (P.F1).
  - SUBSYSTEMS §3/§9: B560C RθJT 10 °C/W, RθJA 50 °C/W cited from research/parts/datasheet-verification.md (Diodes DS13012 rev 19-2). Closes the thermal half of B.F6 (the compensation half closed 2026-11-07).
  - No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit. No value, part, net or footprint change.
- Open: re-run `copperhead export bom --supplier jlcpcb` so outputs/jlcpcb-bom.csv includes R5 (C21189).

## 2026-11-07 — Docs only, rev E design-record corrections (research/proposals/rev-e/rev-e-proposal.tex section 5 and section 7; research/references/buck-converter-design-guidelines/). (1) BR-B: wherever SPEC, SUBSYSTEMS or BOM bound C5's ESR at about 80 mohm, change the bound to about 40 mohm. TI SLVS632L rev L eq. 9 gives ESRmax = 1/(2 pi Cout fco), about 40 mohm for 220 uF at 18 kHz, which is the datasheet's own figure; the fitted 27 mohm still passes. Record that the TPS5430 compensation check is now done against SLVS632L eqs. 7-9: fco about 16 kHz with C5+C6 = 242 uF, within the 3-30 kHz range; ESR zero about 27 kHz. This closes verification finding B.F6's compensation half. (2) BR-C and P.Q2: the U1 junction estimate. SLVS632L 7.2.1.2.8.3 at 9 V, 3 A with RDS(on) max 230 mohm gives Ptot about 1.5 W; with the EVM's 45 C/W that is Tj about 118 C at 50 C ambient, inside 125 C by about 7 C. The record's 80-85 C used typical RDS(on) and an unverified 33 C/W. Mark it tight and to be measured. (3) P.F1: DEVPLAN T10. With J1 reversed, the expected reverse input current is Q1 drain leakage (microamps), not 0.2 mA. D2/R1 form no loop. A reading near 0.2 mA means Q1 is conducting or damaged; take the pass limit from the DMP6023LE IDSS. Correct the SUBSYSTEMS 1 reverse-input bullet. (4) B.F1: R1 current equals D2 current, max(0, Vin - Vz)/100 kohm: at 18 V at most 66 uA, none on reverse input. Remove the 120 uA and 180 uA figures. (5) R.F2: strike stale text in LAYOUT and SPEC that says the board is 55 x 35 mm, unfilled, unstitched or not manufacturable, and the superseded VIN_RAW bullet from 9af7427. (6) B.F3: point SUBSYSTEMS 2's FB1 rating at research/parts/datasheet-verification.md, not jlcpcb-sourcing.md. (7) DEVPLAN, for rev E bring-up: add a shorted-output test at 9 V and 18 V for 60 s, logging D3 temperature and the SW waveform. It decides whether D3 needs an 8 A+ part or the 5 V wiring an external fuse: at the 4-6 A current limit D3 can dissipate 2.6-4.5 W against a 5 A rating and 50 C/W (verification B.F5 and B.Q1, proposal E9). Add Vout measured at the J2 screws at 3 A (P.F2, R.Q2, E5). Add the hot-plug input ringing check through 2 m of cable, which decides whether C10 is fitted (E7). Drop the R4 item that asks to measure the SW pour area, since the pour is gone, and say T5 uses TP8 as the probe ground. (8) DECISIONS and CHANGELOG entries. Do not edit .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean.

- Change: docs-rev-e-record-corrections
- Files: docs/SPEC.md, docs/SUBSYSTEMS.md, docs/BOM.md, docs/LAYOUT.md, docs/DEVPLAN.md, openspec/changes/docs-rev-e-record-corrections/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean

## 2026-11-07 — Rev E design-record corrections, docs only (research/proposals/rev-e/rev-e-proposal.tex §5, §7; research/references/buck-converter-design-guidelines/)

- Change: docs-rev-e-record-corrections
- Files: docs/SPEC.md §3/§4/§6, docs/SUBSYSTEMS.md §1/§2/§4/§9, docs/BOM.md (C5, R1 rows; no-load roll-up), docs/LAYOUT.md (Fine, Unrouted, item 1, 9af7427 VIN_RAW bullet, via-density Open), docs/DEVPLAN.md (R4, R10, §2 C5 row, T5, T8, T10, new T13–T15), .copperhead/constraints.json (`power.output_filter_compensation`, `thermal.tj_max_C`, `pcb.copper_oz`), openspec/changes/docs-rev-e-record-corrections, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - BR-B / B.F6: C5 ESR bound changed from ~80 mΩ to ~40 mΩ (TI SLVS632L eq. 9, 220 µF at 18 kHz). Compensation check recorded against eqs. 7–9: fco ≈ 16 kHz with 242 µF (3–30 kHz range), ESR zero ≈ 27 kHz. The fitted 27 mΩ passes. This closes the compensation half of B.F6.
  - BR-C / P.Q2: U1 Tj ≈ 118 °C at 9 V / 3 A / 50 °C (Ptot ≈ 1.5 W with RDS(on) max 230 mΩ, EVM 45 °C/W). That leaves ≈ 7 °C margin, so it is marked tight and to be measured. It replaces the 80–85 °C estimate, which used typical RDS(on) and an unverified 33 °C/W.
  - P.F1: DEVPLAN T10 and SUBSYSTEMS §1. Reverse input current is Q1 drain leakage (µA), and D2/R1 form no loop. A reading near 0.2 mA means Q1 is conducting or damaged. The pass limit is the DMP6023LE IDSS, which is not yet recorded in research/.
  - B.F1: R1 current = D2 current = max(0, Vin − Vz)/100 kΩ, ≤ 66 µA at 18 V and none on reverse. The 120 µA and 180 µA figures are removed.
  - R.F2: stale 55 × 35 mm, unfilled, unstitched and not-manufacturable text struck in LAYOUT and SPEC. The superseded 9af7427 VIN_RAW bullet is struck.
  - B.F3: SUBSYSTEMS §2 FB1 rating now cites research/parts/datasheet-verification.md.
  - DEVPLAN: T13 shorted output (E9, B.F5, B.Q1), T14 Vout at the J2 screws (E5, P.F2, R.Q2), T15 hot-plug ringing deciding C10 (E7). R4 SW-pour item dropped. T5 uses TP8 as probe ground.
  - No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit. No value, part, net or footprint change.
- Open: read the DMP6023LE IDSS from DS37199 into research/parts/datasheet-verification.md before T10. Add the B560C RθJT/RθJA to that file (B.F6 D3 half).

## 2026-11-06 — Docs only: bring the design record in step with board commits c502f61 (E1), efb38ed (E2, E3), 6e5b905 (E4) and the silkscreen part of 06b32da (E14). The person chose to make these with a KiCad pcbnew script and a kicad-cli zone refill, because copperhead do cannot refill zones. Read those commit messages and the board for exact coordinates. Each change is from research/proposals/rev-e/rev-e-proposal.tex, and the layout findings are from the rev D sliding-window layout review. Record the following. (1) docs/LAYOUT.md placement rows for C3, C1, C9, R4 and D4, and the routed bullets for VIN_BUCK at C3/C1, GND C1.2 to the D3 anode, EN at C9, SW (now only pads and traces, no SW zones), +5V (the LED feed is gone; the Kelvin feed is unchanged) and /LED_A. Also the zone list (SW_copper_F.Cu and SW_copper_B.Cu deleted), the D3 anode GND vias under the body, the solid zone connections on D3.2, C1.2, C2.2, C5.2, C6.2 and D1.2, the 1.0 mm spokes on J1/J2 GND, the removed stitching vias, and the silkscreen moves (C1, U1, D4, R4, and J1 -GND now vertical). (2) DECISIONS: one entry per change with its why: E1 C3 loop ~64 mm2 with a via in series, now ~57 mm2 all on F.Cu, through pad centres; E2 SW copper 102.6 mm2 F.Cu plus 123.2 mm2 B.Cu, now 47.1 mm2 F.Cu and none on B.Cu, D3 cooled from its anode, B560C RthJT 10 C/W and RthJA 50 C/W from Diodes DS13012 rev 19-2, read 2026-11-06; E3 the spokes; E4 the B.Cu slot from the LED and Kelvin feeds. Say they were made outside copperhead do at the person's request, that they postdate rev D (1393414) so they belong to rev E, and that outputs/ is stale until the next export. (3) A CHANGELOG entry. (4) openspec requirements, each citing docs/BRIEF.md Physical: the HF input capacitor has its GND pad on U1 GND copper on F.Cu with no via in its loop; no switch-node copper beyond the pads and the PH/D3/L1 traces; the power GND pads have solid zone connections; no +5V feed other than the Kelvin sense runs on B.Cu. (5) docs/SPEC.md and docs/SUBSYSTEMS.md wherever they describe the SW pour, D3 cooling or the LED feed. Do not edit the .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/. Drift must stay clean. At this HEAD, KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues.

- Change: docs-sync-board-rev-e-e1-e4-e14
- Files: docs/LAYOUT.md, docs/SPEC.md, docs/SUBSYSTEMS.md, openspec/changes/docs-sync-board-rev-e-e1-e4-e14/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Docs synced to board commits c502f61 (E1), efb38ed (E2, E3), 6e5b905 (E4) and the silkscreen of 06b32da (E14) (rev E)

- Change: docs-sync-board-rev-e-e1-e4-e14
- Files: docs/LAYOUT.md (placement C1, C3, C9, R4/D4; Routed SW, R4 feed, GND C1.2→D3, LED_A, C3.2; new Rev E section; J1 silk; accepted-for-rev-A note), docs/SPEC.md §4, docs/SUBSYSTEMS.md §3/§9, openspec/changes/docs-sync-board-rev-e-e1-e4-e14 (four requirements), docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - The person made these commits with a pcbnew script and kicad-cli zone refill, outside copperhead do (it cannot refill zones). Coordinates read from the live board; loop and area figures are the person's (no git repository here).
  - E1: C3 to (130.55, 126.5) 270° at U1 pins 7/6, GND on F.Cu to U1.6, no via; loop ≈ 64 → ≈ 57 mm². C1 to (131.4, 130.4), C9 to (131, 133.6).
  - E2: SW_copper_F.Cu and SW_copper_B.Cu and four SW vias deleted; SW 102.6 + 123.2 mm² → 47.1 mm² F.Cu only. D3 cooled from its anode (GND vias under the body; B560C RθJT 10 °C/W, RθJA 50 °C/W, Diodes DS13012 rev 19-2).
  - E3: solid zone connections on D3.2, C1.2, C2.2, C5.2, C6.2, D1.2; 1.0 mm spokes on J1.2/J2.2.
  - E4: R4/D4 to (157.5, 123.5)/(157.5, 127) beside C6, F.Cu feed from the +5V trunk; B.Cu LED feed removed; Kelvin feed unchanged.
  - E14: C1, U1, D4, R4 references moved; J1 -GND vertical at (112.4, 115.5).
  - Postdates rev D (1393414), so rev E. No .kicad_pcb, .kicad_sch, schematic.intent.json, outputs/ or releases/ edit.
- Open: outputs/ stale until the next export. Bench: D3 rise at 18 V / 3 A (DEVPLAN T8) and SW ringing at TP5 with TP8 (T5). docs/DEVPLAN.md R4 still refers to measuring SW pour area (now moot).
- Verification: person-reported KiCad DRC with zone refill and parity clean at HEAD; copperhead ERC, DRC and drift run in this change.

## 2026-11-06 — Rev E changes E6, E7 and E10 (research/proposals/rev-e/rev-e-proposal.tex; docs/BRIEF.md Parts and Interfaces as amended in cd3d0b8): bring the schematic in line with board commits 06b32da and 373fb8f. Those commits placed and routed three new footprints with a pcbnew script and a zone refill, by the person's choice. TP8 no longer carries exclude_from_bom, the same as TP1-TP7. In schematic.intent.json add three parts. (1) C10: libId Device:C_Polarized, value 100uF, footprint Capacitor_SMD:CP_Elec_8x10, group '2. Optional input EMI filter (bridged in rev A)', pin 1 on VIN_BUCK, pin 2 on GND. It is the bulk input capacitor, Panasonic EEE-FK1H101P, not fitted (research/parts/input-bulk-capacitor.md). (2) R5: libId Device:R, value 0R, footprint Resistor_SMD:R_0603_1608Metric, group '5. Enable', pin 2 on EN, pin 1 on a new net named exactly EN_EXT, the way EN is named. (3) TP8: libId Connector:TestPoint, value TP_GND3, footprint TestPoint:TestPoint_Pad_1.0x1.0mm, group '7. Test points', pin 1 on GND. Move J3.1 and TP7.1 from EN to EN_EXT, so EN keeps U1.5, C9.1 and R5.2. Re-draft the schematic. The only board edit allowed is linking the C10, R5 and TP8 footprints to their new symbols (the footprint path/sheetfile fields, as copperhead sync would write them). Make no copper, placement, attribute or silkscreen change. Then update the docs. docs/BOM.md: a C10 row with MPN cell '—', like J3/C7/C8, because it is not fitted, with a rationale naming EEE-FK1H101P and LCSC C178548; an R5 row (0R 0603) using UniOhm 0603WAF0000T5E (LCSC C21189, JLCPCB Basic), marked UNVERIFIED until its datasheet is read; a TP8 row with MPN '—', like the other test points. docs/PINOUT.md: J3 pin 1 is now EN_EXT, through R5. docs/SPEC.md and docs/SUBSYSTEMS.md, where they describe the EN path, the input capacitors and the test points. DECISIONS and CHANGELOG (rev E). openspec: requirements for the GND probe pad beside TP5, the 0 ohm EN series position and the unfitted bulk capacitor footprint, each citing docs/BRIEF.md. Do not touch releases/ or outputs/. KiCad DRC with zone refill and schematic parity must report 0 violations, 0 unconnected and 0 parity issues. ERC and drift must stay clean.

- Change: rev-e-en-series-bulk-cap-gnd-probe
- Files: schematic.intent.json, docs/BOM.md, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/PINOUT.md, docs/SPEC.md, docs/SUBSYSTEMS.md, docs/LAYOUT.md, openspec/changes/rev-e-en-series-bulk-cap-gnd-probe/specs/board/spec.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Schematic synced to board commits 06b32da and 373fb8f: C10, R5, TP8 and net EN_EXT (rev E, E6, E7, E10)

- Change: rev-e-en-series-bulk-cap-gnd-probe
- Files: schematic.intent.json, tps5430-12-v-to-5-v-industrial-buck-converter.kicad_sch (re-drafted), tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (path/sheetname/sheetfile on C10, R5, TP8 only), docs/BOM.md, docs/PINOUT.md, docs/SPEC.md §3/§5/§7, docs/SUBSYSTEMS.md §0/§2/§3/§5/§7, docs/LAYOUT.md (placement row), .copperhead/constraints.json (`protection.ena_pin_max_V`), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/rev-e-en-series-bulk-cap-gnd-probe (three requirements)
- Summary:
  - The person placed and routed C10, R5 and TP8 in board commits 06b32da and 373fb8f (pcbnew script and zone refill). This run brings the schematic and docs in line.
  - Intent: C10 Device:C_Polarized 100uF CP_Elec_8x10 (group 2), pin 1 VIN_BUCK, pin 2 GND. R5 Device:R 0R R_0603_1608Metric (group 5), pin 1 EN_EXT, pin 2 EN. TP8 Connector:TestPoint TP_GND3 TestPoint_Pad_1.0x1.0mm (group 7), pin 1 GND. New net EN_EXT = R5.1, J3.1, TP7.1; EN = U1.5, C9.1, R5.2.
  - Board: only `(path …)`, `(sheetname …)`, `(sheetfile …)` added on C10 (/1166895f-…), R5 (/20288dde-…) and TP8 (/2e46c8ff-…), as copperhead sync writes them. No copper, placement, attribute or silkscreen change. The board already had J3.1/TP7.1/R5.1 on EN_EXT.
  - BOM: C10 MPN `—` (EEE-FK1H101P, LCSC C178548, not fitted); R5 0603WAF0000T5E, LCSC C21189, UNVERIFIED; TP8 MPN `—`.
  - Constraints: `protection.ena_pin_max_V` holds (series only, no pull-up); `power.iin_shutdown_mA` unchanged (C10 unfitted, R5 no path to GND); `components.voltage_derating` (C10 50 V); `assembly.qty` (C10 and TP8 out of BOM and CPL).
  - releases/ and outputs/ untouched; outputs/ is stale until the next export.
- Open: confirm with `kicad-cli pcb drc --refill-zones --schematic-parity` (copperhead run_drc does not run parity). Verify R5 against the UniOhm datasheet. Re-export outputs/ and the JLCPCB BOM so R5 appears and C10/TP8 do not.
- Verification: ERC clean. DRC clean (copperhead run_drc). Legibility 0 errors (1 low-utilization advisory, A3 kept). Drift clean. verify_symbols: only the accepted copperhead_power findings.

## 2026-11-06 — Rev E change E11 (research/proposals/rev-e/rev-e-proposal.tex), docs only: in docs/BOM.md set the MPN cell of J3, C7 and C8 to the (no part) marker that copperhead's BOM exporter treats as nothing to buy, so copperhead export bom --supplier jlcpcb omits all three instead of emitting J3 as a fitted line valued EN with no LCSC part and C7/C8 as lines valued DNP. Keep each row's value, footprint, and its text saying the part is not fitted (DNP) in rev A to E and why. Serves copperhead verification findings F.F1, G.F1 and S.F2 and the rev D sweep's assembly-parity items. The schematic DNP attribute itself is not possible: a previous run found the drafting engine always writes (in_bom yes) (dnp no) and refuses direct edits, so record in DECISIONS that the DNP state lives in docs/BOM.md and the board's exclude_from_pos_files, and that the schematic attribute waits on the engine. Update the docs/BOM.md sentence about which parts the generated BOM omits, and CHANGELOG (rev E). Do not touch the .kicad_sch, .kicad_pcb, schematic.intent.json, releases/ or outputs/. Drift, ERC, DRC and parity must stay clean.

- Change: bom-dnp-no-part-marker
- Files: docs/BOM.md, docs/DECISIONS.md, docs/CHANGELOG.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — DNP parts J3, C7, C8 carry the no-part MPN marker (rev E, E11)

- Change: bom-dnp-no-part-marker
- Files: docs/BOM.md (C7, C8, J3 rows; header sentences on the LCSC cell and the DNP footprints), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/bom-dnp-no-part-marker
- Summary:
  - Per research/proposals/rev-e/rev-e-proposal.tex §E11 (findings F.F1, G.F1, S.F2, rev D sweep assembly-parity), the MPN cell of J3, C7 and C8 changes from `DNP` to `—`. Before this, `copperhead export bom --supplier jlcpcb` emitted J3 as a fitted line valued EN with no LCSC part and C7/C8 as a line valued DNP.
  - Marker choice: the literal `(no part)` text is not in the workspace and the exporter source is not available. `—` is the marker TP1–TP7, H1 and H2 already carry, and those rows are already omitted from outputs/jlcpcb-bom.csv.
  - Kept: values (EN, DNP, DNP), footprints, empty LCSC cells, and each row's not-fitted (DNP in rev A to E) rationale.
  - The DNP state lives in docs/BOM.md and the board's `exclude_from_pos_files`. The schematic DNP attribute waits on the drafting engine, which always writes `(in_bom yes) (dnp no)` and refuses direct edits.
  - No .kicad_sch, .kicad_pcb, schematic.intent.json, releases/ or outputs/ edit. No net, value or footprint change.
- Open: re-run `copperhead export bom --supplier jlcpcb` and confirm outputs/jlcpcb-bom.csv no longer lists J3, C7 or C8.
- Verification: drift clean. ERC clean. DRC clean. Schematic parity unaffected (no board or schematic edit).

## 2026-11-06 — Add the copperhead mark to the front silkscreen (F.Silkscreen) as board graphics, per docs/BRIEF.md Physical (2026-11-06: the mark on the front silkscreen, about 4 mm across, in a free corner clear of pads, courtyards and other silkscreen; from rev E). The mark is /opt/copperhead-workspace/brand/mark.svg scaled to 4.0 mm overall: centre (104.0, 136.5) in the free lower-left corner (no footprint, courtyard or silkscreen in x 101 to 107, y 133.5 to 139.5; board edge at x 99.95 and y 140.05). Draw a circle of radius 0.92 mm, line width 0.4 mm, and four straight ticks of line width 0.4 mm from 0.92 mm to 1.80 mm from the centre along +x, -x, +y and -y (tick ends at (104.92,136.5)-(105.80,136.5), (103.08,136.5)-(102.20,136.5), (104,137.42)-(104,138.30), (104,135.58)-(104,134.70)). No net, footprint, schematic, BOM or copper change. Record it in docs/LAYOUT.md silkscreen, DECISIONS (the person asked for it on 2026-11-06 after rev D was released; it belongs to rev E; rev D and its public release have no mark) and CHANGELOG, and add an openspec requirement citing docs/BRIEF.md Physical. Do not touch releases/ or outputs/. DRC must stay at 0 violations (no silk_overlap, silk_edge_clearance or silk_over_copper); ERC, parity and drift must stay clean.

- Change: copperhead-mark-front-silk
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb, docs/LAYOUT.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC clean, DRC clean

## 2026-11-06 — Copperhead mark added to the front silkscreen (rev E)

- Change: copperhead-mark-front-silk
- Files: tps5430-12-v-to-5-v-industrial-buck-converter.kicad_pcb (one gr_circle and four gr_line on F.SilkS only), docs/LAYOUT.md (Silkscreen), docs/DECISIONS.md, docs/CHANGELOG.md, openspec/changes/copperhead-mark-front-silk (requirement)
- Summary:
  - Per docs/BRIEF.md Physical (2026-11-06), the copperhead mark (brand/mark.svg, ring with four crosshair ticks) is on F.SilkS, 4.0 mm overall, centred at (104, 136.5) in the free lower-left corner.
  - Circle r 0.92 mm, width 0.4 mm. Ticks 0.4 mm wide from 0.92 to 1.80 mm out along +x, −x, +y, −y. Ink extent x 102.0–106.0, y 134.5–138.5.
  - Requested by the person on 2026-11-06 after the rev D release, so it belongs to rev E. Rev D and its public release have no mark.
  - No net, footprint, track, via, zone, schematic or BOM change. releases/ and outputs/ untouched; outputs/ does not carry the mark until the next export.
- Verification: DRC clean (0 violations; no silk_overlap, silk_edge_clearance or silk_over_copper). ERC clean. Drift clean. Schematic parity unaffected (board graphics only; no footprint or symbol change).

## 2026-11-06 — Re-export the fabrication package into outputs/ with export_outputs from the current commit 1393414, which is the rev D release (releases/rev-d/, frozen from e03bac1). outputs/ was last exported at be48dd2 and is stale: since then the FB route was straightened with R3 and TP6 moved (956cc1e) and J1/J2 were turned so their wire openings face outward over the left and right board edges, with TP4 moved to (147.3, 101.8) and VIN_RAW/+5V re-routed (4698973, docs/BRIEF.md Physical 2026-11-06). Regenerate the gerbers with job file, drill, DXF outline, STEP, board.svg and schematic.svg, and regenerate outputs/BOM.csv from docs/BOM.md (no part changed, so it should be unchanged in content). Keep outputs/jlcpcb-bom.csv as it is: copperhead export bom rewrote it at 1393414 and it is byte-identical to releases/rev-d/bom/jlcpcb-bom.csv. Rewrite outputs/README.md for this export: say it matches rev D (commit 1393414, board unchanged since e03bac1), list what was regenerated, remove text that is now wrong (the old TP4 position (147.5, 109.08), the 'next revision after rev A' framing, 'workspace is not a git repository'), record that KiCad DRC with zone refill and schematic parity reports 0 violations, 0 unconnected and 0 parity issues at e03bac1, that terminal orientation was verified from footprint rotation and kicad-cli 3D renders, and that the files to send to a fab are the frozen ones in releases/rev-d/ (which also holds the placement file); keep the fab notes (1.3 mm finished J1/J2 holes, 2 oz copper on the order form, tented vias) and add that JLCPCB's placement preview must show J1's openings toward the left edge and J2's toward the right. Do not modify the board, schematic, schematic.intent.json or anything in releases/. Every export must succeed. Drift must stay clean.

- Change: outputs-reexport-rev-d
- Files: outputs/, outputs/README.md, docs/CHANGELOG.md, docs/DECISIONS.md
- Verification: ERC not required, DRC clean

## 2026-11-06 — Fabrication package re-exported at rev D (commit 1393414)

- Change: outputs-reexport-rev-d
- Files: outputs/gerbers/* (with gbrjob), outputs/*.drl, outputs/outline.dxf, outputs/board.step, outputs/board.svg, outputs/schematic.svg, outputs/README.md, .copperhead/renders/, docs/DECISIONS.md, docs/CHANGELOG.md
- Summary:
  - outputs/ was last exported at be48dd2 and was stale. It predated board commits 956cc1e (FB route straightened, R3 and TP6 moved) and 4698973 (J1/J2 turned so the wire openings face outward, TP4 to (147.3, 101.8), VIN_RAW/+5V re-routed).
  - Pre-export `run_drc` was clean. `export_outputs` re-ran at commit 1393414, the rev D release commit (releases/rev-d/, frozen from e03bac1; the board is unchanged since e03bac1; hashes as given by the person). All six artifacts succeeded: gerbers with gbrjob, drill, outline.dxf, board.step, board.svg, schematic.svg. `export_svg` sch and pcb renders regenerated.
  - Board orientation re-read from the board file before writing the README: J1 `(at 105.6 110.42 -90)` (−90 = 270°), J2 `(at 154.4 109.08 90)`, TP4 `(at 147.3 101.8)`.
  - outputs/BOM.csv: compared row by row with docs/BOM.md. No part changed, every row still matches (22 fitted placements, C9 fitted with LCSC C1588), so the file was not rewritten; its content is unchanged.
  - outputs/jlcpcb-bom.csv untouched: written by `copperhead export bom` at 1393414, byte-identical to releases/rev-d/bom/jlcpcb-bom.csv (person-reported).
  - outputs/README.md rewritten for rev D. It now states that the package matches rev D (1393414, board unchanged since e03bac1), lists the regenerated artifacts and the 956cc1e/4698973 changes it carries, and records KiCad DRC with zone refill and schematic parity at 0 violations, 0 unconnected, 0 parity issues at e03bac1 (person-reported). It records terminal orientation as verified from the footprint rotation (re-read in this run) and kicad-cli 3D renders (person-reported), and states that the files to send to a fab are the frozen ones in releases/rev-d/, which also hold the placement file. Fab notes kept (1.3 mm finished J1/J2 holes, 2 oz on the order form, tented vias). New pre-order check: JLCPCB's placement preview must show J1's openings toward the left edge and J2's toward the right.
  - Removed stale README text: TP4 at (147.5, 109.08), the 'next revision after rev A' framing, and 'workspace is not a git repository'.
  - PNG renders not produced: no copperhead tool writes them.
  - No .kicad_pcb, .kicad_sch, schematic.intent.json or releases/ edit. No net, value or footprint change.
- Verification: DRC clean before export. Drift check run. ERC not re-run: no schematic edit.

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
