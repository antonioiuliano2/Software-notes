---
description: Finally starting to do some physics here
---

# FairShip simulation

## Simulation for Charm cross section and Muon flux measurements

Charmdet folder contains the geometry classes for the detectors used in charm cross section and Muon flux measurements.

### Simulations for charm cross section measurement

In general:

Option `--CharmdetSetup 1` activates charm cross section geometry, while `--CharmdetSetup 0` activates muon flux geometry.

Only the most useful options have been explained here. For the complete list of available options please refer to the related scripts.

No particular cuts are usually done with the respect to the Default FairShip phyisics options. However, by default for hit with kinetic energy lower than `100 MeV` the software will NOT save the MCTrack, to avoid memory consumption. If tracks with lower energy need to be studied, this threshold can be lowered in the simulation script. To do that, put `MCTracksWithEnergyCutOnly = False` in `macro/run_simScript.py` and launch the simulation with `-F` option \(deepcopy\).

FairShip used cuts are shown in gconfig/SetCuts.C. Energy thresholds for interactions are usually 1 MeV

Charm production simulations are done from `macro/run_simScript.py`. Example syntax:  
`python $FAIRSHIP/macro/run_simScript.py --charm 1 -A charmonly --CharmdetSetup 1 -f Cascadefile -n 1000 -o outputfolder`

Useful options:

* `--charm 1`: activates `charmdet` configuration instead of SHiP standard \(both

  for charm cross section and muon flux measurements\)

* `-A charmonly`: activates

  charm production simulation

* `-f`: input file with charm vertices \(if you have Kerberos configured \(e.g.

  by default on `lxplus`\), this will be taken directly

  from `/eos/ship/data/Charm/Cascade-parp16-MSTP82-1-MSEL4-978Bpot.root` by

  default\)

* `-n`: number of events
* `-o`: output of folder where geometry and output of simulation will be saved  

General POT simulations are done from `muonShieldOptimization/run_MufluxfixedTarget.py`. Example of syntax:

`python $FAIRSHIP/muonShieldOptimization/run_MufluxfixedTarget.py --CharmdetSetup 1 -G -e 0.001 -n 1000 -o outputfolder`

It is a derivation of the fixed target simulation used in SHiP, applied to `charmdet` geometry.

Useful options:

* `-e`: Energy cut for adding tracks to `Geant4` propagation \(choosing a high cut

  allows to save memory for larger simulations\)

* `-n`: Number of events
* `-o`: output of folder where geometry and output of simulation will be saved
* `-r`: number of run \(can be used as folder naming if `-o` option is not used\)
* `-f` : force overwriting of directory \(DANGEROUS: if used in a wrong

    directory, it will delete it. DO NOT USE IT together with `-o` option\)  

Different options for proton generation:

* `-V`: default one, proton interactions generated with `Pythia` and `EvtGen` is

  used for decays

* `-P`: both proton interactions and decays handled with `Pythia`
* `-G`: most basic simulation: one 400 GeV proton directly sent to `Geant4`

Details can be found here: 

{% embed url="https://cds.cern.ch/record/2280572." %}



All simulations use `Geant4` for propagation. IMPORTANT: Both `-V` and `-P` generate all interactions in target. Due to small dimensions of target used in `charmdet` measurement, many protons pass through without interacting. To correctly simulate surviving protons and their tracks in detectors, use `-G` option.

For any question or doubt about these simulations, contact Thomas Ruf \([mailto:thomas.ruf@cern.ch](mailto:mailto:thomas.ruf@cern.ch)\) or Antonio Iuliano \([mailto:antonio.iuliano@cern.ch](mailto:mailto:antonio.iuliano@cern.ch)\)



### Index of MCTrackID definitions

First, **charm** simulation: each event produced two charmed hadrons, charm and anti-charm. Their IDs depends on what was passed to Geant4 within the simulation:

