# Physics-informed neural networks (PINNs) and physics-informed operators

status: first-pass
family: physics-informed
key papers: [raissi2019pinn], [wang2021pideeponet]

## Core idea
Represent the solution as a neural network u_θ(x, t) and minimize the PDE residual at collocation points plus boundary/initial/data misfits. Forward problems (solve) and inverse problems (infer parameters/fields from sparse data). PI-DeepONet applies the same loss to an operator network.

## Architecture
MLP (tanh etc.) with automatic differentiation for derivatives.

## Training data and loss
Residual + BC/IC + optional data; loss weighting is critical.

## Mesh / geometry handling
Mesh-free collocation.

## Generalization
A vanilla PINN solves one instance; no generalization across parameters without retraining (hence PI-DeepONet / parametric PINNs).

## Cost
Training is the solve; usually much slower than classical solvers for forward problems at equal accuracy ⚠ (widely reported; cite reviews).

## Benchmarks reported
Burgers, Schrödinger, Navier–Stokes cylinder wake inverse problem (identify viscosity) ⚠ unverified. TODO(numbers).

## Known failure modes / limitations
- Optimization pathologies (stiff gradient imbalance, spectral bias, causality violation in time).
- Poor for high-Re / multiscale flows.
- Strongest niche: inverse problems and data assimilation with sparse measurements.

## Relation to other methods
Physics loss can be attached to LDNet, DeepONet, FNO (PINO) etc.

## Open questions
Physics residuals in latent-dynamics models where the decoder is a neural field (residual computable by autodiff on NN_rec).
