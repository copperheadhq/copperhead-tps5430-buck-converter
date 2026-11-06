# TPS5430 12 V to 5 V industrial buck converter

## Purpose
A compact, non-isolated DC buck converter for an industrial installation, accepting nominal 12 VDC and providing regulated 5 VDC at up to 3 A.

## Parts
- TPS5430DDAR 3 A, 500 kHz buck regulator (Texas Instruments)
- SRP1265A-150M 15 µH shielded power inductor (Bourns)
- B560C-13-F 5 A, 60 V Schottky catch diode (Diodes Incorporated)
- EEH-ZA1A221P 220 µF, 10 V hybrid output capacitor (Panasonic)
- Two GRM32ER71H106KA12L 10 µF, 50 V X7R input capacitors (Murata)
- GRM32ER71A226KE20L 22 µF, 10 V X7R output bypass capacitor (Murata)
- SMBJ18A-13-F input transient-voltage suppressor (Diodes Incorporated)
- DMP6023LE-13 P-channel MOSFET for input reverse-polarity protection (Diodes Incorporated)
- Two Phoenix Contact MKDS 1,5/2-5.08 (1715721) two-position, 5.08 mm screw terminals. These replace the Würth 691137710002 first named here, because KiCad has no footprint for the Würth part; pitch and position count are the same (part selection, 2026-11-05).
- 10.0 kΩ upper and 3.24 kΩ lower 1% feedback resistors, setting approximately 5.0 V (1.221 V reference × (1 + 10.0/3.24) ≈ 4.99 V, per the TPS5430 datasheet)
- 10 nF bootstrap capacitor, 100 nF local input bypass capacitor, and labeled input/output test points
- Green LED with series resistor on the 5 V output as a power indicator. It shows that 5 V is present; it is not a power-good signal, because the TPS5430 has no power-good output.
- Littelfuse 0154004.DRT replaceable 4 A slow-blow fuse in an OMNI-BLOK surface-mount holder, as the input fuse (see `research/parts/input-fuse.md`). 4 A rather than 3 A for margin over the 1.96 A worst-case input current at 50 °C; decided 2026-11-05.
- Optional footprints for an input EMI filter (LC or π), fitted or bridged at assembly; rev A ships with the filter bridged

## Power
- Nominal input: 12 VDC
- Intended operating input range: 9 to 18 VDC
- Output: 5 VDC, up to 3 A continuous with adequate airflow and copper area, at 0 to 50 °C ambient
- Non-isolated topology with common input and output ground
- Reverse-polarity MOSFET and SMBJ18A TVS protection at the input
- External input fuse recommended in the installation; board includes a replaceable 4 A fuse

## Interfaces
- 2-pin screw terminal for VIN and GND, wired to the installation's 12 V SELV DC supply
- 2-pin screw terminal for 5 V and GND, wired to the 5 V load
- Test points for protected VIN, 5 V, GND, switch node, feedback, and enable
- TPS5430 enable pulled high by default, with pads allowing an external contact or open-drain signal to disable the converter

## Physical
- Two-layer PCB, targeted around 55 by 35 mm; grown to 60 by 40 mm in layout so the two M3 holes have free corners beside the terminals and the 12.5 mm inductor (2026-11-05)
- Two M3 mounting holes on opposite corners with copper and component keepouts
- Screw terminals at opposite board edges, each with its wire openings facing outward over its board edge, so the field wires enter from outside the board and nothing sits in front of the clamps (2026-11-06; rev A to C had both terminals facing inward)
- Components on the top side where practical
- Large copper pours and thermal vias beneath the TPS5430 exposed pad
- Tight high-current switching loop, with the catch diode and input bypass capacitor adjacent to the regulator
- Feedback divider kept away from the switch node and routed from the output capacitor
- Use 2 oz copper if available; otherwise widen power paths and use pours on both layers with stitching vias
- Maintain creepage and clearance suitable for the board's maximum 18 V operating input
- Clear polarity and terminal labels on silkscreen
- Assembly: fab-assembled PCBA (for example JLCPCB or PCBWay), prototype run of about 5 to 10 boards. Footprints and parts should be ones that assembler can place.

## Environment
- Ambient: 0 to 50 °C, natural convection, in an industrial installation (control cabinet)
- EMC: no specific standard is targeted for rev A. The SMBJ18A and the optional filter footprints are there so the board can be pre-scanned and improved later without a new layout.
- Size the design for 3 A continuous operation while checking regulator, diode, inductor, connector, and copper temperatures during validation

## Must not
- Must not be connected directly to hazardous voltage; it is not safety-isolated.
- Must not be damaged by reverse-polarity input within the 9 to 18 V range.
- Must not provide galvanic isolation, remote sensing, current telemetry, or a digitally controlled output; these are out of scope.

## Sources
- [Texas Instruments TPS5430 product page and datasheet](https://www.ti.com/product/TPS5430), for the 5.5–36 V input range, 3 A rating, 500 kHz operation, 1.221 V reference, feedback network, the absence of a power-good pin, and layout guidance
- [Bourns SRP1265A series](https://www.bourns.com/products/magnetic-products/power-inductors-smd-high-current-shielded/srp1265a-series), for the shielded 15 µH power inductor family
- [Diodes Incorporated B560C product page](https://www.diodes.com/part/view/B560C), for the Schottky catch diode ratings

## Open questions

## Assumptions
- C9, the 1 nF EN-to-GND capacitor, is fitted on rev A (decided 2026-11-06 after the copperhead design review, finding P.F3/S.F1): the TPS5430 enable is held high only by its internal 1.5 MΩ pull-up and is wired off-board to a contact, so without it switching noise or a field-wire transient could turn the converter off. A series resistor for ESD at J3 is not added; if the EN wiring runs outside the cabinet, add one in rev B.
- The 12 V source is SELV DC and shares ground with the 5 V load. If not, the board is unsuitable as designed.
- Input wiring may be noisy, so reverse-polarity and TVS protection are included.
- The DMP6023LE gate is clamped (zener plus series resistor) so its gate-source voltage stays within the ±20 V rating when the TVS clamps a surge (SMBJ18A clamps near 29 V). Without this, a surge could destroy the reverse-polarity FET.
- Natural convection is available. Above 50 °C ambient, or in a sealed hot enclosure, full-load operation will need derating.
- No industrial EMC or surge standard (such as IEC 61000-6-2) applies to rev A. If one does, expect a higher-energy TVS (SMCJ or 5KP class) and a fitted input filter.
- Parts were selected from known, commonly distributed families. Live distributor stock and lifecycle status could not be checked here and should be confirmed before ordering.
