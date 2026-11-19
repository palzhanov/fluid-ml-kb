# Benchmarks and methodological critiques

status: first-pass
family: meta
key papers: [takamoto2022pdebench], [mcgreivy2024weak]

## PDEBench
Benchmark suite with many 1D/2D/3D PDEs (advection, Burgers, diffusion–reaction, Darcy, shallow water, compressible and incompressible NS) and baselines (U-Net, FNO, PINN) ⚠ unverified exact list. Includes forward and inverse tasks.

## McGreivy & Hakim (2024)
Systematic review of ML-for-fluid-PDE papers arguing that most reported speedups over classical solvers compare against weak baselines (not equal-accuracy or not state-of-the-art solvers), and that positive results are over-reported. Recommends equal-accuracy or equal-runtime comparisons against strong solvers.

## Implications for this KB
- Every note's "Benchmarks reported" section should record the baseline solver and whether comparison was at equal accuracy.
- comparison.csv has no "speedup" column on purpose until numbers are checked.

## Candidate additions
FlowBench, CFDBench, FD-Bench (see sources.md "To add next").
