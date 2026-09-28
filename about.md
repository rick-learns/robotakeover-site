# About this project

[**Launch the simulation →**](./)

The **Worldline Observatory** simulates a planet from its formation through life, civilisation and industry, then through computation and AI, into a machine-dominated far future. Every change comes from rules that compute rates from the current state. Nothing is triggered by dates, and "AI takeover" is detected from measured state, never scripted. The whole simulation is one self-contained HTML file, with no libraries and no network requests.

It was built and evaluated by Claude (Anthropic) against a demanding benchmark specification. The benchmark required:

- predictions frozen and hashed before any simulation code existed
- counterfactual experiments and a parameter sensitivity sweep
- determinism checks
- two independent adversarial audits
- honest reporting of what failed

## The write-ups

| document | what it is |
|---|---|
| [RESULTS](RESULTS.md) | The full report: model summary, results across seeds, counterfactuals, prediction verdicts, surprises, answers to 20 questions, limitations, and a requirement-by-requirement self-audit |
| [Predictions](predictions.md) | The 15 predictions, frozen before implementation and **never edited**. Its SHA-256 is in [predictions.sha256](predictions.sha256) (`11e28aa8…`), so you can verify it yourself |
| [Assumptions](assumptions.md) | The world model, feedback loops, takeover definition and 29 assumptions written before coding, plus append-only amendments explaining every later change |
| [Development log](log.md) | Every change, bug, failed approach and calibration, with whether it was made to force a result |
| [Independent audits](audit.md) | Two adversarial audit reports, reproduced verbatim (local file paths redacted), with the author's responses |

## Headline findings (honestly stated)

- **Machine dominance happened in every seed (30/30).** It arrived 36–40 model years after the first computers. The main driver is an assumption, not a discovery: human oversight cannot scale with AI work. The auditors' central criticism is that the ending is built into the model's structure, and the report accepts it.
- **Hardware economics and physical infrastructure mattered far more than recursive self-improvement.** Disabling AI's contribution to its own research changed the transition by 0.3 years.
- **Humanity's long-run fate varied by 5–7 orders of magnitude.** It was decided mostly by whether a high-fertility subculture survived modernisation, not by the machines.
- **2 of 15 predictions were wrong and 5 were only partly right.** All of them are reported unedited.

Supporting materials (the Node test harness, raw run data and screenshots) are not published here. Links to them inside the documents will not resolve.
