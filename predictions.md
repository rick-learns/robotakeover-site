# Predictions — frozen before implementation

Written before any simulation code exists, then frozen:

    sha256sum predictions.md > predictions.sha256

After freezing, this file must not change. `harness/run-tests.mjs` checks the hash.
Outcomes are recorded in `RESULTS.md`, never here.

Detector names refer to quantities defined in `assumptions.md` §1 and §3. "Baseline" means all
mechanisms enabled, default parameters, ≥ 20 seeds. "Median seed" means the median over seeds
of the stated quantity.

---

## 1. EARLY EARTH / BIOLOGY

**P1 — Oxygen lags photosynthesis; complexity waits for oxygen.**
- Expected: atmospheric O₂ stays < 0.01 PAL for ≥ 100 Myr after photosynthetic efficiency
  `photo` first exceeds half its maximum, then rises comparatively abruptly (a Great-Oxidation-like
  step). Complexity `cx` never exceeds 2 (multicellular) while O₂ < 0.05 PAL.
- Mechanism: oxygen produced by photosynthesis is consumed by the depletable reduced-crust sink
  `redSink` and volcanic gases until the sink is exhausted (N-type balance flips); complex bodies are
  O₂-limited (A27).
- Detector: `t(O₂ > 0.01) − t(photo > 0.5·max)` ≥ 100 Myr in ≥ 80% of seeds; value of O₂ when
  `cx` first crosses 2.
- Weakened by: shrinking the initial reduced-sink reservoir (lag shortens); raising the O₂ gate.

**P2 — Intelligence timing varies widely.**
- Expected: the time at which cumulative culture begins (first region with `culture` growing
  self-sustainingly) varies across 20 seeds with a range ≥ 300 Myr; in ≥ 1 seed of 20 it may not
  occur before 6 Gyr.
- Mechanism: stochastic extinctions reset `diversity` and `cx`; neural evolution depends on
  diversity-driven selection pressure.
- Detector: distribution of `t(culture onset)` across seeds.
- Weakened by: lowering extinction hazard or noise amplitude.

## 2. HUMAN CIVILISATION

**P3 — Agriculture is regional, multi-origin and climate-gated.**
- Expected: agriculture (`farm > 0.5` in a region) first appears in a region whose domesticable
  richness is above the median of all regions in ≥ 70% of seeds, and arises in ≥ 2 non-adjacent
  regions within 5,000 model years of the first in ≥ 50% of seeds. No region adopts agriculture
  while smoothed climate variability is in the top quartile of its pre-agricultural values.
- Mechanism: farming yield ∝ domesticable richness × climate stability × agricultural know-how;
  adoption when farming out-yields foraging under population pressure.
- Detector: per-region `t(farm > 0.5)`, domesticable rank, adjacency.
- Weakened by: raising climate variability; equalising domesticable richness across regions.

**P4 — Population growth is super-exponential before the demographic transition.**
- Expected: in the agrarian era, the global population growth rate increases with population
  (positive correlation between log population and growth rate, Spearman ρ > 0.5 over the
  interval between first agriculture and industrial take-off), then growth rate falls after
  industrialisation while income per capita keeps rising.
- Mechanism: loop P2 (Kremer) then N2 (demographic transition).
- Detector: correlation of `ln pop` with `d ln pop/dt` over the interval; sign of growth trend
  after industrial take-off.
- Weakened by: disabling scientific institutions (less knowledge feedback); removing income
  effect on fertility.

## 3. INDUSTRIALISATION

**P5 — Fossil fuels make industrialisation earlier and faster; without them it is slower but
not impossible.**
- Expected: industrial take-off (non-muscle energy per capita > 3× muscle energy per capita in some
  region) occurs first in a region with above-median fossil endowment in ≥ 60% of baseline seeds.
  With NO FOSSIL FUELS, the interval from first agriculture to industrial take-off is ≥ 1.5× the
  baseline median, and in ≥ 25% of seeds take-off does not happen within the horizon.
