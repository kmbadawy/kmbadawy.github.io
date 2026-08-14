---
title: "Aren't we all tired from plotting bands?"
date: 2026-08-13
draft: false
math: false
summary: "I hold the solution"
---

Surfing the web for a band structure plotting tool turns out to be a trivial task. But what is not trivial
is the waiting time for these tools to parse the astronomical PROCAR and DOSCAR files ! I had a cup of tea 
and consulted 'Claude Code' as I obviously cannot code this good. I gave the ideas, Claude coded it ! The key
is to shorten the parsing time, which thankfully the beloved VASPKIT software does it incredibly fast. 

I present to you pltband.py !

A one script that will read the BAND.dat and PBAND files to plot a nice band structure plot 
(like those you see in Nature journals). Briefly, code can:

1. plot spin, non-spin, or projected band structure. 

2. Highlight bands by their index

3. Truncate at a kpath or plot around a certain kpoint

4. compare with wannier90_band.dat file

5. Flexible formatting changes 

All of this is done using a single command line. It is not perfect but after all we dont want to waste time plotting 
bands or waiting for parsing files.

More details are found at https://github.com/kmbadawy/pltband.git

You can find plotting examples in our paper https://doi.org/10.1038/s41524-026-02029-6 