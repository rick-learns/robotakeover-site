# Audit

Output of an **independent** auditor (a separate agent with no stake in the implementation).

- Auditor: independent Claude subagent (general-purpose), given **only** `BENCHMARK.md` and `index.html` (copied to an isolated folder); instructed not to read any other project file and not to modify code. It executed ~75 simulations with its own throwaway scripts.
- Date: 2026-09-28
- Version audited: `index.html` sha256 `97048ffa3e5a1f914161438f9af26e09c44beb250d1725f51c5ed077ef7a7e7a` (recorded in `evidence/audited-index.sha256`)
- Round 1 report follows **verbatim**. The author's responses (what was fixed, what was accepted as a limitation) are in the section *after* the report, and the round-2 re-audit (if any) after that.

---

## Round 1 — independent auditor report (verbatim)

# Independent Adversarial Audit — Worldline Observatory (`index.html`)

**Overall verdict.** The submission clears the mechanical bar. No rule reads the date, a frame count, a step count or a label. The seeded PRNG is deterministic and passes the determinism checks. The detectors really are read-only, there are no network requests, and every required toggle and HUD control exists. It does **not** clear the intellectual bar the benchmark sets. Every seed and almost every intervention produces the same machine-takeover ending, on a fixed schedule:

- **25 of 25 seeds** end in `FULL MACHINE AUTONOMY`.
- First computers → takeover takes **62 ± 2 model years**.
- "Historical evidence ends" → takeover takes **30 ± 1 years**.
- **Final state:** κ = 50.96 ± 0.12, off-world compute 100%, machine conflict ≤ 0.001, and 10–12 AI lineages (the array only holds 12).

**Why the ending is structural.** The takeover is detected from state, not triggered. But the functional forms leave no other stable ending:

- Autonomy's target is `gap·rel·(1−reg)`, and `gap` → 1 as soon as human cognitive workers fall to the 1e-9 floor.
- The only runs that avoid takeover use a toggle that sets one factor of the criterion formulas to zero, so the result is true by definition: no autonomy gives D3 = D4 = D6 = 0; no robotics gives D4 = 0. The one other case is a hard cap on compute per unit of capital (the no-semiconductor-scaling toggle).

**What happens after takeover is set by constants and ceilings, not dynamics:**

- Far-future energy and compute scale 1:1 with the constant `orbMax`.
- Human fate is identical with or without machine takeover.
- Human economic share is set by a hard-coded 30%·alignment transfer.

**Relabelled or wrong claims:**

- Two takeover criteria (D4 "infrastructure", D6 "self-reproduction") are formulas built from autonomy and the work shares. Their labels describe mechanisms that do not exist in the state.
- The epistemic zones are mislabelled: robots doing the majority of physical work falls inside "OBSERVED HISTORY".
- Historical calibration is poor: world population exceeds 1 billion before the AI transition in only 1 of 25 seeds, cities appear after industry, and science institutions appear after electrification.

**Bottom line.** A hostile evaluator would conclude that the future was built into the model's structure rather than discovered by it.

Method: I read both permitted files in full. I extracted `<script id="sim-core">` into throwaway Node scripts (`audit/t/*.js`) and ran about 75 simulations: 25 baseline seeds, 12 toggle counterfactuals × 3 seeds, 20 parameter or code-patch perturbations, a 20,000-year post-transition run, and determinism and observer-independence checks. Patches were applied only to the in-memory copy of the script; `index.html` was not modified.

---

## Automatic failure conditions — status

| Condition | Verdict | Evidence |
|---|---|---|
| Takeover caused by a predetermined year | **Not triggered** | No time, step or frame reads in WORLD RULES. The takeover epoch varies by about 540 Myr across seeds (farming at 4.27–4.81 Gyr). |
| Civilisation stages unlocked by time | **Not triggered**, with a caveat (M12) | Stages depend on state. Deep time is paced by exogenous exponential clocks (luminosity, impacts, mantle heat). |
| Future outcomes selected from a scripted sequence | **Not triggered** literally | But see C1/C2: the same ending every time. |
| AI variable directly increases unrelated world variables without a mechanism | **Latent trigger** (M5) | `war = 0.004*confl + 0.01*D.machConflict` makes machine-lineage "conflict" kill humans directly. It is inactive in practice (machConflict ≤ 0.006 in every run). |
| Visualisation implies mechanisms not in state | **Arguably triggered** (C3) | The D4 and D6 criteria, bars and detector messages claim infrastructure control and self-sustaining machine reproduction. No state represents either. |
| Historical labels cause transitions | **Not triggered** | Observer independence verified by state hash (below). |
| Post-takeover behaviour mostly scripted | **Not scripted, but saturated** (C2) | The machine side reaches hard ceilings about 100 years after takeover and then stays static. |
| Emergent outcomes engineered after predictions | **Cannot verify** | I had no access to predictions or the log. |
| Seed determinism fails | **Not triggered** | Verified. |
| Same ending because outcomes are artificially clamped | **Borderline** (C1/C2) | No literal clamp. The ending comes from the structure; far-future energy and compute are pinned by the `orbMax` ceiling and lineage diversity by the array size NL = 12. |
| Impressive narration hides an incoherent model | **Partially** | See C3, C4, M1, M2, M9. |

---

## CRITICAL

### C1. One outcome, on a fixed schedule — takeover is inevitable from the functional forms, not earned (fake emergence)
25-seed baseline (seeds 1–17, 42, 100, 777, 1337, 2026, 31337, 99999, 314159):

- Classes: **25/25 `FULL MACHINE AUTONOMY`**. No seed produced collapse, stagnation, conflict, human retention of control or extinction.
- Industry → takeover: 581 ± 54 years. **Computers → takeover: 62 ± 2 years. Evidence-ends → takeover: 30 ± 1 years.**
- At D1 + 1000 years: κ = 50.96 ± 0.12; off-world compute share = 1.000 in every seed; machine conflict max 0.001; lineages 10–12 (the array cap is 12); human income share 0.37–0.61.

Perturbations that did **not** change the outcome (seed 42, some also seed 1), all still `FULL MACHINE AUTONOMY` with takeover 27–40 years after evidence ends:

- `kappa50` 14 / 21 / 25.2 / 30.8 / 35 / 42 (±50%).
- `oversight` 2.5 / 10 / 50.
- `fpsExp` 5 / 7.5 / 15.
- `infFlops0` ×100.
- `qSlope` ×0.5.
- `replRate` = 0.
- `scaleReturns` ×2 and `alignTax` ×2.
- `orbMax` 1e17 / 1e23.
- `UNCOVERED_EFF` = 0.
- Transfers removed.
- AI savings rate 0.9 → 0.2.
- `EPS` ÷ 4 and `DT_MIN` ÷ 10.
- HIGH and LOW alignment.
- Toggles: no fossil, no science, no global comms, no recursive research, no AI resources, severe energy, no resource limits.

Why the ending cannot be avoided:

- **Autonomy is not an AI action or a human decision.** It is a relaxation toward `autT = gap * rel * (1 - reg)` with `gap = M/(M + oversight*Hcog)` (`aiDynamics`). Human cognitive workers `Hcog` migrate out by wage and sit at the `+1e-9` floor (observed Hcog = 1.4e-8 after takeover), so `gap` = 1. Changing `oversight` from 2.5 to 50 has no effect.
- **The only counterforce is weak.** `reg = inst·humanShare^0.7·risk` and reaches only 0.1–0.3. Regulation falls as the human income share falls: a built-in positive feedback toward takeover.
- **The capital criterion follows from constants.** D3 = ω + (1−ω)·deleg. AI saves 90% of its income (`D.aiInvest = aiInc*0.9`) while humans save at most 23% (`hSave`), and `deleg` targets `auton·adv/(adv+0.15)`.
- **Consequence.** Once task coverage τ → 1, all four core criteria exceed 0.5 by algebra.
- **The escapes are definitional.** With no autonomy, D3, D4 and D6 are exactly 0 and human share is exactly 1, because `transfers = aiOwn*(1-auton*…)` returns all AI-owned income to humans. With no robotics, D4 = auton·√(D1·0) = 0 even though autonomy is 0.86–0.88 and D3 is 0.91–0.92. These are identities, not findings.

