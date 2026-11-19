# POD-NN (non-intrusive POD + neural network regression)

status: first-pass
family: projection ROM (non-intrusive)
key papers: [hesthaven2018podnn]

## Core idea
Compute a POD basis from snapshots, then train an MLP to map parameters μ (and t) directly to the POD coefficients. No projection of equations; purely data-driven regression of the reduced coefficients.

## Architecture
MLP: μ → coefficients; reconstruction u ≈ V c(μ).

## Training data and loss
Snapshots on a fixed mesh; MSE on POD coefficients.

## Mesh / geometry handling
Fixed mesh.

## Generalization
Parameter interpolation; time usually as an input; no dynamics.

## Cost
Trivial online cost.

## Benchmarks reported
Nonlinear Poisson, driven cavity (steady incompressible NS) ⚠ unverified. TODO(numbers).

## Known failure modes / limitations
Inherits the linear-subspace n-width limitation; no dynamics, so time-dependent inputs u(t) are hard.

## Relation to other methods
Structurally a DeepONet with POD trunk; ancestor of POD-DL-ROM.

## Open questions
—
