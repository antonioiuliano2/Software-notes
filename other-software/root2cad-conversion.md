---
description: Long requested!
---

# ROOT2CAD Conversion



## ROOT2CAD GitHub repository

Kindly provided by the Electron Ion Collider software group: [https://github.com/eic/root2cad?tab=readme-ov-file](https://github.com/eic/root2cad?tab=readme-ov-file)

It allows to convert from the usual ROOT Geometry format into the CAD format, GLTF

### Installation

Please refer to the README.md notes, but there is a caveat to be mentioned here. Even if it just refers to node >13, newest version installation will fail, due to "assert" being removed after node 22:

{% embed url="https://stackoverflow.com/questions/73746631/unexpected-identifier-error-when-import-json-file-in-nodejs" %}

After some tests, I manage to have a working installation with node 18.20.0 (npm 10.7.0, automatically installed by the formed). Use

* nvm install 18
*   npm install -g root2cad



Then, it should work (do `nvm use 18` when you open a new terminal)

### Example of usage with FairShip/SNDSW Geometry

* Listing volumes:
* root2cad geofile\_full.conical.PG\_13-TGeant4.root FAIRGeom --ls --ls-depth 2
* root2cad geofile\_full.conical.PG\_13-TGeant4.root FAIRGeom volTarget -o Target.gltf

Then we can check it and use it how we need to
