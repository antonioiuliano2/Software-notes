---
description: Measure of interaction length
---

# Analysis of proton interactions and vz fit

## Introduction

This analyis has been provided to me by Valerio. You can find it, along with my changes, at the following Git (I finally know ho to compare commits in jupyter notebooks :) ).

{% embed url="https://github.com/antonioiuliano2/macros-ship/tree/master/analisi_charmdata/interaction_vertices_analysis" %}

The repository antonioiuliano2/macros-ship will be labelled $SHIPMACROS for the rest of this section.

## Jupyter notebooks

### Run the notebooks

The first step is to run 6 jupyter notebooks, one for each CHARM target configuration.&#x20;

They are labelled as CH\*\_protonstudy.ipynb and they are the programs doing the "bulk" of the job:

1. Producing the position and molteplicity histograms of reconstructed vertices
2. Computing efficiency
3. Re-scaling according to the efficiency and the number of protons of MC and Data runs.

All runs are re-scaled to the number of protons from the MonteCarlo simulation

### Merge the results

After running all 6 notebooks, you will have a lot of ROOT files. We need to merge them into two unique file&#x73;_, full\_vz.root_ and _with\_bkg\_full\_vz.root._ I have written a short bash macro for this:

$SHIPMACROS/analisi\_charmdata/interaction\_vertices\_analysis/mergevz.sh

## Compute errors

We need now to compute the errors for the bin content, starting from the Poisson errors and propagate them according to all the performed scaled oprations. This is done by

$SHIPMACROS/analisi\_charmdata/interaction\_vertices\_analysis/[charm\_analysis.py](https://github.com/antonioiuliano2/macros-ship/blob/master/analisi_charmdata/interaction_vertices_analysis/charm_analysis.py)

and a file _log\_charm\_errors.txt_ is produced. At the end of the file, a table of errors for the graph points is computed (currently 29, before 49, after we rebinned runs from CH3 to CH6 from 3 to 6). The table starts like this:

Errors list: 'ibin - err\_DATA - err\_MC - err\_MC\_pr - err\_MC\_hd'&#x20;

0 46.67896612809255 202.93841430345316 200.0399960007998 34.17601498127012

_....._

and it ends at the end of the files. This table alone must be copied into a file _errors.dat,_ which will be used in the next step.

## Global fit

This is the final, and the most thrilling, part of our analysis of proton interactions: we draw all the histograms together and we perform a global fit on vz -> exponential decrease for protons, polynomial of second or third degree for hadron interactions (should we use Chebychev polynomials for the paper?). The fit is performed first separately on the two components, then totally for the total distribution with the function:

expo(0) + pol3(2)

The fit is performed to the Monte Carlo and Data samples. From the results, we can infer the **interaction length** of our target
