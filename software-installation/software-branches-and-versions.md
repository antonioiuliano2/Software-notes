---
description: Because you always forget where your code is
---

# Software branches and versions

## Analysis/Readout scripts

All scripts MUST be on Git repository, and status of repository clones should be checked regularly. I have three main repositories:

1. macros-ship: the largest one, with all simulation, FEDRA conversion and data analysis codes;
2. Tutorial-DESY: containing the code for DESY19 electron testbeam data;
3. macros-snd: the newest one, for now very small. It will be updated as SND marches on;

All this code, serving for my personal purpose only, has no branches \(I commit directly on master\).

## FairShip branches

The main active branches of my FairShip forks are the following:

1. charmsim: for SHiP-charm simulation;
2. shipdesy: again, for DESY19 data;
3. annaritasnd\_tungsten: temporary, for Annarita's fork of FairShip, with SND prototye software

## FEDRA software versions

There are many versions, especially on nusrv9. They are built on SVN, which is the ancestor of GIT:

1. Default one, used for standard alignment/tracking reconstruction. It SHOULD NOT be used for vertex reconstruction \(but now it would crash, so I would immediately notice it\)
2. The official SVN repository, a clean clone can be found in: /ship/CHARM2018/fedra\_official/. It must be used for shower reconstruction, and when updated but clean code needs to be used;
3. My test SVN repository: /ship/CHARM2018/fedra\_test: it is the only one which contains the vertex tree IO routines, so it is the one used for SHIP-CHARM vertex reconstruction.

lxplus version should match my test SVN repository \(version 3\).





