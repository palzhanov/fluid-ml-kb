# Convolutional Neural Operators (CNO)

status: first-pass
family: operator learning
key papers: [raonic2023cno]

## Core idea
Build an operator out of convolutional (U-Net-like) layers while respecting the continuous–discrete equivalence: operate on band-limited functions, with up/downsampling and activations done in an alias-aware way so that the discrete network represents a well-defined continuous operator.

## Architecture
U-Net style with alias-free interpolation and activation (upsample before nonlinearity, then filter and downsample) ⚠ unverified details.

## Training data and loss
Supervised field pairs; the paper introduces the RPB (Representative PDE Benchmarks) suite ⚠ verify name.

## Mesh / geometry handling
Regular grids.

## Generalization
Emphasizes out-of-distribution tests within PDE families.

## Cost
Comparable to U-Net.

## Benchmarks reported
Reports gains over FNO and other baselines on a suite including Navier–Stokes, Poisson, wave, Allen–Cahn, compressible Euler ⚠ unverified list. TODO(numbers).

## Known failure modes / limitations
Regular grids only; geometry via masking.

## Relation to other methods
Same group (Mishra, ETH) as Poseidon, which uses a transformer backbone instead. U-Net baselines are the natural comparison.

## Open questions
- Is "aliasing-free" practically important on the fluid problems we care about?
