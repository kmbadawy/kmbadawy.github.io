---
title: "Wise Tips"
url: "/wise-tips/"
summary: "Quick, bite-sized research and computational tips."
ShowToc: false
ShowReadingTime: false
math: false
---

Short, standalone tips — the kind of thing I'd tell a new student in
passing. Click a title to expand it. New ones get added whenever I run into
something worth writing down.

<div class="wise-tips">

<details class="tip">
<summary>Always check your convergence before you trust your physics</summary>

A beautiful band structure or a clean topological invariant means nothing
if the underlying SCF, k-mesh, or energy cutoff hasn't converged. Before
interpreting any result — spin texture, Berry phase, magnetic anisotropy —
re-run it at a tighter setting and confirm the number doesn't move. If it
moves, you were reading noise, not physics.

</details>

<details class="tip">
<summary>Read the code's default before you blame your physics</summary>

A "wrong" result is very often a default you didn't know you inherited —
a convention, a unit, a flag that changed between versions. Before
questioning your Hamiltonian or your setup, check what the software
actually assumed on your behalf. Defaults are invisible until they bite
you.

</details>

<details class="tip">
<summary>Write down what you ran, not just what you got</summary>

Six months from now you won't remember which INCAR, which k-mesh, or which
commit of your script produced a given plot. A one-line log next to every
result — even just the command and the date — saves hours of "wait, how
did I get this number?" later.

</details>

<details class="tip">
<summary>GIGO applies to literature too</summary>

Garbage in, garbage out isn't just about input files — it applies to the
papers you build on. Before trusting a reported value or convention,
check whether the paper itself converged its calculation, and whether its
definition of a quantity matches yours. Assumptions inherited silently
from a paper are still garbage in.

</details>

</div>
