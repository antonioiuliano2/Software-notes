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

The algorithm follows the steps designed by Maria during her thesis.



A dataset containing the base-tracks for shower candidate is produced, then provided as input to a Random Forest classifier.

There are two versions, one for simulation csv and the other for data csv

Lettura\_csv analyses the ML output and it produces histograms

### Dataset preparation from simulation

The scripts need to be launched in the following order \(Concat\_dataframe.py is launched twice\):

1. Proiezioni.py
2. Inizio\_sciame.py
3. Rect.py
4. Rect\_crescenti.py
5. Taglio\_Theta.py
6. Concat\_dataframe.py
7. Ricerca\_new.py
8. Concat\_dataframe.py
9. Random\_Forest\_Ishower.py

Proiezioni computes the "next" variables with the projections of the coordinates in the next plate. Inizio\_sciame provides a list of shower injectors. After this, a 1 cm x 1 cm selection in the transverse plane is performed \(Rect.py\). A pyramid is built with increasing rectangles with slope 140 micron and intercept 500 micron. Taglio Theta provides selections in angle and impact parameter. Concat dataframes merges files from different showers \(it is called twice\). Ricerca new actually does the final dataset building, with the selection over the projects. Random Forest Ishower finally does training and test over showers.

### Dataset preparation from data

The scripts need to be launched in the following order \(Concat\_dataframe.py is launched twice\):

1. Proiezioni\_Theta.py

Between 1 and 2, produce "Inizio\_candidati\_sciami.csv" Segments with the same TrackID within first three plates with theta &lt;= 50 mrad. Take last segment of the track

1. Data\_rect.py
2. Data\_rect\_crescenti.py
3. Data\_taglio\_Theta.py
4. Concat\_dataframe.py
5. Ricerca\_complete.py
6. Concat\_dataframe.py
7. Random\_Forest\_Ishower.py

The scripts work as the previous ones, but using as input data instead  of True Monte Carlo information. 

### Energy measurement and histogram production

Let us assume we are analyzing a true Monte Carlo simulation. First, we need to launch Lunghezza sciami ricostruiti RF, which collects events according to their classification \(11, 00, 10, 01\). 

Then, the produced file can be read with Istogramma\_pyroot.py, to produce histograms. Finally Erec.py provides an estimation of energy resolution \(without the calibration step, we assume to already have the parameters\)



## HTCondor workflow

To speed up times by reconstructing the 360 showers in parallel, I have developed a procedure which uses HTCondor submission jobs \(see HTCondor section in other sofware\). The procedure is now as follows:

1. Launch "Proiezioni.py"
2. Submit HTCondor script "PreReco.sub" \(calling "PreReco.sh"\) 
3. Merge dataframes with "Concat\_dataframe.py"
4. Submit HTCondor script "Ricerca\_new.sub" \(calling "Ricerca\_new.sh"\)
5. Merge dataframes with "Concat\_dataframe.py"
6. Launch "Random\_Forest\_Ishower.py"

Note: the python scripts now have input options with parsearg: check first the options by starting the help: for example "python Proiezioni.py -h"

For data the workflow is the same, only file names change:

1. Launch "Proiezioni\_Theta.py"
2. Submit HTCondor script "PreReco\_Data.sub" \(calling "PreReco\_Data.sh"\) 
3. Merge dataframes with "Concat\_dataframe.py"
4. Submit HTCondor script "Ricerca\_complete.sub" \(calling "Ricerca\_complete.sh"\)
5. Merge dataframes with "Concat\_dataframe.py"
6. Launch "Random\_Forest\_Ishower.py"

**File Locations:**

* **$DESYMACROS/condor\_scripts/** \(for HTCondor scripts\);
* **$DESYMACROS/Dataset\_preparation\_ML\_analysis/RUN3\_simulazioni** \(scripts for Monte Carlo\)
* **$DESYMACROS/Dataset\_preparation\_ML\_analysis/RUN3\_Data** \(scripts for real data\) 

