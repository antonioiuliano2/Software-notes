# Momentum measurement

## Measurement with Multiple Coulomb Scattering

There are many algorithms in FEDRA for momentum measurement through MCS. The code is written in src/libEdr/EdbMomentumEstimator.h. The function PMS\(\) launch the algorithm according to parameter eAlg:

* **eAlg 0: PMSang**\(eTrack\);
* **eAlg 1: PMSang\_base\(**eTrack\);
* **eAlg 2: PMSang\_base\_A** \(eTrack\);
* **eAlg 3: PMScoordinate**\(eTrack\);
* **eAlg 4: PSMang\_corr**\(eTrack\)

