# Geant4 Simulation

## Introduction

Simulation of muons at the landfill Tufo Colonoco in Isernia.

### Software dependencies

* Geant4 (version: 10.7.4)
* ROOT (for visualization and plotting scripts, tested version 6.40.02)

### Build instructions

Only the first time, retrieve the repository with

`git clone git@github.com:antonioiuliano2/MURA_ISERNIA.git ISERNIA`

Setup your Geant4 installation

`source /yourprepath-to-Geant4-install/bin/geant4.sh`

Build the Geant4 application

```
mkdir Build_ISERNIA
cd Build_ISERNIA
cmake ../ISERNIA/
make -j4
```

Create a link to the analysis folder `ln -s ../ISERNIA/analysis/ ./analysis`

## Simulation input file preparation

### Generate muons

Before launching any simulations, generate the muons file:

```
root -l
 .L ../ISERNIA/analysis/testcode.cpp
 main()
```

The muons are distributed according to Boganova and al. Find reference, check Stromboli paper?

copy or link the resulted csv file into the analysis folder

Note: test file is 100k muons, sensible studies are done with 400 mil file

It can be done easily in HTCondor, since the output is simply a text file, and order is not important. I have checked that already seeds are different (different productions have different entries)

### Copy ISERNIA map file

It contains the area of approx 5.5x6 km around the landfill with a 0.5x0.5 m step. The second file is a cruder version of this map with 1x1 m voxels that I used for all the simulations.

The map files are originally in a GeoTiff format, then converted into plain csv data.

{% hint style="info" %}
To do: add info about GeoTiff->csv conversion
{% endhint %}

Copy the "isernia.csv" and "map\_1m" files into the analysis folder

The "filter\_map.C" script can be used to filter the map\_1m into a smaller version.

The map is used to build voxels which becomes Geant4 volumes. The number of voxels is very high (24605889!), and try to visualize the geometry leads to a crash. It may be useful to create a branch with the reduced geometry for visual inspection -> done, using filtered\_map (is this the region of the landfill?).

In the meantime, the script yzprofile.cpp make a zy profile of the map, with the detector position

#### About the map\_1m file

This is the "cruder" map version with 1 m x 1 voxel:

* **Headers**: two lines, xmin and xmax, followed by ymin and ymax;
* **Columns**: (x,y,z), where x and y are the voxel coordinates, and z is the height. It is currently filled in the geometry with the same density (standard rock, 2.65 g/cm3).

The geometry is defined in **A01DetectorConstruction.cc** and the voxels in **VoxelParametrisation.cc.** There are 24,605,889 voxels in the whole file.

The **filter\_map.C** script creates a cross section of the map, at the following coordinate ranges:

* x in (431867.400 + 2454. + 100., 431867.400 + 2454. + 400.)
* y in (4610136.370 + 2917., 4610136.370 + 2917. + 350.)

The resulting file contains 105,651 voxels,  so a factor about 1./233 less. It is suited for visualization in Geant4 and export to gdml, also for testing different implementations of densities.&#x20;



### Launch simulation



nohup ./mura run\_gps\_.mac > out &

This produces:

* lengths (total lengths that each muon traverses in rock)
* absorbed\_num (the numbers of absorbed muons)

Other files are empty since they are commented by default

### Histograms



First, add a line at the beginning of each file with the numbers of lines.

Then, execute the Histo.C script in analysis to read the above files.

This will make the plots on the sensitivity, according to traversed length and number of absorbed muons, at different angles.

To make the plot of the segments, we first need to store the segments, obviously. So we need to uncomment the line in SteppingAction.

## Split Voxels

### Geometry

Splitting voxels at landfill, only the upper part must be different from rock density (1.0 g/cm^3 instead of 2.6 g/cm^3).

Geant4 does NOT allow two separate parameterization classes from the same logical volume. What I needed to do is to define a ComputeMaterial function to set the material according to voxel number. For information, check user guide [https://geant4.web.cern.ch/documentation/pipelines/master/bfad\_html/ForApplicationDevelopers/Detector/Geometry/geomPhysical.html?highlight=g4vpvparameterisation](https://geant4.web.cern.ch/documentation/pipelines/master/bfad_html/ForApplicationDevelopers/Detector/Geometry/geomPhysical.html?highlight=g4vpvparameterisation) and example `examples/extended/runAndEvent/RE02`

The material at voxel parameterization is not part of the logical volume, so both the geant4 UI and the GDML will not report it. The only way to confirm the right material is to visualize the geometry (only the landfill part) and cross-check the full simulation at stepping actions log with a few number of muons. Did that confirms that everyting worked.

### Lengths and Absorbed muons

Note that the condition of "absorbed" is NOT done by Geant4 directly, it is **hard-coded** in the SteppingAction code. The code was originally written for voxels of uniform density, so the length is simply multiplied by the density at the exit of the volume. Here, instead, the volume is made of voxels of different densities, so we need to multiply by the density **at each step**. After this change, I noticed the change in the number of absorbed muons.

### General notes and future work:





Needed to temporary disable MultiThreading since it requires some changes in the code to be supported in Geant4 10.7.4

Re-enabling it should not be complex, to be fixed. -> It can be fixed, but then file writing becomes messed up between multiple threads. It needs more time, so I will delay it after the updates to the single thread version



