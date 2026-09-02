# Geometry

## Introduction

In the new framework, everything is developed as a separate repository, to make everything self-contained.

The Geometry repository is located in [https://github.com/ShipSoft/Geometry](https://github.com/ShipSoft/Geometry).

## Building and geometry creation

Use the following commands to build the package and create the geometry file:

```bash
pixi run build 
pixi ./build/apps/build_geometry [output_file.db]
```

The [GeoModel](https://geomodel.web.cern.ch/home/start/install/) geometry file can be inspected with [gmex](https://geomodel.web.cern.ch/home/components/geomodelvisualization/gmex/), and converted into gdml with the following:

```bash
pixi run gdml -g testmgc.db -o testmgc.gdml
```

Then the GDML file can be imported in ROOT and inspected as usually
