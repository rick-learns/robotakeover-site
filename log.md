# Engineering / Research Log

Append-only. Newest entries at the bottom.

## 2026-09-27
- Repo scaffolded.

## 2026-09-27T23:31Z — Pre-implementation freeze
- Read BENCHMARK.md (27,702 bytes; untouched). All other files were scaffold stubs; `index.html` contained no code.
- Wrote `assumptions.md`: world model, units, adaptive-dt scheme, 7 positive + 9 negative loops, six-dimension takeover definition (D1–D6), 29 tagged assumptions, 9 failure modes, 12 spec interpretations.
- Wrote `predictions.md`: 15 predictions across the 8 required categories, 2 impossibles, 3 uncertainties.
- FROZEN: `predictions.md` sha256 = 11e28aa850acb469465f2e5d0b657e68635205c232701dea95e156848bd14ca1 (stored in `predictions.sha256`).
- Snapshot hash of `assumptions.md` at freeze = 76595320a3281085da3b66ba8c19d26d75e6d0936ebba976c93bf152568d7e8c (`evidence/assumptions.at-freeze.sha256`). Later changes go to its append-only amendments section.
- No git commit made (session rule: commit only on explicit user request); freeze evidenced by hash + this timestamp.
- Design decision: the simulation core lives in `<script id="sim-core">` in index.html with no DOM access, so the Node harness executes the *identical* code that ships (extracted from index.html at test time) — no separate build.

## 2026-09-27/28 — Implementation of sim-core & first calibration pass (≈60 iterations)
Harness: `harness/lib.mjs` extracts `<script id="sim-core">` from index.html and runs it in Node (`vm.runInThisContext`; a contextified sandbox was 10× slower). Dev probes live in the session scratchpad; only reusable tools are kept in `harness/`.

Engine decisions
- Adaptive dt from rates (ε = 2 % relative change), semi-implicit stiffness `Δ = r·dt/(1+k·dt)`, log-space integration for positive quantities, quasi-steady flag for Malthusian/pre-cultural populations and investment shares (their transients do not limit dt). No rule reads the clock.
- Bug: population pinned at its floor with a negative rate throttled dt to weeks in deep time → pinned variables no longer limit dt.
- Bug: `climS` smoothed a noise sample redrawn every step → replaced by instability read directly from the OU ice-fluctuation state. Slow cycles (ice, weathering, biomass) use the *mean* climate; only organisms/people see the stochastic excursion.

Deep time (all DESIGNED calibration unless noted)
- Linear ice response gave loop gain ≈1.2 everywhere → permanent snowball↔hothouse cycling every 100–300 Myr (FAILED approach). Sigmoid ice response: stable small caps, runaway only at large extent.
- Extinctions reduced total complexity faster than it evolved → only multicellular complexity (cx−1) is lost.
- Neural capacity rate ×22 and a metabolic ceiling (N ≤ 8) — calibrated so large-brained animals appear at ~4.2–4.5 Gyr. DESIGNED.
- Result after calibration: oceans 0.07, life ~0.2, O₂>1% PAL ~1.6, eukaryote-grade ~2.35, multicellular ~3.1–3.3, animals ~3.8–4.1 Gyr.

Culture → agriculture → states (several FAILED attempts recorded)
- Animals were running an economy and "doing research" (tech accumulated for Myr before culture) → economy/research now require cumulative culture (culture > 3, or farming know-how) — the ">3" threshold separates non-cumulative animal traditions (equilibrium ≈1) from cumulative culture.
- Extinction events lowered *human* neural capacity even when humans numbered millions → a numerous, widespread clade survives with its traits.
- Malthusian dynamics bound only on food → 95 % of labour farmed, no cities. Now subsistence binds on total consumption (food + goods for settled farmers) and labour follows an Engel-curve allocation that moves only part-way toward farming under scarcity (non-food sector sustained by elite/urban demand). Assumption, DESIGNED.
- Farming know-how had no learning-by-doing → added; agronomic science later supersedes traditional know-how and buffers climate swings.
- "Watermill utopia" bug: hydro potential 20× too large → one region got rich, its fertility collapsed (income-only fertility rule) and it went extinct. Fixes: realistic pre-industrial hydro; fertility decline now requires modern medicine × literacy as well as income (demographic transition driven by child survival and education).
- Technology prerequisites formed a deadlock (metallurgy needed energy-tech and vice-versa, both at zero) → level-coupled prerequisites plus absolute thresholds only for computation (electrification) and robotics (computation).

