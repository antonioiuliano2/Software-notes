# Access simulation output

The simulation produces two files:

* a geometry file, called **geofile\*.root (**full name depends on the simulation launched);
* a file with the **cbmsim** TTree containing the hits in the detectors, called **ship\*.root**

## Inspecting geometry file

The geometry file contains the positions and dimensions of all the volumes implemented in this simulation:&#x20;



```bash
python $SNDSW/macro/getGeoInformation.py -g geofile.root
```

will print all mother volumes (i.e. the main volume for each detector class). To inspect a volume **myvolume** in more detail, the following options can be added:

```bash
python $SNDSW/macro/getGeoInformation.py -g geofile.root -v myvolume -l nlevels
```

where **nlevels** repretents the required depth to inspect the hierarchy volume (i.e. daughters, granddaughters volumes, etc.).

The positions of the volumes are in the global sndsw reference system.

Position of a node can be accessed from its path, with the function local2Global(path).

Example:

```python
local2Global("/cave_1/Detector_0/volTarget_1/Wall_0/Row_0/Brick_0/Emulsion_0")
```

&#x20;

It will return a dictionary with position information

## Reading simulation file

In order to analyze the produced simulation file, a script needs to be written to access the **cbmsim** output TTree. Each entry of this tree is an event (in this case, a neutrino interaction), and it contains:

* A TClonesArray with the produced particles, objects of the **ShipMCTrack** class
* TClonesArrays with the **FairMCPoints** left by the particle in each detector (each detector has a different branch: EmulsionDetPoint, ScifiDetPoint, MuFilterPoint). To save memory, emulsion hits are disabled by default. They can activated by setting c.EmulsionDet.PassiveOption = 1 in **$SNDSW/geometry/shipLHC\_geom**_**\_**_**config.py** before launching the simulation

Accessing this information in a Python interface in sndsw environment is quite straightforward, thanks to the PyROOT interface. It is enough to loop into the TTree and access the branches:

```python
import ROOT as r
inputfile = r.TFile.Open("ship.conical.Genie-TGeant4.root") 
inputtree = inputfile.Get("cbmsim") 
for event in inputtree: 
    tracks = event.MCTrack
    mufilterhits = event.MuFilterPoint
    print (tracks[0].GetPx()) 
```

Direct loop-based access like this small example are usually faster in a ROOT C++ based script, which however requires to set the branches beforehand. Two examples can be seen here:

* Example witihn sndsw environment: the simplest way is to use TTreeReader class ([https://root.cern.ch/doc/v608/classTTreeReader.html](https://root.cern.ch/doc/v608/classTTreeReader.html)):

```cpp
TChain treechain("cbmsim");
treechain.Add("file1.root");
.....//eventually add more file to the tchain
TTreeReader reader(&treechain);
TTreeReaderArray<ShipMCTrack> tracks(reader,"MCTrack");
TTreeReaderArray<MuFilterPoint> mufilterpoints(reader, "MuFilterPoint");
//start the loop
const int nentries =treechain.GetEntries();
for(int ientry = 0;ientry<nentries;ientry++){
 reader.SetEntry(ientry);
 cout<<tracks[0].GetPx()<<endl;
} 
```

* Example without sndsw environment: [https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC\_Reader.C](https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC\_Reader.C) and [https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC\_Reader.](https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC\_Reader.C)h files: here the methods are not called, but the information is retrieved directly from the TTree branches  containing the attributes.&#x20;

## Note about copying and cloning TTree

CopyTree() and CloneTree() are useful commands to select a TTree subsample. However, due to FairROOT and ROOT type variables, before using them with new files the following command needs to be launched:

```python
ROOT.gInterpreter.ProcessLine('typedef double Double32_t')
```

If you do not do that, it will corrupt **any branch with double variable**



## Checking geometry overlaps

Currently, there are too many SciFi volumes, therefore default checkoverlaps leads to memory errors. Need to apply the following function:

def checkOverlaps():

sGeo = ROOT.gGeoManager

for n in range(1,6):

&#x20;   Hscifi = sGeo.FindVolumeFast('ScifiVolume'+str(n))

&#x20;   removalList = \[]

&#x20;   for x in Hscifi.GetNodes():

&#x20;         if x.GetName().find('Scifi')==0: removalList.append(x)

&#x20;   for x in removalList: Hscifi.RemoveNode(x)

sGeo.SetNmeshPoints(10000)

sGeo.CheckOverlaps(0.1)  # 1 micron takes 5minutes

sGeo.PrintOverlaps()

\# check subsystems in more detail

for x in sGeo.GetTopNode().GetNodes():

&#x20;  x.CheckOverlaps(0.0001)

&#x20;  sGeo.PrintOverlaps()



Inspecting geometry values also helps:

python -i run\_simSND.py -n 0 –PG,

&#x20;

emu = modules\["EmulsionDet"]

emu. GetConfParF("EmulsionDet/TotalWallZDim")