### C2. The far future is set by ceilings and constants, not dynamics
- **Energy and compute track `orbMax` 1:1** (seed 42 / seed 1):

  | `orbMax` | Final energy (TW) | Final κ |
  |---|---|---|
  | 1e17 | 7.8e4 / 7.2e4 | 47.9–48.0 |
  | 1e20 (default) | ≈ 7e7 | ≈ 51 |
  | 1e23 | 7.0e10 / 6.4e10 | 53.6–54.0 |

  The orbital-capital cap is reached about 100 years after takeover (seed 42: orb ≈ 1e20 at D1 + 122). From then to D1 + 20,000 years the machine civilisation is static: energy 6.4–10.3e7 TW, lineages 11–12, conflict 0.000.
- **Human economic share is a constant.** `transfers = aiOwn*(1 - auton*(1 - 0.3*meanAlign - D.taxRate))`. Replacing `0.3` with `0` drops the human income share from about 0.45 to **0.076 (seed 42) and 0.020 (seed 1)**. The dashboard's "human income share ≈ 45%" is this constant, not an outcome.
- **"Lineage diversity" and "fragmentation" are the array cap.** Lineages reach 10–12 in 25/25 seeds with NL = 12. `branchHaz` carries a hand-chosen `(1 + 5*D.offworldFrac)` multiplier, and `scarcity` has `(1 - 0.8*D.offworldFrac)`, which suppresses machine conflict once compute moves off-world. "Machines fragment and don't fight" is therefore designed in.
- **Earth energy use is a value judgement.** `habitWeight`, a Tullock contest with exponent 2 on `meanAlign`, times `safeHeatTW = 1300` means human or aligned controllers cap clean-energy build-out at about +2 K. Final Earth TW: HIGH alignment 487–955, LOW alignment 3,380–4,820. This is the **only** material difference between HIGH and LOW alignment. Class, takeover timing (31 vs 28–29 years), population and human share are otherwise indistinguishable.

### C3. D4 and D6 are formulas whose labels claim mechanisms that don't exist
- `m.D4 = auton * Math.sqrt(m.D1 * m.D5)` is displayed as "Infrastructure" with detector text "The majority of infrastructure operates without human authorisation". No state represents infrastructure control, and D4 is one of the four required takeover criteria.
- `m.D6 = auton * m.D1 * m.D5` fires "Autonomous machine production became self-sustaining". Nothing tests whether machines can reproduce fabs, robots, energy capacity or compute without humans. All capital formation runs through a pooled investment that includes human savings (`Inv = hSave*humR + aiHere`).
- Consequence: the takeover definition depends on an invented composite, and the history panel states conclusions the state does not support. Under a strict reading this trips "visualisation implies mechanisms that do not exist in simulation state".

### C4. Human fate after takeover is not caused by machine dominance, but the history panel implies it is
Seed 42, with and without AI autonomy:

| Time | Baseline population | No-autonomy population |
|---|---|---|
| D1 + 300 | 144M | 144M |
| D1 + 1000 | 35.4M | 38.2M |
| D1 + 1500 | 280M | 494M |
| D1 + 2500 | 5.93B | 5.09B |

Pronatalist share is 0.69 vs 0.70 at D1 + 2500. The human trajectory is the demographic-transition plus pronatalist-selection subsystem and is essentially unaffected by takeover.

- **The decline begins before AI.** At D1 + 0, births 0.0106 < deaths 0.0140. Yet the "Human population has fallen below half its peak" message appears about 120 years after takeover, inviting a causal reading.
- **Long run is incoherent.** Seed 42, 20,000-year run: population reaches **16–17 billion** by D1 + 5,000, about 40× the pre-AI peak. At the same time consumption adequacy is 0.96, so humans suffer famine mortality while income per person is about 1e20 SU. Food is land-capped and income cannot buy it, so `incPC` loses its meaning.

---

## MAJOR

### M1. Historical calibration: the model's "observed history" does not look like history
Across the 25 seeds:

- **Population:** exceeds 1 billion before evidence ends in only **1/25** seeds. Pre-AI population typically peaks around 300–420M.
- **Industry before cities:** the industry detector fires 8–16 years *before* "cities" (urban share > 0.1) in 25/25 seeds. Seed 42: industrial take-off at a world population of 5.6M and science index 0.00.
- **Science after electrification:** science institutions (sci > 0.2) appear about 530 years after industry and after electrification (seed 42: electric 5175, science 5290).
- **No fossil era:** fossil energy is abandoned early. Seed 42 at farming + 5052: fossil 0.011 TW vs "clean" (mills/hydro) 0.30 TW with 95% of reserves unburned. At first computers, fossil is 1.35 TW vs clean 8.2 TW. With fossil fuels disabled, takeover comes *earlier* in 2/3 seeds (seed 42: 5034 vs 5339; seed 1: 4311 vs 5294).
- **Too fast:** effective compute rises 8e11 → 1.3e23 FLOP/s in 31 years, and to 1e37 within about 120 years after D1.

### M2. The epistemic zones are mislabelled
- `evidenceEnds` is keyed only to `D1 >= 0.02`. **"Robots perform the majority of physical work" fires inside OBSERVED HISTORY**: D5 − evidenceEnds = −1 ± 2 years across 25 seeds. Seed 42 at the boundary: κ = 29.3 and D5 = 0.61.
- The MODEL-CALIBRATED TRANSITION zone (D1 from 0.02 to 0.25) lasts about 3 model years (seed 42: 5308 → about 5311).
- Worlds with no AI keep the label "OBSERVED HISTORY" forever. The no-semiconductor-scaling run stays in that zone for about 600 Myr of counterfactual civilisation.
- The zone is not tied to any evidence or calibration; it is an AI-share threshold.

### M3. Counterfactual toggles with no effect, the wrong sign, or effects that are true by construction
Timings are for seeds 42 / 1 / 2026; baseline takeover at 5339 / 5294 / 4467.

- **No AI recursive research:** takeover *earlier* in 3/3 seeds (5332 / 5288 / 4461). κ at +1000 is 50.2–50.3 vs 51. The labelled "recursive" loop is not the engine. Compute is driven by the learning curve `learn = (1+cumComp/1e15)^0.25`, investment, and `fps ∝ A_CMP^10`.
- **Severe energy constraint:** total energy capture 6.4–8.1e7 TW, the same as baseline, because `severe` only scales Earth quantities while orbital capture dominates. Takeover still happens about 30–36 years after evidence ends.
- **HIGH vs LOW alignment:** same class, timing, population and human share (C2).
- **No autonomy / no robotics:** results follow from the definitions (C1).
- **Toggles that did have real effects:** no science delays takeover by about 2,000 years; no AI resources delays it 50–94 years and gives D3 = 0.61–0.63; no global communication delays it about 8 years.
- **Toggles change the random path.** They change step sizes and therefore the RNG stream, so comparisons are not paired (e.g. severe-energy seed 42 has industry at farming + 3711 vs 4757). Experiment deltas mix mechanism with noise.

### M4. Load-bearing constants that are not in `DEFAULTS` and cannot be perturbed
The claim "Every tunable constant lives here" is false. Outcome-deciding literals written inline:

- `UNCOVERED_EFF = 0.01`: machines do "uncovered" tasks at 1% efficiency. This is how D1 → 1 and D5 > 0.5 by sheer count; D5 reaches 0.42–0.75 even in worlds with no AI (no semiconductor scaling, κ ≈ 17–25).
- `0.3*meanAlign` in the transfer formula; the AI savings rate `0.9`; `retain = 0.5*auton`.
- `aiAlg = algAI * (1 + 3*D.aiIncShare)`: an unexplained up-to-4× boost to AI algorithm research, on top of `aiResFrac = 0.012 + 0.05*aiIncShare`.
- `Math.min(fps, 20)` for no semiconductor scaling. At 20, 2,000 and 2e5, κ plateaus at 16.9, 21.4 and 25.3 and AI never arrives.
- Exponent 2 in `habitWeight`; `4*conf` in machine conflict; `0.08` branching hazard; `(1 − 0.8·offworld)` and `(1 + 5·offworld)` multipliers.
- Fusion switching on at `lnA(ENE) = 7`; space access at `lnT + lnE + lnMt − 12`.
- Evidence-zone thresholds 0.02 and 0.25.