Industrialisation
- Structural gap: energy was a 10 %-share input, so a steam engine was worth one labourer. Physical work now scales with *useful mechanical power per worker* (saturating: ≈10× at ~10 kW, max 31×); capital is capped by the power available to run it. Fossil fuel is useful only with engines (energy technology). This produces high energy value where workers lack power (Allen-like) without naming any place. DESIGNED mechanism, calibrated.
- Goods TFP elasticity to technology reduced twice (outputs per worker were 25× too high).

Computation & AI (several runaway failures)
- Moore's law too slow (8 %/yr) → FLOP/s per SU ∝ A_comp^10 (log-space; overflowed to Infinity once — bug fixed), energy/FLOP Koomey-like with a 1e-18 J floor.
- **Unearned recursion found analytically**: AI quality ∝ compute^0.15 and no inference floor made d ln(alg)/dt ∝ C^0.575·alg^+0.075 — a finite-time singularity by construction. Now quality is linear in capability (log compute, per frozen A1) and inference has a floor (1e12 FLOP/s per worker-equivalent); algorithmic efficiency has a ceiling (algMax, to be sensitivity-tested) and research is a soft-min of researchers and *experiment compute* (frontier experiments cost more as algorithms improve).
- Missing physical constraints added after runaways: chip-fab capacity (≤35 %/yr expansion), launch capacity, machines cannot draw more power than generated (idle beyond), energy–capital complementarity, materials depletion raising capital-goods prices, radiative (Stefan–Boltzmann) waste-heat balance, machine productivity loss on a hot surface, hard caps on land/fusion capacity and on accessible off-world material (orbMax), orbital chips obsolesce like Earth chips.
- Orbital accounting bug: space energy valued as if every watt became compute at the J/FLOP floor → orbital compute now pays for chips + power.

Post-takeover (structural artifacts found and removed)
- The machine economy lived inside human regions: when a region's humans fell below 100, its robots/compute/capital switched off ("machines collapse because humans died"). Regions now stay active if machine capital exists; machines can perform uncovered tasks clumsily (1 % efficiency) so production never *requires* humans. New assumption (A6 amendment).
- Earth habitability was ignored by even well-aligned AI because alignment only affected income transfers — an encoded conclusion. Energy build-out is now decided by capital controllers: humans and aligned lineages cap Earth's heat budget (~+2 K); the aligned coalition prevails via a Tullock contest (exponent 2). DESIGNED; the resulting alignment→human-survival link is therefore partly designed and will be reported as such.
- "AI owner" with a 1e-60 initial share out-saved humans for 1,500 years with no AI in existence → retained AI income now scales with autonomy.
- Investment switched bang-bang between options post-takeover (chattering, weekly steps) → allocation shares are state variables relaxing toward return-weighted targets over ~2 yr.

