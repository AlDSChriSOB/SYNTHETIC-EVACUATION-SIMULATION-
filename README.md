# SYNTHETIC-EVACUATION-SIMULATION-
```markdown
# Pedestrian Evacuation Simulation

**Factorial computational study (research prototype)**  
Author: Christian Somtoo Obiechina

This repository contains a self-contained Jupyter notebook that runs a synthetic agent-based pedestrian evacuation experiment. It explores how exit width, crowd size, desired walking speed, and the presence of an obstacle affect the time required to evacuate 90 % of agents (`t90`).

> **Important**  
> All results are synthetic simulation outputs. The model is **not** calibrated or validated against real evacuation data. Findings describe the behaviour of *this* model under its stated assumptions only and should not be treated as empirical evidence about real human crowds.

---

## Overview

The experiment is a full factorial design:

| Factor              | Levels                  |
|---------------------|-------------------------|
| Crowd size (`n`)    | 40, 80 agents           |
| Exit width          | 0.8 m, 1.2 m, 1.6 m     |
| Desired speed       | 1.0 m/s, 1.3 m/s (mean) |
| Obstacle            | absent / present        |

- **24 conditions × 10 seeded replicates = 240 simulations**
- Primary outcome: time to evacuate 90 % of agents (`t90`)
- Secondary outcomes: fraction evacuated, mean evacuation time, throughput
- Statistical analysis: paired Wilcoxon signed-rank tests (Holm-adjusted) and ordinary least-squares factorial regression with two-way interactions

---

## Model summary

Agents move inside a 10 m × 10 m square room toward a centred exit on the top wall. Motion is governed by:

- Desired velocity directed toward the nearest point on the exit opening
- Inter-agent soft repulsion (separation radius 0.8 m)
- Optional circular obstacle (radius 0.5 m, centre near the exit)
- Hard speed cap (2.0 m/s)
- Euler integration with Δt = 0.05 s, horizon T = 120 s

Agents that have not exited by the horizon are treated as right-censored for the `t90` calculation.

---

## Requirements

```bash
Python ≥ 3.10
numpy
pandas
scipy
matplotlib
```

No other external packages are required.

---

## How to run

1. Clone the repository and open `evacuation_simulation.ipynb` in Jupyter / JupyterLab / VS Code.
2. Run all cells in order (or simply execute the final `main()` cell after the definitions).
3. Results are written to the `./results` directory:

| File                              | Description                                      |
|-----------------------------------|--------------------------------------------------|
| `evacuation_simulation_replicates.csv` | One row per simulation (240 rows)             |
| `agent_level_results.csv`         | Per-agent evacuation times                       |
| `scenario_summary.csv`            | Means and SDs by condition                       |
| `hypothesis_tests.csv`            | Paired Wilcoxon tests + Holm adjustment          |
| `factorial_regression.csv`        | OLS coefficients, SEs, t-statistics, p-values    |
| `t90_by_condition.png`            | Box-plot figure of `t90` by exit width and crowd |
| `run_metadata.json`               | Experimental settings and package versions       |

Typical runtime on a modern laptop: **~10 seconds** for the full 240 simulations.

---

## Key results (illustrative)

Under the model’s assumptions:

- Wider exits reduce `t90` (median difference ≈ −0.55 s for 1.6 m vs 0.8 m).
- Larger crowds increase `t90` (median difference ≈ +0.85 s for 80 vs 40 agents).
- An obstacle in front of the exit increases `t90` (median difference ≈ +0.75 s).
- Desired speed has the largest main-effect magnitude in the factorial regression.

All three primary hypotheses remain significant after Holm correction. Interaction terms involving speed are also detectable.

---

## Reproducibility

- Random seeds are fixed (`BASE_SEED = 20260101` + replicate index).
- The same seed is reused across conditions for a given replicate, enabling paired comparisons.
- All numerical settings (room size, time step, repulsion strength, etc.) are defined at the top of the notebook and recorded in `run_metadata.json`.

---

## Citation

If you use or adapt this code, please cite:

> Obiechina, C. S. (2026). Pedestrian Evacuation Simulation: A Factorial Computational Study (research prototype). GitHub repository.

---

## Licence

This code is released for research and educational purposes. The author makes no warranty regarding its suitability for any real-world safety or design application.
```
