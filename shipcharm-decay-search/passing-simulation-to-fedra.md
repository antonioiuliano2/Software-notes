# Passing simulation to FEDRA

## Summary of all steps

### Performing simulations

Naturally, the first step is to actually launch a MC simulation. I refer to FairShip simulation section for all details concerning FairShip. Currently, the simulations we are performing are:

* **Test configuration for charmed hadron's propagations \(--TrackingCharm\),** charmed hadrons and other primary particles passed from Pythia to Geant4: charmed hadrons have MCTrackIDs 1 and 2 \( and MCMotherID 0\), other primaries have MCMotherID -1. Charm decay daughters should not contain **intermediate states;**
* **Proton background simulation \(G4 only\):** a single 400-GeV proton is passed to Geant4 for propagation and interaction. The produced particles have mother ID 0. Need to select ProcID 23 to study daughters of hadronic inelastic interactions.

Two files are produced, a simfile and a geofile, both .root. \(actual names of files depend on the simulation and on FairShip version, so I do not report them, for example ship\_conical\* and geofile\_full\*\).

### Reproducing Target Mover spills

This step is need to spread the hit on the whole surface, as it was during the 2018 test beam. 5 and 10 spills configurations:

`python $FAIRSHIP/macro/runMufluxDigi.py -cs 1 -f simfile.root -g geofile.root -n ntotevents -ns nspillevents`

For now, we simulate the same number of events for all spills \(in the reality, the number was different also between spills of the same run\).

A file simfile\_rec.root is produced.

### Conversion to FEDRA

The code which currently does it is 

{% embed url="https://github.com/antonioiuliano2/macros-ship/blob/master/FEDRA/fromFairShip2Fedra.C" caption="fromFairShip2Fedra.C" %}

We launch it with $MACROSSHIP/FEDRA/fromFairShip2Fedra\("simfile.root"\). Parameters should be read from a .rootrc file. A directory b000001 containing empty subdirectories p001...p057 should be prepared. At the end of the scripts, these directories will be filled with converted couples trees.

#### Default .rootrc parameters:

* **FairShip2Fedra.nevents** 10000 \#how many events to convert
* **FairShip2Fedra.nbrick** 1 \#it will set the brick numbers in the IDs 
* **FairShip2Fedra.nplates** 29 
* **FairShip2Fedra.ngrains** 70 \#placeholder value, it cannot be 0 because it will not read the couples
* **FairShip2Fedra.minkinenergy** 0.1 \#cut on kinetic energy 
* **FairShip2Fedra.dosmearing** 1 \#applying smearing in angle
* **FairShip2Fedra.useefficiencymap** 0 \#use angular efficiency map instead of fixed value
* **FairShip2Fedra.emuefficiency** 0.85 \#fixed value of efficiency in plates
* **FairShip2Fedra.angres** 0.003 \#angular resolution

### FEDRA reconstruction

The next steps is to perfom tracking and vertexing according to FEDRA usual application \(see FEDRA basic workflow section for details\). \(Mis\)Alignment for now is not simulated, so we just have to launch:

* makescanset....
* emtra -set=1.0.0.0 -new -v=2
* ln -s b000001.0.0.0.trk.root linked\_tracks.root
* root -l charm\_vertexing.C \(copied from $MACROS-SHIP/FEDRA\) 

The vertex file contain only a vtx tree. I am not saving anymore EdbVertexRec \(or EdbPVRec\) objects due to crash when the files are too big. The tree should contain all the information to analyze and draw the reconstructed vertices.

Currently tracking and vertices are performed in four different quarters. Tracking is performed locally on lxplus, and is done consecutively. Tracks files are stored in subfolders reconstruction\_output/firstquarter \(secondquarter, thirdquarter,fourthquarter\), then vertexing is performed on parallel with HTCondor.

## Create MC vertex log

After vertex reconstruction, it is useful to produce a list with the main features of the vertices we are looking for: that is, charm decay vertices. Each MC event produces two charmed hadrons, which then decay in two separated points. Our task is to reconstruct these decays. We can use MC information to study how well the events are reconstructed by FEDRA.

1. Storing TrackIDs of charm hadrons and daughters
2. Identification of primary vertices
3. Identification of secondary vertices
4. Extra-tracks \(not associated to any vertex\)

* [ ] **To do**: merge scripts 2 and 3 since they basically loop from the same file.



### Storing TrackIDs of charm hadrons and daughters

This step does not involve the reconstructed files, but the original MC true simulation. It will save the lists of charm hadrons and charm daughters ID into a Pickle python file. 