* **Default configuration**, charmed hadrons, decay daughters and other primaries passed from Pythia to Geant4: the first charmed hadron has MCTrackID 1 and MCMotherID 0, the other primary particles have MCMotherID -1, the ID of the second charmed hadron is not fixed \(it has be to looked from its pdgcode\). Charm decay daughters can contain **intermediate states;**
* **Test configuration for charmed hadron's propagations \(--TrackingCharm\),** charmed hadrons and other primary particles passed from Pythia to Geant4: charmed hadrons have MCTrackIDs 1 and 2 \( and MCMotherID 0\), other primaries have MCMotherID -1. Charm decay daughters should not contain **intermediate states;**
* **Proton background simulation \(G4 only\):** a single 400-GeV proton is passed to Geant4 for propagation and interaction. The produced particles have mother ID 0. Need to select ProcID 23 to study daughters of hadronic inelastic interactions.

#### Making charm \(and beauty\) hadron cascades

The `macro/MakeCascade.py`script is a stand-alone simulation \(i.e. it not uses TGeo either TVirtualMC\) with a tuned version of Pythia6. It will simulate the production of charm or beauty hadrons, also simulating cascade production from hadrons. It has the following options:

* `-n` for number of events to simulate;
*  `-m` to choose between charm and beauty production \(default charm\);
* `-E` for beam energy in GeV \(default 400\);
* `-t` name of output file;
* `-s` for seed \(by default it uses current time\);
* `-P` to store all particles produced together with charm \(mostly needed in our emulsion reconstruction studies\);

Also, it is not an option, but it uses the fraction of proton in nucleus to simulate interactions on proton or neutron in Pythia: by default it uses molybednum \(0.43\), but I often change it to lead \(0.40\).

Moreover, in very small targets the cascade leads to unphysical peaks at the end of the target \(in the following FairShip simulation\). To avoid that, I select only primary charm `k==1` from the produced tree, given `k` an integer number representing cascade depth

The script `macro/makeDecay.py` makes the charm hadron decays, to save muon and neutrino spectra. I have added a modified version to compare the spectra with `k==1` and `k>1`. Attention: Ds are not correctly handled by Pythia6, which produces more hadrons than expected, makeDecay will report the difference at the end, usually a factor 7.7/10.6.

### Simulations for muon flux measurements

* For full simulation, proton \(400 GeV\) on SHiP muflux target, plus detector setup:

  `python  run_MufluxfixedTarget.py -e ecut -P --CharmdetSetup 0`

  more options available \(boosting di-muon BR, di-muon cross sections,

  charmonium, charm, ...\), see `--help`

* For fast simulation, with muons from external file:

  \`python  run\_simScript.py --MuonBack -n 1000 --charm=1 --CharmdetSetup=0 -f

  inputFile`, where`inputFile\`:

  `/eos/experiment/ship/data/Mbias/background-prod-2018/pythia8_Geant4_10.0_withCharmandBeauty0_mu.root`

  \(+ 66 more files\)

* For digitization:

  \`python runMufluxDigi.py -f ship.conical.FixedTarget-TGeant4.root -g

  geofile\_full.conical.FixedTarget-TGeant4.root\`→

  `ship.conical.FixedTarget-TGeant4_dig.root` Track reconstruction & analysis use

  `drifttubeMonitoring.py`, will automatically detect if it is MC data.

## Simulation for neutrino interactions

{% hint style="info" %}
`run_simScript.py` has a cut on kinetic energy, by default set at 100 MeV, to avoid to produce too heavy files \(space is precious\). Particles with lower energy are produced, but only the hits are saved \(we lose `MCTrack` information\). This can be changed at line 33, by setting MCTracksWithHitsOnly = False and                                                             MCTracksWithHitsOrEnergyCut = True
{% endhint %}

Neutrino interactions are simulated with GENIE MonteCarlo software, then the produced particles are sent to Geant4 for propagation.  
Starting from the 2018 spectra produced by Thomas, they can be found in:

* /eos/experiment/ship/data/Mbias/background-prod-2018/pythia8\_Geant4\_charm\_nu\_1.0.root
* /eos/experiment/ship/data/Mbias/background-prod-2018/pythia8\_Geant4\_charm\_nu\_10.0.root

Thomas suggested to use the 1 GeV file untile 10 GeV, then the 10 GeV files. So I created a neutino\_merged.root file with these features.

In this file you can find 1D histogram of neutrino momentum, along with 2D histograms of pt vs p. The 1D histograms are the input for GENIE, while the 2D ones will be used by FairShip generator to generate in the transverse plane.

### Genie simulation

Most of this material here is credit to Annarita.

First \(and important!\), we need to set Genie User Decay Settings, to tell Genie not to make tau leptons and charmed hadrons decay, because we want to let Geant4 decay them.

This is set in UserPhysicsOption.xml, setting the option DecayParticlewithCode = pdgcode, as false, with pdg codes for charmed hadrons:

* ± 421    
* ± 411  
* ± 431
* 4122  
* 4112  
* 4212  
* 4222  

and for tau leptons:

* ± 16  

Export GXMLPATH variable with the path of the modified UserPhysicsOptions.xml will tell Genie your request.

If you do not have splines, you have to create them by launching

`gmkspl -p #nu -t 1000822040[0.014], 1000822060[0.241], 1000822070[0.221], 1000822080[0.524] -n 500 -e 400 -o out_file_name.xml`

