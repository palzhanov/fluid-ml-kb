# Autoencoder ROMs: CAE-ROM, DL-ROM, POD-DL-ROM

status: first-pass
family: DL-ROM / autoencoder ROM
key papers: [lee2020cae], [fresca2021dlrom], [fresca2022poddlrom]

## Core idea
Replace the linear POD subspace with a nonlinear manifold learned by an autoencoder, to beat the n-width barrier.
- **CAE-ROM** [lee2020cae]: convolutional decoder defines the manifold; dynamics obtained by projecting the full-order residual onto the manifold's tangent (manifold Galerkin / LSPG). Intrusive.
- **DL-ROM** [fresca2021dlrom]: non-intrusive. A feedforward net maps (μ, t) → latent code; a decoder maps latent → full field; an encoder is used in training only to align the latent space.
- **POD-DL-ROM** [fresca2022poddlrom]: first compress with (randomized) POD, then run DL-ROM on the POD coefficients. Much cheaper training, larger problems.

## Architecture
Conv/dense encoder–decoder; latent dimension near the intrinsic dimension of the solution manifold (e.g., number of parameters + 1 for time).

## Training data and loss
Snapshots on fixed mesh; loss = reconstruction + latent alignment between encoder output and the (μ,t)→latent net.

## Mesh / geometry handling
Fixed mesh (conv layers need structured reshaping, POD step makes it mesh-agnostic but still fixed-mesh).

## Generalization
- Parameters: good interpolation.
- Time: DL-ROM treats t as an input, so no genuine dynamics and no time-varying forcing; CAE-ROM keeps dynamics via projection.
- Geometry: no.

## Cost
DL-ROM/POD-DL-ROM online: one forward pass. CAE-ROM online cost depends on full-order residual evaluations (needs hyper-reduction).

## Benchmarks reported
Cardiac electrophysiology, unsteady NS (flow around a cylinder), fluid–structure interaction ⚠ unverified exact set. TODO(numbers).

## Known failure modes / limitations
- Time-as-input means no causal dynamics and no response to new input signals (motivation for LDNet). (our observation)
- Fixed mesh.

## Relation to other methods
Direct predecessor of LDNet from the same Politecnico di Milano group; GCA-ROM extends to unstructured meshes with graph convolutions.

## Open questions
- Head-to-head DL-ROM vs LDNet on identical data and budgets.
