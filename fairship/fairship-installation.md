---
description: The first and most troublesome step of every study
---

# FairShip installation

## Software build with aliBuild

### Local installation

Currently used SHiPBuild software \(12th Collaboration Meeting\).

After installation, to redo build \(local modification, global updates, ...\):

`> cd SHIPBuild` `> FairShip/aliBuild.sh`

Set environment variables with the command

`>alibuild/alienv enter --shellrc FairShip/latest`

### Lxplus installation \(or anywhere else accessing CVFMS\)

The computing group name on lxplus is ship-cg. It can be set from the CERN resource page.

#### For 2020 configuration:

1. Source the environment from CVMFS

```bash
source /cvmfs/ship.cern.ch/SHiP-2020/latest/setUp.sh
```

    2. Do your installation with aliBuild

```bash
aliBuild build FairShip --default fairship --always-prefer-system --config-dir $SHIPDIST
```

#### For 2018 configuration:



1. Source the environment from CVMFS

```bash
source /cvmfs/ship.cern.ch/SHiP-2018/latest/setUp.sh
```

     2. Do your installation with aliBuild

```bash
aliBuild build FairShip --default fairship-2018 --always-prefer-system --config-dir $SHIPDIST
```

#### Entering the environment



     3. Use your new installation

```bash
alienv enter FairShip/latest
```

* **On HTCondor use**

```bash
eval `alienv load FairShip/latest`
```

{% hint style="info" %}
The source at 1. must be done everytime we enter a new lxplus session!

Do not mess up the two configurations! We need to exit the enviroment and re-enter if we want to move from 2018 to 2020 configurations \(or vice-versa\)
{% endhint %}



## Run FairShip simulation

Standard run\_simScript.py simulation, with --Genie option and no options about the detector. Usage:

python $FairShip/macro/run\_simScript.py --Genie -f genie\_nu.root -n nevents -o outputfile.

Important: we have to provide Genie with the positions where we want to generate these neutrinos. Look for

Geniegen.SetPositions\(\)

usually we want to set it from the start to the end of the neutrino target.

## Using c++ macros

\(Updated: This can be avoided by using TTreeReader and removing the include lines, thanks to cling \(clang?\) compiling and loading the shared libraries itself\)

root -l  
`>>#include "FairMCPoint.h"  
>>#include "TMCProcess.h  
>>.x mymacro()`

## Checking geometry output  Event display can be launched in the following way:

`python -i $FAIRSHIP/macro/eventDisplay.py -f simulationfile.root -g geofile.root`

\(actual names of `simulationfile.root` and `geofile.root` depend on the launched simulation\)

Event display is slow, for quick checks it may be more useful to display single volumes directly with ROOT GL \(as from \`[https://github.com/antonioiuliano2/macros-ship/blob/master/drawrootvolume.py](https://github.com/antonioiuliano2/macros-ship/blob/master/drawrootvolume.py)\`\)

Positions and dimensions of volumes can be checked in the following way: `python $FAIRSHIP/macro/getGeoInformation.py -g geofile.root`

Useful options:

* `-v`: name of the volume to expand \(see list of volume daughters\)
* `-l`: 'depth' level of the subnode expansion \(how many daughters are showed\)  



