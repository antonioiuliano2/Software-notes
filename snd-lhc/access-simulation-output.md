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



\*\*\*\*



