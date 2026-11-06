# Specification: TPS5430 12 V to 5 V industrial buck converter

Source of truth for requirements, derived from `docs/BRIEF.md`. Items the brief
does not state are proposed defaults marked **ASSUMED**. Every budget below is
also recorded in `.copperhead/constraints.json` (key in brackets).

## 1. What the device is
A compact, non-isolated DC/DC buck converter for an industrial control cabinet.
It takes a nominal 12 VDC SELV supply and delivers a regulated 5 VDC at up to
3 A continuous. It is built around the TI TPS5430 (500 kHz, 3 A, 1.221 V reference,
no power-good output) with an external Schottky catch diode. Input and output
share a common ground. It is a fab-assembled two-layer PCB with screw terminals
at opposite edges.

## 2. Electrical budgets

| Parameter | Min | Typ | Max | Source / note | Key |
|---|---|---|---|---|---|
| Operating input voltage (at J1, after protection) | 9 V | 12 V | 18 V | Brief, Power | `power.vin_operating_V` |
| Input transient survival (TVS clamp) | – | – | ~29 V clamp (SMBJ18A at Ipp) | Brief. Must stay below TPS5430 VIN abs max (40 V) and 36 V operating limit; input caps rated 50 V | `power.vin_transient_clamp_V` |
| Reverse input voltage, no damage | -18 V | – | – | Brief, Must not | `protection.reverse_polarity_V` |
| Output voltage setpoint | – | 4.99 V | – | 1.221 V × (1 + 10.0 k/3.24 k) | `power.vout_V` |
| Output voltage tolerance (line, load, temp, divider) | 4.85 V | 5.0 V | 5.15 V | **ASSUMED** ±3 %. Stack (change review-docs-fab-notes, 2026-11-06): datasheet Vref 1.196–1.245 V over temperature (TI SLVS632L rev L, research/parts/datasheet-verification.md) with the fitted 0.1 % R2/R3 (ratio 4.0802–4.0926) gives **≈ 4.880–5.095 V, which meets the budget** with ≈ 30 mV margin low and ≈ 55 mV high; ripple (≈ ±7 mV) fits inside that margin | `power.vout_V` |
| Output current, continuous | 0 A | – | 3 A | Brief, at 0 to 50 °C, natural convection | `power.iout_A` |
| Output ripple, peak-to-peak (20 MHz BW) | – | – | 50 mVpp | **ASSUMED** | `power.vout_ripple_mVpp` |
| Efficiency at 12 V in, 5 V / 3 A out | 85 % | – | – | **ASSUMED**. Needed to keep the 1.96 A worst-case input current estimate true | `power.efficiency_min_pct` |
| Input current, worst case (9 V, 3 A out) | – | – | 2.0 A | Brief estimate 1.96 A | `power.iin_max_A` |
| Input current, no load, enabled (incl. LED, divider) | – | – | 20 mA | **ASSUMED**. TPS5430 Iq 2 mA typ, 4.4 mA max (TI SLVS632L rev L; was ~3 mA), LED ~2 mA, divider ~0.4 mA. Worst-case roll-up ≈ 7–11 mA (docs/BOM.md) | `power.iin_noload_mA` |
| Input current, disabled (EN low) | – | – | 1 mA | **ASSUMED**. LED is on 5 V, so it is off when disabled | `power.iin_shutdown_mA` |
| Inductor peak current | – | ~3.25 A | must stay **below the TPS5430 minimum current limit (4.0 A)** at 3 A out, so full load never trips the limit; and L1 Isat must exceed the **TPS5430 maximum current limit (6.0 A)**, so a current-limit event cannot saturate L1. L1 SRP1265A-150M Isat is 12.5 A (Bourns SRP1265A rev 06/24, VERIFIED) > 6.0 A: OK. 3.25 A < 4.0 A: OK. Corrected 2026-11-06 (change review-docs-fab-notes, design review P.F1); the row previously stated the criterion backwards (peak > current-limit min). Both limits are VERIFIED: IHS(OC) 4.0 A min, 5.0 A typ, 6.0 A max (TI SLVS632L rev L, p. 5; research/parts/datasheet-verification.md, max rechecked 2026-11-06) | Derived, ripple ~0.48 A p-p at 18 V | `power.inductor_peak_A` |
| Power-indicator LED current | 1 mA | 2 mA | 3 mA | **ASSUMED** | `power.led_current_mA` |

Switching frequency is fixed by the TPS5430 at 500 kHz (internal oscillator).

