# Data Format

## Introduction

The new Data Format is by definition RNTuple compatible. It is available at the following repository: [https://github.com/ShipSoft/data-model/tree/main](https://github.com/ShipSoft/data-model/tree/main)

It contains the following:

* Event Header (event metadata);
* MCParticle (MC/generation);
* SimHit (Simulation, i.e. old MCPoint);
* SimParticle (Simulation, contains also end point);
* SimResult (bundle of everything sim related);
* RecParticle (reconstruction)

The data format is used by sea\_cucumber for event display
