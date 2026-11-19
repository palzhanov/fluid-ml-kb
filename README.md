# fluid-ml-kb

A working knowledge base on machine learning for fluid dynamics and, more broadly, data-driven surrogates for parametrized PDEs: reduced-order models, latent dynamics, neural operators, graph/mesh models, physics-informed methods and generative surrogates.

The goal is not a reading list. It is a place to (1) understand each method well enough to say what it actually assumes, (2) compare methods on the same axes, and (3) find research directions in the gaps.

## Layout

| Path | What lives there |
|---|---|
| [`sources.md`](sources.md) | Every paper we rely on, with arXiv/DOI links. Each entry has a `key` used everywhere else. |
| [`taxonomy.md`](taxonomy.md) | How the method families relate, and the axes we use to compare them. |
| [`notes/`](notes/) | One note per method (or tight method cluster), all following [`notes/_TEMPLATE.md`](notes/_TEMPLATE.md). |
| [`comparison.csv`](comparison.csv) | Methods × properties matrix. The fastest way to spot gaps. |
| [`directions.md`](directions.md) | Cross-cutting shortcomings and candidate research directions. The part we argue about. |
| `papers/` | Optional local PDFs, named `<key>.pdf` (see `papers/README.md`). |

## Conventions

- **Cite by key.** Notes refer to papers as `[lu2021deeponet]`, matching `sources.md`.
- **Mark confidence.** Anything not checked against the paper itself is tagged `⚠ unverified`. A first-pass note is written from general knowledge of the paper plus verified bibliographic metadata, not a line-by-line read. Replace the tag when someone checks the PDF.
- **No invented numbers.** Benchmark figures go in only when copied from the paper, with the table/figure number. Otherwise write `TODO(numbers)`.
- **Status field.** Every note starts with `status: first-pass | checked-against-paper | deep-dive`.
- **Disagreements go in `directions.md`**, not silently into notes.

## Workflow

1. Add a paper: append it to `sources.md` (and optionally drop the PDF in `papers/`).
2. Write or extend a note in `notes/` from the template; add/adjust its row in `comparison.csv`.
3. If it changes the picture, update `taxonomy.md` and open or comment on an item in `directions.md`.
4. Changes go through PRs so we can review each other's reading.
