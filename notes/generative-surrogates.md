# Generative and long-rollout surrogates: PDE-Refiner, autoregressive diffusion, GenCast

status: first-pass
family: generative
key papers: [lippe2023pderefiner], [kohl2023acdm], [price2024gencast]

## Core idea
Use diffusion-style iterative denoising to model either the next state or its distribution.
- **PDE-Refiner**: after a one-step prediction, a few denoising-like refinement steps recover low-amplitude high-frequency components that MSE-trained models neglect; improves long-rollout stability.
- **ACDM** [kohl2023acdm]: autoregressive conditional diffusion models for turbulent flow, benchmarked against deterministic surrogates; probabilistic samples.
- **GenCast**: diffusion-based ensemble weather forecasting at global scale.

## Architecture
U-Net / transformer denoisers conditioned on previous states.

## Training data and loss
Many trajectories; denoising score-matching loss.

## Mesh / geometry handling
Mostly regular grids (GenCast: sphere mesh ⚠ verify).

## Generalization
Better long-horizon statistics; captures uncertainty and multimodality.

## Cost
Multiple denoising steps per time step; much more expensive than deterministic surrogates.

## Benchmarks reported
PDE-Refiner: Kuramoto–Sivashinsky, 2D Kolmogorov flow ⚠; ACDM: incompressible wake, transonic flow, isotropic turbulence ⚠. TODO(numbers).

## Known failure modes / limitations
Cost; physical consistency of samples not guaranteed.

## Relation to other methods
Generative latent dynamics (diffusion in an LDNet-like latent space) seems underexplored ⚠ check.

## Open questions
See directions.md D5.
