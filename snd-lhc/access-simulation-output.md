# Access simulation output

The simulation produces two files:

* a geometry file, called **geofile\*.root \(**full name depends on the simulation launched\);
* a file with the **cbmsim** TTree containing the hits in the detectors, called **ship\*.root**

## Inspecting geometry file

The geometry file contains the positions and dimensions of all the volumes implemented in this simulation: 



```bash
python $SNDSW/macro/getGeoInformation.py -g geofile.root
```

will print all mother volumes \(i.e. the main volume for each detector class\). To inspect a volume **myvolume** in more detail, the following options can be added:

```bash
python $SNDSW/macro/getGeoInformation.py -g geofile.root -v myvolume -l nlevels
```

where **nlevels** repretents the required depth to inspect the hierarchy volume \(i.e. daughters, granddaughters volumes, etc.\).

The positions of the volumes are in the global sndsw reference system

## Reading simulation file

In order to analyze the produced simulation file, a script needs to be written to access the **cbmsim** output TTree. Each entry of this tree is an event \(in this case, a neutrino interaction\), and it contains:

* A TClonesArray with the produced particles, objects of the **ShipMCTrack** class
* TClonesArrays with the **FairMCPoints** left by the particle in each detector \(each detector has a different branch: EmulsionDetPoint, ScifiDetPoint, MuFilterPoint\). To save memory, emulsion hits are disabled by default. They can activated by setting c.EmulsionDet.PassiveOption = 1 in **$SNDSW/geometry/shipLHC\_geom**_**\_**_**config.py** before launching the simulation

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

* Example witihn sndsw environment: the simplest way is to use TTreeReader class \([https://root.cern.ch/doc/v608/classTTreeReader.html](https://root.cern.ch/doc/v608/classTTreeReader.html)\):

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

* Example without sndsw environment: [https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC\_Reader.C](https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC_Reader.C) and [https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC\_Reader.](https://github.com/antonioiuliano2/macros-snd/blob/master/tutorials/SNDLHC_Reader.C)h files: here the methods are not called, but the information is retrieved directly from the TTree branches  containing the attributes. 



\*\*\*\*



