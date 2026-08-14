---
title: "Wannier90 vs. WannierTools band mismatch: use_ws_distance"
date: 2026-08-14
draft: false
tags: ["wannier90", "wanniertools", "tight-binding"]
math: false
summary: "A silent interpolation discrepancy between Wannier90-interpolated bands and WannierTools bands, traced to a default that changed in Wannier90 v3."
---

If your WannierTools-interpolated bands don't sit exactly on top of the
Wannier90 band-interpolation check (`wannier90.wout` vs. `wannier90_band.dat`),
before suspecting your Hamiltonian construction, check `use_ws_distance`.

Since Wannier90 v3, `use_ws_distance = true` is the default. It changes how
matrix elements are distributed among Wigner-Seitz-degenerate lattice vectors
during the Fourier transform to `hr.dat`. WannierTools (and other codes that
read `hr.dat` directly) don't always assume the same convention, and the
degeneracy list / weighting used to build `hr.dat` may not fully encode which
convention was used.

Fix: force consistency explicitly rather than relying on defaults matching
across codes:

```text
use_ws_distance = false
```

in `wannier90.win`, and regenerate `hr.dat`. Re-check that the Wannier90
disentanglement/interpolation bands still overlay the DFT bands before trusting
downstream WannierTools/WannierBerri results — the mismatch is easy to miss
because both interpolations look individually reasonable, they just disagree
with each other.
