---
title: "Parallelizing z2pack WCC scans with multiprocessing"
date: 2026-08-14
draft: false
tags: ["z2pack", "topology", "python"]
math: true
summary: "z2pack surfaces/lines are embarrassingly parallel over the (kx, ky) grid — a minimal multiprocessing.Pool pattern with thread-pinning to avoid oversubscription."
---

z2pack's Wilson loop tracking over a Brillouin-zone surface is independent
per line, so scanning $(k_x, k_y)$ for a Z2 surface calculation parallelizes
trivially over `multiprocessing.Pool`. The one thing that will quietly wreck
performance is BLAS/OpenMP oversubscription: each worker process spawns its
own thread pool for the underlying linear algebra, and with enough workers
that saturates the node before you're anywhere near the process count.

Pin threads per worker before spawning:

```python
import os
os.environ["OMP_NUM_THREADS"] = "1"
os.environ["MKL_NUM_THREADS"] = "1"
os.environ["OPENBLAS_NUM_THREADS"] = "1"

import multiprocessing as mp
import z2pack

def run_line(ky):
    result = z2pack.surface.run(
        system=system,
        surface=lambda kx, kz: [kx, ky, kz],
        pos_tol=1e-3,
        gap_tol=2e-2,
        move_tol=0.3,
    )
    return ky, result

if __name__ == "__main__":
    ky_grid = [i / 50 for i in range(51)]
    with mp.Pool(processes=os.cpu_count()) as pool:
        results = pool.map(run_line, ky_grid)
```

Set the thread-pinning environment variables *before* importing any package
that touches BLAS (numpy, scipy, z2pack) — setting them after import is a
no-op for most BLAS backends since they read the env at first use.