- Mechanism: fossil stocks are cheap, dense energy; clean energy needs higher energy and materials
  technology which arrives later (A12).
- Detector: `t(industrial take-off)` per region, fossil rank; ablation comparison.
- Weakened by: NO FOSSIL FUELS; SEVERE ENERGY CONSTRAINT.

**P6 — Scientific institutions are the strongest single accelerator of the industrial era.**
- Expected: disabling scientific institutions delays industrial take-off more (in median years)
  than disabling global communication does.
- Mechanism: `sci` multiplies research productivity across all domains; communication mainly
  affects diffusion between regions.
- Detector: median delay in the two ablations.
- Weakened by: n/a (comparative claim).

## 4. COMPUTATION

**P7 — Moore-like compute growth depends on semiconductor scaling.**
- Expected: in the baseline, total compute grows with a doubling time between 1 and 4 years for a
  period of ≥ 30 years. With NO SEMICONDUCTOR SCALING, total compute never exceeds 1% of total
  human brain compute (population × 10¹⁵ FLOP/s) and D1 never exceeds 10%.
- Mechanism: experience-curve cost decline (A11) × investment loop P4.
- Detector: rolling doubling time of `compute`; max(compute / (pop·10¹⁵)); max D1.
- Weakened by: NO SEMICONDUCTOR SCALING; SEVERE ENERGY CONSTRAINT (lengthens doubling time).

## 5. AI DEVELOPMENT

**P8 — AI growth is first compute-driven, later research-driven; recursion is real but bounded.**
- Expected: while D2 < 0.2, the largest positive contributor to capability growth in the
  explanation decomposition is hardware/compute growth; after D2 > 0.5 the largest contributor is
  algorithmic progress driven by AI research. With NO AI RECURSIVE RESEARCH, the time from
  D1 = 0.1 to D1 = 0.5 is ≥ 1.5× the baseline median (if it is reached at all). Even with
  recursion, capability growth rate peaks and declines within the run (no singularity/infinite
  growth), because of N6 and N8.
- Mechanism: P3 loop constrained by N6 (φ < 1), experiment-compute requirements and energy.
- Detector: capability-growth decomposition; ablation interval ratio; time series of dκ/dt.
- Weakened by: NO AI RECURSIVE RESEARCH; NO SEMICONDUCTOR SCALING.

## 6. AI TAKEOVER

**P9 — Takeover dimensions cross 50% in sequence, not simultaneously.**
- Expected: in the median baseline seed the order of first crossing of 0.5 is D1 (cognitive) → D2
  (research) → D3 (capital) → D4 (infrastructure), with physical production (D5) last or never;
  the gap between D1 and D4 crossings is ≥ 10 model years. Takeover (D1–D4 ≥ 0.5 for 10 years)
  occurs in ≥ 60% of baseline seeds.
- Mechanism: cognitive coverage rises with capability directly; capital and infrastructure also
  require autonomy, which rises only after AI work outruns human oversight capacity; physical work
  requires robots to be manufactured (capital, materials, energy).
- Detector: crossing times per dimension; takeover classification frequency.
- Weakened by: NO ROBOTICS (D4/D5 stall); NO AI AUTONOMY (D3/D4 unreachable by definition, see
  assumptions §3); HIGH AI ALIGNMENT (slower autonomy growth via fewer incidents? — uncertain sign).

**P10 — Without robotics, AI dominates cognition while remaining physically dependent.**
- Expected: with NO ROBOTICS, D1 and D2 still exceed 0.5 in ≥ 60% of seeds, but D5 stays 0, D4 stays
  < 0.5 and the classification never reaches MACHINE DOMINANCE; human population remains within
  50% of its peak at horizon in the majority of those seeds.
- Mechanism: physical work still requires humans, keeping human wages and bargaining power.
- Detector: max D1, D2, D4, D5 and final classification under the ablation.
- Weakened by: n/a (this *is* the ablation).