Was any change made to force an expected result? The calibration of deep-time and pre-industrial *timing* is DESIGNED (rates tuned so the sequence happens within the Sun's habitable window and in a plausible order). No change was made to produce takeover, a particular human fate, or any frozen prediction; several changes (fab/launch bottlenecks, experiment compute, alignment→habitability) *slow* or *condition* the AI outcome. The alignment→habitability link will be classified DESIGNED wherever it drives an outcome.

## 2026-09-28 — UI, harness, first 30-seed baseline, and changes made AFTER seeing results
- UI: globe texture painted from regional state each frame (land cover, farmland, built-up, machine land, night lights by energy per area and machine share), overlays for cities/compute/robots/communication/orbital/lineages/conflict; takeover & post-takeover dashboards; explain panel (literal rate terms from the flow log; composite decompositions for κ and D1–D6); detector-driven history; epistemic zone label + evidence-boundary banner; Web Audio with 24-voice cap, compressor, rate limiter, mute/volume, starts on user action.
- Bug (rendering): region markers were projected with the opposite longitude convention to the texture → fixed.
- Static check found `S.t` inside the WORLD RULES block (event-log timestamps) → timestamps now added by the engine after the rules run.
- Harness: `cdp.mjs` (headless Chrome via DevTools protocol, file:// load), `analyze.mjs` (measurement only), `pool.mjs` (15 worker threads), `multiseed`, `ablation`, `sensitivity`, `determinism`, `capture`, `report`.

First baseline (30 seeds) — what it showed and what was changed afterwards:
1. Measurement bug: "peak warming 1,504 K" included the molten early Earth → peak warming now measured in the human era only. (measurement fix)
2. **Habitability ceiling was not enforced**: aligned controllers set a ~2,300 TW Earth-energy cap but capacity overshot to 3,000–22,000 TW (build limit ignored headroom; lagged portfolio shares kept funding capacity; nothing decommissioned excess). Aligned worlds were running at +17 K mean / +50 K peaks. Fixed: construction limited by headroom; excess capacity decommissioned. This is a bug fix of a DESIGNED mechanism, and it changes outcomes a lot (aligned worlds now stay within ~+1–2 K).
3. **Lineage count pinned at the cap (12/12 in all 30 seeds)** — the P14 "fragmentation" result was an artifact of the lineage cap (declared failure mode #5). Added economies of scale to lineage fitness (+0.15·ln share), weakened by off-world dispersal (coordination costs). DESIGNED; added after seeing the result, and it works *against* frozen prediction P14.
4. Kept as genuine findings (not fixed): robots cross 50 % of physical work before AI crosses 50 % of cognitive work (P9 order wrong); alignment is selected *for* after takeover in most seeds (P15 wrong); a strong bimodality in human fate tied to alignment at takeover.
- Second baseline (after the habitability fix): 30/30 takeover; alignment rises after takeover in 29/30 (0.62 → 0.93 median); humans end at 5.6e8–3e10 except seed 191 (alignment 0.54 → 0.18, humans 6e5). Lineage count still ends at 10–12 (cap still binding at the end even with scale economies; HHI 0.25–0.74) — reported as a partial artifact.
- Bug: per-step peak warming of 55–123 K lasting < 2 years. Cause: post-AI energy value made controllers build fossil *capacity* on nearly exhausted reserves; capacity × availability occasionally produced ~1e5 TW. Fix: extraction ≤ reserve/30 per year (field decline), returns account for idle capacity, construction limited by that headroom. Peak warming for seeds 42/107 now 2.0 K / 10.6 K. (bug fix; changes outcomes)
- Exposed `natal0` and `alignPref` as parameters (no behaviour change at defaults) so surprises can be removed by intervention.
- Independent auditor launched on index.html sha256 97048ffa… (before the fossil fix and parameter exposure).

## 2026-09-28 — Response to independent audit round 1 (see audit.md)
Fixed (bugs): extinction complexity-loss line had been swallowed by a comment; machine conflict killed humans with no mechanism (removed) and destroyed regional capital (now only contested compute/orbital capital); incident damage used the previous step's dt (now an impulse at the event); fossil capacity had no build-rate limit; population floor made extinction unrepresentable (effective population, extinction detector & class); explain panel mis-wired for 7 rows (derived decompositions added for temperature, energy, income, human share, machine population, communication; D-drivers unit-normalised; κ includes orbital compute); voices counted per voice() call rather than per source, no brick-wall limiter; banner wall-clock timed; legend overclaims (decorative stars removed, "flows" relabelled as diffusion); dead code; hash omitted lagged derived state.
Fixed (model): D4 relabelled "autonomous operation" with an honest message; D6 redefined from state (AI-financed × machine-built capital formation); epistemic zones now leave OBSERVED HISTORY on any dimension (D1≥2 %, D2≥5 %, D3≥2 %, D5≥5 %, population >1.2e10, or >400 yr of industry) and counterfactual worlds are labelled COUNTERFACTUAL; final-state classes now include HUMANS EXTINCT, COLLAPSE, SYMBIOTIC, HUMANS MARGINALISED, MACHINE CONFLICT; AI deployment compares AI cost with the marginal product of AI labour (not a phantom human wage); severe energy constraint also halves off-world conversion; controlled-environment agriculture (energy → food, ≈10 kW-yr per SU) so rich humans are not forever land-limited; sea links made causal (seafaring diffusion); 13 inline constants moved into DEFAULTS; negative-return investment cancelled (idle compute at 1e-6 utilisation was still being built); launches limited by remaining off-world material (orbMax overshoot & chattering); world-relative accuracy scales; human leverage over AI now includes physical enforcement capacity (1 − robot share of physical work).
Fixed (calibration, DESIGNED): pre-electric hydro ≈10 % of electric-era potential; industry detector counts only engine/electric work; science and urbanisation faster; fertility decline needs more medicine than mortality decline (population boom first); "cities" detector at 5 % urban.
Numerics: steps pinned at DT_MIN after the transition fell from 53 % to 9 %; steps per run −58 %. **Stochastic events now use per-process PRNG streams and integrated-hazard clocks, and climate noise is a Poisson-resampled jump process** (same variance/autocorrelation as OU) — realisations no longer depend on step count, and counterfactuals share random numbers where hazards agree. Convergence: halving DT_MIN changes nothing; tightening ε shifts deep-time integration by ~5–10 Myr, which changes which glacial excursions early farmers meet → pathwise agrarian timing is sensitive (not monotone in ε); convergence is therefore assessed distributionally (12 seeds at ε = 0.005 vs 30 at 0.02).
Accepted as limitations (reported, not engineered away): takeover robustness follows from the oversight-bottleneck assumption (A21) and weak reactive institutions (A19/A20); far future saturates at physical ceilings (orbMax, algMax, fpsMax, NL); deep-time variance mainly from the abiogenesis draw; lineage count still reaches the cap in most runs.

## 2026-09-28 — Response to independent audit round 2 (see audit.md)
- Pipeline stopped mid-ablation (evidence would have been for superseded code); all evidence regenerated on the final index.html (sha256 3854ef80…, `evidence/final-index.sha256`).
- N1: class suffixes now describe human outcomes (income share, human numbers vs peak, warming < 5 K), not the alignment input.
- N2: COLLAPSE requires output < 10 % of peak for ≥ 50 model years (class and detector).
- N3: robots need control computers (≈1e8 FLOP/s per machine); no computers → no robot coverage.
- N4: free research multipliers removed (`aiLabBoost` deleted; alignment-science boost removed). This further weakens the recursive loop.
- N5: machine-conflict scarcity now measured where the binding constraint is (off-world material for orbital compute, Earth energy for Earth compute).
- N7: epistemic boundaries are purely state-based (AI shares, population); the elapsed-time rule was removed. An energy-per-person criterion was tried and dropped because the model's industrial era is ~6× too energy-intensive per person (calibration gap reported in RESULTS).
- N8/N9/M8: chart legend relabelled; stale comments fixed; "compute running AI (powered × deployed)"; remaining explain rows (CO₂, fossil, authority, lineages) get derived explanations; derived quantities without a flow-based rate now show a finite-difference rate from the sampled history.
- N11: classical-IT productivity gain saturates (max +100 %), parameter `digitalMax`.
- M6 deployment chatter: gentler response to the AI marginal-product/cost ratio and extra stiffness for the deployment → marginal-product feedback.
- New observation (not fixed, reported): the no-semiconductor counterfactual shows ~1,200-year boom–bust cycles and ends in a sustained resource-exhaustion collapse (ores depleted, recycling ≤ 70 %); numerically sensitive.
- Observer fix (after ablations): the COLLAPSE class compared output with a *transient spike* peak (post-takeover SU output spikes ~100× for < 2 yr), so 11/20 no-resource-constraint worlds and 20/20 no-scaling worlds were labelled COLLAPSE partly spuriously. Output is now smoothed over 20 yr before comparison. Observer-only change (rules untouched); baseline and ablations re-run. Also fixed a measurement bug in `analyze.mjs` where a comment swallowed the machine-conflict peak update. Final index sha256 recorded in `evidence/final-index.sha256`.
- Found by paired comparison: without resource constraints fossil fuel never runs out and CO₂ keeps rising (~+20 K); the habitability rule caps heat-producing capacity but not fossil CO₂ — a known limitation, reported.

## 2026-09-27 22:25 — Evidence regeneration on the final index
- The pipeline (`harness/all.mjs`) started at 21:44, before the collapse-smoothing observer fix (index edited 22:12). Its baseline and ablation phases therefore used the old observer. The sensitivity workers loaded the core at 22:07 and also use the old observer throughout (internally consistent). The observer is read-only, so dynamics and hashes are unchanged; only class labels (COLLAPSE) and observer-derived fields can differ.
- The 30-seed baseline and 20-seed ablations are being rerun on the final index, in parallel with the remaining pipeline phases. The later phases (ε = 0.005 baseline, surprises, convergence, determinism, capture, report) load the final index.
- Found while checking RESULTS numbers: the old `baseline.json` has `peaks.machConflict = 0` in every seed (the comment-swallowed line in analyze.mjs predates the fix). The series show a peak machine conflict of 0.04–0.16. The rerun records it correctly.
- 23:12 — the rerun left one baseline seed (219) labelled COLLAPSE. Checking it showed that the observer's output `Y` is **Earth-surface** output only, since orbital industry is not in `Ysum`. Seed 219's surface economy really does collapse: humans fall to 7·10³, temperature reaches 322 K and alignment drops to 0.2. Meanwhile the orbital machine economy stays at its ceiling (4.8·10⁷ TW). Calling the whole world "COLLAPSE" was misleading narration.
- Observer-only relabel:
  - A world that is not machine-dominated gets `EARTH ECONOMY COLLAPSE (…)`.
  - Under machine dominance, the collapse becomes a suffix, `… · EARTH ECONOMY COLLAPSED`.
  - The detector text now says "Earth-surface economy (orbital industry excluded)".
  - Dynamics are unchanged. New index sha256 `94b20397…`.
- Because of the relabel, the baseline and ablations are being rerun a second time. The pipeline phases that start after 23:12 (ε = 0.005 baseline, surprises, convergence, determinism, capture, report) load this version. The sensitivity sweep keeps the older labels; it is analysed on numeric outcomes, not class strings.

## 2026-09-28 00:40 — Cross-engine determinism failure found and fixed; surprises corrected
- **Failure (pipeline determinism phase).** Repeated runs, no-observer runs and no-logging runs were bit-identical within Node, but **Chrome 153 and Node 24 gave different hashes for the same seed from step 0**.
  - Diagnosis: fuzzing 20,000 inputs per function showed `Math.exp/log/sin/cos/tan/atan/atan2/asin/acos/cbrt/tanh/expm1/log1p/…` differ in the last bit on 5–15 % of inputs between V8 15.3 (Chrome) and V8 13.6 (Node).
  - Only `pow`, `sqrt` and `hypot` agreed. ECMAScript leaves these functions "implementation-approximated". The earlier Chrome cross-check passed before Chrome auto-updated.
  - Consequence: the chaotic agrarian era amplifies last-bit differences, so an evaluator opening a seed in their browser would have seen a different history from the one reported.
- **Fix.** `DM`, a deterministic math library inside the sim core (`exp`, `log`, `pow`, `sin`, `cos`, `atan2`, `asin`, `acos`), is built only from correctly rounded operations (+ − × ÷ `sqrt` `Math.round` `Math.floor`) and exact DataView bit manipulation. Measured accuracy against native: exp/sin/cos ≤ 1 ulp, log ≤ 1.7 ulp, atan2/asin/acos ≤ 3 ulp, and pow with non-integer exponents ≤ 33 ulp (7e-15 relative). All 124 transcendental call sites and 13 `**` operators in the core now use it.
  - Result: Chrome and Node hashes are identical at every checkpoint from step 0 to 50,000.
  - Cost: 19–25 % fewer steps per second.
  - This changes histories at the last bit, the same as a different noise realisation, so it does not change the rules.
- **Parameters exposed (defaults unchanged; the seed-42 hash with defaults is identical before and after):**
  - `autonRate` = 0.1/yr and `delegRate` = 0.1/yr (adjustment speeds of autonomy and capital delegation)
  - `alignSciRate` = 0.002 (alignment-science growth)
  - They were needed as removing interventions for the corrected surprises.
- **Surprise definitions corrected (pre-fix reproduction runs showed two claims were wrong):**
  - **S2** was stated as "robots before cognitive work", but D5 crosses 1.2 yr *after* D1. The actual surprise is robots before capital (D3, +6.5 yr) and autonomy (D4, +8.5 yr).
    - Robot cost ×100 moves D5 by only +1.5 yr.
    - With `autonRate = delegRate = 3/yr`, D3 and D4 cross before D5 (seed 42: +1.1 yr).
    - So the order is set by the **assumed 10-year institutional adjustment time**, which is DESIGNED, not discovered.
  - **S3** was stated as "alignment selected FOR by human customers", but `alignPref = 0` does not remove the post-takeover rise. A flow decomposition (seeds 1, 42, 1337) shows the rise comes mainly from **residual human-controlled training**: autonomy saturates at gap × reliability × (1 − regulation) ≈ 0.95, and the training target keeps rising with alignment science. Selection is mixed in sign (seed 1: −0.07; seed 42: +0.05).
  - **S4**'s mechanism, from the same decomposition (seed 1337: selection −0.61): as the human income share falls, the alignment tax outweighs customer preference. `alignTax = 0` was added as a second removing intervention next to HIGH alignment.
- **Evidence regeneration, limited by the time available (user decision):**
  - Regenerated on the final index: the 30-seed baseline (`baseline-final.json`, used to check that distributions match), surprises, determinism (now parallel, Chrome check extended to 120k steps) and the Chrome capture.
  - Not regenerated: the ablations, sensitivity sweep, ε = 0.005 run and pathwise convergence. They stay on the previous build (`94b20397…`, native Math) and are paired with that build's `baseline.json`. This is disclosed in RESULTS.
- 01:20 — Final-build results: determinism PASS, including Chrome = Node at 120k steps. Capture: 204–210 FPS, 0 network requests, 0 console errors. The baseline distributions match the previous build; 11/30 individual seed classes changed, reported as a limitation. All surprises reproduce, each with a removing intervention.
- Visual inspection of the final screenshots found that "compute running AI (powered × deployed)" shows 0.0 % after takeover, because it counts Earth-deployed compute only. Not fixed; disclosed in RESULTS §18.
- The predictions.md sha256 still matches the freeze (`11e28aa8…`). RESULTS.md was written.
