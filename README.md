# Hospital Resource Allocation During Peak Demand: A Multi-Objective Optimization Approach

Staffing and bed allocation for a hospital during a demand surge, modelled as a multi-objective integer optimization problem with a queueing-based waiting-time approximation and a genetic algorithm (GA).

## Problem

A hospital plans hour by hour over a 7-day horizon (168 periods). For each period it decides how many nurses, doctors and technicians to schedule and how many ICU and general beds to open (5 x 168 = 840 integer variables), while patient arrivals fluctuate and include a multi-day surge.

## Model

- **Arrivals:** time-varying Poisson rate `λ_t` = hourly pattern x weekday/weekend factor x surge factor (1.8x on days 3-5, 0.7x at weekends) x noise.
- **Waiting time:** queueing approximation `W_t ≈ λ_t / (μ Σ_s x_{s,t} − λ_t)`.
- **Objectives (minimized):**
  1. Total patient waiting time `Z1 = Σ_t W_t`
  2. Operational cost `Z2` (staff and resource costs)
  3. Workload imbalance `Z3` (variation of staffing level across periods)
- **Constraints:** service capacity `μ Σ_s x_{s,t} ≥ λ_t` for every period, and resource limits `y_{r,t} ≤ B_r`.

## Method

Weighted-sum genetic algorithm with a penalty for constraint violations.

| Parameter | Value |
|---|---|
| Population / generations | 50 / 100 |
| Selection | Tournament (k = 3) |
| Crossover | Two-point, rate 0.80 |
| Mutation | Gaussian (σ = 3 staff units), rate 0.15 |
| Elitism | 10% |
| Objective weights (w1, w2, w3) | 0.40, 0.35, 0.25 |

A static plan (constant staffing and beds in every period) is used as the baseline. Sensitivity analyses vary the objective weights and the surge intensity.

## Repository contents

- `MILP_MOF.pdf`: full report (formulation, GA design, synthetic case study, limitations).
- `Untitled.ipynb`: plotting notebook for the figures (GA convergence, arrival pattern, baseline vs optimized allocation, weight sensitivity).

## Status

This is a synthetic case study, not an evaluation on real hospital data. The notebook currently draws the figures from illustrative values; the end-to-end GA run that produces the report's results is not yet included in this repository. Results should be read as a demonstration of the modelling approach until that code is added.

## Limitations and future work

- The optimization is deterministic: forecast arrivals are treated as known, so forecast uncertainty is ignored.
- The M/M/c assumptions (exponential service times) may not match real treatment durations, and patients are not differentiated by acuity.
- Shift continuity, staff preferences and union rules are omitted.
- Planned extensions: stochastic programming with demand scenarios, patient segmentation, G/G/c queues, staff rostering, and online re-optimization.

## Requirements

Python 3 with `numpy`, `scipy`, `pandas` and `matplotlib`.

## Author

Idris Karel Seunda Ekwe
