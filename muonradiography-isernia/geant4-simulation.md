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

The map files are originally in a GeoTiff format, then converted into plain csv data.

{% hint style="info" %}
To do: add info about GeoTiff->csv conversion
{% endhint %}

Copy the "isernia.csv" and "map\_1m" files into the analysis folder

The "filter\_map.C" script can be used to filter the map\_1m into a smaller version.

The map is used to build voxels which becomes Geant4 volumes. The number of voxels is very high (24605889!), and try to visualize the geometry leads to a crash. It may be useful to create a branch with the reduced geometry for visual inspection.

In the meantime, the script yzprofile.cpp

### Launch simulation



nohup ./mura run\_gps\_.mac > out &

This produces:

* lengths (total lengths that each muon traverses in rock)
* absorbed\_num (the numbers of absorbed muons)

Other files are empty since they are commented by default

### Histograms



First, add a line at the beginning of each file with the numbers of lines.

Then, execute the Histo.C script in analysis to read the above files.

### General notes and future work:



Needed to temporary disable MultiThreading since it requires some changes in the code to be supported in Geant4 10.7.4

Re-enabling it should not be complex, to be fixed.

