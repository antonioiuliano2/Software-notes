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

[https://github.com/antonioiuliano2/macros-ship/blob/master/ecn3\_ship/loop\_eduardmuonfile.C](https://github.com/antonioiuliano2/macros-ship/blob/master/ecn3\_ship/loop\_eduardmuonfile.C)

After analysis, resulted TTrees and plots are stored in my EOS:

/eos/user/a/aiuliano/public/sims\_FairShip/EduardSimulationsSHiPECN3/

Using the function create\_tree(), the information from the 3 scoring plane is stored into an NTuple.

Before launching this code, it is necessary to activate my kerberos credentials: kinit aiuliano@CERN.CH

Then, using read\_tree(0), I read the data from scoringplane 0 with RDataFrame and store into output file (use 1,2 for reading different tree).

plotmuondensity(0) plots the muon density with large bins, and compute muon density for cm2 in an area&#x20;

## MuonDIS generation

Starting from a datasets of muons crossing a scoring plane, the next step is to generate their Deep Inelastic Scattering (DIS) interactions, in order to have the products of this interactions.

Currently, this step is doing with Pythia6, even if continuous work is being done to move forward to Pythia8. The script I am referring to is from [https://github.com/olantwin/muonDIS](https://github.com/olantwin/muonDIS), where I have then adapted for the input **pickle** format by Luis, into this code,[ makeMuonDISmod.py](https://github.com/antonioiuliano2/macros-ship/blob/master/muon\_background/makeMuonDISmod.py).

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
