# Encoder-based latent dynamics: Koopman autoencoders, SINDy/LaSDI, LED

status: first-pass
family: latent dynamics
key papers: [lusch2018koopman], [brunton2016sindy], [fries2022lasdi], [vlachas2022led]

## Core idea
Encode the high-dimensional state into a latent z, learn how z evolves, decode back.
- **Koopman AE** [lusch2018koopman]: latent dynamics are linear (z_{k+1} = K z_k), with an auxiliary net to handle continuous spectra.
- **SINDy** [brunton2016sindy]: sparse regression over a library of candidate terms to discover interpretable ODEs (originally on low-dimensional data).
- **LaSDI** [fries2022lasdi]: autoencoder + SINDy in the latent space, with interpolation of the identified coefficients across parameters.
- **LED** [vlachas2022led]: autoencoder + recurrent (LSTM) latent propagator, alternating between cheap latent steps and short bursts of the full solver (multiscale coupling).

## Architecture
Encoder/decoder (dense or conv) + latent propagator (linear, sparse polynomial, RNN).

## Training data and loss
Trajectories on a fixed grid; reconstruction + prediction losses (multi-step).

## Mesh / geometry handling
Fixed grid.

## Generalization
- Time: explicit dynamics, can extrapolate; stability not guaranteed.
- Parameters: LaSDI by coefficient interpolation; others via conditioning.
- Geometry: no.

## Cost
Cheap latent rollouts; LED trades accuracy vs cost by how often the full solver is called.

## Benchmarks reported
Koopman AE: pendulum, fluid flow past cylinder ⚠; LaSDI: Burgers, 2D Burgers ⚠; LED: Kuramoto–Sivashinsky, FitzHugh–Nagumo, flow past cylinder ⚠. TODO(numbers).

## Known failure modes / limitations
- Linear Koopman latents struggle with chaotic/continuous-spectrum systems.
- SINDy library choice; noise sensitivity.
- Encoder needs full-state snapshots on the training grid.

## Relation to other methods
LDNet removes the encoder; LED's solver coupling is a hybrid idea relevant to directions.md.

## Open questions
- Is the encoder a feature (enables assimilation/restarts) or a burden (needs full fields)? See directions.md D2.
