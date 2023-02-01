---
description: How full we want our detector
---

# Muon flux

Study of neutrino detection and bacgkround rejjection in the new location

## Simulation files by Eduard

Prepared information of muon hits in 3 scoring planes:

* sco\_0Point, z = -3100;
* sco\_1Point,  z = -2800;
* sco\_2Point, z = -2600;

Analysis performed with script:

[https://github.com/antonioiuliano2/macros-ship/blob/master/ecn3\_ship/loop\_eduardmuonfile.C](https://github.com/antonioiuliano2/macros-ship/blob/master/ecn3\_ship/loop\_eduardmuonfile.C)

After analysis, resulted TTrees and plots are stored in my EOS:

/eos/user/a/aiuliano/public/sims\_FairShip/EduardSimulationsSHiPECN3/

Using the function create\_tree(), the information from the 3 scoring plane is stored into an NTuple.

Before launching this code, it is necessary to activate my kerberos credentials: kinit aiuliano@CERN.CH

Then, using read\_tree(0), I read the data from scoringplane 0 with RDataFrame and store into output file (use 1,2 for reading different tree).

plotmuondensity(0) plots the muon density with large bins, and compute muon density for cm2 in an area&#x20;

##
