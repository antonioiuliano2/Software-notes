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

This class is the old one. It is written to accept data in various formats, and then perform shower reconstruction on them. I usually pass the data by building EdbPVRec objects in them. I currently launch the following calls to this class:

1. Object instatiation: EdbShowerRec \* showerrec = new EdbShowerRec\(\);
2. Reset Arrays: showerrec-&gt;ResetInBTArray\(\) and showerrec-&gt;ResetRecoShowerArray\(\);
3. Load Initiator Basetracks: showerrec-&gt;SetInBTArray\(myInBTArray\);

   This can be checked if worked with: showerrec-&gt;PrintInitiatorBTs\(\);

4. Load PVRec container: showerrec-&gt;SetEdbPVRec\(myEdbPVRec\);
5. Set AliSub: showerrec-&gt;SetUseAliSub\(0\);
6. Start actual reconstruction: showerrec-&gt;Execute\(\);
7. Check output of shower reconstruction: showerrec-&gt;PrintRecoShowerArray\(\);

A tree called **Shower.root** should be produced. This contains the information about the segments contained in the shower and the ouput of the neural network

