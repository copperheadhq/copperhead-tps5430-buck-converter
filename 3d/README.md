# 3D models

Exported with KiCad 10.0.6 from `hardware/` at rev D (`1393414`). Component bodies are generic KiCad library models, except F1 and L1, which use the EasyEDA models in `hardware/lib/3d/`. They are not the exact manufacturer parts. Use them for fit, not for tolerance.

| File | For |
| --- | --- |
| `tps5430-buck.step` | full model: tracks, pads, zones, silkscreen and mask |
| `tps5430-buck-mechanical.step` | board and component bodies only; give this one to an enclosure designer |
| `tps5430-buck.glb` | the full model for a web or Blender viewer |
| `tps5430-buck-board-only.stl` | bare outline with holes, for a fit-check print |

J1 and J2 are horizontal-entry terminals: J1's wire openings face the left edge and J2's the right edge.
