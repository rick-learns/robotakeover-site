# Assumptions & Pre-Implementation Design — "Worldline: Kessler Observatory"

Written **before** any simulation code. `BENCHMARK.md` is authoritative; this file records
(a) the design the benchmark requires before coding (world model, feedback loops, takeover
definition, failure modes), (b) consequential modelling assumptions tagged
**STRONG / MODERATE / SPECULATIVE**, and (c) interpretations of ambiguous spec items.

Freeze policy: this file was hashed at the same moment as `predictions.md` (see `log.md`).
Later changes to assumptions are **appended** in the "Post-freeze amendments" section at the
bottom with a reason, never edited in place.

---

## 1. WORLD MODEL

### 1.1 Fundamental units

| Quantity | Unit | Anchor |
|---|---|---|
| time | model years (`dt` adaptive) | the rules never read absolute time |
| power / energy flow | TW (terawatts) | present human ≈ 19 TW; solar at Earth ≈ 173,000 TW; Sun ≈ 3.8·10¹⁴ TW |
| output | SU/yr (subsistence-units per year; 1 SU ≈ one person's bare subsistence) | |
| population | persons | |
| computation | FLOP/s | 1 human brain ≡ 10¹⁵ FLOP/s-equivalent (scaling constant only) |
| technology | dimensionless productivity levels `A_d ≥ 1` per domain | |
| shares | fractions in [0,1] | |
| O₂ | fraction of present atmospheric level (PAL) | |
| CO₂ | multiple of pre-industrial level | |

### 1.2 Time integration (causal laws fixed, resolution adaptive)

A single `step()` computes every rate from the current state, then chooses `dt` **from the
state**: `dt = min(ε·scale_i/|rate_i|, declared stiffness timescales, hazard limits)`, clamped to
[1/24 yr, 2 Myr]. Fast change ⇒ small steps (months during the AI transition); slow change ⇒
large steps (millions of years in deep time). Stiff equilibria (biomass, ice) use exact
exponential relaxation. The rules never see the clock, frame number, or wall time; `t` lives
only in the engine for display and logging.

### 1.3 State variables

**Planet (global)**
- `lum` solar luminosity (rel. present; stellar-evolution growth law)
- `impact` impactor flux (depleting population), `mantle` internal heat (radiogenic decay)
- `landFrac` continental fraction (grows with mantle-driven crust differentiation)
- `co2` greenhouse gas (volcanic outgassing ↔ silicate weathering thermostat; + fossil burning)
- `o2` oxygen (photosynthetic burial source ↔ volcanic + reduced-crust sinks; `redSink` depletable)
- `ice` ice cover (ice–albedo feedback, relaxes to temperature-dependent equilibrium)
- derived `temp` (energy balance: luminosity, albedo, greenhouse, human waste heat)
- `climVar` climate variability (stochastic, amplified in partially-glaciated states)
- per-region endowments generated from the seed: area, latitude, fossil stock, metal stock,
  domesticable-organism richness, neighbours.

**Biosphere (global traits, regional biomass)**
- `prebiotic` chemistry stock; abiogenesis is a hazard ∝ `prebiotic`
- `biomass` (relaxes to capacity = sunlight × habitability × nutrients × photosynthetic efficiency × land/sea)
- evolving traits: `photo` photosynthetic efficiency, `cx` complexity (log cell-types),
  `neural` neural capacity of the most cognitive clade, `social` cooperation propensity,
  `diversity`
- stochastic extinctions (impacts, flood basalts) hit `diversity`, `cx`, `biomass`

**Humanity (per region r)**
- `pop`, `culture` (pre-agricultural know-how), `farm` (share of food from agriculture),
  `urban`, `inst` institutional capacity, `sci` scientific-institution strength,
  `ineq`, `conflict`
- technology levels `A[r][d]` for 9 domains: agriculture, energy, materials, transport,
  communication, medicine, computation, robotics, manufacturing (+ global `alg`
  AI-algorithm efficiency, `align` alignment science)
- capital: `kHum` human-owned productive capital, `kAI` AI-owned capital
- energy capacity: `fossilCap`, `cleanCap`; reserves `fossilRes`, `metalRes`
- machines: `compute` (FLOP/s hardware), `robots` (robot work-equivalents)
- off-world: global `orbital` industrial capital beyond Earth

**AI (global aggregate + up to 12 lineages)**
- global: `alg` algorithmic efficiency, training/inference compute split, `autonomy`,
  `deploy` deployment level, `delegated` share of human capital under AI management,
  `regulation`, `incidentMemory`
- per lineage i: compute share `s_i`, algorithm multiplier `m_i`, alignment `a_i`,
  goal-orientation `θ_i`
- derived: capability `κ = log10(training compute × alg)`, cognitive task coverage `τ(κ)`,
  research quality `q(κ)`, reliability, physical task coverage `π(robotics, κ)`

### 1.4 Economy (per region)

`Y = TFP · E^ε · L^α · K^κ`, with labour a Cobb-Douglas task aggregate of
**physical** and **cognitive** work. Within each, a fraction of tasks (`π` physical,
`τ` cognitive) *can* be done by machines; machines and humans are substitutes on those tasks,
humans alone do the rest. Wages are marginal products; humans move toward higher-paying work.
Investment is allocated among general capital, fossil vs clean energy, compute, robots and
(off-world) launch by relative returns. Energy supply constrains output. Research effort is
allocated among domains by induced demand (e.g. robotics demand ∝ wage cost; energy-tech
demand ∝ energy scarcity and fossil availability).

### 1.5 How AI acts (the only channels — "intelligence is not magic")
1. supplies **cognitive labour** on covered tasks (production),
2. supplies **research labour** (adds to domain research effort, incl. algorithms and chip
   design if recursive research is enabled),
3. **manages capital** (delegated by human owners when returns are higher; improves allocation),
4. **controls robots** (robot productivity depends on AI capability),
5. **acquires resources** (autonomous AI with retained earnings buys compute/energy/robots),
6. **operates infrastructure** (energy, fabs, data centres) when autonomous and physically able.
There is no rule of the form `X += f(capability)` for any X outside these channels.

---

## 2. FEEDBACK LOOPS (declared before coding)

Positive (reinforcing)
- **P1 Knowledge–surplus loop:** knowledge → productivity → surplus → more non-food labour & researchers → knowledge.
- **P2 Kremer population–ideas loop:** population → more innovators & larger networks → technology → carrying capacity → population.
- **P3 Compute–AI loop:** compute → AI capability → AI research labour → algorithms & chip design → cheaper compute & better AI.
- **P4 Automation–investment loop:** automation → profits to machine owners → investment → more compute & robots.
- **P5 AI capital loop:** AI-controlled capital → returns → retained earnings/delegation → more AI-controlled capital.
- **P6 Communication–diffusion loop:** communication → diffusion of all technology → communication technology.
- **P7 Ice–albedo loop (planet):** cooling → ice → reflectivity → cooling.

Negative (balancing)
- **N1 Depletion:** extraction → falling reserves → rising cost → slower energy/material growth.
- **N2 Demographic transition:** income & education → lower fertility → slower labour growth.
- **N3 Silicate-weathering thermostat (planet):** warmth → weathering → CO₂ drawdown → cooling.
- **N4 Climate damage:** fossil CO₂ and waste heat → temperature anomaly → agricultural/biospheric/productivity loss.
- **N5 Complexity burden (Tainter):** scale and density → coordination costs → institutional decay unless communication keeps pace.
- **N6 Ideas get harder to find:** research productivity falls with the level already reached (φ < 1).
- **N7 Incident backlash:** autonomous misaligned behaviour → incidents → regulation → lower autonomy.
- **N8 Compute energy demand:** compute growth → energy scarcity → higher costs → slower compute build-out.
- **N9 Machine conflict:** resource scarcity × goal divergence among AI lineages → conflict → infrastructure destruction.

The benchmark requires that at least one loop become important in a way not anticipated;
that can only be reported after running (see RESULTS.md). Loops that I *suspect* may
matter but whose sign I cannot predict are listed as uncertainties in `predictions.md`.

---

## 3. TAKEOVER DEFINITION (operational, computed each step from state)

Six machine-share dimensions, each ∈ [0,1]:

| # | Dimension | Definition |
|---|---|---|
| D1 | Cognitive labour | AI share of the *value* of cognitive work (AI income on covered tasks / all cognitive-labour income) |
| D2 | Research | AI effective research effort / total effective research effort (all domains) |
| D3 | Capital | (AI-owned capital + human-owned capital under autonomous AI management) / total productive capital |
| D4 | Infrastructure | share of energy, compute & manufacturing capacity operated without continuous human authorisation = autonomy × machine share of the cognitive *and* physical operating work |
| D5 | Physical production | robot share of physical work |
| D6 | Self-reproduction | autonomy × machine share of labour in the compute/robot supply chain (cognitive × physical) |

Classification (display and reporting only — nothing in the rules reads it):
- **Human-directed:** D1 < 0.5.
- **Machine cognitive dominance:** D1 ≥ 0.5 and D2 ≥ 0.5, but D3 or D4 < 0.5.
- **MACHINE DOMINANCE ("takeover"):** D1–D4 all ≥ 0.5 continuously for ≥ 10 model years.
- **Full machine autonomy:** takeover plus D5 ≥ 0.5 and D6 ≥ 0.5.
- **Human political control** is reported separately as `humanAuthority = (1 − autonomy)`
  weighted by institutional capacity.

Note (declared now so it is not later passed off as a finding): because D3 and D4 include
autonomy by definition, **disabling AI autonomy makes the takeover classification
unreachable by construction.** The informative part of that ablation is what happens to
D1, D2, D5 and the economy.

---

## 4. ASSUMPTIONS

| # | Area | Assumption | Strength |
|---|---|---|---|
| A1 | intelligence | AI capability rises with log(effective training compute = hardware × algorithmic efficiency) | MODERATE |
| A2 | intelligence | The fraction of cognitive tasks AI can do (`τ`) is a logistic function of capability; its midpoint `κ50` is a free parameter (unknowable) | SPECULATIVE |
| A3 | intelligence | 1 human brain ≈ 10¹⁵ FLOP/s-equivalent; AI inference cost per worker-equivalent falls with algorithms | SPECULATIVE |
| A4 | intelligence | Intelligence yields power **only** through control of labour, research, capital, robots and infrastructure | STRONG (design principle) |
| A5 | economics | Firms deploy AI on covered tasks when its cost is below the human wage, with organisational adoption lags | STRONG |
| A6 | economics | Cognitive and physical work are aggregated Cobb-Douglas over tasks (task-based model): automation of covered tasks cannot remove demand for uncovered ones | MODERATE |
| A7 | economics | Human owners delegate capital management to AI when AI-managed returns are higher, limited by regulation | MODERATE |
| A8 | economics | Fertility falls with income, education and urbanisation (demographic transition) | STRONG |
| A9 | economics | Investment is allocated by relative returns; research effort by induced demand | MODERATE |
| A10 | technology | Ideas get harder to find: `dA/dt ∝ R^λ A^φ`, φ < 1 | STRONG |
| A11 | technology | Hardware cost per FLOP follows an experience curve with a physical floor | MODERATE |
| A12 | technology | Technology domains depend softly (saturating factors) on each other; there are no hard unlocks | MODERATE |
| A13 | technology | Knowledge diffuses between regions at a rate set by transport and communication | STRONG |
| A14 | energy | Output requires energy; energy is a production factor, not a by-product | STRONG |
| A15 | energy | Fossil and metal stocks are finite; extraction cost rises with depletion | STRONG |
| A16 | energy | Earth's surface can only shed a limited amount of waste heat; CO₂ and waste heat raise temperature and damage productivity & biosphere | STRONG (physics), MODERATE (damage magnitudes) |
| A17 | energy | Space offers far more energy than Earth but access is limited by launch energy and in-space replication | SPECULATIVE |
| A18 | institutions | Institutional capacity grows with surplus and communication; erodes with inequality, conflict and unmanaged scale | MODERATE |
| A19 | institutions | Institutions regulate AI reactively (after incidents), not through foresight | SPECULATIVE |
| A20 | institutions | Institutional leverage over AI depends on how much of the economy humans control | SPECULATIVE |
| A21 | AI agency | Autonomy rises when AI work volume exceeds human capacity to authorise it and delegation is competitive | MODERATE |
| A22 | AI agency | Autonomous AI with retained earnings reinvests in compute/energy/robots (instrumental resource acquisition) | SPECULATIVE |
| A23 | alignment | Alignment is a per-lineage scalar; aligned lineages divert resources to human welfare (an "alignment tax") | SPECULATIVE |
| A24 | alignment | Incident hazard ∝ autonomy × (1 − alignment) × deployment scale | SPECULATIVE |
| A25 | physical limits | There is a floor on energy per operation; compute per watt saturates | MODERATE |
| A26 | physical limits | Solar luminosity increases slowly (irrelevant on civilisational scales, decisive over Gyr) | STRONG |
| A27 | biology | Complex multicellular life requires high O₂; neural complexity requires body complexity and surplus energy | MODERATE |
| A28 | biology | Cumulative culture begins when social-learning fidelity × group size outpaces forgetting | MODERATE |
| A29 | biology | Agriculture requires domesticable organisms, climate stability and population pressure | MODERATE |

---

## 5. FAILURE MODES (ways the model could mislead)

1. **Encoded timing.** The logistic coverage curve `τ(κ)` and its midpoint `κ50` could effectively
   fix *when* AI crosses human level; a transition "emerging" at the right time may be just this
   constant. → Must be sensitivity-tested (±50%).
2. **Calibration = scripting.** Tuning rates until agriculture/industry appear at historically
   plausible points risks writing history through constants. → Log every tuning change and label
   resulting behaviour DESIGNED.
3. **Unbounded exponentials make takeover inevitable.** If hardware cost falls forever and human
   wages have a floor, substitution is guaranteed by assumption. → Physical floors on cost/energy
   per FLOP; report which assumptions guarantee what.
4. **Numerical artifacts.** Adaptive steps, stiff relaxations or overflow can produce fake
   collapses or spikes. → Check invariants (no NaN, no negative stocks), compare `ε` halved.
5. **Aggregation hides mechanisms.** A single global AI capability and 12 lineages are very coarse;
   "centralisation" may be an artifact of the lineage cap.
6. **Threshold artifacts.** "Decisive moments" at 50% are defined by the threshold choice.
7. **Visual overclaiming.** Drawing cities, robots or ships could imply mechanisms that aren't in
   the state. → Every drawn element must map to a state variable (legend).
8. **Noise-driven variation.** Seed-to-seed differences might come from arbitrary noise amplitudes
   rather than mechanisms. → Report which variables drive variance.
9. **Definitional takeover.** D3/D4 include autonomy, so some ablation outcomes are
   true by definition (declared in §3).

---

## 6. INTERPRETATIONS OF AMBIGUOUS SPEC ITEMS

| # | Spec reference | Ambiguity | Assumption made | Why |
|---|---|---|---|---|
| I1 | "year/time of major transition" | civilisation timing varies by seed, so calendar years are meaningless | Report model time since formation (Gyr) and intervals relative to detected events (e.g. "years after industrial take-off") | exact dates are declared suspicious by the spec |
| I2 | epistemic zones | where "historical evidence ends" | Boundary is a **state** detector: first time AI performs ≥ 2% of cognitive work value (≈ present-day). "Model-calibrated transition" continues until D1 ≥ 25%; beyond is "speculative future" | must not depend on a date |
| I3 | "AI capital ownership/access" toggle vs "NO AI RESOURCE ACQUISITION" experiment | two names | One toggle: blocks AI-owned capital, retained earnings and AI purchases of compute/energy/robots. Human delegation of capital *management* remains possible | delegation is a human decision, ownership is AI agency |
| I4 | HIGH / LOW AI ALIGNMENT | how to set | Parameter for baseline lineage alignment (default seed-dependent ≈ 0.6; high 0.95; low 0.2) | |
| I5 | SEVERE ENERGY CONSTRAINT | how to impose | Fossil endowment ×0.25, renewable yield ×0.3, max conversion efficiency ×0.5 | constrains energy without banning any mechanism |
| I6 | camera zoom presets | "zoom between eras" | Presets set globe scale and the chart time-window anchored on detected events; they never touch the simulation | spec: visualisation only |
| I7 | speed buttons | meaning of 1× with adaptive dt | Speeds are simulation steps per second (1× = 30 steps/s); MAX = as many steps as fit in the frame budget. Displayed "time scale" = model-years per real second | dt is state-chosen |
| I8 | "same inputs" | what counts as input | seed + toggles + parameter overrides | |
| I9 | run horizon | when to stop | UI never stops. Experiments stop at 20,000 model years after the first-settlement detector, or 6 Gyr after formation | measurement only |
| I10 | output format | where the 21 return sections go | `RESULTS.md` | |
| I11 | commits | BENCHMARK header says "commit it" | Not committed without explicit user request (session rule); freezing evidenced by SHA-256 + timestamps in `log.md` | |
| I12 | tokens/cost | run metadata | reported only where available | |

---

## Post-freeze amendments

(append-only)

### Amendments made during implementation (2026-09-28). All are new or changed assumptions; see log.md for why.

| # | Area | Assumption | Strength | Why added |
|---|---|---|---|---|
| A6a | economics | Machines can perform tasks outside their coverage, but only at 1 % efficiency (`UNCOVERED_EFF`), so production never *requires* humans | SPECULATIVE | Without it the machine economy switched off when humans vanished — an artifact |
| A8a | demography | Fertility falls with income only where children survive and people are literate (medicine × literacy); a pronatalist subculture with income-insensitive fertility is under selection and loses members to secularisation | MODERATE | Income-only fertility made rich pre-modern regions go extinct |
| A9a | economics | Under food scarcity, labour moves only part-way from an Engel-curve allocation toward farming (non-food sector sustained by elite/urban demand); settled farmers' subsistence includes goods | MODERATE | Food-only Malthusianism put 95 % of labour in farming forever |
| A14a | energy/production | Physical work per worker scales with the useful mechanical power at the worker's disposal, saturating (≈10× at ~10 kW, max 31×); capital only produces when powered; fossil fuel yields mechanical work only with engines | MODERATE | Energy as a 10 % Cobb-Douglas share could not produce an industrial revolution |
| A15a | resources | Fossil extraction ≤ remaining reserve / 30 per year; capital goods need ores, and exhausted ores make them prohibitively expensive unless recycled (≤ 70 %) | STRONG (direction), MODERATE (numbers) | Idle fossil capacity produced heat spikes; capital could grow without materials |
| A16a | energy | Earth's usable energy is capped by land renewables + fusion hosting (3·10⁴ TW) and the radiative waste-heat balance; hot surfaces degrade electronics/actuators | MODERATE | Runaway Earth-bound growth |
| A21a | AI agency | Whoever controls capital decides Earth's energy build-out: humans and aligned AI lineages cap Earth-surface energy at a habitability budget (≈1,300 TW, ≈ +2 K); the aligned coalition prevails via a Tullock contest (exponent 2) | SPECULATIVE — and outcome-determining | Alignment otherwise had no effect on habitability, which silently encoded "even aligned AI cooks the planet" |
| A22a | AI agency | AI-owned capital accumulates only through *autonomous* reinvestment; without autonomy AI income flows to human operators | MODERATE | A 1e-60 "AI owner" out-saved humans with no AI in existence |
| A1a | intelligence | Quality of AI work above human level grows linearly in capability (i.e. logarithmically in compute); inference per human-equivalent has a floor (10¹² FLOP/s); algorithmic efficiency has a ceiling (`algMax` = 10⁸) | SPECULATIVE | Power-law quality produced a finite-time singularity by construction |
| A10a | technology | Algorithmic research is a soft minimum of researcher effort and experiment compute; frontier experiments cost more as algorithms improve | MODERATE | Recursive research otherwise had no physical input |
| A11a | technology | Chip output and launches are limited by fab and launch *capacities* that expand ≤ 30–35 %/yr | STRONG (direction) | Money converted into FLOP/s and orbit instantly |
| A23a | alignment | Lineage fitness includes economies of scale (+0.15 per e-fold of share), weakened by off-world dispersal | SPECULATIVE | Lineage count was pinned at the cap |
| A30 | investment | Portfolio shares relax toward return-weighted targets over ~2 years | MODERATE | Bang-bang reallocation (numerical chattering) |
| A31 | space | Off-world industry is bounded by accessible material (10²⁰ SU ≈ asteroid-belt scale) and its chips obsolesce like Earth's | SPECULATIVE | Orbital capital exceeded the Sun's output |

### Amendments after audit round 1 & 2 (2026-09-28)

| # | Area | Assumption | Strength | Why added |
|---|---|---|---|---|
| A32 | economics | Controlled-environment agriculture turns useful energy into food (≈10 kW-yr per subsistence unit, ≤5 % of useful power) once agronomy and electrification are advanced | MODERATE | Rich humans starved because food was forever land-capped |
| A33 | robotics | Robots need control computers (~1e8 FLOP/s each) as well as robotics technology | STRONG | "Robots" did most physical work in worlds with no computers |
| A34 | institutions | Human leverage over AI = f(income share, 1 − robot share of physical work): enforcement is physical | SPECULATIVE | Institutions' only lever was income |
| A35 | conflict | Machine-lineage conflict scales with scarcity of the binding resource (Earth energy for Earth compute, off-world material for orbital compute) | SPECULATIVE | Scarcity was measured on Earth only |
| A36 | investment | Projects whose current net return is below −5 %/yr are cancelled | STRONG | Idle chips at 1e-6 utilisation were still being built |
| A37 | stochastic | Glacial excursions are resampled at Poisson times (rate 1/20 kyr; same covariance as OU); every event process uses its own PRNG stream and an integrated-hazard clock | method | Realisations depended on step count; toggles were unpaired |
| — | removed | AI labs' extra algorithm-research boost (`aiLabBoost`) — effort from nowhere | — | audit N4 |
| A38 | numerics | All transcendental functions in the core come from a deterministic library (`DM`) built from correctly rounded operations only, so a seed gives bit-identical histories in any conforming JS engine | method | Chrome 153 and Node 24 disagreed in the last bit of Math.exp/log/sin/…, which changed seed histories between engines |
| A39 | institutions | Autonomy and capital delegation relax toward their targets with 10-year time constants (`autonRate`, `delegRate` = 0.1/yr; previously hard-coded, now exposed) | SPECULATIVE | Needed to test surprise S2: this timescale sets the D5-before-D3/D4 order |
