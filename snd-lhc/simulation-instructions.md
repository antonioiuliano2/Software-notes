# Simulation Instructions

## Simulations

### Neutrino GENIE simulations

Neutrino fluxes have been provided by Francesco Cerutti \(CERN FLUKA  team\), who simulated the proton interactions at LHC in FLUKA and scored the neutrinos. They have been forwarded to me by Annarita. All the files have been download from her link to my EOS folders.

Information provided by LHC Fluka simulation team:

* x=y=z=0 is IP1. p-p collisions are sampled around IP1 taking into account the longitudinal spread. A half crossing angle of 150 urad in the vertical plane is implemented \(upwards\). This was taken into account, together with the scoring plane longitudinal coordinate z = 48386 cm, to determine the famous R distance using the neutrino absolute coordinates x and y.
* x dir cosine is the component of the neutrino direction \(represented by a vector of length 1\) along the x axis.
* Particles carry the generation number 1 if they are direct products of p-p collisions. The generation number is increased by any further event \(such as a particle decay: the products of the decay of a p-p collision product get the generation number 2\).
* The statistical weight is different from 1 \(and has then to be considered\) in case of former charged pion/kaon decay.
* tau \(anti\)neutrinos were scored at 22.18 m and then projected to z = 483.86 cm
* for normalization purposes, the provided tau \(anti\)neutrino population comes from 385,720,000 p-p collisions, the electron \(anti\)neutrino population comes from 56,000,000 p-p collisions.

The first step to launch the simulation is to set the GENIE environment:

`export GALGCONF=yourpath/config/`

This environment extends the energy range to the LHC one.

I usually have a bash script with this command already configured for my location \(lxplus, local machine\).

If the cross section splines are not present, they need to be produced. This must also done if the target material changes. Currently for the TP we use "W 95%: Ni 3%: Cu 2%". 1000741840\[0.95\],1000280580\[0.03\],1000290630\[0.02\]. The command to produce the spline is :

```bash
python /afs/cern.ch/work/a/aiuliano/public/macros-snd/makeSNDGenieEvents.py --MS -t tungstenTP --nupdg 16 -o ./
```

Then,  we need to actually launch `gevgen` from a script. Annarita has provided me a script example, which I have then modified to adapt it to a common interface with the FairShip one. I launch it with the following command:

```bash
python /afs/cern.ch/work/a/aiuliano/public/macros-snd/makeSNDGenieEvents.py --FS -t tungstenTP --nupdg 16 -p CCDIS -n 100000 -o ./
```

We need to provide in the code as input the cross section file and neutrino flux file. \(flux sections are stored as histograms, with the same notation used in FairShip\).

This produces the GENIE simulation file, which is used as input for Geant4 \(GenieGenerator\) simulation.

### Geant4 simulation

Geant4 simulation is performed from the GenieGenerator, exactly the same used in FairShip. We just need to call the SND@LHC geometry and the right input file:

```bash
python $SNDSW/macro/run_simScript.py --shiplhc --Genie -f inputfile -n 10000 -o outputfolder
```

Then the simualtion and geometry files are produced, same as FairShip simulations produced by GenieGenerator.

### Genie simulations in acceptance

Due to close timelines for the TPs, the GENIE simulations used by Martina for background studies have not been propagated to sndsw. To ensure that only the component of the spectrum within acceptance has been considered, the flux has been selected from Francesco's files to be within detector acceptance, with the following file:

{% embed url="https://github.com/antonioiuliano2/macros-snd/blob/master/nu\_simulations/Acceptance1DSpectra.C" %}

The selection is x within \[-47.6, -8\] cm  and y within \[ 15.5, 55.1\] cm \(neutrino fluxes at the scoring plane, . An output file neutrinos\_SNDacceptance.root has been produced. This file is used for all the GENIE simulations, in the different processes and neutrino flavours, required for the TP of SND@LHC.

