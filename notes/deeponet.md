# DeepONet

status: first-pass
family: operator learning
key papers: [lu2021deeponet], [wang2021pideeponet]

## Core idea
Approximate an operator G: a ↦ u by G(a)(y) ≈ Σₖ bₖ(a) tₖ(y), motivated by the universal approximation theorem for operators (Chen & Chen, 1995). The output is a linear combination of learned basis functions of the query location y, with coefficients depending on the input function a.

## Architecture
- **Branch net**: takes a sampled at fixed "sensor" locations, outputs coefficients bₖ.
- **Trunk net**: takes query point y (space, time), outputs basis functions tₖ(y).
- Output: dot product plus bias. Variants: stacked vs unstacked, POD-DeepONet (trunk replaced by POD modes), physics-informed DeepONet.

## Training data and loss
Pairs (a, u at query points). MSE at query points. PI-DeepONet adds the PDE residual at collocation points and can train with few or no solution samples [wang2021pideeponet].

## Mesh / geometry handling
Input side: requires a at a fixed set of sensors (a limitation for varying meshes). Output side: query anywhere. Geometry variation is not native.

## Generalization
- Across input functions: core use case.
- Geometry: no, without extensions.
- Time: time is usually a trunk coordinate, so extrapolation beyond the training window is weak. ⚠ general observation
- Resolution: output continuous; input tied to sensors.

## Cost
Cheap inference; training scales with number of (a, y) pairs.

## Benchmarks reported
Original paper: ODE/PDE operators including diffusion–reaction and integral/derivative operators. TODO(numbers).

## Known failure modes / limitations
- The linear "reconstruction" in the trunk basis acts like a linear ROM in output space; for advection-dominated problems with slow Kolmogorov n-width decay this needs many basis functions (same issue as POD). (our observation, also discussed in error-analysis literature ⚠ cite)
- Fixed sensor locations.

## Relation to other methods
POD-NN is essentially DeepONet with a POD trunk and scalar parameters. LDNet's reconstruction net is a nonlinear trunk conditioned on a dynamical latent code.

## Open questions
- Nonlinear decoders (e.g., NOMAD-style) vs linear trunk on advection-dominated flows.
