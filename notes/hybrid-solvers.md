# Hybrid ML + numerical solvers (learned corrections)

status: first-pass
family: hybrid solver
key papers: [kochkov2021mlcfd]

## Core idea
Keep a classical (differentiable) CFD solver at coarse resolution and learn a correction (interpolation stencils, closure, or residual correction) trained end-to-end through the solver so coarse simulations match fine ones. [kochkov2021mlcfd] does this for 2D turbulence with JAX-CFD.

## Architecture
CNN producing learned interpolation coefficients inside a finite-volume scheme ⚠ unverified exact variant ("learned interpolation" vs "learned correction").

## Training data and loss
Downsampled high-resolution DNS; multi-step unrolled loss through the solver.

## Mesh / geometry handling
Structured grids (periodic in the paper).

## Generalization
Reported generalization to different forcing/Reynolds numbers and larger domains ⚠ unverified. Physics structure of the solver helps stability.

## Cost
Claims significant speedup at matched accuracy versus the baseline solver ⚠ unverified magnitude. TODO(numbers).

## Known failure modes / limitations
Requires a differentiable solver; periodic domains; 2D.

## Relation to other methods
Closure modelling for ROMs is the ROM-community analogue; LED's solver coupling is another hybrid.

## Open questions
Learned closures for POD-Galerkin / LDNet in turbulent regimes.