I am currently using Annarita splines. Actually Genie manual does not recommend that, but to use official splines if no particular physical requests are asked. I did not see any difference last time I checked with the official splines. I will probably use official splines for future generations \(not current ones\)

Launch Genie simulations with:

`gevgen -n Nevents -p neutrinocode -t targetcode -e 0.5,350 --run nrun -f neutrinofile,histoname --cross-sections splines --message-thresholds $GENIE/config/Messenger_laconic.xml --seed nseed`

Convert the output in gst format:

`gntpc -i gntp.0.ghep.root -f gst --message-thresholds $GENIE/config/Messenger_laconic.xml"`

All these operations are written in macro/makeGenieEvents.py Usage: &gt; python -i &gt; makeEvent\(100\) &gt; makeNtuples\(\).

Input neutrino fluxes have been produced by Thomas, in the main proton on target simulation \(the same which produces the muon spectra\). The most recent ones are from 2018 and are in the following folder:

/eos/experiment/ship/data/Mbias/background-prod-2018/pythia8\_Geant4\_10.0\_withCharm\_nu.root \(10 GeV cut\)

/eos/experiment/ship/data/Mbias/background-prod-2018/pythia8\_Geant4\_1.0\_withCharm\_nu.root \(1 GeV cut\)

Both files are weighted to correspond to 1 proton spill \( $$4 \times 10^{13}$$ protons on target\).

### FairShip GenieGen simulation

FairShip run\_simScript.py simulation with `-Genie` option launchs the `GenieGen` class in shipgen. The syntax is the following:

```bash
python $FAIRSHIP/macro/run_simScript.py --Genie -f nufluxfile.root -n nevents -o outputfolder
```



It will use the information about neutrino interaction from the Genie simulation to do the following steps:

1. From p-pt association \(2D spectra\) and random generation of phi angles, obtain kinematic information of event.
2. Place the interactions in the target \(z randomly generated, x and y propagated from target according to angles\)
3. Compute a weight of the event, according to the density of material.
4. Pass the weighted event to Geant4 for usual propagation and save of MC information

The weight is computed as $$\sum_i(x_i*\rho_i)$$ , from density of material and length transversed in material. In fact, it is related to the rate of interactions, according to the rate formula from the cross section:

$$
R=L\sigma = I \frac{\rho l N_A}{A} \sigma = I \frac{wN_A}{A}\sigma
$$

Indeed, we expect more interactions in more dense material \(i.e. many in the lead, few in emulsion and very few in air gaps\), but if you do not use the weight you will see the positions uniformly distributed there! Therefore, when plotting the hits always use weighted histograms. Weight is the same for all particles of the same events, so you can use MCTrack\[0\].GetWeight\(\) to find it

{% hint style="info" %}
By default, FairShip will generate neutrino interactions in a vast z region, because many detectors study neutrino interactions as a background source. Usually, we want to study only the interactions happening in the detector. We then set GenieGen positions as:

`Geniegen.SetPositions(ship_geo.target.z0, ship_geo.NuTauTarget.zC-ship_geo.NuTauTarget.zdim/2, ship_geo.NuTauTarget.zC+ship_geo.NuTauTarget.zdim/2)`

