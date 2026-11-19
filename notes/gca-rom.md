# GCA-ROM (graph convolutional autoencoder ROM)

status: first-pass
family: DL-ROM / autoencoder ROM, graph/mesh
key papers: [pichi2024gcarom]

## Core idea
Autoencoder ROM where encoder and decoder are graph convolutional networks operating directly on the unstructured finite-element mesh, with pooling to a small latent code; an MLP maps parameters to the latent code. From Rozza-adjacent collaborators (Pichi; Hesthaven at EPFL).

## Architecture
GNN encoder with mesh pooling → dense bottleneck → GNN decoder; parameter MLP to latent.

## Training data and loss
FE snapshots for parametrized problems, reconstruction + latent-matching loss (as DL-ROM) ⚠ unverified exact loss.

## Mesh / geometry handling
Unstructured meshes natively; fixed mesh across samples in the main setup ⚠ check if geometry variation tested.

## Generalization
Parametric (including bifurcating problems ⚠ unverified); time handling limited.

## Cost
Cheap online.

## Benchmarks reported
Several parametrized PDE problems including fluid flows ⚠ unverified. TODO(numbers).

## Known failure modes / limitations
Same as DL-ROM: no genuine dynamics; fixed mesh topology.

## Relation to other methods
Graph analogue of DL-ROM; links ROM community to MeshGraphNets-style models.

## Open questions
Combine with latent dynamics (graph encoder + LDNet-style ODE).
