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
something worth writing down. I will keep it professional and not stress on 
the importance of checking convergence against energy cutoff and k-mesh !!!!!


<div class="wise-tips">

<details class="tip">
<summary>Ensuring Wannier90 model convergence is a 'worth it' headache</summary>

Apart from obtaining a correct match between the band structure from VASP and that from
wannier90. Ensure that your Fermi surface / surface state / Gap in k-plane of wanniertools 
obey the underlying symmetries of the system. Sometimes you get a perfect band plot match 
between W90 and VASP but the Fermi surface or surface state would look crooked and violated 
inversion/time reversal if relevant.

</details>

<details class="tip">
<summary>Read the code's default before you blame your physics</summary>

A "wrong" result is very often a default you didn't know you inherited —
a convention, a unit, a flag that changed between versions. Before
questioning your setup, check what the software actually assumed on your behalf.
Defaults are invisible until they bite you.

</details>

<details class="tip">
<summary>Write down what you ran, not just what you got</summary>

Six months from now you won't remember which INCAR, which k-mesh, or which
commit of your script produced a given plot. A one-line log next to every
result — even just the command and the date — saves hours of "wait, how
did I get this number?" later.

</details>

<details class="tip">
<summary>VASP versions =>6 require special care with wanniertools </summary>

In wanniertools software, the tag 'Package' determies the SPIN ordering produced by 
the first-principles package. VASP versions 6 and above order spins similar to 
Quantum Espresso. To get the correct spin-texture and projections on surface states
use the tag Package='QE'. 

</details>

</div>
