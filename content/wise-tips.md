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

<details class="tip">
<summary>MetaGGA Functionals caution </summary>

The R2SCAN-L and SCAN-L are the Laplacian dependent functionals recently introduced in 
VASP 6.4.X. Be careful using them with v6.2 as no warnings are issued and the parser 
assumes you have chosen R2SCAN/SCAN. The SCAN functional has numerical instabilities that 
most probably will yield your phonon calculations to have serious issues with lots of imaginary modes. 
R2SCAN is more stable and yields better phonon band structures. 
Have a read: https://pubs.acs.org/doi/10.1021/acs.chemmater.1c03222

</details>

<details class="tip">
<summary>Volume Relaxation </summary>

It is very useful to use the option ISIF=8 (vasp6.4.1) since it conserves the initial cell shape. 
Most of the time, for instance in a 2H phase, the cell is defined using “a” and “b” axes. The other 
skew related components (non-diagonal) in the transformation matrix (xy, xz, etc.) are zero in an ideal 
lattice. However, using ISIF options that change the cell shape might induce small values (~0.03) into 
these non-diagonal components, such small deviations cause symmetry problems (only increasing your computing time!) 
in other software like phonopy and they are only artifacts. Unless you know that you need shape optimization, 
volume-only changes are ideal. You can check how far your system is from equilibrium by inspecting the 
“external pressure” entry in OUTCAR. Note that some system properties (ex: magnetic moment) is very sensitive 
to pressure. Always make sure to finalize your relaxation with ISIF=2 to avoid any errors related to the 
change of basis sets.

</details>

</details>

<details class="tip">
<summary>SOC </summary>

In case your system is spin-polarized, it might sound ideal to start from a CHGCAR or a WAVECAR of a previously 
converged magnetic calculation. However, this by default sets the magnetic moment components in x and y to be 
zero in a non-collinear calculation. This might not be ideal in case the system of interest is experiencing 
magnetic frustration or in-plane magnetization for other reasons. It might be more beneficial to start from a 
CHGCAR of a converged non-magnetic SCF and then setting the initial MAGMOM tag in the INCAR. Doing this will allow 
the system to relax and change all of its magnetic moments components in a non-collinear calculation. Be careful to 
start from a CHGCAR because starting from non-magnetic WAVECAR will ignore the MAGMOM tag and set the magnetization to zero.

Also, for wannier90 and MAE calculations, I come from the future, ISYM=-1 and GGA_COMPAT=F tags change the results. 

</details>

<details class="tip">
<summary>R2SCAN-L </summary>

The “deorbitalized” version of R2SCAN (R2SCAN-L) is reported to run at the speed of GGA and giving the same accuracy of R2SCAN. 
According to my experience, it is substantially faster. However, it takes many iterations to reach accuracies of 1E-6 and below 
unlike R2SCAN (apparently requiring higher k-points than R2SCAN), in a spin polarized highly correlated system. I have also noticed 
slight reduction in magnetization as expected from the deorbitalization. In my experience, R2SCAN is much more stable and converges 
pretty well more than SCAN and R2SCAN-L. Do your own testing, I’m only giving you heads up!

</details>

<details class="tip">
<summary>HSE and speed-up mainly </summary>

I urge you to make use of the new OpenACC GPU port of VASP (If you have gpu resources). I have noticed substantial improvement of speed 
especially with HSE calculations (mainly any calculation with HF part). I also strangely noticed lower performance when using R2SCAN 
and R2SCAN-L metaGGA functionals compared to cpu-only. In general, calculations of very large supercells and complex functionals/correlations 
etc. might run significantly faster on GPU (cross fingers you do not run out of gpu memory).

Routine tags that substantially reduce computing time at fixed KPOINTS and ENCUT are LREAL and PREC. It is always a good choice to keep ICHARG=1 during your routine relaxation and SCF calculations to reduce numerous iterations. Be extremely careful when sacrificing accuracy for speed. For example, sacrifice of LREAL and PREC during phonon calculations has bad consequences!

</details>

<details class="tip">
<summary>PHONOPY</summary>

Large number of displacements due to broken symmetries are a headache. Use the bash loop below to setup all your SCF calculations. Make sure that your POSCAR, POTCAR, INCAR, KPOINTS, submission script, and (very useful) WAVECAR files are in the parent directory. Modify N to be the number of POSCARs generated by phonopy.

N=700
for i in $(seq -w "$N"); do
mkdir "$i"
mv "POSCAR-$i" "$i"/POSCAR
cp INCAR POTCAR KPOINTS CHGCAR WAVECAR run.sh "$i"/
cd "$i"
sbatch run.sh #submission script
cd ../
done

Also, PHONOPY results are senstive to the tolerance, check your POSCAR! Many times you would get the correct symmetry by increasing tolerance to 1E-4 instead of the astronomical postion tolerance of 1E-6. It is not worth fighting with machine accuracies for your own health :)

</details>


</div>
