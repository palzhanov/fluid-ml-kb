# fluid-ml-kb

Companion notebooks for the notes [Machine learning for fluid dynamics](https://palzhanov.com/ml-fluids/).

Each method section of the notes gets a small, self-contained Jupyter notebook that demonstrates the idea on a fluid-flavoured toy problem.

| Notebook | Notes section | Topic |
|---|---|---|
| [`notebooks/01-pod-burgers.ipynb`](notebooks/01-pod-burgers.ipynb) | 2.1 | POD and POD-Galerkin ROM for 1D viscous Burgers |

## Running

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Notebooks are committed with their outputs so they can be read without running them. Interactive outputs such as animation players need JavaScript, which GitHub's preview does not run: open the notebook in [nbviewer](https://nbviewer.org/github/palzhanov/fluid-ml-kb/tree/main/notebooks/), Colab or Jupyter to use them.
