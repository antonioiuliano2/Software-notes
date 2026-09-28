---
description: Launch Geant4 simulation
---

# Aegir

## Introduction

The aegir repository is the following:&#x20;

{% embed url="https://github.com/ShipSoft/aegir" %}

It uses **phlex** to launch the simulation, according to workflows provided in jsonnet files

### Example of workflows

You can find the examples in the repository, I have tested the following:

* Particle gun;
* GENIE reader (reads a **rootracker** file);

Also the geometry can be provided as the following:

* Builtin (for simple test);
* GeoModel file (DB from the official Geometry repository);
* GDML file (for compatibility with other formats, and for FairShip and sndsw simulations;

Parameters are provided directly in the jsonnet, as standard JSON dictionary.

Remember that **sensitive volumes** need to be pointed to aegir as a list.

The Geometry **does not know** which volume is sensitive. (different from the previous FairShip approach)
