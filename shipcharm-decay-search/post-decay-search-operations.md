---
description: Further annotations
---

# Post decay search operations

## Required input

Following the decay search, a root file with decay search information should have been produced. Each entry of this tree correspond to a **primary vertex** , and there are vectors for associated **secondary vertices** and **their tracks**.

The script 

[https://github.com/antonioiuliano2/macros-ship/blob/master/analisi\_charmsim/selection\_decaysearch\_sim.C](https://github.com/antonioiuliano2/macros-ship/blob/master/analisi_charmsim/selection_decaysearch_sim.C) 

will add new branches, with further variables and boolean checks.

## Provided new variables

The new computed variables \(that is, no simple booleans\) are the following ones:

* **vtx\_mc\_ev :** MCEventID of the primary vertex. Computed as the most common MCEventID of its tracks;
* **dsvtx\_vtx2\_dl**: Decay length of charm candidate. Computed as the 3D distance between the points of primary and secondary vertices;
* **dsvtx\_vtx2\_tau: Approximated estimated mean life of charm candidate. Computed from average kink angle and impact parameter of daughter track to primary vertex;** 
* **dsvtx\_vtx\_phicharm**: Phi angle of candidate charmed hadron. Computed from the positions of primary and secondary vertices
* **dsvtx\_vtx2\_endingdeltaphi**: Angular difference between a tracked charm candidate and its estimation. Computed from the angles of a reconstructed track linking primary and secondary vertex. If no such track exists, **return -10**, if more than one exist, **return 10**  
* **dsvtx\_vtx2\_meanphi**: Average phi angle of charm daughter candidates. Computed from the angles of tracks starting at the secondary vertex.
* **dsvtx\_vtx2\_phidifference**: Difference of the aforementioned average mean phi angle and the estimated phi angle of charmed hadron candidate.

## Provided checks

Additional boolean variables are provided, to select events according to the following criteria

* **dsvtx\_vtx2\_2starting:** The secondary vertex contains at least two tracks starting from it
* **dsvtx\_vtx2\_2starting\_trk:** The track belongs to a secondary vertex which satifies the previous condition
* **dsvtx\_vtx2\_2goodtrks:** The secondary vertex contains at least two tracks with at least 3 segments
* **dsvtx\_vtx2\_2goodtrks\_trk:** The track belongs to a secondary vertex which satifies the previous condition



* **dsvtx\_vtx2\_trk\_charmdaughter:** The track is a charm daughter \(MCMotherID 1 or 2\)
* **dsvtx\_vtx2\_charmdaughter:** The vertex contains at least one charm daughter \(see previous condition\);
* **dsvtx\_vtx2\_trk\_samevent:** The track is from the same MCEvent of the primary vertex;
* **dsvtx\_vtx2\_samevent:** the secondary vertex contains at least one track from the same MCEventID of the primary vertex \(see previous condition\) 
* **dsvtx\_vtx2\_charmdaughtersamevent**: The secondary vertex contains at least one charm daughter, which is also from the same MCEventID of the primary vertex \(it practically combines, for the same track, the previous conditions\)
* **dsvtx\_vtx2\_positivedz**: The difference between z position of secondary and primary vertex is larger than 0 \(i.e. secondary vertex is downstream of the primary one\)
* **dsvtx\_vtx2\_trk\_positivedz**: The track belongs to a secondary vertex which satifies the previous condition

Finally, a good secondary vertex is estimated by requiring positive dz and at least one charmdaughter from the same MC event. This provides the last variable:

* **vtx\_topology:** 1 for single charm, 2 for double charm, 0 otherwise.