But this mostly depends on what we want to study and it should be kept under control
{% endhint %}

## Simulation for muon background

Muon background is simulated with `--MuonBack` option from `macro/run_simScript.py`. It takes as input the muon fluxes produced by Thomas and stored in 67 large ROOT files \($N$ a number from 0 to 66\):

/eos/experiment/ship/data/Mbias/background-prod-2018/pythia8\_Geant4\_10.0\_withCharmandBeauty$N$000\_mu.root

They are weighted, according to the production computing facility. The weights are transferred to the `run_simScript` simulation and are to be included in the histogram maps. Included weights, the muons correspond to the protons from one spill \( $$4 \times 10^{13}$$ protons on target\). 

An excellent example of script to produce muon flux maps is the `macro/flux_map.py` script.



## Checking geometry of a simulation

Every FairShip simulation produces a geometry file \(named \`geofile\*.root\`\), along with the simulation tree file. This allows to check the status of the geometry at the time the simulation has been performed.

### Launching the Event Display

FairShip itself uses an event display with the following syntax:

```bash
python -i $FAIRSHIP/macro/eventDisplay.py -f simulationfile.root -g geofile.root
```

Which opens a display of the whole detector, and it additionally offers a display of the events from the simulationfile. Being very slow, it is often useful to check the vislvl option in the script, at the following position \(line 1061\):

```python
fMan.Init(1,4,10) # default Init(visopt=1, vislvl=3, maxvisnds=10000), ecal display requires vislvl=4
```

Increasing the vislvl allows to more subvolumes, but makes the EventDisplay even slower. To display events in a reasonable way, I usually launch a small partner simulation, with only the neutrino detector volumes active. This however must be taken with caution, because it is not guaranteed that partner simulation uses same geometry of original simulation.

### Inspecting volume sizes and positions

Another important check is done by the `getGeoInformation.py` script:

```bash
python $FAIRSHIP/macro/getGeoInformation.py -g geofile.root -v myvolume -l nlevels
```

Positions in FairShip reference system and half-dimensions of all mother volumes are reported. myvolume is also expanded to show the daughters until nlevels. Moreover,  the medium of the volume is reported

## Before pushing new changes

### Checking overlaps and extrusions

Changes in positions and/or dimensions of geometrical volumes may cause overlapping between two volumes or extrusions of a daughter node with respect to its mother node \(i.e., it exits from the region covered by the mother volume\). 

Luckily, there is an automated tool to provide this check, and it should be launched as much frequently as possible \(and ALWAYS before opening a pull request to ShipSoft master repository\). At the start of `macro/run_simScript.py,` simply set debug flag to 2 instead of 0. It will report the magnetic fields at the start of the program, while at the end it will perform the overlap/extrusion check.

### Backward-compatibility check

FairShip is not only used for launching simulations, but also for analyzing the results of them \(providing reconstruction, even if unluckily is not implemented in our subdetectors\). Therefore, after changing methods and attributes of some class, we must confirm that the program can still access old data without errors. To do that, follow this procedure, proposed by Thomas:

1. Launch a simulation, then reconstruction and analysis with the master FairShip version \(official repository\):

```text
python $FAIRSHIP/macro/run_simScript.py
python $FAIRSHIP/macro/ShipReco.py -f ship.conical.Pythia8-TGeant4.root -g geofile_full.conical.Pythia8-TGeant4.root
python -i $FAIRSHIP/macro/ShipAna.py -f ship.conical.Pythia8-TGeant4_rec.root -g geofile_full.conical.Pythia8-TGeant4.root
```

2. Read the produced data with the new compiled FairShip version, by launching analysis and reconstruction:

```bash
python -i $FAIRSHIP/macro/ShipAna.py -f ship.conical.Pythia8-TGeant4_rec.root -g geofile_full.conical.Pythia8-TGeant4.root
python $FAIRSHIP/macro/ShipReco.py -f ship.conical.Pythia8-TGeant4.root -g geofile_full.conical.Pythia8-TGeant4.root
```