## 3. Protection
- **Reverse polarity:** DMP6023LE-13 P-MOSFET in the high-side input path. No damage for any input from -18 V to +18 V.
- **MOSFET gate clamp:** zener plus series resistor holds |Vgs| ≤ 20 V (the DMP6023LE rating, Diodes DS37199 rev 3-2, VERIFIED) even while the TVS clamps near 29 V. **ASSUMED** design target |Vgs| ≤ 15 V in steady state. D2 BZT52C12 Vz is 11.4–12.7 V at 5 mA (Diodes DS18004 rev 38-2, VERIFIED 2026-11-06), so |Vgs| ≤ 12.7 V. [`protection.pfet_vgs_max_V`]
- **TVS:** SMBJ18A-13-F across the protected input. Standoff 18 V, so it does not conduct within the operating range. Clamp voltage stays below every downstream part rating.
- **Fuse:** Littelfuse 0154004.DRT 4 A slow-blow in an OMNI-BLOK SMD holder (decided 2026-11-05, docs/BRIEF.md Parts, research/parts/input-fuse.md). Its 75 % continuous limit is about 3.0 A at 25 °C and ≈ 2.94 A at 50 °C (4 A × 0.75 × ~0.98 rerating; cold resistance 18.6 mΩ, melting I²t 34.4 A²s; Littelfuse 154/454 series, VERIFIED 2026-11-06), against a 1.96 A worst-case load (~49 % of rating). The FB1 0 Ω link and the input path upstream of the converter must be rated ≥ 4 A so the fuse opens first. [`protection.fuse_rating_A`]
- **EN:** pulled high by default by the TPS5430 internal ENA pull-up. No external pull-up is fitted, on purpose: the ENA abs max is 7 V (TI SLVS632L rev L, VERIFIED 2026-11-06; internal 1.5 MΩ pull-up), so it must never be tied to VIN (9–29 V). An external dry contact or open-drain to GND disables the converter. It must never be driven high. [`protection.ena_pin_max_V`]
- **Output filter / compensation:** the TPS5430 is internally compensated, so L1 15 µH, C5 220 µF hybrid (ESR ≤ ~80 mΩ) and C6 22 µF must not change without re-checking the datasheet compensation guidance. [`power.output_filter_compensation`]
- **Feedback divider:** R2 10.0 kΩ and R3 3.24 kΩ.
  - The architecture first set them at 0.5 % (the brief says 1 %). The fitted parts are 0.1 %, 25 ppm/°C (RT0603BRD0710KL, RT0603BRD073K24L; Yageo RT series V.17, VERIFIED).
  - Tolerance stack (change review-docs-fab-notes, 2026-11-06, design review B.F1): datasheet Vref 1.196–1.245 V over temperature (TI SLVS632L rev L) and divider ratio 4.0802–4.0926 give Vout ≈ 4.880–5.095 V. **This meets the ±3 % budget (4.85–5.15 V).**
  - Margin is ≈ 30 mV on the low side and ≈ 55 mV on the high side. The datasheet Vref range already covers line, load and temperature at the reference, so the only remaining terms are ripple (≈ ±7 mV) and layout IR drop to J2.
  - With 1 % parts the same stack would be ≈ 4.814–5.165 V, which fails on both sides. That is why the brief's 1 % is not used.
  - See docs/SUBSYSTEMS.md §4.
- Voltage ratings, minimum 1.5× working voltage **ASSUMED**: input caps 50 V (≥ 29 V clamp), output caps 25 V (≥ 7.7 V; C5 EEH-ZA1E221P and C6 GRM32ER71E226KE15L, raised from 10 V by the JLCPCB substitutions in research/parts/jlcpcb-sourcing.md, 2026-11-06), catch diode 60 V (≥ 29 V). [`components.voltage_derating`]

## 4. Thermal
- Ambient 0 to 50 °C, natural convection, inside a control cabinet. [`env.ambient_C`]
- TPS5430 junction temperature ≤ 125 °C at 50 °C ambient, 3 A, worst-case input. **ASSUMED** margin below the 150 °C abs max. [`thermal.tj_max_C`]
- Catch diode, inductor, screw terminals, and fuse temperature rise ≤ 40 °C above ambient at full load. **ASSUMED** [`thermal.component_rise_C`]. The B560C dissipates roughly 1.2 to 1.5 W at 3 A (Vf ≈ 0.7 V × (1 − D)), so its copper area is important.
- Thermal vias and pours under the TPS5430 exposed pad (must connect to GND). Measure all of the above during validation.

## 5. Interfaces
| Ref (planned) | Function | Detail |
|---|---|---|
| J1 | Input | Phoenix MKDS 1,5/2-5.08 (1715721; IEC 17.5 A 250 V, UL 300 V 10–15 A, Phoenix datasheet 12/21/2021, VERIFIED 2026-11-06), 2-pos 5.08 mm screw terminal: VIN, GND. 12 V SELV DC. Replaces the brief's Würth 691137710002 because no Würth footprint is installed (see docs/BOM.md, DECISIONS). Fab: finished plated hole ≥ 1.3 mm for the 0.9 mm square pins (outputs/README.md) |
| J2 | Output | Phoenix MKDS 1,5/2-5.08 (1715721, same ratings as J1, VERIFIED 2026-11-06), 2-pos 5.08 mm screw terminal: +5V, GND. Same ≥ 1.3 mm finished-hole fab note |
| Test points | Debug | Protected VIN, +5V, GND, switch node (SW), feedback (FB), enable (EN) |
| EN pads | Control | Open = run. Short to GND = off |
| LED | Indicator | Green. Shows 5 V present. **Not** power-good (the TPS5430 has none) |

