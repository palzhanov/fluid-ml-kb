# Fourier Neural Operator (FNO) and the neural operator framework

status: first-pass
family: operator learning
key papers: [li2021fno], [kovachki2023neuralop]

## Core idea
Stack layers of the form v ↦ σ(W v + K v), where K is a kernel integral operator. In FNO, K is parametrized in Fourier space: FFT, multiply the lowest k_max modes by learned complex weights, inverse FFT. [kovachki2023neuralop] gives the general framework and universal approximation results.

## Architecture
Lift → L Fourier layers (spectral conv + pointwise linear + nonlinearity) → project. Truncation to k_max modes per dimension.

## Training data and loss
Input/output field pairs on a regular grid; relative L2 loss. Time-dependent problems either autoregressively (2D + recurrent) or with time as an extra dimension (3D FFT).

## Mesh / geometry handling
Requires uniform grids for FFT. Claims of discretization invariance: weights are defined on modes so the model can be evaluated on other grids; zero-shot super-resolution works when the high-frequency content is negligible, and has been criticized as aliasing-sensitive [raonic2023cno].

## Generalization
- Across input functions/coefficients: core use case (Darcy, Burgers, Navier–Stokes in the original paper).
- Geometry: not native; see Geo-FNO/GINO.
- Time: autoregressive rollouts accumulate error.
- Resolution: see above.

## Cost
O(N log N) per layer; very fast inference on GPUs.

## Benchmarks reported
2D Navier–Stokes vorticity (periodic torus), Darcy flow, Burgers. TODO(numbers).

## Known failure modes / limitations
- Spectral truncation struggles with shocks/sharp fronts and with high-frequency content.
- Aliasing between grids [raonic2023cno].
- Comparisons often against weak numerical baselines [mcgreivy2024weak].

## Relation to other methods
Generalizes to GINO/Geo-FNO for geometry; CNO replaces Fourier layers with alias-aware convolutions; transformer operators re-interpret attention as kernel integration.

## Open questions
- What exactly is gained over a well-tuned U-Net at matched cost? (several benchmarks suggest the gap is small ⚠ cite PDEArena/CNO results)