It requires only the simulation file and an output file \*.p to save the IDs. It is launched as the following:

```bash
python $MACROSSHIP/analisi_charmsim/writecharmdaughters.py -s $SIMDIR/inECC_ship.conical.Pythia8CharmOnly-TGeant4_dig.root -co $SIMDIR/charmlist.p 
```

### Identification of primary vertices

Given the format of FairShip simulation for SHiP-Charm, particles produced from the primary vertex have MotherId equal to -1 \(the two charms, however, have MotherId 0\). Then, I can recognize the primary vertices in the MC simulation, by requiring that most of the associated tracks have MotherID equal to -1. 

If more than one vertex in the same MCEvent share this feature \(primaries associated to different vertices\), I save only the vertex with more tracks. Therefore the associated tracks are added to the list \(nota bene: the list is defined for tracks, not for vertices\)

It requires the simulation file and the file with vertices reconstructed by FEDRA. It will produce the first version of the CSV list with the vertices. Is is launched as the following:

```bash
python $MACROSSHIP/analisi_charmsim/search_primary_vertices.py -f $VERTEXDIR/vertextree_test.root -o $VERTEXDIR/MCVertexlist.csv
```

### Identification of secondary vertices

Here, the lists saved at step 1. will be used. I do a loop on the tracks associated to the vertices: if the MCTrackID matches the one of a charm's daughter, I save the information about track and vertex in the CSV file. 

A recent modification: tracks connected to the mother charm hadron are now saved at this step, and not at the following one. This means that a track is considered connected \(feature 3\), only if it is associated to a vertex. Otherwise it will belong to feature 4 \(extra tracks\) Alcuni sono collegati alla traccia madre \(cioè l'adrone charmato\), e quindi l'MCTrackID sarà riconosciuto come adrone charmato, essendo il primo segmento.

It is launched as the following: 



```bash
python $MACROSSHIP/analisi_charmsim/search_secondary_vertices.py -f $VERTEXDIR/vertextree_test.root -s $SIMDIR/inECC_ship.conical.Pythia8CharmOnly-TGeant4_dig.root -c $SIMDIR/charmlist.p -o $VERTEXDIR/MCVertexlist.csv
```

### Extra track tagging

We tag as extra tracks charm daughters which have been reconstructed as tracks, but not associated to any vertex.

It is launched as the following:

```bash
python $MACROSSHIP/analisi_charmsim/extratracks.py -t $TRACKSDIR/linked_tracks.root -f $VERTEXDIR/vertextree_test.root -s $SIMDIR/inECC_ship.conical.Pythia8CharmOnly-TGeant4_dig.root -c $SIMDIR/charmlist.p -o $VERTEXDIR/MCVertexlist.csv
```

### Current information provided in the list

The aforementioned procedure gives CSV files with the following columns:

* ntracks;
* ivtx;
* itrk;
* MCEventID;
* MCTrackID;
* CharmMotherPdg;
* CharmMotherPx;
* CharmMotherPy;
* CharmMotherPz;
* MCMotherID;
* predmolt;
* preddecaylength;
* quantity;
* vx;
* vy;
* vz;
* topology

## Current topologies percentages 

The list can be read with a script to count the different MC topologies, and build a more easiliy readable vertex log:

```bash
python $MACROSSHIP/analisi_charmsim/countvertextopologies.py MCVertexlist1.csv MCVertexlist2.csv MCVertexlist3.csv MCVertexlist4.csv
```

The number of CSV files to be passed as input can be as much as it is needed.

### from CH1\_charmcascade\_07\_02\_20:

|  | Primary | Secondary | Connected | Extra | Missing |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Primary | 10.9% | 18.5% | 1.9% | 9.7% | 8.1% |
| Secondary | - | 13.0% | 2.2% | 8.8% | 5.6% |
| Connected | - | - | 0.13% | 0.85% | 0.76% |
| Extra | - | - | - | 2.4% | 4.0% |
| Missing | - | - | - | - | 13.2% |

### from CH2\_charmcascade\_14\_02\_20:

|  | Primary | Secondary | Connected | Extra | Missing |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Primary | 11.3% | 18.5% | 1.8% | 9.9% | 8.1% |
| Secondary |  | 13.3% | 2.1% | 8.7% | 5.6% |
| Connected |  |  | 0.07% | 0.8% | 0.7% |
| Extra |  |  |  | 2.3% | 4% |
| Missing |  |  |  |  | 12.5% |



