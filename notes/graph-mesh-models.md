# Graph / mesh time-steppers: MeshGraphNets, MP-PDE

status: first-pass
family: graph/mesh
key papers: [pfaff2021meshgraphnets], [brandstetter2022mppde]

## Core idea
Learn a one-step (or few-step) update u(t) → u(t+Δt) with message passing on the simulation mesh.
- **MeshGraphNets**: encode–process–decode GNN on mesh (and world-space) edges; next-step prediction with training noise to stabilize rollouts; supports adaptive remeshing.
- **MP-PDE**: message passing solver with tricks for stable autoregressive rollouts (pushforward trick, temporal bundling) and generalization across PDE parameters.

## Architecture
Node/edge MLP encoders, L message-passing layers with residual updates, decoder to state increment.

## Training data and loss
Many trajectories; one-step MSE (+ noise injection / pushforward).

## Mesh / geometry handling
Native unstructured meshes; geometry varies between samples.

## Generalization
- Geometry and mesh: strong selling point.
- Time: autoregressive drift over long horizons.
- Larger domains than trained: reported to some extent for MeshGraphNets ⚠ unverified.

## Cost
Per-step cost O(edges × layers); many steps per trajectory; receptive field limited by layer count (long-range effects such as pressure in incompressible flow are hard).

## Benchmarks reported
MeshGraphNets: cylinder flow, airfoil (compressible), cloth, deforming plate. MP-PDE: 1D Burgers/KdV-type families, 2D ⚠. TODO(numbers).

## Known failure modes / limitations
- Error accumulation; incompressibility/global constraints poorly handled by local message passing.
- Fixed Δt.

## Relation to other methods
GCA-ROM uses graph convs as AE; multiscale/hierarchical GNNs address long-range; GINO uses GNO layers for encoding.

## Open questions
Latent-space time stepping on graphs (graph encoder + latent ODE) vs full-graph stepping.