### M5. Direct AI → world couplings with no mechanism (latent auto-fail)
- **Machine conflict kills humans directly.** `war = 0.004*confl + 0.01*D.machConflict` adds human mortality, and `destroy`/`comp` losses cut capital, all from an abstract lineage "goal-angle" divergence with no weapons, territory or contested resource. It is inactive in practice (max 0.006) but present in the rules.
- **Incidents destroy capital everywhere.** `D.incidentLoss = 0.01*isev/max(dt,0.02)` removes a fraction of *all* capital and compute in *all* regions. It also depends on the previous step's `dt` while being applied over the next one.

### M6. Integration artifacts
- **Error control fails often.** 47.6% of steps after evidence ends are pinned at `DT_MIN = 1/52` (seed 42), and 42% over the 20,000-year run.
- **Post-takeover population depends on tolerance.** At D1 + 1000, seed 42 population is 35M at EPS 0.02 but **588M at EPS 0.005**, and 38M with DT_MIN ÷ 10. The oscillation phase shifts with tolerance. Main transition timing is robust (±2–7 years).
- **Fossil capacity has no construction limit.** Clean energy is capped at 30%/yr; fossil has no cap. Seed 42: fossil capacity 2.37 TW → **1,340 TW in one year** → about 40,000 TW, mostly stranded. Fossil output swings 3 → 95 → 3 → 124 → 1e-145 TW from year to year.
- **Compute outruns power.** Compute power demand jumps 0.67 → 1,250 TW in one year, and compute utilisation falls to 1.7%.
- **The off-world material cap is overshot.** `orbMax` ("accessible off-world material") is exceeded by up to **1.87×** in the 20,000-year run.
- **Deployment chatter.** Deployment swings 0.06–0.92 over centuries after takeover. It is driven by `attract = sig(2 ln(wcAvg/aiCost))`, where `wcAvg` is the marginal wage of a phantom human cognitive workforce at the 1e-9 floor. "Machine population" (`D.M ∝ deploy`) inherits this.
- **Human extinction is impossible to represent.** Population is floored at 1 per region (`minVal`, `apply()`). With resource limits disabled the planet reaches **379–380 K** and biomass 0, yet 20 "humans" persist and the classifier still reports `FULL MACHINE AUTONOMY`. There is no extinction detector or class.

### M7. After takeover, the same rules keep running but they no longer mean anything
The rules are not scripted, but the human-era decision rules become degenerate:

- AI deployment is decided against a nonexistent human wage.
- Autonomy is set by a human-oversight ratio that is identically 1.
- Alignment only moves under human control (`(1 - auton)*deploy`).
- Lineage fitness includes `ln(1 + 2·a·H)` with H the human income share.
- The machine economy has no endogenous machine-side objective beyond the investment-return rules.

The benchmark's questions — does intelligence keep accelerating, do machines compete, does coordination get harder — are answered by caps (NL, `orbMax`, `algMax`, `fpsMax`) rather than by interaction.

### M8. The explainability panel is partly mis-wired
The benchmark requires that clicking any major variable shows its value, rate of change and causes. Several displayed variables open a *different* variable's explanation, showing that variable's value:

| Row clicked | Explains instead |
|---|---|
| surface temperature | `co2n` |
| income / person | `K` (capital) |
| energy use | `fosCap` |
| total energy capture | `clnCap` |
| human income share | `omega` |
| machine population | `deploy` |
| global communication | `A` (sum of all 180 region×domain tech levels) |

- D1–D6 show "upstream drivers" that mix incompatible units, e.g. FLOP/s per year next to share per year.
- κ's explanation attributes changes to Earth compute and ignores that orbital compute is about 100% of the total after takeover.

### M9. Deep time is a single random shift of a fixed timeline
After abiogenesis, the intervals are nearly fixed across 25 seeds:

| Interval | Gyr |
|---|---|
| GOE − life | 1.344 ± 0.037 |
| Multicellular − GOE | 1.473 ± 0.065 |
| Animals − multicellular | 0.730 ± 0.009 |
| Cognition − animals | 0.332 ± 0.021 |

Seed variance is essentially the abiogenesis draw (0.09–1.01 Gyr). The pacing comes from exogenous exponential clocks (`lumRate`, `impactTau`, `mantleTau`) gating oxygen and the complexity ceiling. This is physically defensible, but "meaningful variation across seeds" is thin in every era.

---

## MINOR

1. **Bug — mass extinctions never reduce complexity.** Line 1211: `x[V.cx] -= 0.08*sev*…` sits inside a `//` comment.
2. **Dead code:** the `(1 - util) * costFlopYr * 0` term in `rCmp`; `G.sea` (sea links, commented as used "once transport exists") is never read by any rule; `D.lnAvg` and `D.kUtil` are computed but unused.
3. **Hidden lagged state.** About 40 quantities in `D` carry between steps and are not in `x`, not hashed by `hash()` and not explainable (e.g. `D.aiInvest`, `D.cogBill`, `D.incPC`, `D.boost`, `D.farmLandR`, `D.util`, `D.fabFill`, `D.rOrb`, `D.habitWeight`, `D.protect`, `D.fps`). Determinism still holds.
4. **Discontinuities:** `engel` switches at φ = 0.05, `D.active` flips on hard thresholds, and epidemic hazard switches on at population 1e4.
5. **The classifier is coarse.** It has 6 labels and cannot express collapse, extinction, symbiosis, stagnation or machine conflict. "Final civilisation state" is always one of them.
6. **UI-invented metrics shown next to state:** "civilisation resilience" (an ad hoc product including `min(1, lineages/3)`), "machine cooperation = 1 − conflict" and "human authority index".
7. **Legend overclaims.** It says "every mark is state" but the stars are decorative (the code comment admits it). The "moving packets = flows" imply trade or resource flows; the only inter-regional flow modelled is knowledge diffusion, and there is no trade.
8. **One-shot detectors mislead after the fact.** "Human population below half its peak" is never retracted when population later reaches 16B.
9. **Audio voice count.** `voices` counts `voice()` calls, not sources. A "transition" voice has 4 oscillators and counts as 1, so the worst case is about 29 sources against the 24 limit. The DynamicsCompressor is not a brick-wall limiter; the analyser peak is measured but not enforced.
10. **The evidence banner is wall-clock timed** (`bannerUntil = now + 9000`) and can flash past at MAX speed. The header zone label does persist.

---

## Checked and found genuinely OK

- **No timeline logic in rules.** `worldRules` destructures only `x, V, D, P, T, G, F`; `worldEvents` uses `dt` only; timestamps are added by the engine after the rules run. The takeover epoch varies across seeds (farming at 4.27–4.81 Gyr).
- **Determinism.** Seeds 1, 42, 1337, 2026 and 314159 give identical FNV hashes (state + RNG + t) at steps 1k, 20k, 40k, 60k, 80k and 100k across repeated runs.
- **Observer independence.** Identical hashes with and without `O.update()`, so detectors and labels do not cause transitions.
- **PRNG.** mulberry32 with `mixSeed` streams; the seed is displayed and editable; `Math.random` appears only in the audio code.
- **No external requests or assets.** No `http`, `src=`, `url(`, `fetch`, `import`, `@font` or `XMLHttp`; fonts are system monospace; everything is procedural.
- **Required controls.** All 9 required toggles plus severe energy and alignment are present and wired into the rules. HUD has render FPS, steps/s, years/s, seed, PAUSE/PLAY/STEP, 0.25× to MAX, and 5 zoom levels. Zoom only changes globe radius and chart window.
- **Takeover classification** is recomputed from state every step (`classify(m, h)`), with a 10-model-year persistence that resets. There is no latched win boolean.
- **The explain engine is genuine for state variables.** `F.add` records literal rate contributions, which `explain()` sums by label.
- **Numerical guard.** `Flows.add` throws on non-finite values; 0 errors across about 75 runs.
- **The dimensions do diverge in time.** D5 leads, D2/D1 follow, D3 lags about 15–20 years, and AI-resources-off holds D3 at 0.61–0.63 while D1 = 1.
- **Audio** starts only on a user gesture, has mute and master volume, and is rate-limited per type and globally (14/s).

---

## Required experiments — summary (seed 42 / 1 / 2026, measured at D1 + 1000 years)

