---
description: Or simply local correction? I do not know anymore
---

# Local reconstruction in cells

There are two ways

## Map correction with volume tracks

Studied by Valeri, four plates at the times, correcting and computing

## Mapping together small cells

Cells can be processed in parallel with HTCondor. First sub and sh scripts have been written, but they mostlly use link.rootrc and creates hundreds of files! Not viable in the long terms.

The files then can be merged together.

The procedure is:

* create symbolic link to tracks.raw.root as usual;
* copy the link.rootrc files from [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/firstlink.rootrc](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/firstlink.rootrc) and [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/secondlink.rootrc](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/firstlink.rootrc);
* copy [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/firstlinking\_cell.sh](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/firstlinking\_cell.sh) and [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/secondlinking\_cell.sh](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/secondlinking\_cell.sh)
* check that the files options are correct for the analyzed brick
* copy firstlink.rootrc in link.rootrc&#x20;
* Submit in condor [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_first.sub](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_first.sub)
* Check if files have been produced correctly with [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/filecheck.sh](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/filecheck.sh). Eventually rerun condor for missing files with [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_first\_reco.sub](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_first\_reco.sub)
* copy secondlink.rootrc in link.rootrc
* Submit in condor [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_second.sub](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_second.sub)
* Again, check files with [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/filecheck\_secondlinking.sh](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/filecheck\_secondlinking.sh). Eventually, rerun condor with [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_second.sub](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/condorlinking\_second.sub)
* Merge linked files with [https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/merge\_cells.sub](https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/condor\_scripts/merge\_cells.sub)

## Overlapping cells

Cells must overlap (about 2.5 mm for 1 cm cell), at least during alignment and tracking!

Otherwise we lose a lot of tracks going from a cell to another!

Passing tracks can be actually used to match overlapping cells together, so the corrections go smootly from a cell to another.
