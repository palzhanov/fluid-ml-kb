# Shortcomings and research directions

status: first-pass (to argue about)

Each direction lists the gap, the evidence for it in this repo, a first experiment, and the risk. **Novelty is not checked yet**: our knowledge of the literature runs to about mid-2026 and is incomplete, so every item needs a targeted literature search before we invest in it. Mark items `checked-novelty: yes/no` once done.

## Cross-cutting shortcomings

1. **Two communities, two benchmark cultures.** ROM papers test on few, specific engineering problems with few training solves; operator-learning papers test on large generic suites. Almost nobody runs both kinds of method on the same data at the same budget (see taxonomy.md, "Two research traditions").
2. **Weak baselines.** Speedup claims are rarely at equal accuracy against a strong solver [mcgreivy2024weak]. The same applies against strong ROM baselines (stabilized POD-Galerkin + hyper-reduction).
3. **Almost no error certification outside certified RB.** Every ML row in `comparison.csv` has `error_certification = no`.
4. **Time-varying inputs are rare.** Most operator-learning benchmarks are initial-value or steady problems. Only LDNet and operator inference (plus control-oriented ROMs) treat inputs u(t) as first-class, which is exactly what control, digital twins and coupled multiphysics need.
5. **Geometry and dynamics are handled by disjoint method sets.** Methods that handle varying geometry (GINO, GNOT, Transolver, CORAL, MeshGraphNets) are either steady or full-field autoregressive; methods with compact latent dynamics (LDNet, LaSDI, Koopman AE, LED) assume fixed geometry.
6. **Deterministic small latents for chaotic flows.** Turbulent and transitional flows are not well described by a small deterministic ODE; generative models handle this but at high cost and on full fields.

## Candidate directions

### D1. Geometry-conditioned latent dynamics (LDNet × neural-field operators)
- **Gap:** shortcomings 4 + 5. No row in `comparison.csv` has `geometry_varies = yes` **and** `time_treatment = continuous-time latent ODE` **and** `inputs = time-varying`.
- **Idea:** condition LDNet's reconstruction network (and possibly its dynamics network) on a geometry code from an SDF/point-cloud encoder (as in GINO/CORAL), so one model covers a family of domains under time-varying inputs.
- **First experiment:** 2D unsteady flow past parametrized obstacles (shape family) with time-varying inflow; compare against LDNet per-geometry, DINo/CORAL, and Transolver autoregressive.
- **Risk:** the latent ODE must now absorb geometry-dependent dynamics (e.g., shedding frequency); may need geometry in NN_dyn too. ⚠ novelty to check.

### D2. Data assimilation for encoder-free latent models
- **Gap:** LDNet has no encoder, so it cannot correct its latent state from new measurements; encoder-based models need full snapshots.
- **Idea:** treat s(t) as the state of a filter (EnKF/particle filter or a learned variational filter) with NN_rec as the observation operator at sensor points. The latent dimension is small, so ensemble methods are cheap.
- **First experiment:** twin experiment on an LDNet test case with noisy sparse sensors and a perturbed input signal; measure recovery.
- **Risk:** low. This feels publishable as a methods + application paper (cardiac or flow digital twin). ⚠ novelty to check (LDNet follow-ups from the Milan/EPFL groups may already do this).

### D3. A posteriori error estimation for ML ROMs
- **Gap:** shortcoming 3.
- **Idea:** because LDNet/neural-field decoders are differentiable in x, the PDE residual of the decoded field can be computed cheaply by autodiff; use a dual-weighted or residual-based estimator, or conformal prediction calibrated on held-out solves, to produce error bars online.
- **First experiment:** correlate residual norm vs true error on held-out inputs for LDNet and DL-ROM; then calibrate.
- **Risk:** residual norm may correlate poorly with error for convection-dominated problems; certification in the RB sense is likely out of reach, but calibrated bounds are useful.

### D4. A common lens: learned reduced representations
- **Gap:** POD modes, DeepONet trunk bases, Transolver slices, LDNet's latent state and INR codes are all learned reduced representations, but are analysed in separate frameworks.
- **Idea:** a survey/benchmark paper measuring each on the same problems by intrinsic dimension needed for a target accuracy, n-width behaviour on advection-dominated flows, and online cost at equal accuracy (per McGreivy & Hakim).
- **Risk:** large engineering effort; high value as a community resource. Could start small (3 problems × 6 methods).

### D5. Stochastic / generative latent dynamics for chaotic flows
- **Gap:** shortcoming 6.
- **Idea:** keep LDNet's compact latent + neural-field decoder, but make the latent dynamics stochastic (latent SDE, or diffusion/flow matching on latent trajectories). Generative cost is paid in a tiny latent space, not on full fields.
- **First experiment:** Kolmogorov flow or turbulent channel statistics; compare spectra and long-time statistics against ACDM/PDE-Refiner at matched compute.
- **Risk:** a small latent may still be insufficient; decoder must produce small-scale detail (might need a stochastic decoder too). ⚠ novelty to check (latent diffusion for PDEs exists in some form).

### D6. Structure-preserving latent dynamics
- **Idea:** constrain NN_dyn to be stable/dissipative (Lyapunov-constrained, port-Hamiltonian, or contractive), so long-horizon rollouts under unseen input signals stay bounded.
- **First experiment:** stress-test LDNet with long and out-of-distribution input signals; compare unconstrained vs constrained.
- **Risk:** constraints may hurt accuracy on genuinely unstable dynamics (vortex shedding onset).

### D7. Physics-informed training of latent dynamics with few solves
- **Idea:** add PDE residuals of the decoded field (autodiff in x, finite differences in t through the latent ODE) to the LDNet loss to reduce the number of required high-fidelity simulations.
- **Risk:** PINN optimization pathologies return; may help mainly in the very-few-data regime.

### D8. Benchmark with time-varying inputs (ROM-style) for the ML community
- **Gap:** shortcoming 4; PDEBench-like suites are mostly initial-value problems.
- **Idea:** release a dataset of parametrized flows driven by time-varying inputs (inflow profiles, actuation, moving boundaries) with strong classical baselines included.
- **Risk:** dataset papers need sustained maintenance; but this could anchor D1, D3, D5.

## Our discussion
_Add your reactions below, dated. Disagreements welcome._
