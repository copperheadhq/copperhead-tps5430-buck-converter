# 3D models

Exported with KiCad 10.0.6 from `hardware/` at rev E (release commit `c50e737`). Component bodies are generic KiCad library models, except F1 and L1, which use the EasyEDA models in `hardware/lib/3d/`. They are not the exact manufacturer parts. Use them for fit, not for tolerance.

| File | For |
| --- | --- |
| `tps5430-buck.step` | full model: tracks, pads, zones, silkscreen and mask |
| `tps5430-buck-mechanical.step` | board and component bodies only; give this one to an enclosure designer |
| `tps5430-buck.glb` | the full model for a web or Blender viewer |
| `tps5430-buck.stl` | the assembled board as one mesh, with every footprint populated; GitHub previews it in 3D |
| `tps5430-buck-board-only.stl` | bare outline with holes, for a fit-check print |

The parts that are not fitted (C7, C8, C10 and J3) are left out of the STEP and GLB models, as they would be on an assembled board; their pads remain. `tps5430-buck.stl` and the renders in `images/` include them, to show every footprint.

J1 and J2 are horizontal-entry terminals: J1's wire openings face the left edge and J2's the right edge.