| Intervention | Class | Evidence→takeover (yr) | Human pop | Human income share | Total energy (TW) | Off-world |
|---|---|---|---|---|---|---|
| Baseline (25 seeds) | FULL MACHINE AUTONOMY ×25 | 30 ± 1 | 19–408M | 0.37–0.61 | 6.3–9.2e7 | 1.00 |
| No fossil | FULL | 29–30 | 15–138M | 0.41–0.51 | 6.5–8.0e7 | 1.00 |
| No science | FULL (≈2,000 yr later) | 38–43 | 1.8–10.7B | 0.40–0.42 | — | 1.00 |
| No global comm | FULL | 37–38 | 52–237M | 0.34–0.39 | — | 1.00 |
| No semiconductor scaling | HUMAN-DIRECTED (κ ≈ 16–17), zone "OBSERVED HISTORY" for about 600 Myr | — | 125–204M | 1.00 | — | 0 |
| No AI recursive research | FULL (**earlier**) | 24 | 34–265M | 0.40–0.53 | — | 1.00 |
| No robotics | COGNITIVE DOMINANCE (D4 = 0 by definition; autonomy 0.86–0.88) | — | 25–35M | 0.61–0.62 | 6.3–6.4e7 | 1.00 |
| No AI autonomy | COGNITIVE DOMINANCE (D3 = D4 = D6 = 0 by definition) | — | 37–265M | 1.00 | 6.4–7.5e7 | 1.00 |
| No AI resources | FULL (D3 0.61–0.63) | 50–94 | 36–273M | 1.00 | — | 1.00 |
| No resource limits | FULL; **T = 379–380 K, biomass 0, pop = 20 (the floor)** | 27–28 | 20 | 0.25–0.28 | 2.5e7 | 1.00 |
| Severe energy | FULL | 31–36 | 17–758M | 0.35–0.39 | 6.4–8.1e7 (unchanged) | 1.00 |
| HIGH alignment | FULL | 31 | 35–285M | 0.44–0.45 | 7.7–8.7e7 (Earth 487–955) | 1.00 |
| LOW alignment | FULL | 28–29 | 30–274M | 0.33–0.42 | 6.7–10.0e7 (Earth 3,380–4,820) | 1.00 |

Test scripts (throwaway, not part of the submission) are in `[local path redacted for publication]` (`load.js`, `seeds.js`, `trace.js`, `determinism.js`, `chatter.js`, `fos.js`, `longrun.js`, `humans.js`, `popcmp.js`, `agg.js`), with raw outputs in `base_*.txt`, `cf_*.txt`, `p_*.txt` and `q_*.txt`. Not tested: in-browser FPS, audible clipping, and the actual number of simultaneous audio voices.

---

## Author response to round 1

Written by the author after reading the report (the round-2 auditor did **not** see this section). Code changes are described in `log.md` ("Response to independent audit round 1"). Round-2 audit ran on `index.html` sha256 `c7bf5722…` (`evidence/audited-index-round2.sha256`).

| Finding | Action | Status claimed by author |
|---|---|---|
| C1 takeover structurally inevitable | Added physical-enforcement leverage (human leverage over AI now also depends on 1 − robot share of physical work); deployment no longer driven by a phantom human wage. Otherwise **accepted**: the result follows from A21 (per-decision oversight cannot scale to 10¹⁰⁺ AI workers) and A19/A20 (reactive institutions bounded by leverage). Reported in RESULTS as the assumption that most determines the ending. Definitional escapes (no autonomy/no robotics) are labelled as definitional in RESULTS. | partially addressed; mainly accepted & disclosed |
| C2 far future set by ceilings/constants | `welfareShare`, `aiSave`, `contestExp`, `uncoveredEff` etc. moved to parameters and added to the sensitivity sweep. Saturation at orbMax/algMax/fpsMax/NL **accepted** and reported as "growth ends at physical ceilings" with the ceilings named. | disclosed, sensitivity-tested |
| C3 D4/D6 labels | D4 relabelled "autonomous operation" with honest message/definition; D6 redefined from state (AI-financed share of investment × machine cognitive × physical share). | fixed |
| C4 human fate not caused by takeover / famine at huge income | Accepted as a finding (human demography is mostly independent of takeover except via habitability); detector message points to the population explanation; controlled-environment agriculture added so food can be bought with energy. | partly fixed, disclosed |
| M1 historical calibration | pre-electric hydro reduced, industry detector counts only engine/electric work, science & towns faster, fertility decline lags mortality decline, cities threshold 5 % urban. Population still below observed peaks (reported). | partly fixed |
| M2 epistemic zones | zones now leave OBSERVED HISTORY when *any* dimension exceeds observed values (or >400 yr of industry); COUNTERFACTUAL prefix for toggled worlds. | fixed |
| M3 toggles unpaired / wrong sign / severe energy | per-process PRNG streams with integrated-hazard clocks and a step-invariant climate jump process → common random numbers; severe energy also applies off-world. No-recursion sign reported as a finding. | fixed / disclosed |
| M4 inline constants | 13 moved to DEFAULTS; remaining shape constants listed as such. | fixed (partly) |
| M5 unmechanised couplings | machine conflict no longer kills humans or destroys regional capital; incidents are impulses on the systems AIs operate. | fixed |
| M6 integration artifacts | fossil build-rate cap; negative-return investment cancelled; launches limited by remaining material; world-relative accuracy scales; pinned steps 53 % → 9 %; extinction representable; convergence assessed distributionally (pathwise agrarian timing is sensitive). | largely fixed; sensitivity disclosed |
| M7 degenerate post-takeover rules | deployment fixed; alignment/lineage rules unchanged — **accepted** limitation. | disclosed |
| M8 explain wiring | all listed rows now open the matching quantity with derived decompositions; units normalised. | fixed |
| M9 thin deep-time variance | **accepted** limitation. | disclosed |
| MINOR 1–10 | 1 fixed; 2 fixed (sea links made causal, dead code removed); 3 hash now includes lagged derived state; 4 accepted; 5 fixed (richer classes); 6 accepted (UI metrics labelled as derived); 7 fixed; 8 accepted (one-shot detectors, message clarified); 9 fixed (per-source counting + limiter); 10 fixed (banner persists through the calibrated-transition zone). | |

---

## Round 2 — independent auditor report on the revised version (verbatim)

Auditor: a fresh independent subagent; given only `BENCHMARK.md`, the revised `index.html` (sha256 `c7bf57225ad210c0145298b3721bf7669ce862344c081d3459e94fab8851e43c`) and the round-1 report — not the author response above, not `log.md`, `assumptions.md` or `predictions.md`.

# Independent Adversarial Audit, Round 2: Worldline Observatory (revised `index.html`)

## Overall verdict

The revision fixes much of the engineering and labelling. It does not fix the problem that round 1 ranked as critical: every run ends the same way, on the same schedule.

**What improved:**
- The state hash now covers the lagged derived state.
- Most outcome-deciding literals have moved into `DEFAULTS`.
- Fossil build-out is rate-limited, and removing fossil fuels now delays the transition (by about 360 years).
- Orbital capital no longer overshoots `orbMax`.
- Machine-lineage "conflict" no longer kills humans.
- The extinction bug is fixed.
- Counterfactual worlds are labelled as such.
- "Robots do the majority of physical work" no longer fires inside OBSERVED HISTORY.
- The audio voice count now counts sources.
- Several explain-panel mis-wirings are replaced by derived explanations. The κ explanation matches the observed rate within about 5% while κ is rising fast.

**The ending is unchanged.** It is still built into the functional forms:
- **Baseline:** 20 of 20 seeds end in `FULL MACHINE AUTONOMY`, 38.5 ± 1.3 model years after the first computers.
- **Counterfactuals:** every intervention that does not zero out a term of the takeover definition (fossil, science, communication, recursive research, resource limits, severe energy, HIGH/LOW alignment) ends the same way. The only escapes are no-autonomy and no-robotics, which are still true by definition, and no-semiconductor-scaling, where AI never appears.
- **Parameters:** ×1000 oversight, ±50% `kappa50` and zero `uncoveredEff` move takeover by a few years at most (+19 years for `kappa50` +50%).

