---
description: List of geometry classes
---

# Sndsw geometry

## Introduction

The **shiplhc** folder is the one dedicated to SND@LHC. It is made of the following components:

* EmulsionDet class;
* SciFi class;
* MuFilter class \(also containing veto\);

## Geometry classes and functions

Each component of the SND@LHC detector has its own geometry class.

 The ConstructGeometry\(\) function contains the required geometry objects and commands which describe our geometry. The geometry employs classes and methods from ROOT TGeo \([https://root.cern/manual/geometry/](https://root.cern/manual/geometry/)\)  


## Event display

EventDisplay is launched with a similar macro of the FairShip one:

```text
python -i $SNDSW/macro/eventDisplay_shipLHC.py -f simulationfile.root -g geofile.root
```

It will show the tunnel with the detector and all components:

* Hide all components except the tunnel with SND;
* Go to Guides -&gt; select center -&gt; pick center -&gt; select SND with mouse;
* Go to Clipping -&gt; select plane -&gt; select Edit in Viewer;
* Set aX+ = -1.0 , bX+ = 0.
* Set d = -220 and click Apply
* ROOT.gGeoManager.SetNsegments\(80\) to make picture more smooth

Something like this should appear:

![SND geometry event display](../.gitbook/assets/snddisplay_withveto_tunnelopened.png)

 

