---
description: What happens inside the black box?
---

# Geant4 interface

## Geant4-VMC

FairShip, apart from various MonteCarlo generators for interaction productions \(Pythia, Genie, MadDump, etc.\), always uses Geant4 for particle propagation and hit recording in the detector.

Geant4 is called by the Geant4-VMC interface, explained in the ROOT documentation:

{% embed url="https://root.cern.ch/geant4-vmc" %}

Basically it loads the TGeo geometry and it runs the Geant4 there.

### Geant4 configuration

{% hint style="info" %}
All these files are listed for reference only. I always use FairShip default values and I avoid to meddle with them, since most of the time I can simply change an option in run\_simScript.py or in the generator. Otherwise it would be very difficult to understand what is going on.
{% endhint %}

The main scripts for geant4 configurations are gconfig/g4Config.C and gconfig/g4config.in. They build the Geant4RunConfiguration and the ShipStack of particles.

Cuts are defined in gconfig/SetCuts.C. Usually they are always at 1 MeV, for various production processes. I still do not know how the energy cut is related to range cut, since from Geant4-VMC guide:

In Geant4, there is defined a unique cut in range which is then converted to an energy threshold per particle and material. The advantage is that you keep the same spatial resolution of your energy deposit over the whole detector.

By the way, this is the cut at production. As told in FairShip simulation, there is another more stringent cut also for saving MCTrack information from an hit \(kinetic energy &gt; 100 MeV by default, it can be removed from run\_simScript.py\)

### Accessing Geant4 information

After launching a fake run\_simScript simulation \(i.e. just 1 event\), with python -i flag in order to leave python open, we can launch the following commands: 

```python
mygMC = ROOT.TGeant4.GetMC()
mygMC.StartGeantUI()

```

then it will open the Geant4 interface, where we can dump various kinds of information, for example:

```text
/cuts/dump
/particle/list
/particle/select D+
/particle/properties/dump
/particle/process/dump
```

For information and all the commands the Geant4-VMC and Geant4 user guides are optimal references

## Treatment of FairMCPoints

MCPoints register the passage of a particle in an active volume, by combining the information from all the Geant4 steps happened in that volume. Position is computed as mean position between the steps when track entered and exited the volume, momentum is the one at the exit, while energy loss is summed from all the energy depositions within each step.

By default, many FairRoot and FairShip classes **do not store** hits with null total energy loss in the volume \(fELoss==0.\). Since this is troublesome for hits in ECC bricks, I am currently investigating the source