Terminal current rating must cover 3 A at 50 °C with margin. **ASSUMED** ≥ 1.5× (4.5 A). [`connector.current_A`]

## 6. Mechanical and layout
- 2-layer PCB, targeted about 55 × 35 mm; final outline 60 × 40 mm, (100,100)–(160,140), set in layout on 2026-11-05 to fit the mounting holes (limit ≤ 60 × 40 mm, agreed with the person). [`mech.board_mm`]
- Two M3 mounting holes on opposite corners, with copper and component keepout of ≥ 3.5 mm radius around each hole centre **ASSUMED**. Fitted as H1 (103,103) top-left and H2 (157,137) bottom-right, MountingHole_3.2mm_M3 NPTH, no net.
- Screw terminals at opposite board edges, each with its wire openings facing outward over its own board edge and nothing in front of the clamps (docs/BRIEF.md Physical, 2026-11-06). Since board commit 4698973 (rev D): J1 at (105.6, 110.42) 270° on the left edge and J2 at (154.4, 109.08) 90° on the right edge. The horizontal MKDS wire entry is footprint local +y, and each clamp face sits about 0.9 mm inside its edge. In rev A to C both faced inward. Components on the top side where practical.
- Copper: 2 oz preferred. With 1 oz, widen power paths and pour both layers with stitching vias. Power-path trace/pour sized for 3 A at ≤ 20 °C rise **ASSUMED**. [`pcb.copper_oz`] **Set 2026-11-05:** the board stackup is 2 oz (70 µm) on F.Cu and B.Cu with a 1.44 mm FR4 core (1.6 mm overall). GND zone outlines exist on both layers (unfilled until KiCad's filler runs; stitching vias come in a later change). U1 pad 9 (all 11 pads: exposed pad, pad copper and 8 thermal-via pads) connects solid to GND through pad-level `zone_connect 2` overrides, set 2026-11-06 in change u1-pad9-solid-zone-connect. The earlier project DRC rule did not take effect and was removed (`thermal.tj_max_C`).
- Tight switching loop: input bypass cap (100 nF + 10 µF), TPS5430 VIN/PH, and catch diode adjacent. Feedback divider near the FB pin, routed from the output cap, away from SW.
- Clearance: maximum working voltage is 18 V (29 V transient). Minimum conductor spacing is 0.2 mm **ASSUMED**, which exceeds the IPC-2221 B2 requirement for ≤ 30 V and is within JLCPCB/PCBWay standard capability. [`pcb.min_clearance_mm`]
- Silkscreen: polarity (+/−) and function labels on both terminals, and test point names.

## 7. Assembly and sourcing
- Fab-assembled PCBA (JLCPCB or PCBWay), 5 to 10 prototype boards. [`assembly.qty`]
- All parts must be placeable by the assembler. Passives 0603 or larger **ASSUMED**. The screw terminals and fuse holder may need to be hand-soldered or sent as consigned/extended parts.
- Confirm stock and lifecycle before ordering. This was not checked when the brief was written.
- Optional input EMI filter footprints (LC or π). Rev A ships with the filter **bridged** (0 Ω / jumper fitted).

## 8. Out of scope / must not
- Not safety-isolated. Must not be connected to hazardous voltage. SELV input only. [`safety.selv_only`]
- No galvanic isolation, remote sensing, current telemetry, digital output control, or power-good output.
- No specific EMC or surge standard is targeted for rev A. If IEC 61000-6-2 or similar applies later, expect an SMCJ/5KP-class TVS and a fitted filter.

## 9. Open questions
1. ~~**Fuse rating.**~~ **DECIDED 2026-11-05:** F1 is 0154004.DRT (4 A slow-blow, same OMNI-BLOK holder and footprint), per docs/BRIEF.md Parts and research/parts/input-fuse.md. The 3 A part ran at about 87 % of rating at 1.96 A worst-case, above the 75 % continuous guideline. The 4 A part runs at about 49 %. The 50 °C rerating is about 98 %, so ≈ 2.94 A continuous (Littelfuse 154/454 series, VERIFIED 2026-11-06, research/parts/datasheet-verification.md). Closed.
2. ~~Final board dimensions and mounting-hole coordinates are set in layout.~~ **SET 2026-11-05 (layout draft):** the outline is 60 × 40 mm, (100,100)–(160,140). It grew from the 55 × 35 mm target to free a corner beside L1, inside the person's 60 × 40 mm limit. H1 is at (103,103) and H2 at (157,137), M3 NPTH with no net, each with an r ≥ 3.5 mm copper and component keepout on both layers. See docs/LAYOUT.md.
3. Confirm the SRP1265A-150M saturation and RMS ratings cover ~3.25 A peak at 50 °C ambient, with margin.
4. Confirm the efficiency (≥ 85 %) and B560C thermal assumptions on the bench.
