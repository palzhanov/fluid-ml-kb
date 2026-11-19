# PDE foundation models: Poseidon

status: first-pass
family: foundation model
key papers: [herde2024poseidon]

## Core idea
Pretrain a large operator model on a diverse set of PDE trajectories (mostly compressible/incompressible flow families) and fine-tune on downstream PDEs with few samples. Poseidon uses a multiscale operator transformer (scOT) with time-conditioned normalization and an "all2all" training strategy that uses every pair of time steps in a trajectory ⚠ unverified details.

## Architecture
Swin-style hierarchical vision transformer adapted to operators; lead-time conditioning.

## Training data and loss
Large pretraining corpus of 2D fluid trajectories; fine-tuning on unseen PDEs.

## Mesh / geometry handling
Regular 2D grids.

## Generalization
Sample-efficiency gains on downstream tasks including physics not seen in pretraining ⚠ unverified scope. TODO(numbers).

## Cost
Large pretraining cost; inference comparable to other transformers.

## Known failure modes / limitations
2D regular grids; unclear transfer to 3D, complex geometry, or ROM-style tiny-data engineering problems.

## Relation to other methods
Contrasts with the ROM philosophy (one problem, few solves, certified error).

## Open questions
Could foundation-model features serve as a pretrained decoder/encoder for problem-specific latent dynamics models?
