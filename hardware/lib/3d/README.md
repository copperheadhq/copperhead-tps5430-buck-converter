# 3D models

Models for footprints whose KiCad library entry has no 3D model. Fetched
2026-11-06 with `easyeda2kicad --lcsc_id <id> --full` from EasyEDA/LCSC; only
the model files are kept.

| Ref | Part | LCSC | Model | Placement on the KiCad footprint |
| --- | ---- | ---- | ----- | -------------------------------- |
| L1 | Bourns SRP1265A-150M | C189912 | `l1/easyeda_l1.3dshapes/IND-SMD_L12.8-W13.5-H6.5.step` | offset 0, rotation 0 on `Inductor_SMD:L_Bourns_SRP1245A` |
| F1 | Littelfuse 0154004.DRT | C206915 | `f1/easyeda_f1.3dshapes/FUSE-SMD_L9.7-W5.0-H3.8.step` | offset 0, rotation 0 on `Fuse:Fuseholder_Littelfuse_Nano2_154x` |

Alignment was checked by rendering the board with `kicad-cli pcb render`. These
are vendor-community models for visual and enclosure checks, not dimensional
references; the datasheets in `research/parts/` are.
