# Geometry-aware neural operators: Geo-FNO and GINO

status: first-pass
family: operator learning
key papers: [li2023geofno], [li2023gino]

## Core idea
- **Geo-FNO**: learn (or prescribe) a deformation from the physical irregular domain to a latent uniform grid, then apply FNO there.
- **GINO**: encode a point cloud / mesh (with an SDF of the geometry) into a regular latent grid via a graph neural operator, apply FNO on the grid, decode back to arbitrary query points with another graph operator.

## Architecture
GINO: GNO (radius graph kernel integral) encoder → FNO on latent grid → GNO decoder. Inputs include signed distance functions.

## Training data and loss
Simulations over families of geometries (GINO: 3D car shapes, surface pressure for drag ⚠ unverified exact dataset).

## Mesh / geometry handling
Varying geometry is the point. Geo-FNO needs a deformation map that may be hard to learn for topologically different shapes.

## Generalization
Across shapes in the training distribution; out-of-distribution shapes untested/limited.

## Cost
GINO reports large speedups vs CFD for drag prediction ⚠ unverified magnitude TODO(numbers).

## Known failure modes / limitations
- Steady problems mostly; unsteady with geometry variation is less explored.
- Latent grid resolution limits thin features (boundary layers).

## Relation to other methods
Compare with Transolver (learned physics slices), CORAL (neural fields), GCA-ROM (graph AE for ROM).

## Open questions
- Unsteady, geometry-varying flows with a latent-dynamics model (LDNet + geometry code?).