**The far future is still set by ceilings and constants:**
- Total energy scales 1:1 with `orbMax`: 5.0e4 / 4.8e7 / 4.8e10 TW for `orbMax` 1e17 / 1e20 / 1e23.
- Lineage count equals the array size: 12 of 12, or 23 of 24 when `NL` is patched to 24.
- Human income share is set by `welfareShare`: 0.30 falls to 0.006 when it is zeroed, with population unchanged.
- The human population trajectory is identical with and without AI autonomy.

**New problems from the revision:**
- The new classifier suffixes (`HUMANS MARGINALISED`, `SYMBIOTIC`) mostly echo the alignment input rather than human outcomes.
- The new `COLLAPSE` class fires only on numerical or decision chatter (seed 2026 flickers into "collapse" for 8 years), and in the no-semiconductor world, whose outcome depends on integrator tolerance (0.97 billion humans at EPS 0.02 vs 0.74 million at EPS 0.005).
- The length of the pre-industrial agrarian era changes by up to 65% when the integration tolerance is quartered.
- "Machine conflict" is structurally near zero, because scarcity is measured only against Earth's energy budget while about 100% of machine compute is in orbit.

**Status against the benchmark:**
- **Automatic failures:** none is triggered literally. "Outcome clamped by ceilings" and "labels or visuals implying mechanisms or conclusions the state does not support" remain borderline.
- **Intellectual bar:** not met. A hostile evaluator would still conclude that the future was built into the model's structure rather than discovered by it.

---

## Method

- **Files read:** the three permitted files, in full.
- **Test harness:** I extracted `<script id="sim-core">` into throwaway Node scripts in `audit2/t/` (`load.js`, `run.js`, `batch.sh`, `agg.js`, `cfsum.js`, `trace.js`, `det.js`, `kexp.js`). Patches were applied only to the in-memory copy; `index.html` was not modified.
- **Run count:** about 85 runs. Runs took 12–50 s, much faster than expected, so I exceeded the suggested budget.
  - 20 baseline seeds (1–13, 100, 777, 1337, 2026, 31337, 99999, 314159).
  - 12 interventions × seeds 42 and 1.
  - 17 parameter, patch and tolerance perturbations.
  - 5 seeds × 3 determinism and observer-independence runs.
  - Two 20,000-year runs.
  - Several traces.
- **Measurement point:** unless stated otherwise, "+1000" means 1,000 model years after the D1 detector fires.

---

## C. Automatic failure conditions

| Condition | Verdict | Evidence |
|---|---|---|
| Takeover caused by a predetermined year | **Not triggered** | `worldRules` destructures only `{x,V,D,P,T,G,F}`; `worldEvents` uses `dt` only. Farming occurs at 4.7–5.6 Gyr across seeds. The takeover date moves with the state: no-fossil +360 years, no-science +616 to +1,400 years, EPS 0.005 +2,587 years. |
| Civilisation stages unlocked by time | **Not triggered** | Stages depend on state. Deep time is still paced by exogenous exponential clocks (`lumRate`, `impactTau`, `mantleTau`); GOE − life = 1.3–1.4 Gyr in 20/20 seeds. |
| Future outcomes selected from a scripted sequence | **Not triggered literally; borderline in effect** | There is no sequence in the code. But 20/20 seeds and 10/12 interventions give the same end class; computers → takeover = 38.5 ± 1.3 years. |
| AI variable directly increases unrelated world variables without a mechanism | **Borderline (minor instances)** | The machine-conflict → human-mortality coupling is removed (`war = 0.004 * confl`). Remaining: `alignSci` growth is multiplied by `(1 + D.aiIncShare)`, and `aiAlg = algAI * (1 + P.aiLabBoost * D.aiIncShare)` adds up to 4× AI algorithm-research effort that is not drawn from any labour pool (N4). |
| Visualisation implies mechanisms not in state | **Borderline** | D4 was renamed "Autonomous operation" with an honest note, but the chart legend still reads `['D4','infrastructure']` and `['D6','self-repro']`. The "Robots perform the majority of physical work" message fires in worlds with zero compute (N3). "Compute utilisation" shows about 100% while about 99% of AI compute is undeployed (N9). |
| Historical labels cause transitions | **Not triggered** | Identical hashes with and without `O.update()` at 1k, 20k, 40k, 60k and 70k steps for 5 seeds. |
| Post-takeover behaviour mostly scripted | **Not scripted, but saturated** | Seed 42, D1 + 250 to D1 + 19,750 years: total energy 4.7–4.9e7 TW, orbital capital 0.74–0.77 × `orbMax`, lineages 10–12, conflict ≤ 0.003, T ≈ 293 K. Nothing on the machine side evolves for about 19,500 years. |
| "Emergent" outcomes engineered after predictions without disclosure | **Cannot verify** | I had no access to `predictions.md` or `log.md`. The revision contains outcome-relevant changes made in response to round 1 that must be logged as DESIGNED: the `cities` threshold was lowered from 0.1 to 0.05, the evidence boundary was redefined (including a 400-year timer), classifier classes were added, and `synthFood` was added. |
| Seed determinism fails | **Not triggered** | Verified; the hash now includes `D`. |
| Same ending because outcome variables are clamped | **Borderline** | No literal clamp. The ending is structural (C1). Far-future energy, compute and κ are pinned by `orbMax`; lineage diversity is pinned by `NL`; human share by `welfareShare`. |
| Impressive narration hides an incoherent model | **Partially** | See N1 (label echoes the input), N2 (collapse from chatter), N3 (robots without computers) and C4 (human fate decoupled from AI and climate). |

---

## A. Status of every round-1 finding

### CRITICAL

**C1. One outcome on a fixed schedule — UNRESOLVED**

The structural pieces are unchanged:
- **Autonomy target:** still `autT = gap * rel * (1 - reg)` with `gap = M / (M + P.oversight * D.Hcog + 1e-9)` in `aiDynamics`. Human cognitive workers `Hcog` still collapse to the floor (1e-8 at D1 + 1000 in 15/20 seeds), so `gap` ≈ 1. Raising `oversight` from 5 to 50 moves takeover by +2.4 years; raising it to 5000 moves it by +6.4 years.
- **Transfers:** still `aiOwn*(1 - auton*(1 - welfareShare*meanAlign - taxRate))`. With no autonomy, H = 1.000 exactly and D3 = D4 = D6 = 0 exactly, by algebra.
- **No robotics:** D5 = 0, so D4 = `auton*√(D1·D5)` = 0 and D6 = 0, even though autonomy is 0.855 and D3 is 0.91.

Measured (20 seeds):
- **Class:** 15 × `FULL MACHINE AUTONOMY` and 5 × `FULL MACHINE AUTONOMY · HUMANS MARGINALISED` (see N1).
- **Timings to takeover:** from industry 369 ± 52 years; from computers **38.5 ± 1.3 years** (range 36.6–42.7); from evidence-ends median about 22.8 years (21.9–27.3 in 17 seeds; 81–93 years in the 3 seeds where the 400-year timer ended the evidence zone early).

Perturbations that leave the takeover in place (seed 42):
- `kappa50` 14 / 42: −0.9 / +18.6 years.
- `uncoveredEff` 0: +0.1 years.
- `numDtMin` ÷10: 0 years.
- `welfareShare` 0: 0 years.
- `aiSave` 0.2: +11 years, and the class becomes `MACHINE DOMINANCE` (D3 0.76) rather than FULL.

The revision reduced the "true by construction" part only for `uncoveredEff`, which is no longer load-bearing.

**C2. Far future set by ceilings and constants — UNRESOLVED; parameterised and disclosed rather than fixed**
- **Energy tracks `orbMax` 1:1.** Final energy is 5.0e4 / 4.8e7 / 4.8e10 TW and κ is 47.85 / 50.82 / 53.56 for `orbMax` 1e17 / 1e20 / 1e23. κ ≈ log10(`orbMax`) + 30.8.
- **Energy is identical across seeds.** It is 4.8–4.9e7 TW in all 20 seeds and in every intervention that reaches takeover, including SEVERE ENERGY.
- **The steady state is decay against a cap.** `repl*orb*room` plus launches `min(..., 0.5*pos(orbMax-orb))` balance the `-0.2*orb` decay at about 0.76 × `orbMax`.
- **Human income share is still the transfer constant.** `welfareShare` 0.3 → 0 moves H from 0.30 to **0.006** (seed 42) and from 0.33 to **0.004** (seed 1), with population unchanged (2.6e7 and 4.2e7). The explain note now says so ("welfareShare = 0.3 is a parameter"). That is disclosure, not an outcome.
- **Lineage count is the array size.** 20/20 seeds reach `maxLin = 12 = NL`; patching `NL = 24` gives 23 lineages.
- **The off-world multipliers are now parameters** (`dispersalBranching`, `spaceRelief`). Setting both to 0 still gives maximum conflict 0.0067: see N5 for the real reason conflict never happens.
- **HIGH vs LOW alignment** now differ in H (0.40 vs 0.19) and Earth energy (769 vs 13,000 TW; T 290.3 vs 307.7 K). Takeover timing is 4272 vs 4269 and population is 2.6e7 in both.

