---
description: How full we want our detector
---

# Muon flux

Study of neutrino detection and bacgkround rejjection in the new location

## Simulation files by Eduard

Prepared information of muon hits in 3 scoring planes:

* sco\_0Point, z = -3100;
* sco\_1Point,  z = -2800;
* sco\_2Point, z = -2600;

Analysis performed with script:

[https://github.com/antonioiuliano2/macros-ship/blob/master/ecn3\_ship/loop\_eduardmuonfile.C](https://github.com/antonioiuliano2/macros-ship/blob/master/ecn3_ship/loop_eduardmuonfile.C)

After analysis, resulted TTrees and plots are stored in my EOS:

/eos/user/a/aiuliano/public/sims\_FairShip/EduardSimulationsSHiPECN3/

Using the function create\_tree(), the information from the 3 scoring plane is stored into an NTuple.

Before launching this code, it is necessary to activate my kerberos credentials: kinit aiuliano@CERN.CH

Then, using read\_tree(0), I read the data from scoringplane 0 with RDataFrame and store into output file (use 1,2 for reading different tree).

plotmuondensity(0) plots the muon density with large bins, and compute muon density for cm2 in an area&#x20;

## MuonDIS generation

Starting from a datasets of muons crossing a scoring plane, the next step is to generate their Deep Inelastic Scattering (DIS) interactions, in order to have the products of this interactions.

Currently, this step is doing with Pythia6, even if continuous work is being done to move forward to Pythia8. The script I am referring to is from [https://github.com/olantwin/muonDIS](https://github.com/olantwin/muonDIS), where I have then adapted for the input **pickle** format by Luis, into this code,[ makeMuonDISmod.py](https://github.com/antonioiuliano2/macros-ship/blob/master/muon_background/makeMuonDISmod.py). The pickle files come from simulation of all muons from the full sample (\~around 400M events, corresponding to \~506M muons).

Coordinates transfer from Luis system (meters, with offset in z) to FairShip (cm):\


z -= 50

z \*= 100

x\*=100

y\*=100

An example of launching this code is the following:

```bash
python /afs/cern.ch/work/a/aiuliano/public/macros-ship/muon_background/makeMuonDISmod.py 
-f $INPUTFILES/muons_data_0.pkl 
-nJobs $ProcId 
--nPerJobs $NEVENTS 
-nDISPerMuon 2
```

&#x20;Where we notice that we simulate 2 DIS events per muon (this number may also be much larger, like 10k, if more muon statistics is needed). It should be an even number, since **half the interactions will be generated on protons, and half on neutrons.**

The output will be a TClonesArray of vectors:

* Each entry is a particle of the event;
* Each index of the vector is a variable of this particle, following **id,px,py,pz,E** order;

{% hint style="info" %}
The DIS interaction in Pythia has a Threshold, by default 2 GeV^2. The threshold is set with myPythia.SetPARP(2,2)
{% endhint %}

myPythia.SetPARP(2,...) provides the minimum Q^2 (transferred quadrimomentum) to generate the event. If the muons are too soft, the center-of-mass energy with the nucleon will be too small, \*\*making the generation crash\*\*. Either skip these muons, or lower the threshold (0.5 GeV^2 is a possibility)

The output is two vectors:

* InMuon: kinematics about the incoming muon, weight and cross section information;
* Particles: Daughter from muon interaction

### MuDIS vertex location and propagation in Geant4

This is done with MuDISGenerator in FairShip.

It will take the weight from the Pythia6 output file and insert it as the muon weight, then compute Mean Material Budget to provide the weight pho \* dL, inserted as the weight of daughter particles.

### Normalization of muons

The 67 muon background files in /eos/experiment/ship/data/Mbias/background-prod-2018/, named pythia8\_Geant4\_10.0\_withCharmandBeauty\*\_mu.root, contain about 412 million events (with about 506 million muons).  Events with **muons after hadron absorber,** Weights adjusted to **5E13 pot**

These were obtained by merging/shuffling the results of three productions of muon background events. A muon background event was produced by generating a 400 GeV proton-nucleon interaction with Pythia, followed by cascade re-interaction, and passing it to Geant4 to transport through the target and hadron absorber, then saving the generated event parameters if at least one muon with energy above an energy cut "ecut" exited from the hadron absorber.

&#x20;The three productions of muon background events were:&#x20;

* Generation of minimum bias events,&#x20;
* applying a 10 GeV ecut,&#x20;
* and enhancing resonances (rho, omega, ...) by a factor 100.&#x20;

There are 415726905 events, resulting from 65.041 billion PoT (protons on target). If one wants to normalize to an SPS SHiP spill of 5e13 PoT, each muon must be weighed by 768.75 (= 5e13 / 65.041e9). Some must be weighed by 7.6875, because they emerged from a chain started by an enhanced resonance. Generation of muons from charm, by forcing charm production at generator level in the proton-nucleon intraction. The number of events produced corresponds to 153.3 billion PoT. Charmed hadrons were decayed by Pythia. The weight for the same 1-spill normalization is 326.16 = 5E13 / 153.3E9. Similarly, it is sometimes 100 times smaller, 3.2616 (due to enhanced resonances). Generation of muons from beauty, as above, but charm replaced by beauty. 5336 billion PoT equivalent. The weight is 9.37 ( = 5E13 / 5336E9) or 100 times smaller.

Useful references:&#x20;

* /eos/experiment/ship/data/Mbias/background-prod-2018/README&#x20;
* Description of the SHiP FixedTargetGenerator: CERN-SHiP-INT-2017-001 [https://cds.cern.ch/record/2280572/](https://cds.cern.ch/record/2280572/)&#x20;
* Heavy Flavour Cascade Production in a Beam Dump: CERN-SHiP-NOTE-2015-009 [https://cds.cern.ch/record/2115534/](https://cds.cern.ch/record/2115534/)

