# Momentum measurement

## Measurement with Multiple Coulomb Scattering

There are many algorithms in FEDRA for momentum measurement through MCS. The code is written in src/libEdr/EdbMomentumEstimator.h. The function PMS\(\) launch the algorithm according to parameter eAlg:

* **eAlg 0: PMSang** \(eTrack\): default version, modified by Magali at the end of 2008
* **eAlg 1: PMSang\_base \(**eTrack\): original version \(Momentum estimation by multiple scattering \(Annecy algorithm Oct-2007\)\)
* **eAlg 2: PMSang\_base\_A**  \(eTrack\): include asymmetrical errors in scattering VS ncell graphs, based on ChiSquare distribution
* **eAlg 3: PMScoordinate** \(eTrack\): momentum estimation by coordinate method
* **eAlg 4: PSMang\_corr** \(eTrack\): with corrections by Ash

Momentum measurement with PMSang was used also in SHiP-charm by Valerio