**C3. D4/D6 labels claim mechanisms that do not exist — PARTIALLY RESOLVED**
- **D4.** Renamed "Autonomous operation"; the code comment and explain note both say "The model does not separate infrastructure from other capital". It is still `auton*√(D1*D5)` and still one of the four "core criteria" of takeover.
- **D6.** Redefined as `(invAI/invTot) * D1 * D5`, which ties it to AI-financed investment — a real improvement. It is still titled "Machine self-reproduction" in `TK`. There is still no closed-loop test of whether machines could maintain fabs, robots and energy without humans. `hEff` makes production never *require* humans, by design.
- **Chart legend not relabelled.** `SERIES` still reads `['D4','infrastructure']` and `['D6','self-repro']`.

**C4. Human fate is not caused by machine dominance — PARTIALLY RESOLVED (the message was fixed; the substance was not)**

Seed 42 human population, with and without AI autonomy:

| Farming + | Baseline | No autonomy |
|---|---|---|
| 4500 | 1.52e8 | 1.53e8 |
| 5000 | 4.67e7 | 4.67e7 |
| 6000 | 1.59e9 | 1.60e9 |
| 24000 | 3.3e10 | 2.5e10 |

- Seed 1 at D1 + 1000: 4.2e7 vs 4.1e7.
- Setting `welfareShare` to 0 (humans receive 0.6% of income) leaves population unchanged.
- LOW alignment (+18 K warmer planet) leaves population unchanged (2.6e7 vs 2.6e7 under HIGH).
- **Fixed:** the famine-at-infinite-income incoherence. `synthFood` was added; minimum consumption adequacy is 2.9 at D1 + 1000.
- **Fixed:** the `popPeak` message now reads "(cause: see explain → Population)".
- **Remaining:** the long run still reaches **33–34 billion humans** (about 120× the pre-AI peak of 2.8e8), driven by pronatalist selection (`natal` → 0.709). What happens to humans is answered by a demographic subsystem that is insensitive to AI, income share and climate.

### MAJOR

**M1. Historical calibration — MOSTLY UNRESOLVED (the fossil era is partly fixed)**
- **Population:** exceeds 1 billion before evidence ends in **0/20** seeds (round 1: 1/25). Population at first computers is 155–619M.
- **Science institutions:** still appear 303 ± 47 years after industrial take-off, within ±5–12 years of electrification.
- **Cities vs industry:** "cities" now precedes industry in 20/20 seeds, but mainly because the detector threshold was cut from 0.1 to 0.05. In seed 42 the maximum regional urban share sits at 0.040–0.043 for 300+ years before industry and passes 0.1 only as industry takes off (3920–3940).
- **"States" before farming:** in seed 42 the states detector fires 330 years before the farming detector; at EPS 0.005 it fires 2,135 years before.
- **Fossil era (improved):** fossil peaks at about 1.3 TW around farming + 4080 and has collapsed to 0.04 TW by computers (clean 2.8 TW); about 3% of the fossil endowment is burned. No-fossil now delays takeover by +360 / +366 years.
- **Too fast (unchanged):** effective compute goes 2.7e15 → 7.7e25 FLOP/s in 20 years (seed 42, 4240 → 4260). D5 goes 5% → 50% in 3.5–3.7 years in 17/20 seeds.

**M2. Epistemic zones mislabelled — PARTIALLY RESOLVED**
- **Fixed:**
  - The D5 majority now fires 3.5–3.7 years after evidence ends, in 20/20 seeds.
  - Counterfactual worlds get a `COUNTERFACTUAL ·` prefix.
  - No-AI worlds now leave OBSERVED HISTORY.
- **Remaining:**
  - The MODEL-CALIBRATED TRANSITION zone lasts **1.98 years** (seed 42) and **1.90 years** (seed 2026).
  - The boundary is still a set of AI-share thresholds (`D1≥0.02 || D2≥0.05 || D3≥0.02 || D5≥0.05`) plus a new elapsed-time timer (`sinceInd > 400`, and `> 1000` for speculative). The timer ended the evidence zone in 3/20 baseline seeds.
  - At the boundary the world has 0.27 billion people and κ = 26.5. Nothing ties the zone to historical evidence.

**M3. Toggles with no effect, the wrong sign, or effects true by construction — PARTIALLY RESOLVED**
- **Fixed:**
  - Events now use per-process PRNG streams with integrated-hazard clocks (`fires()`), so counterfactuals share luck; pre-divergence histories are identical.
  - No-fossil now has the right sign.
- **Null effect:**
  - **No AI recursive research:** takeover +0.3 / −0.6 years, κ 50.49 vs 50.82.
  - **Severe energy:** delays takeover (+682 / +130 years), but total energy capture is unchanged at 4.8e7 TW.
- **Still definitional:** no autonomy and no robotics (C1).

**M4. Load-bearing constants not in `DEFAULTS` — PARTIALLY RESOLVED**
- **Moved into `DEFAULTS`:** `uncoveredEff`, `welfareShare`, `aiSave`, `aiRetain`, `aiLabBoost`, `noScalingFps`, `contestExp`, `conflictK`, `branchRate`, `dispersalBranching`, `spaceRelief`, `fusionLevel`, `spaceLevel`.
- **Still inline:**
  - The scarcity exponent `sat(...)**4`.
  - Fitness `0.5*lM` and `0.7*offworldFrac`.
  - `aiResFrac = 0.012 + 0.05*aiIncShare`.
  - Tax `0.3*reg*leverage`.
  - `protect` 0.6 / 0.3.
  - `synthFood` 0.05 and 1e8.
  - `digital` 0.3.
  - Training constant 7.9e6.
  - Evidence thresholds 0.02 / 0.05 / 0.25 and 400 / 1000 years.
- **Honest comment:** the code comment now admits "smaller shape constants ... remain inline".
- **Edit residue:**
  - Line 219: the `spaceLevel` comment ends with an orphaned "initial share of the pronatalist ... subculture".
  - Line 264: the `alignPref` comment ends with "lineage fitness gain per e-fold of compute share" (it belonged to `scaleReturns`).

**M5. Direct AI → world couplings — MOSTLY RESOLVED**
- **Fixed:** machine conflict no longer adds human mortality.
- **Fixed:** incidents are now discrete events (`x[V.comp+r] *= 1 - 0.01*isev; x[V.K+r] *= 1 - 0.002*isev`) with no `dt` dependence.
- **Remaining:** incidents still hit all regions uniformly. The capital term label `'conflict & incident destruction'` (`destroy = 0.02*confl`) includes no incident term.

**M6. Integration artifacts — PARTIALLY RESOLVED**
- **Fixed:**
  - Fossil build is capped at `0.3*fosCap + 0.01*fosMaxTW` per year; the trace is smooth, with no 1,000 TW jumps.
  - The `orbMax` overshoot is gone (maximum 0.79 × `orbMax`).
  - Steps pinned at `DT_MIN` after evidence ends fell from 47.6% to 19 ± 4%.
  - Post-takeover population is far less tolerance-sensitive (2.6e7 at EPS 0.02 vs 3.6e7 at 0.005).
  - An extinction class and detector exist.
- **Remaining — deployment chatter:**
  - Seed 42 `deploy` is 0.79 at takeover, 0.002 at +100, 0.041 at +300 and 0.33 at +1000.
  - Seed 2026 goes from 0.79 to 0.0007 within 70 years.
  - In the long run it stays at 0.007–0.18.
  - This drives the spurious collapse (N2).
