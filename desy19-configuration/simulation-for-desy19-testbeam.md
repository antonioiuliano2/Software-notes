# Simulation for DESY19 testbeam

## Instructions

In 2019, a testbeam with emulsion and SciFi detectors has been performed, to study electromagnetict shower reconstruction. Emulsions have been exposed to electron beams.

8 Different configurations have been used, changing thickness of material and beam energy.

The simulation geometry has been implemented in a new branch of my personal FairShip fork:

{% embed url="https://github.com/antonioiuliano2/FairShip/tree/shipdesy" caption="shipdesy branch" %}



The simulation is launched in the following way:

```bash
python $FAIRSHIP/macro/run_simScript.py --desy19 nrun --PG --pID 11 -n nevents -o outputdir
```

with nrun going from 1 to 8

## Run configurations

Here there is a review of the emulsion geometry and electron energy in the 8 configurations:

| RUN | N. Emulsions | Electron energy \[GeV\] | Additional Notes |
| :--- | :--- | :--- | :--- |
| 1 | 57 | 6 |  |
| 2 | 43 | 6 |  |
| 3 | 29 | 6 |  |
| 4 | 15 | 6 |  |
| 5 | 29 | 2 |  |
| 6 | 29 | 4 |  |
| 7 | 19 | 6 | Passive Material is tungsten, not lead |
| 8 | 15 | 6 | Second ECC with 29 films downstream |

### Multiple simulations

As the analysis marches on, larger statistics samples are needed. However, the recommended way is not to have a single simulation with more events, but many different simulations with the same number of electrons expected on data \(360\). This way, the reconstruction can be performed separately for each simulation session, with the same density of real data.

Of course, the **seed** should be different for each sample. In this case, this is already provided within $FAIRSHIP/macro/run\_simScript.py code, which by default uses ROOT.gRandom.SetSeed\(0\). This, as explained in TRandom::SetSeed reference, ensures a different value for each simulation. Therefore, the 360 electrons are fired at different positions in each simulation sample.

## Checking geometry of a simulation

Every FairShip simulation produces a geometry file \(named \`geofile\*.root\`\), along with the simulation tree file. This allows to check the status of the geometry at the time the simulation has been performed.

### Launching the Event Display

Copy of FairShip Event Display with the following syntax:

```bash
python -i $FAIRSHIP/macro/desy19_eventDisplay.py -f simulationfile.root -g geofile.root
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

## 

