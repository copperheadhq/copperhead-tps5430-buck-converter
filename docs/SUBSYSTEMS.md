# Subsystems: TPS5430 12 V to 5 V industrial buck converter

Architecture derived from `docs/SPEC.md` (budgets) and `docs/BRIEF.md` (parts).
Each value carries a one-line rationale. Values marked **VERIFY** come from memory
of the manufacturer datasheet and must be confirmed against the datasheet in the
parts stage before the BOM is frozen. Planned refdes and net names below are the
names schematic capture must use.

## 0. Block diagram (prose)

The signal and power flow runs in a single line, left to right:

```
J1 (VIN_RAW, GND)
 → F1 4 A slow-blow                        [VIN_FUSED]
 → Q1 DMP6023LE P-FET, reverse-polarity    [VIN_PROT]   (gate: D2 BZT52C12 zener, Vz 11.4–12.7 V, + R1 100 kΩ)
 → D1 SMBJ18A TVS to GND, TP1 "VIN"
 → optional EMI filter (C/L/C footprints, L position bridged in rev A)  [VIN_BUCK]
 → C1 10 µF/50 V at U1 VIN/GND pins, C3 100 nF/50 V at the VIN_BUCK hop, C2 10 µF/50 V bulk (+ C10 100 µF/50 V bulk footprint from rev E, not fitted)
 → U1 TPS5430 (PH = SW, BOOT via C4 10 nF, ENA = EN, VSENSE = FB)
 → L1 15 µH, D3 B560C catch diode PH→GND
 → C5 220 µF hybrid + C6 22 µF X7R          [+5V]
 → J2 (+5V, GND), TP2 "5V", LED indicator D4 + R4
Feedback: +5V (at C5) → R2 10.0 kΩ → FB → R3 3.24 kΩ → GND
Enable:   EN pin (internal pull-up, C9) ← R5 0 Ω (rev E) ← EN_EXT (J3 pads, TP7) to GND for off
```

Input and output share one ground (GND). There is one power domain at the output (+5V)
and no secondary rails.

### Planned nets
| Net | Between | Note |
|---|---|---|
| VIN_RAW | J1.1 → F1 | Unprotected input; may be reversed |
| VIN_FUSED | F1 → Q1 drain | |
| VIN_PROT | Q1 source → D1, TP1, filter input | Protected input |
| VIN_BUCK | Filter output → C1–C3, U1 VIN | Equal to VIN_PROT while the filter is bridged |
| Q1_GATE | Q1 gate, D2 anode, R1 | Gate clamp node |
| SW | U1 PH, L1, D3 cathode, C4, TP | Switch node |
| BOOT | U1 BOOT, C4 | |
| FB | U1 VSENSE, R2, R3, TP | |
| EN | U1 ENA, C9, R5.2 | ENA side of the R5 series position |
| EN_EXT | R5.1, J3.1 (EN pads), TP7 | Off-board side of R5, added in rev E |
| +5V | L1, C5, C6, R2, R4, J2.1, TP2 | |
| GND | All returns, U1 exposed pad | |

## 1. Input and protection

**Order: J1 → F1 → Q1 → D1 (TVS).** The fuse is first, so every downstream fault is fused. The TVS sits on the protected side of Q1, so a reversed supply cannot forward-bias the unidirectional SMBJ18A and blow F1. The brief's "must not be damaged by reverse polarity" therefore holds with no fuse replacement.

- **F1: Littelfuse 0154004.DRT, 4 A slow-blow, OMNI-BLOK.** This follows docs/BRIEF.md Parts (decided 2026-11-05, research/parts/input-fuse.md) and closes SPEC §9 Q1.
  - At 1.96 A worst-case input it runs at ~49 % of rating, inside the 75 % continuous guideline with margin for 50 °C rerating (about 98 % at 50 °C, Littelfuse 154/454 series, VERIFIED 2026-11-06). The 3 A 0154003.DRT ran at ~87 % and was rejected.
  - It uses the same holder and footprint as the 3 A part, so nets and layout do not change.
  - Everything in series upstream of the converter (J1, Q1, FB1, input copper) must carry ≥ 4 A so the fuse opens first.
  - The fuse dissipates ≈ 0.07 W at 1.96 A (cold resistance 18.6 mΩ, melting I²t 34.4 A²s; Littelfuse 154/454 series, VERIFIED 2026-11-06). The 50 °C rerating is about 98 %, so 4 A × 0.75 × 0.98 ≈ 2.94 A continuous, above the 1.96 A worst case.