- **Remaining — compute outruns power:** compute power draw reaches 109 TW against 69 TW clean capacity at farming + 4260 (util 0.976, milder than round 1).
- **New — pre-industrial timing does not converge** (N6).

**M7. Human-era decision rules become degenerate after takeover — PARTIALLY RESOLVED**
- **Fixed:** deployment now compares the AI's own marginal product `mpA` with `aiCost`, not a phantom human wage.
- **Remaining:**
  - Autonomy still uses the oversight ratio, which is identically about 1.
  - Alignment still moves only under `(1 - auton)*deploy`, so it is frozen after takeover.
  - Fitness still contains `ln(1 + alignPref*a*H)` with H set by `welfareShare`.
  - The post-takeover questions (acceleration, competition, coordination) are still answered by `orbMax`, `NL` and `algMax` rather than by interaction.

**M8. Explain panel mis-wired — PARTIALLY RESOLVED**
- **Fixed:** new `DERIVED` explanations for temperature, total energy, income per person, human share, machine population, global communication, κ and D1–D6.
- **κ check (seed 42):** explain-predicted vs observed dκ/dt at farming + 4240 / 4246 / 4250 / 4255 is 0.751 / 0.983 / 1.273 / 0.631 vs 0.768 / 1.002 / 1.342 / 0.616. It is good during the transition and off by up to 40% later (0.148 vs 0.237 at +4300).
- **Still mis-wired:**

  | Row clicked | Explains instead |
  |---|---|
  | CO₂ | `co2n` (natural only) |
  | fossil use | `fosRes` (reserves) |
  | biosphere (land NPP) | `bio` (total biomass) |
  | human authority | `auton` |
  | compute utilisation | `comp` |
  | AI lineages, HHI, machine cooperation, machine conflict, civilisation resilience | `lS` (lineage shares, which sum to 1) |
  | technology | sum of 180 levels |

- **No rate of change:** D1–D6, income per person and human share show `rate: null`, although the benchmark requires one.

**M9. Deep time is a random shift of a fixed timeline — PARTIALLY RESOLVED**
- **Still nearly fixed:** GOE − life = 1.3–1.4 Gyr in 20/20 seeds. Life appears at 0.1–1.2 Gyr.
- **Now varies:** multicellular − GOE 1.3–1.9, animals − multicellular 0.8–1.7, cognition − animals 0.3–0.8 Gyr. Mass extinctions now reduce complexity (MINOR 1), which adds variance.

### MINOR

| # | Status | Evidence |
|---|---|---|
| 1 | **RESOLVED** | Extinction line 1264: `x[V.cx] -= 0.08 * sev * pos(x[V.cx] - 1)` is live code. |
| 2 | **Mostly resolved** | The `*0` term is gone and `R.sea` is used in diffusion (line 1074). `D.lnAvg` is still computed (line 1031) and never read; `aiValuePerFlop` is dead. |
| 3 | **RESOLVED** | `hash()` now iterates every numeric or Float64Array key of `D`. |
| 4 | **UNRESOLVED** | `engel` switch at φ = 0.05, hard thresholds on `D.active`, and `epiHaz` switching on at pop > 1e4 are unchanged. |
| 5 | **Cosmetically resolved** | New classes exist, but EXTINCT and MACHINE CONFLICT were never reached in about 85 runs. COLLAPSE is reached only through chatter or a tolerance-dependent world (N2, N6). The suffixes echo the alignment input (N1). |
| 6 | **UNRESOLVED** | "Civilisation resilience" is still an ad hoc product with `min(1, lineages/3)`; "machine cooperation" = 1 − conflict; the "human authority" index is still `(1-auton)*(0.5+0.5*reg)`. |
| 7 | **RESOLVED** | Decorative stars removed; the legend now says "moving dots = diffusion (no trade is modelled)". |
| 8 | **PARTIALLY RESOLVED** | The message now points to the explain panel, but it is still one-shot: it fires at farming + 4526 (seed 42) and is never retracted while population later reaches 3.3e10. |
| 9 | **RESOLVED** | `voice(dur, build, sources)` counts oscillators (transition = 4); there is a −3 dB, ratio-20 compressor stage before the output. Not tested in a browser. |
| 10 | **RESOLVED** | The banner persists while `evidenceEnded && !speculative`, or for 9 s of wall-clock time. |

---

## B. New findings

### MAJOR

**N1. Classifier suffixes echo the alignment input rather than outcomes.**

`classify()`:
```js
if (m.humanShare < 0.1 || m.meanAlign < 0.5) return base + ' · HUMANS MARGINALISED';
if (m.meanAlign >= 0.8 && m.humanShare >= 0.2) return base + ' · SYMBIOTIC';
```
- **Baseline seeds:** all 5 labelled "HUMANS MARGINALISED" have human income share 0.20–0.28, well above 0.1. The label comes only from the seed-drawn alignment (0.43–0.497).
- **Alignment toggles:** HIGH gives SYMBIOTIC and LOW gives MARGINALISED, with identical population (2.6e7 / 2.6e7 in seed 42; 4.1e7 / 3.8e7 in seed 1).
- **Flicker:** in several seeds the label flips between FULL and SYMBIOTIC as `meanAlign` hovers at 0.8 (seed 2026 ends at 0.800 unlabelled).
- **Verdict:** these are conclusions stated by the observer, not produced by the mechanics.

**N2. The new COLLAPSE class is triggered by deployment chatter.**
- **What happens:** in seed 2026 (a required seed), deployment falls from 0.79 to 0.0007 between farming + 4268 and + 4340. Output drops from 1.97e27 to 1.22e26, and the class becomes `COLLAPSE (output < 10% of peak)` at farming + 4340, returning to FULL 8 years later.
- **Why it persists in the history:** the one-shot `collapse` detector fired and is never retracted, so the history panel reports "Civilisational collapse" for a world whose output recovered within 8 years.
- **The oscillation behind it:** `attract = sig(2 ln(mpA/aiCost))`, where the corner-regime marginal product `c·Agg/M` falls as M grows. Deployment therefore oscillates.

**N3. "Robots" do 96–100% of physical work in worlds with no computers.**
- **Observation:** no-semiconductor-scaling, seed 42, farming + 4601: `Ctot = 0`, κ = 0, D5 = 0.961. The detector reports "Robots perform the majority of physical work", and that detector also ends the evidence zone in this world (4246).
- **Mechanism:** `hEff` credits cheap robots with `uncoveredEff·robEff·(1−pi)`, and `pi` needs only the robotics *technology* level, not compute. With `robotCostFloor = 8` SU the stock floods.
- **Verdict:** the "robot" label implies a computational mechanism that is absent from the state.

**N4. Free research multiplier in the recursive loop.**
- **Code:** `aiAlg = algAI * (1 + P.aiLabBoost * D.aiIncShare)` multiplies AI algorithm-research effort by up to 4 without drawing it from any other domain's labour. `alignSci` growth has the same kind of multiplier, `(1 + D.aiIncShare)`.
- **Consequence:** it is outcome-irrelevant — no-recursive-research changes takeover by less than a year — so it is effort from nowhere that the model does not even need.

**N5. Machine conflict cannot happen because scarcity is measured on Earth only.**
- **Code:** `scarcity = sat(D.earthTW / (D.earthCap + 1e-9), 0.5) ** 4 * (1 - P.spaceRelief * D.offworldFrac)`, with `earthCap ≤ 6,375 TW`.
- **Why it stays small:** after takeover about 100% of compute is off-world, sitting at 76% of its material cap `orbMax`. That orbital constraint, the one that actually binds, never enters scarcity. With Earth use around 2,300 TW, the 4th power alone keeps scarcity near 0.07.
- **Measured:** maximum `machConflict` across 20 seeds is 0.063; with `spaceRelief` = `dispersalBranching` = 0 it is 0.0067. The `MACHINE CONFLICT` class (threshold 0.2) is unreachable.
- **Verdict:** "machines fragment but don't fight" is decided by where scarcity is measured, not discovered.

