# Taxonomy

status: first-pass

Most methods here approximate one of four objects. Naming which one a method learns is the quickest way to see what it can and cannot generalize over.

| Object learned | Typical input → output | Examples |
|---|---|---|
| **Parameter-to-solution map** | μ (and t) → u(x; μ, t) on a fixed discretization | POD-NN, DL-ROM, POD-DL-ROM |
| **Operator between function spaces** | input field a(x) → solution field u(x) | DeepONet, FNO, CNO, GINO, Transolver |
| **Time-stepper / flow map** | u(t) → u(t+Δt) | MeshGraphNets, MP-PDE, PDE-Refiner, LC-CFD |
| **Reduced (latent) dynamics** | z(t), input(t) → dz/dt, then decode | LDNet, LED, Koopman AE, LaSDI, DINo |

Generative models (diffusion, flow matching) learn a **distribution** over one of the above instead of a point estimate.

## Families

```
                       ┌───────────────────────────────┐
                       │ Classical projection ROMs     │  POD-Galerkin, certified RB
                       │ (intrusive, linear subspace)  │  [hesthaven2016rb]
                       └──────────────┬────────────────┘
               non-intrusive ─────────┼───────── nonlinear manifold
                       │                              │
         ┌─────────────▼─────────┐        ┌───────────▼────────────┐
         │ Operator inference,   │        │ Autoencoder / DL-ROMs  │
         │ POD-NN regression     │        │ CAE-ROM, DL-ROM,       │
         │ [peherstorfer2016opinf]│       │ POD-DL-ROM, GCA-ROM    │
         │ [hesthaven2018podnn]  │        └───────────┬────────────┘
         └───────────────────────┘                    │ learn dynamics in latent space
                                          ┌───────────▼────────────┐
                                          │ Latent dynamics        │
                                          │ LDNet, LED, Koopman AE,│
                                          │ LaSDI/SINDy, DINo      │
                                          └────────────────────────┘

         ┌────────────────────────┐   ┌─────────────────────────┐   ┌─────────────────────┐
         │ Operator learning      │   │ Graph / mesh models     │   │ Physics-informed    │
         │ DeepONet, FNO, CNO,    │◄─►│ MeshGraphNets, MP-PDE,  │   │ PINNs, PI-DeepONet  │
         │ Geo-FNO, GINO          │   │ GCA-ROM (as encoder)    │   │ (loss, not arch.)   │
         └──────────┬─────────────┘   └─────────────────────────┘   └─────────────────────┘
                    │ attention as kernel integral
         ┌──────────▼─────────────┐   ┌─────────────────────────┐   ┌─────────────────────┐
         │ Transformer operators  │   │ Hybrid ML + solver      │   │ Generative          │
         │ Galerkin TF, GNOT,     │   │ learned corrections /   │   │ PDE-Refiner, ACDM,  │
         │ Transolver, UPT        │   │ closures: LC-CFD        │   │ GenCast             │
         └──────────┬─────────────┘   └─────────────────────────┘   └─────────────────────┘
                    │ pretrain on many PDEs
         ┌──────────▼─────────────┐
         │ Foundation models      │
         │ Poseidon               │
         └────────────────────────┘
```

Physics-informed training is orthogonal: it is a loss term that can be attached to almost any architecture above (PINN = MLP + residual loss; PI-DeepONet = DeepONet + residual loss).

## Comparison axes

These are the columns of `comparison.csv`. Each one is a place where methods genuinely differ.

1. **Object learned** (see table above).
2. **Intrusive?** Needs access to the solver's operators/residuals, or only snapshot data.
3. **Discretization**: fixed grid / fixed mesh / arbitrary points / query-anywhere (continuous in x).
4. **Geometry variation**: can the domain shape change between samples?
5. **Time treatment**: steady, time as input coordinate, autoregressive stepping, continuous-time latent ODE.
6. **Parametric inputs**: scalars μ, input functions a(x), time-varying forcing/controls u(t).
7. **Latent structure**: none, linear (POD), nonlinear AE, explicitly dynamical (latent ODE), Koopman-linear.
8. **Physics in the loop**: none, residual loss, conservation built in, coupled to a numerical solver.
9. **Uncertainty**: deterministic, ensemble, probabilistic/generative.
10. **Error certification**: a posteriori bounds (certified RB) vs empirical test error only.
11. **Data regime**: few high-fidelity simulations (ROM tradition) vs thousands (ML tradition).
12. **Online cost scaling** with mesh size N.

## Two research traditions

A large part of the field's confusion comes from two communities with different defaults:

- **Numerical analysis / ROM** (Quarteroni, Rozza, Hesthaven, Manzoni, Willcox, Carlberg): small data, fixed geometry or parametrized geometry, emphasis on error control, offline/online splitting, often intrusive. Benchmarks are specific engineering problems (cardiac EP, cavity flow, cylinder wake).
- **ML / neural operators** (Anandkumar, Karniadakis, Mishra, Welling, Thuerey, DeepMind): large data, emphasis on resolution invariance and scaling, benchmark suites (PDEBench, PDEArena, FNO's Navier–Stokes, airfoils), increasingly foundation-model framing.

LDNets sit deliberately between them: ROM-style small data and latent dynamics, but with neural-field decoding that queries anywhere in space. Several items in `directions.md` come from that boundary.
