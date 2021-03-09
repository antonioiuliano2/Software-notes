---
description: Good Training for a good estimation
---

# Preparing dataset for ML studies

## Scipy\_conversion

The code to prepare numpy arrays with EdbSegP information and store them into a pandas dataframe is the following

```
https://github.com/antonioiuliano2/Tutorial-DESY19/blob/master/scipy_conversion/desy19_fedrautils.py
```

It can build a dataframe with position, angles, true MC info and other required information.

I have also added an option to project the position to a new list of z positions, if we want to merge data from different configuration \(z positions should not differ too much, to avoid big uncertainties on the positions\)

### Background/signal dataset preparation

The signal with the electromagnetic showers is provided from simulation.

The background is provided from real data, which especially for the first plates is dominated by background. 2 possible datasets are envisioned:

1. 1st -2nd plates only background: we take the data from the first two plates, then we duplicate it with simple transformation \(1D reflections on single features\) to reproduce them in later plates. Signal is stored from all plates.
2. First 10 plates background/signal: we take all the data from the first 10 plates of a RUN, and the first 10 plates from simulation. This allows to study correlation between features of nearby segments, to separate them from background. We cannot take more plates, otherwise the signal taken from data and treaten as background might be significant.



## Dataset construction algorithm

The algorithm follows the steps designed by Maria 



