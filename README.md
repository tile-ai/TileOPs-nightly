# TileOPs nightly benchmark data

The nightly benchmark run in [tile-ai/TileOPs](https://github.com/tile-ai/TileOPs)
publishes here, and the docs site renders the newest of it.

| Branch | Holds | Written by |
|--------|-------|------------|
| [`snapshots`](https://github.com/tile-ai/TileOPs-nightly/tree/snapshots) | One commit per run: `bench_results.xml`, `test_results.xml`, `meta.json` | the nightly's `publish-bench-data` job |
| `main` | What this repository says about itself | people |

Nothing publishes over `main`, and nothing but the nightly writes `snapshots`:
a commit there is a claim about a measurement that happened.

## What the three files are

| File | What it is |
|------|------------|
| `bench_results.xml` | The benchmark run, as pytest wrote it. One `testcase` per workload; every measurement is a `property` named `<implementation>_<metric>`. |
| `test_results.xml` | The correctness run over the same commit: which ops passed, and the worst absolute error each reported. |
| `meta.json` | What produced the numbers: the commit, the run, the GPU and its settings, the runner image and its digest, and every installed package. |

## Reading a night that is not tonight

```bash
git log --format='%ad %H' --date=short origin/snapshots     # every run, newest first
git show <commit>:meta.json                                 # what it ran on
git show <commit>:bench_results.xml > bench.xml             # what it measured
```

Reproducing one of those numbers takes three things from `meta.json`: `commit`
(the TileOPs source), `environment.image_digest` (the container, by digest —
the tag can have been pushed again since), and `environment.gpu` with the power
cap and clocks recorded beside it.
