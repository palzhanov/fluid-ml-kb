# Reduced basis and projection-based ROMs (incl. operator inference)

status: first-pass
family: projection ROM
key papers: [hesthaven2016rb], [peherstorfer2016opinf]

## Core idea
Approximate the solution manifold {u(μ)} by a linear subspace spanned by N ≪ 𝒩 basis functions (POD modes or greedy snapshots), and obtain reduced coefficients by Galerkin (or Petrov–Galerkin) projection of the full-order equations. Offline/online split: expensive basis construction once, cheap reduced solves per parameter. Certified RB adds rigorous a posteriori error bounds.
Operator inference (OpInf) is the non-intrusive variant: project snapshot data onto the POD basis and fit the reduced operators (linear, quadratic, …) by least squares, assuming the governing equations have known polynomial structure.

## Architecture
Not a neural network. POD/greedy basis + projected operators; hyper-reduction (EIM/DEIM) for nonlinear terms.

## Training data and loss
A few to a few hundred high-fidelity solves. Greedy algorithms pick parameters using the error estimator.

## Mesh / geometry handling
Fixed mesh; geometric parametrization via mapping to a reference domain (affine or via EIM).

## Generalization
- Parameters: strong within the training range, with certified error bounds for elliptic/parabolic problems.
- Geometry: through parametrized maps only.
- Time: POD-Galerkin can be unstable for long horizons in convection-dominated flows (needs stabilization/closure).

## Cost
Online cost independent of 𝒩 only with affine decomposition or hyper-reduction.

## Benchmarks reported
Textbook problems (thermal block, Stokes, Navier–Stokes in channels) ⚠ unverified exact list. TODO(numbers).

## Known failure modes / limitations
- **Kolmogorov n-width barrier**: transport/advection-dominated problems need many linear modes. This is the core motivation for nonlinear-manifold ROMs (CAE-ROM, DL-ROM) and LDNet.
- Intrusive: needs access to the solver's operators (except OpInf).
- Galerkin instability for turbulent / under-resolved flows.

## Relation to other methods
Baseline every ML method in this repo should be compared against when the problem is parametric with a fixed mesh. POD-NN, POD-DL-ROM and POD-DeepONet reuse the POD step.

## Open questions
- Where exactly do ML ROMs beat well-stabilized POD-Galerkin + hyper-reduction at equal online cost? Often not reported.
