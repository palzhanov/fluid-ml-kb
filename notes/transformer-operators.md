# Transformer operators: Galerkin Transformer, GNOT, Transolver

status: first-pass
family: transformer operator
key papers: [cao2021galerkin], [hao2023gnot], [wu2024transolver]

## Core idea
Treat each mesh point (or patch) as a token and attention as a learned kernel integral operator.
- **Galerkin Transformer** [cao2021galerkin]: softmax-free linear attention interpreted as a Petrov–Galerkin projection; linear cost in sequence length.
- **GNOT** [hao2023gnot]: heterogeneous normalized attention to fuse multiple input functions (geometry, boundary conditions, parameters) and a geometric gating mixture-of-experts ⚠ unverified details.
- **Transolver** [wu2024transolver]: "physics-attention" that softly assigns mesh points to a small number of learned slices, attends among slice tokens, then broadcasts back; linear cost and works on arbitrary meshes.

## Architecture
Point/token embedding → stacked (linear/sliced) attention blocks → pointwise decoder.

## Training data and loss
Supervised fields; relative L2.

## Mesh / geometry handling
Point clouds / unstructured meshes; geometry varies across samples (GNOT, Transolver).

## Generalization
Geometry within training distribution; time handled autoregressively or as input.

## Cost
Linear attention variants scale to ~10⁵–10⁶ points ⚠ check reported sizes.

## Benchmarks reported
Transolver: standard operator benchmarks (elasticity, plasticity, airfoil, pipe, Navier–Stokes, Darcy) and industrial-like car/airfoil design tasks ⚠ unverified exact list. TODO(numbers).

## Known failure modes / limitations
- Data hungry; little analysis of extrapolation.
- Slice tokens are a learned low-rank latent, conceptually close to a nonlinear ROM basis, but not analysed that way. (our observation)

## Relation to other methods
Transolver's slices ↔ POD modes / LDNet latent state: a learned reduced representation. Interesting bridge (directions.md D4).

## Open questions
- Can sliced-attention latents be given dynamics (latent time-stepping instead of full-field autoregression)?