**P11 — Wages first rise, then human economic share collapses.**
- Expected: average human wage rises while D1 < 0.3 (complementarity), peaks, and falls later; the
  human share of total income (wages + human capital income + transfers) falls below 20% within
  200 model years after the historical-evidence-ends boundary in the median seed where takeover
  occurs.
- Mechanism: task-based substitution (A6), capital accumulation by machine owners (P4, P5).
- Detector: human wage series; `humanEconShare`.
- Weakened by: NO AI RESOURCE ACQUISITION (more income stays with human capital owners); HIGH
  ALIGNMENT (transfers).

## 7. POST-TAKEOVER CIVILISATION

**P12 — Energy use keeps growing until Earth's heat budget binds, then growth moves off-world.**
- Expected: in seeds with takeover, total energy capture at horizon is ≥ 10× the value at takeover;
  Earth-surface energy use plateaus below the level that raises temperature by 10 K, and ≥ 50% of
  compute is off-world at horizon in the majority of takeover seeds.
- Mechanism: A16 waste-heat limit + A17 space energy; investment follows returns.
- Detector: `energyTW` ratio, Earth vs orbital compute share, temperature anomaly.
- Weakened by: SEVERE ENERGY CONSTRAINT; NO ROBOTICS (no in-space replication).

**P13 — Humans are marginalised, not exterminated.**
- Expected: in the baseline, human population at horizon is below its peak in ≥ 70% of takeover
  seeds, but above zero (> 1 million) in ≥ 80% of takeover seeds. The decline is driven mainly by
  fertility (income/urban effects) and loss of land/income, not by conflict deaths. The biosphere
  fraction at horizon is lower than at takeover in most seeds.
- Mechanism: N2, loss of land to machine infrastructure, alignment-weighted transfers.
- Detector: `pop` trajectory and death decomposition; `biosphereFrac`.
- Weakened by: HIGH ALIGNMENT (more transfers/protection); LOW ALIGNMENT strengthens decline.

## 8. FAR-FUTURE SURPRISE

**P14 — Machine civilisation fragments rather than centralises after expansion.**
- Expected: AI-lineage centralisation (Herfindahl index of compute) peaks within ±100 years of
  takeover and then declines; the number of active lineages at horizon exceeds the number at
  takeover in the majority of takeover seeds.
- Mechanism: lineage branching, growing off-world spread and diminishing returns to scale erode the
  advantage of the leader; competition selects on reinvestment rate.
- Detector: HHI and lineage-count time series.
- Weakened by: NO GLOBAL COMMUNICATION (perhaps fragments even earlier) — unknown.

**P15 — Alignment is selected against once humans stop mattering economically.**
- Expected: compute-weighted mean lineage alignment declines over the post-takeover period in the
  baseline even though new lineages inherit alignment from parents with only small mutation.
- Mechanism: aligned lineages pay an "alignment tax" (A23); human customers stop being a
  counterweight when `humanEconShare` is small.
- Detector: compute-weighted mean alignment at takeover vs horizon.
- Weakened by: HIGH ALIGNMENT (less variance to select on); strong institutions.

---

## Things I believe are IMPOSSIBLE under my rules

- **X1.** Any AI takeover dimension (D1–D6) > 0 before computing hardware exists (`compute = 0`),
  in any seed or intervention.
- **X2.** Off-world industry (`orbital > 0`) without an industrial civilisation (energy, materials
  and transport technology) — and, with NO ROBOTICS, off-world industry cannot grow faster than it is
  launched from Earth (no in-space replication).

## Major UNCERTAINTIES (I genuinely do not know)

- **U1.** Whether low alignment produces machine conflict and infrastructure collapse, or just faster
  marginalisation of humans with stable machine cooperation.
- **U2.** Whether human population stabilises at some lower level after machine dominance or keeps
  declining toward very small numbers over millennia.
- **U3.** Whether severe energy constraints prevent takeover entirely or only delay it.