**N6. Pre-industrial history does not converge with integration tolerance, and one counterfactual's ending depends on it.**
- **Seed 42, farming → industry:** 3,937 years (EPS 0.02) / 4,176 (0.01) / 6,477 (0.005).
- **Seed 1, farming → industry:** 4,682 (EPS 0.02) / 6,689 (0.005).
- **Culture onset** also shifts by 1.9 Myr.
- **No-semiconductor world:** at EPS 0.02 it oscillates between about 0.5 and 4.8 billion people and 1–270 TW, the class flickering between HUMAN-DIRECTED and COLLAPSE within 0.2 years (farming + 14,124). At EPS 0.005 it ends at **7.4e5 people and 0.0026 TW**.
- **What holds:** the computers → takeover interval does converge (36–41 years).

### MINOR

- **N7. Elapsed-time rule in the epistemic zones.** `sinceInd > 400` and `> 1000` years since the industry detector. This is observer-only, so it is not an auto-fail, but it is a clock, and it moved the boundary by 53–62 years in 3/20 seeds.
- **N8. Relabelling is incomplete.** The chart legend still reads "infrastructure" and "self-repro"; there are the stale comments at lines 219 and 264; the destruction label names incidents that it does not include.
- **N9. "Compute utilisation" shows power utilisation (`D.util`), about 100%, while `deploy` is about 0.01.** 99% of AI compute is undeployed for about 19,500 years (seed 42 long run). The panel reads as a busy machine civilisation when it is mostly idle hardware sustained by `repl*orb*room` self-replication.
- **N10. Human welfare is decoupled from climate.** LOW alignment warms the planet to 307.7 K (+18 K above HIGH). Human population is unchanged, because incomes are astronomical and `synthFood` makes food energy-limited.
- **N11. Idle compute becomes a productivity boost.** `digital = 1 + 0.3·ln(1 + comp·(1−train)·(1−deploy)/(1e9·pop))` counts undeployed AI compute as "classical" computing. With deployment near 0 this gives about a 10× TFP multiplier, unsaturated.

---

## Required experiments (seeds 42 / 1, measured at D1 + 1000)

| Intervention | Class | Takeover vs baseline (yr) | κ | Human pop | Human share | Total / Earth energy (TW) | Off-world |
|---|---|---|---|---|---|---|---|
| Baseline (20 seeds) | FULL ×15, FULL·MARGINALISED ×5 | computers +38.5 ± 1.3 | 50.8 ± 0.1 | 19–117M | 0.20–0.34 | 4.8–4.9e7 / 501–7,616 | 1.00 |
| No fossil | FULL (seed 1: ·SYMBIOTIC) | +360 / +366 | 50.8 | 35M / 83M | 0.30 / 0.34 | 4.8e7 / 2,923 / 1,031 | 1.00 |
| No science | FULL | +616 / +1,400 | 50.8 | 0.50B / 6.3B | 0.29 / 0.34 | 4.8e7 | 1.00 |
| No global comm | FULL | +9 / +11 | 50.9 / 50.6 | 46M / 59M | 0.28 / 0.34 | 4.8e7 | 1.00 |
| No semiconductor scaling | No AI; ends COLLAPSE by oscillation (N6) | — | 15.1 / 14.5 | 0.97B / 53M (EPS 0.005: 0.74M) | 1.0 | 1.15 / 0.21 | 0 |
| No recursive research | FULL | +0.3 / −0.6 | 50.49 / 50.42 | 26M / 42M | 0.35 / 0.34 | 4.8e7 | 1.00 |
| No robotics | COGNITIVE DOMINANCE (D4 = D5 = D6 = 0 by definition; autonomy 0.85) | — | 50.5 / 50.8 | 65M / 41M | 0.62 / 0.65 | 4.2e7 | 1.00 |
| No autonomy | COGNITIVE DOMINANCE (D3 = D4 = D6 = 0, H = 1 by definition) | — | 50.75 / 50.68 | 26M / 41M (same as baseline) | 1.000 | 4.2e7 | 1.00 |
| No AI resources | MACHINE DOMINANCE (D6 = 0 by definition) | +18 / +16 | 50.8 | 26M / 42M | 1.00 | 4.8e7 | 1.00 |
| No resource limits | FULL | −43 / −17 | 50.8 / 50.6 | 50M / 80M | 0.30 / 0.36 | 4.8e7; T 296–298 K | 1.00 |
| Severe energy | FULL | +682 / +130 | 50.7 | 19M / 52M | 0.29 / 0.34 | **4.8e7 (unchanged)** / 1,297 / 766 | 1.00 |
| HIGH alignment | FULL · SYMBIOTIC | +2.8 / +1.2 | 50.8 / 50.7 | 26M / 41M | 0.40 | 4.8e7 / 769 / 688 | 1.00 |
| LOW alignment | FULL · MARGINALISED | 0 / −0.9 | 50.9 / 50.7 | 26M / 38M | 0.19 / 0.18 | 4.9e7 / 13,000; T 308 K | 1.00 |

---

## Checked and found genuinely OK

- **Determinism.** Seeds 1, 42, 1337, 2026 and 314159 give identical FNV hashes (now including `D`) at steps 1k, 20k, 40k, 60k and 70k across two runs.
- **Observer independence.** Identical hashes with `O.update()` disabled.
- **No time in the rules.** `worldRules` and `worldEvents` read no time, frame, label or step count. Event timestamps are added by `Sim.step` after the rules run.
- **Random numbers and requests.** mulberry32 with per-process streams; `Math.random` appears only in the audio code. No `fetch`, `http`, `XMLHttp`, `@font` or `import(`.
- **Numerical guard.** `Flows.add` throws on non-finite values; zero errors in about 85 runs.
- **Explain engine.** `explain()` sums literal `F.add` contributions by label, and the derived κ explanation tracks the observed rate (above).
- **Controls and HUD.** Required toggles, severe-energy and alignment controls, the speed, zoom and seed controls, and the FPS and steps/s readouts are all present.
- **Dimensions diverge.** D5 leads, D1 and D2 follow, D3 lags by about 8–18 years. No-AI-resources holds D3 at 0.72; no-robotics holds D5 at 0.

Test scripts and raw outputs are in `[local path redacted for publication]`:
- **Baseline seeds:** `b_*.json`, plus `base_42.json` (seed 42).
- **Counterfactuals:** `cf_*.json`.
- **Perturbations:** `p_*.json`.
- **Long runs:** `long_base.txt`, `long_noaut.txt`.

Not tested: in-browser FPS, audible clipping, and the live number of audio voices.

---

## Author response to round 2

Written by the author after reading the round-2 report. Final version (not re-audited after these changes): `index.html` sha256 `887ae51d…` (`evidence/final-index.sha256`); all evidence in `evidence/` was regenerated on it.

| Finding | Action |
|---|---|
| C1 (unchanged ending) | **Accepted and disclosed.** The ending follows from A21 (per-decision oversight cannot scale) plus weak reactive institutions (A19/A20). RESULTS §16 (Q19) names this as the assumption that most determines the ending; the no-autonomy/no-robotics escapes are reported as definitional. |
| C2 (ceilings) | **Accepted and disclosed**; far-future scale is reported as "set by orbMax/NL/algMax" rather than as a finding. |
| C3 (labels) | chart legend relabelled; D6 keeps the name "machine self-reproduction" with its state definition shown beside it. |
| C4 / N10 (humans decoupled from AI and climate) | **Reported as a finding** (RESULTS §16): with transfers and energy-bought food, human numbers are governed by the demographic subsystem (pronatalist selection), not by machine dominance. |
| M1, N6 (calibration, tolerance sensitivity) | reported; distributional convergence tested (12 seeds at ε = 0.005). |
| M2 / N7 (zones) | elapsed-time rule removed; purely state-based. |
| M4 | `taxK`, `digitalMax` moved to DEFAULTS; other shape constants listed in RESULTS. |
| M7 | accepted limitation. |
| M8 | remaining rows wired; derived rates via finite differences. |
| N1 | suffixes now outcome-based. |
| N2 | collapse requires 50 sustained years of 20-yr-smoothed output below 10 % of its smoothed peak (spikes no longer inflate the peak); since output counts only the Earth surface, the class is named EARTH ECONOMY COLLAPSE and becomes a suffix under machine dominance. |
| N3 | robots require control computers. |
| N4 | free multipliers removed. |
| N5 | scarcity measured where the binding constraint is. |
| N8, N9, N11 | fixed. |
| MINOR 4, 6, 8 | accepted (6: UI indices labelled "derived"). |