- **Q1: DMP6023LE-13 P-FET, high side.** Drain is on VIN_FUSED and source on VIN_PROT. At power-up the body diode conducts first, then the channel enhances.
  - Vds 60 V, which exceeds the 29 V TVS clamp by 2.07×.
  - R_DS(on) is 28 mΩ max at Vgs −10 V (Diodes DS37199 rev 3-2, VERIFIED 2026-11-06). At 2 A that is ≈ 0.11 W, with no heatsinking needed. IDM −50 A covers the SMBJ18A Ipp of 20.5 A.
- **Gate clamp: D2 BZT52C12 zener (Vz 11.4–12.7 V at 5 mA, SOD-123) source→gate, and R1 100 kΩ gate→GND.**
  - At 9 V in: |Vgs| = 9 V. The zener is off, and that is enough enhancement.
  - D2 Vz is 11.4–12.7 V at 5 mA (Diodes DS18004 rev 38-2, p. 2, VERIFIED 2026-11-06; was taken as 12 V ±5 %).
  - At 18 V in: the gate-source voltage magnitude is at most 12.7 V. This meets the ≤ 15 V target with zener current at most (18−11.4)/100k ≈ 66 µA.
  - During a 29 V TVS clamp: the gate-source voltage magnitude is at most 12.7 V, which is under the 20 V absolute limit, with zener current at most ≈ 176 µA.
  - Reverse input: the channel is off and Q1's body diode is reverse-biased, so VIN_PROT stays near 0 V and D2/R1 form no loop: no current flows in D2 or R1. The only reverse input current is Q1 drain leakage (µA class, DMP6023LE IDSS −1 µA max at VDS −60 V, VGS 0 V, TJ 25 °C, Diodes DS37199 rev 3-2, research/parts/datasheet-verification.md; corrected 2026-11-07, finding P.F1).
  - R1's current always equals D2's current, max(0, Vin − Vz)/100 kΩ (finding B.F1): none below Vz and none on reverse input, at most 66 µA at 18 V, ≈ 176 µA only during a 29 V clamp.
  - 100 kΩ is chosen to keep the clamp's standing current well inside the 1 mA shutdown budget.
- **D1: SMBJ18A-13-F.**
  - VRWM 18 V, so it does not conduct across the 9–18 V range. Leakage at VRWM is 5 µA max (Diodes DS19002 rev 20-2, pp. 1, 3, VERIFIED 2026-11-06; was assumed ≤ 1 µA). The shutdown roll-up is ≈ 0.121 mA worst case, under the 1 mA limit (§9).
  - Breakdown is ≥ 20 V. The clamp is ~29 V at Ipp ≈ 20 A, which is below U1's 36 V operating limit and 40 V absolute maximum.
  - Because the TVS is downstream of Q1, Q1 carries the surge current in its channel. Q1's pulsed drain current, IDM −50 A (Diodes DS37199 rev 3-2, VERIFIED 2026-11-06), exceeds the SMBJ18A Ipp of 20.5 A (Diodes DS19002 rev 20-2, VERIFIED).
