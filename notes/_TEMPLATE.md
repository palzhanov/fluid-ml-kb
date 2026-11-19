# <Method name>

status: first-pass | checked-against-paper | deep-dive
family: projection ROM | DL-ROM / autoencoder ROM | latent dynamics | operator learning | graph/mesh | transformer operator | neural field | physics-informed | hybrid solver | generative | foundation model
key papers: [key1], [key2]

## Core idea
One paragraph. What object is being learned (a map between parameters and states, an operator between function spaces, a time-stepper, a latent ODE, a distribution)?

## Architecture
Main components and how information flows. Note what is linear and what is nonlinear.

## Training data and loss
What data it needs (full-field snapshots, sparse sensors, residuals only), how much, and the loss.

## Mesh / geometry handling
Fixed grid? Arbitrary point clouds? Varying geometry? Resolution/discretization invariance claims and whether they hold.

## Generalization
- Across parameters:
- Across geometry:
- In time (extrapolation, long rollouts):
- Across resolution:

## Cost
Offline (data generation + training) vs online (inference). Scaling with degrees of freedom.

## Benchmarks reported
Datasets and headline results, with table/figure references. `TODO(numbers)` if not yet copied.

## Known failure modes / limitations
From the paper itself, from follow-up work, and our own observations (label which).

## Relation to other methods
Which notes this generalizes, specializes, or competes with.

## Open questions
What we would want to test or what is unclear.
