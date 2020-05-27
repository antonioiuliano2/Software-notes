---
description: All starts from one electron
---

# Shower reconstruction

## Introduction

A lot of effort has been done, for OPERA experience and other studies with emulsion, to provide algorithms for shower reconstruction and electron/pion identification.

The starting algorithm is the one designed at Neuchatel by F. Juget:

_Electron/pion separation with an Emulsion Cloud Chamber by using Neural Network_ - JINST 2 P01001 \(2007\)

A lot of variants have been designed since then, but the basic logic should still be valid. The algorithm follows these two steps:

* Research for segments in a cone of given aperture angle \(default 20 mrad\), according to acceptance in position DR and angle DT;
* Application of Neural Network for electron/pion identification

## Libraries

### libShower/EdbShowerRec

This class is the most complete one. It is written to accept data in various formats, and then perform shower reconstruction on them. I usually pass the data by building EdbPVRec objects in them. I currently launch the following calls to this class:

1. Object instatiation: EdbShowerRec \* showerrec = new EdbShowerRec\(\);
2. Reset Arrays: showerrec-&gt;ResetInBTArray\(\) and showerrec-&gt;ResetRecoShowerArray\(\);
3. Load Initiator Basetracks: showerrec-&gt;SetInBTArray\(myInBTArray\);

   This can be checked if worked with: showerrec-&gt;PrintInitiatorBTs\(\);

4. Load PVRec container: showerrec-&gt;SetEdbPVRec\(myEdbPVRec\);
5. Set AliSub: showerrec-&gt;SetUseAliSub\(0\);
6. Start actual reconstruction: showerrec-&gt;Execute\(\);
7. Check output of shower reconstruction: showerrec-&gt;PrintRecoShowerArray\(\);

This operations are now done from the usual ScanSet interface with shower\_reconstruction.C script. The parameters are set in a rootrc file, called showerrec.rootrc. 

**Default parameters:**

* **showerrec.nbrick**: 1
* **showerrec.cpcut:** 1
* **showerrec.trkcut:** nseg&gt;3 && s\[nseg-1\].ePID==52
* **showerrec.ConeRadius:** 800 
* **showerrec.ConeAngle:** 0.1 
* **showerrec.ConnectionDR:** 150
* **showerrec.ConnectionDT:** 0.15
* **showerrec.NPropagation:** 3 
* **showerrec.outdir: ..** 
* **showerrec.env:** showerrec.rootrc
* **showerrec.EdbDebugLevel:** 1

A tree called **Shower.root** should be produced. This contains the information about the segments contained in the shower and the ouput of the neural network

### libShowRec/EdbShowRec

This class has been recently \(2019\) updated by Frank Meisel. It has a larger set of options with respect to the old one, allowing to compare between different parameters and compute efficiency from MC simulations.

From a practical point of view, to use it we need to:

* Copy the folder $FEDRA\_ROOT/src/libShowRec/ShowRec in our user directory
* Modify it as we need for our data/simulation, then launch make to compile it
* Launch it with ./ShowRec/ShowRec and all the options as in the help information.

Required input files:

* A PVRec with the couples. It is needed to have set eEdbPVRec-&gt;eDescendingZ = 1, so the pattern with **larger number** has **lower z;**
* A linked\_tracks.root file, to take injectors from. -LT1 takes the first segment, -LT2 takes the last segment. It is not needed if -LT0 option is used, but this option requires a lot of time, since injectors are taken directly from the couples;

Additional input file:

* ParameterSet tree: this allows to perform simulation with different sets of parameters \(each entry is a different set\). If the tree is not available, only one reconstruction with default values will be performed;
* Particle Gun simulation text file: This text file contains the information from a MC simulation, to provide information about efficiency of reconstruction.

Example of working command \(parameter set from entry 0 to entry 2 of parameter set tree\):

```bash
./ShowRec/ShowRec -ALI2 -NP29 -LP29 -MP14 -FP1 -HPLZ0 -LT1 -OUT2 -PASTART0 -PAEND2
```

It will provide as output many files: a shower tree file, an efficiency tree file, a histo file, and so on...

In order to the MC results to be reliable, you should provide the following variables:

* MCEvt and MCTrack, as with the standard s.eMCEvt and s.MCTrack members;
* PdgCode of the particle, as in s.Flag\(\);
* Momentum of the particle, stored in P\(\);