- **J1: Phoenix MKDS 1,5/2-5.08 (1715721; replaces the brief's Würth 691137710002 because no Würth footprint is installed, see docs/BOM.md), 5.08 mm, 2-pos.** It carries ≤ 2.0 A. Its rating must be ≥ 4.5 A at 50 °C. Phoenix datasheet 12/21/2021 (VERIFIED 2026-11-06): IEC 17.5 A, 250 V (III/3); UL 300 V, 10–15 A. The datasheet read gives no 50 °C derating figure, so the terminal rise is a bench check (DEVPLAN T8).

## 2. Optional input EMI filter (bridged in rev A)

- **Topology:** π footprint between VIN_PROT and VIN_BUCK. It is a shunt cap (DNP, 1210 footprint, 50 V), then a series inductor position, then a shunt cap (DNP, 1210, 50 V).
- **Rev A fit:** the series position is fitted with a 0 Ω link rated ≥ 4 A (2512 high-current jumper, Stackpole HCJ2512ZT0R00, 63.2 A, ≤ 0.5 mΩ per Stackpole SEI-HCJ rev 1/22/2025, pp. 1, 3, VERIFIED in research/parts/datasheet-verification.md; the substitution itself comes from research/parts/jlcpcb-sourcing.md; it replaces the RCL1225, which is a 1225 wide-terminal package that does not fit R_2512), and both shunt caps are DNP. The 4 A rating covers the 2.0 A input with margin and also covers the 4 A F1 rating (0154004.DRT), so the fuse opens before the link does.
- **Footprint:** the series position must also accept a shielded inductor or ferrite bead of ≥ 4 A (Isat and Irms), so it still outlasts the 4 A F1. Any LC fitted later needs damping, such as a bulk electrolytic or an R-C, because a buck converter presents negative input impedance and an undamped LC can oscillate.
- **Placement:** the filter goes on the protected side, so its parts only ever see the clamped voltage (≤ 29 V).
- **C10 bulk input capacitor footprint (rev E, proposal E7; docs/BRIEF.md Parts):** Panasonic EEE-FK1H101P, 100 µF ±20 %, 50 V, size F 8 × 10.2 mm, LCSC C178548 (research/parts/input-bulk-capacitor.md), on VIN_BUCK to GND, CP_Elec_8x10 land as C5. **Not fitted** until the rev D hot-plug test shows whether the field-wired input rings. The TPS5430 datasheet §7.3 recommends about 100 µF bulk when the source is more than a few inches away; its ESR also provides the damping an LC fitted at FB1 would need. 50 V clears the 29 V clamp (`components.voltage_derating`). Unfitted, it adds nothing to the shutdown budget; fitted, its DC leakage (electrolytic, µA class, to be read from the datasheet before fitting) must be added to `power.iin_shutdown_mA`.

## 3. Buck power stage

- **U1: TPS5430DDAR** (HSOP-8 PowerPAD).
  - VIN range is 5.5–36 V and the switching frequency is fixed at 500 kHz. The integrated high-side switch is ~110 mΩ.
  - Maximum duty cycle is ~87 %. Worst-case required duty is D ≈ (5 + 0.5)/(9 + 0.5) ≈ 0.58 at 9 V in, so there is headroom.
  - Current limit IHS(OC) is 4.0 A minimum, 5.0 A typical and 6.0 A maximum (TI SLVS632L rev L, p. 5, VERIFIED 2026-11-06; research/parts/datasheet-verification.md, max rechecked for the L1 Isat criterion). The ≈ 3.25 A inductor peak at 3 A out stays below the 4.0 A minimum, so full load never trips the limit. L1 Isat (12.5 A) exceeds the 6.0 A maximum, so a current-limit event cannot saturate L1 (`power.inductor_peak_A`).
- **Input caps: C1, C2 2 × GRM32ER71H106KA12L (10 µF, 50 V, X7R 1210), plus C3 100 nF 50 V X7R 0603.** From option A (board commit 55dc4db, rev E, 2026-11-07), C1 sits at U1 pins 7/6 and closes the commutation loop. C3 sits upstream at the VIN_BUCK hop.
  - The 50 V rating is ≥ 1.5 × 18 V and also exceeds the 29 V clamp.
  - Effective capacitance at 18 V DC bias is about 7.4 µF each from the Murata typical curve (GRM32ER71H106KA12L, updated 2013/10/01; was estimated ~5 µF), about 15 µF total.
  - Input RMS ripple is Iout·√(D(1−D)) ≈ 1.5 A at D ≈ 0.5. As laid out, the two 10 µF parts do **not** share it equally (change review-docs-fab-notes, 2026-11-06, design review R.F1/P.F2, accepted for rev A):
    - **Option A (board commit 55dc4db, 2026-11-07; research/proposals/rev-e/power-stage-loop/):** this supersedes the E1 bullet below.
      - **C1 placement and loop.** C1 is at (131.4, 127.4). Its VIN pad is on U1 pin 7 copper and its GND pad on U1 pin 6 copper, both on F.Cu. C1.2 joins the D3 anode by a 2.5 mm F.Cu run under D3's body, with no via in the C1–U1–D3 commutation loop. U1's GND pin is not in the commutation loop, so the 10 µF, not the 100 nF, has to close it.
      - **C3 placement.** C3 is at (134.7, 121.0), at the VIN_BUCK hop.
      - **VIN_BUCK route.** VIN_BUCK reaches C1 from C2 by one 1.5 mm B.Cu hop under the SW run, with one via pair at each end: (132.95/133.75, 122.3) and (133.35, 125.9/126.7). The B.Cu run at x 133.6 and its six vias are gone. On two layers VIN must cross SW once.
      - **Results.** Loop 46.1 → 30.4 mm² routed; 9.8 → 6.9 nH with the B.Cu plane; diode leg 10.6 → 8.1 nH. U1 local decoupling went from 2.0 to ≈ 2.7 nH, which was accepted. These figures are person-reported from scripts/loop_inductance.py.
    - **Rev E (E1, board commit c502f61):** C3 now sits at U1 pins 7 and 6 with its GND pad joined to U1.6 on F.Cu, no via in its loop (≈ 57 mm², was ≈ 64 mm² with a via). C1 moved to (131.4, 130.4) and is fed from C3.1 on F.Cu. The two bullets on C3 and C2 below describe rev A to D.
    - **C1** sits between U1 and D3, with a 1.5 mm F.Cu GND run to the D3 anode. It closes the primary input loop and carries most of the ripple. The Murata temperature-rise curve gives about 3–4 °C at 2 A RMS, 500 kHz, so C1 alone covers ≈ 1.5 A RMS.
    - **C3** (100 nF HF bypass) returns to GND through its via and the B.Cu link to U1's exposed-pad copper, because the F.Cu channel between the VIN_BUCK copper and the D3 SW pad is only 0.65 mm.
    - **C2** sits about 12 mm from U1 on the VIN_BUCK trunk and acts as bulk input capacitance only.
    - Rev B candidate: move D3 or C1 to open the F.Cu channel so C3 returns on F.Cu and C2 can join the hot loop (docs/LAYOUT.md Draft quality).
    - **C10** (rev E, not fitted): a 100 µF, 50 V electrolytic footprint on VIN_BUCK for bulk and damping only (§2). It is not part of the HF loop.
- **L1: Bourns SRP1265A-150M, 15 µH, shielded.**
  - Ripple ΔI = (Vin−Vout)·Vout/(Vin·L·f) = 13·5/(18·15 µ·500 k) ≈ 0.48 A p-p at 18 V. At 12 V it is 0.39 A.
  - Peak current is ≈ 3.25 A, below the TPS5430 4.0 A minimum current limit. Isat must exceed the 6.0 A maximum current limit, so a current-limit event cannot saturate L1 (corrected 2026-11-06, change review-docs-fab-notes, design review P.F1; 6.0 A is the IHS(OC) maximum per TI SLVS632L rev L, p. 5, VERIFIED in research/parts/datasheet-verification.md). VERIFIED 2026-11-06 (Bourns SRP1265A rev 06/24, p. 1): Isat 12.5 A, Irms 9 A, DCR 28 mΩ max, body 13.5 × 12.5 mm, 6.2 mm tall. The 50 °C ambient margin is still a bench check (SPEC open question 3, DEVPLAN T5/T8).
  - Copper loss is ≈ 9 A² × DCR (~30 mΩ) ≈ 0.27 W.
- **D3: B560C-13-F, 5 A, 60 V Schottky, SMC.**
  - 60 V is ≥ 2× the 29 V clamp.
  - Conduction loss is Vf·Iout·(1−D). That gives ≈ 0.5–0.7 V × 3 A × 0.7 ≈ 1.0–1.5 W at 18 V in.
  - It must sit inside the tight VIN–C1–U1–D3 commutation loop (option A, board commit 55dc4db: D3 at (136.4, 128.0), cathode level with PH, SW straight at y 124.0, switch node ≈ 48.1 mm² F.Cu only; ≈ 48.4 mm² F.Cu with the rounded SW T-joint copper fillet of board commit f862007, ≈ 48.3 mm² F.Cu with the straight tapered fillet of board commit aa520d7, and changed slightly, not re-measured, with the curved fillets of board commit 79270d0, 2026-11-07, docs/LAYOUT.md) and keep its rise at or below 40 °C. Rev A to D cooled it with ≈ 2 cm² of SW pour; from rev E (E2, E3, board commit efb38ed) the SW pours are deleted (switch node ≈ 47.1 mm² F.Cu only) and D3 is cooled from its anode: solid zone connection on D3.2 to the GND pours (15–19 cm² per layer) and GND vias under the body. B560C RθJT 10 °C/W, RθJA 50 °C/W on 8 mm (0.033 mm thick) pads, TJ −55 to +150 °C (Diodes DS13012 rev 19-2, p. 2, read 2026-11-06; recorded in research/parts/datasheet-verification.md D3 row). This closes the thermal (D3) half of verification finding B.F6; the compensation half was closed by change docs-rev-e-record-corrections. At 1.2–1.5 W the minimum-pad RθJA would give a 60–75 °C junction rise, so the anode copper is what keeps the case rise in budget; verify on the bench (DEVPLAN T8).
- **C4: bootstrap, 10 nF, X7R 0603, 50 V.**
  - It sees only the internal ~8 V BOOT drive, so ≥ 16 V would be enough.
  - 50 V is used so the input-side capacitors share a single voltage class.

## 4. Output, feedback and compensation

- **C5: EEH-ZA1E221P, 220 µF, 25 V hybrid. C6: GRM32ER71E226KE15L, 22 µF, 25 V X7R 1210.** These replace EEH-ZA1A221P (not on LCSC) and GRM32ER71A226KE20L (0 stock) for JLCPCB assembly (research/parts/jlcpcb-sourcing.md, 2026-11-06). Same series, value and case.
  - 25 V is ≥ 1.5 × 5.15 V = 7.7 V.
  - Ripple estimate: ΔI·ESR + ΔI/(8·f·C) ≈ 0.48 A × 27 mΩ (Panasonic ZA series 04 Mar 2015, ESR at 100 kHz 20 °C, VERIFIED 2026-11-06) + 0.5 mV ≈ 13–14 mVpp at 18 V. That is within the 50 mVpp budget.
- **Compensation: internal (TPS5430 has no COMP pin).**
  - The loop is stable only if the output filter is close to the datasheet design range.
  - LC double pole: f_LC = 1/(2π√(15 µH × ≈ 238 µF)) ≈ 2.7 kHz. Total C is C5 220 µF plus C6 at about 17.5–18 µF effective at 5 V bias (Murata GRM32ER71E226KE15L typical curve, updated 11/08/2020, VERIFIED 2026-11-06; earlier estimate 242 µF gave ≈ 2.6 kHz). The shift is under 1 %, so the filter stays close to TI's 5 V reference.
  - ESR zero of the hybrid: with EEH-ZA1E221P at 27 mΩ (Panasonic ZA series 04 Mar 2015, VERIFIED 2026-11-06), 1/(2π × 27 mΩ × 220 µF) ≈ 27 kHz. The EEH-ZA1A221P at ~30 mΩ gave ≈ 24 kHz. The shift is about +11 %, in the same ZA series, and stays close to TI's 5 V reference filter. Checked against the datasheet equations (next bullets); loop stability is still confirmed on the bench (DEVPLAN T6).
  - The 15 µH + 220 µF pairing matches TI's 5 V reference design.
  - **Datasheet compensation check (TI SLVS632L rev L eqs. 7–9; research/references/buck-converter-design-guidelines/; change docs-rev-e-record-corrections, findings BR-B, B.F6):** fco = f_LC²/(85 · Vout). With C5 + C6 = 242 µF nominal, f_LC = 1/(2π√(15 µH × 242 µF)) ≈ 2.64 kHz, so fco ≈ 16 kHz, inside the datasheet's 3–30 kHz range. ESRmax = 1/(2π · Cout · fco) ≈ 40 mΩ for 220 µF at 18 kHz, the datasheet's own figure. The fitted 27 mΩ passes, and its ESR zero (≈ 27 kHz) is not far above the internal 24/54 kHz poles. This closes the compensation half of verification finding B.F6.
  - Constraint: do not add more output ceramic, and do not swap C5 for a lower-ESR or larger part, without re-checking against the datasheet compensation guidance. Doing so moves the ESR zero and the LC pole. C5 ESR must stay at or below about 40 mΩ (SLVS632L eq. 9). The bound was recorded as ~80 mΩ until 2026-11-07, which could have let a substitute put the ESR zero below crossover.
- **Feedback: R2 10.0 kΩ (+5V→FB), R3 3.24 kΩ (FB→GND).**
  - Vout = 1.221 × (1 + 10.0/3.24) = 4.990 V.
  - Divider current is 4.99 V / 13.24 kΩ = 0.38 mA.
  - R2's top is taken at C5's terminal, and the FB trace runs away from SW.
- **Tolerance stack.**
  - **Datasheet stack (change review-docs-fab-notes, 2026-11-06, design review B.F1).** This supersedes the estimates below. Inputs:
    - Vref 1.196–1.245 V over temperature (TI SLVS632L rev L, research/parts/datasheet-verification.md).
    - Fitted 0.1 % R2/R3 (RT0603BRD0710KL, RT0603BRD073K24L). Their worst-case ratio 1 + R2/R3 is 1 + 9.99/3.24324 = 4.0802 to 1 + 10.01/3.23676 = 4.0926.
  - Result: Vout min = 1.196 × 4.0802 ≈ **4.880 V**, and Vout max = 1.245 × 4.0926 ≈ **5.095 V**. **This meets the ±3 % budget (4.85–5.15 V)** with ≈ 30 mV margin low and ≈ 55 mV high.
  - The datasheet Vref range covers temperature at the reference. Ripple (≈ ±7 mV) fits inside the remaining margin, and so does the IR drop from C5 to J2 (Kelvin feed from C5, short +5V run). The low side is the tighter one; check it in DEVPLAN T3.
  - For comparison, 1 % resistors would give ≈ 4.814–5.165 V, which fails on both sides.
  - Earlier estimate (superseded): with 1 % resistors, reference ±1.5 % plus divider ratio ±1.5 % (2 × 1 % × 3.086/4.086), giving ≈ ±3.0 %. That left no room for line, load or ripple error inside the ±3 % budget (4.85–5.15 V).
  - **Decision: specify R2/R3 at 0.5 % (or tighter), 0603, ≤ 50 ppm/°C.** The divider error drops to ±0.75 %, so the worst case is ≈ ±2.3 % plus regulation. The values are the same (E192), and only the tolerance changes from the brief.
- **J2: Phoenix MKDS 1,5/2-5.08 (1715721; replaces the brief's Würth 691137710002 because no Würth footprint is installed, see docs/BOM.md).** It carries 3 A. It must be rated ≥ 4.5 A at 50 °C. Phoenix datasheet 12/21/2021 (VERIFIED 2026-11-06): IEC 17.5 A, 250 V (III/3); UL 300 V, 10–15 A. The datasheet read gives no 50 °C derating figure, so the terminal rise is a bench check (DEVPLAN T8). Fab note: finished plated hole ≥ 1.3 mm for the 0.9 mm square pins (outputs/README.md, board User.Comments note).

## 5. Enable

- **No external pull-up on EN. This is intentional.**
  - TPS5430 ENA has an internal pull-up, so a floating pin means the converter runs. ENA below ~0.5 V disables it.
  - ENA's absolute maximum is 7 V (TI SLVS632L rev L, VERIFIED 2026-11-06; internal pull-up 1.5 MΩ). A pull-up to VIN (9–29 V) would exceed it, and there is no other rail before the converter starts.
  - The SPEC phrase "pulled high by default" is met by the internal pull-up.
- **External control:** a 2-pad footprint (EN, GND) plus a test point. A dry contact or open-drain to GND disables the converter.
  - C9 1 nF 0603 EN→GND noise filter, FITTED in rev A (CL10B102KB8NNNC, LCSC C1588; change fit-c9-en-filter, 2026-11-06; docs/BRIEF.md Assumptions, design review finding P.F3/S.F1). ENA is held high only by the internal 1.5 MΩ pull-up and is wired off-board through J3, so without C9 switching noise or a field-wire transient could turn the converter off. A capacitor cannot pull EN high (`protection.ena_pin_max_V`), and it is not the resistor to GND the datasheet forbids. X7R leakage (nA) is negligible against the 1 mA shutdown budget. With the 1.5 MΩ pull-up it adds an enable delay of about τ ≈ 1.5 ms, harmless at start-up. No ESD series resistor at J3 in rev A; add one in rev B if the EN wiring leaves the cabinet.
  - The external switch must never drive EN high. Open-drain or open-collector only.
  - **R5 0 Ω series position (rev E, proposal E10; docs/BRIEF.md Interfaces):** J3.1 and TP7 are on net EN_EXT, which reaches EN (U1.5, C9.1) through R5 (0603, Uniroyal 0603WAF0000T5E, LCSC C21189; VERIFIED: < 50 mΩ, 1 A rated, 2 A overload, −55 to +155 °C, Uniroyal SMD-SP-001 version 9, Jun. 21 2025, section 7 Ratings, research/parts/datasheet-verification.md). R5 is fitted and is the only path from the EN pads to ENA. Fitted 0 Ω because the EN driver and whether its wiring leaves the cabinet are still unknown; an ESD or current-limiting resistor can replace it later without a new layout. It is series only, not a pull-up and not a resistor to GND, so `protection.ena_pin_max_V` holds. A fitted value must stay small against the 1.5 MΩ internal pull-up so a contact still pulls ENA below threshold, and the external driver must have no diode or pull-down path to GND when unpowered (docs/BRIEF.md Assumptions, finding M.F1).

## 6. Indicator

- **D4: green LED 150060VS75000 (0603; VF about 1.9–2.0 V at 2 mA from the typical curve, 2.0 V typ / 2.4 V max specified at 20 mA), with R4 1.5 kΩ 0603, on +5V.**
  - I = (5.0 − 2.0)/1.5 k ≈ 2.0 mA.
  - D4 VF is specified only at 20 mA (2.0 V typ, 2.4 V max; Würth WL-SMCW rev 002.009, VERIFIED 2026-11-06). At 2 mA it is about 1.9–2.0 V from the typical curve only. Across that VF and Vout 4.85–5.15 V the current spans ≈ 1.9–2.2 mA, and even at the 2.4 V guaranteed-at-20 mA figure it stays ≥ 1.6 mA, within the 1–3 mA budget.
  - The LED shows only that 5 V is present. It is not power-good, because the TPS5430 has no PG pin.

## 7. Test points

The test points are TP_VIN (VIN_PROT), TP_5V, TP_GND (×2, one at each terminal), TP_SW, TP_FB, TP_EN (on EN_EXT from rev E) and, from rev E, TP_GND3 (TP8).
- TP_SW is a small pad placed close to U1 so the switch-node copper does not grow.
- TP_FB is placed at R2/R3 and must not lengthen the FB trace toward SW.
- TP8 (TP_GND3, 1.0 × 1.0 mm pad, rev E proposal E6; docs/BRIEF.md Interfaces; findings L.F1, S.F1) is a GND pad on the U1/D3 ground copper beside TP5, so the switch node can be probed with a scope ground spring instead of a long clip to a GND point 22.6 mm away.

## 8. Not present (intentional)

- **MCU:** none. The design has no digital control, telemetry or digitally controlled output (SPEC §8).
- **Connectivity:** none. There is no bus, no remote sense and no isolation (SPEC §8).
- **UI:** only the power LED (§6) and the EN pads (§5). There is no power-good output (SPEC §8; the TPS5430 has no PG pin).

## 9. Budget roll-ups

| Budget (SPEC) | Limit | Architecture estimate | Status |
|---|---|---|---|
| Shutdown Iin (EN low) | ≤ 1 mA | U1 shutdown ≤ 50 µA max (15 µA typ; TI SLVS632L rev L), plus gate clamp ≤ 66 µA at 18 V with D2 Vz at its 11.4 V minimum (Diodes DS18004 rev 38-2), plus TVS ≤ 5 µA max (Diodes DS19002 rev 20-2), plus input-cap leakage (negligible). LED off. Total ≈ 0.121 mA worst case | OK |
| No-load Iin (enabled, 12 V) | ≤ 20 mA | U1 Iq 4.4 mA max (2 mA typ; TI SLVS632L rev L; was taken as ~3 mA), plus (LED 2 mA + divider 0.38 mA) × 5 V / (12 V × ~0.6 light-load efficiency) ≈ 1.7 mA, plus light-load switching loss of a few mA. Total ≈ 7–11 mA worst case | OK, bench-verify |
| Worst-case Iin | ≤ 2.0 A | 15 W / 0.85 / 9 V = 1.96 A | OK (F1 4 A 0154004.DRT at ~49 % of rating; SPEC §9 Q1 decided) |
| Efficiency at 12 V, 3 A | ≥ 85 % | Losses: diode ≈ 1.2 W, U1 ≈ 0.8 W, L1 ≈ 0.27 W, Q1 + F1 ≈ 0.2 W. Total ≈ 2.5 W, so η ≈ 15/17.5 ≈ 86 % | Marginal, bench-verify |
| Ripple | ≤ 50 mVpp | ≈ 15–30 mVpp | OK; C5 ESR must be ≤ ~40 mΩ for compensation (SLVS632L eq. 9), 27 mΩ fitted |
| Vout tolerance | ±3 % (4.85–5.15 V) | ≈ 4.880–5.095 V from datasheet Vref 1.196–1.245 V and the fitted 0.1 % R2/R3 (§4; change review-docs-fab-notes) | OK, ≈ 30 mV margin low; bench-verify (DEVPLAN T3) |
| Q1 gate-source voltage magnitude | ≤ 15 V target / 20 V abs | ≤ 12.7 V (D2 Vz 11.4–12.7 V, Diodes DS18004 rev 38-2) | OK |
| Voltage derating | ≥ 1.5× | Input caps 50 V (29 V clamp), output caps 25 V (C5 EEH-ZA1E221P, C6 GRM32ER71E226KE15L), D3 60 V, Q1 60 V | OK |
| U1 Tj at 50 °C | ≤ 125 °C | SLVS632L §7.2.1.2.8.3 at 9 V, 3 A, RDS(on) max 230 mΩ: Pcon ≈ 1.15 W + Psw ≈ 0.27 W + Pq ≈ 0.09 W ≈ 1.5 W; × 45 °C/W (TI EVM) gives Tj ≈ 118 °C. The earlier ≈ 80–85 °C used typical RDS(on) and an unverified 33 °C/W (findings BR-C, P.Q2) | TIGHT, ≈ 7 °C margin; measure U1 case at 9 V / 3 A (DEVPLAN T8), derate if above about 100 °C at 50 °C ambient |
| Component rise | ≤ 40 °C | D3 is the critical part, at ~1.2–1.5 W; from rev E it is cooled from its anode GND copper (solid connection, vias under the body), not a SW pour (§3). B560C RθJT 10 °C/W, RθJA 50 °C/W (Diodes DS13012 rev 19-2, research/parts/datasheet-verification.md; B.F6 thermal half closed) | Layout-dependent, bench-verify (DEVPLAN T8) |
| LED current | 1–3 mA | 1.6–2.2 mA | OK |

## 10. Mechanical

- **H1, H2: M3 mounting holes (Mechanical:MountingHole, MountingHole:MountingHole_3.2mm_M3, non-plated).**
  - Two holes on diagonally opposite board corners, per docs/BRIEF.md Physical and SPEC §6 (`mech.board_mm`).
  - Each hole has a copper and component keepout of ≥ 3.5 mm radius on both layers, so a screw head or standoff cannot short to copper.
  - No pins and no net. They are unplated and unconnected on purpose: the board's GND is not bonded to the chassis through the mount (SELV, input and output share ground; chassis bonding is the installation's choice).
