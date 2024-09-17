---
description: and again and again and again...
---

# Software stack test build

## Purpose

During my two year-long astronomical hiatus, one of the few activties I am regularly doing for SHiP is testing new stacks, in support to the Software Coordinator (Oliver).&#x20;

For this, I am connecting to a Virtual Machine provided by [CERN Openstack ](https://openstack.cern.ch/)service, where the latest LTS Ubuntu version (currently 22.04) supported is mounted.

The machine is only accessible from lxplus, and it uses my ssh key for access.



## Test-Building a new stack

Basically, create a path under the "fake" path /cvmfs/ship.cern.ch/. This naming is used to avoid confusion, when the package will be shipped to the CVMFS file system.

Then, clone the cvmfs\_release current master into the "release" with naming convention YY.MM (i.e. 24.09):

```bash
git clone --recurse-submodules https://github.com/ShipSoft/cvmfs_release 24.09
```

Please remember to update shipdist to the latest master (or the version Oliver will point me to)

Then, build FairShip and all related packages as usual:

```
aliBuild build FairShip --config-dir shipdist --defaults release --always-prefer-system
```



