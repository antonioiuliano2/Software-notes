# Simulation Instructions

## Simulations

### Produced neutrino spectra from FLUKA

Neutrino fluxes have been provided by Francesco Cerutti (CERN FLUKA  team), who simulated the proton interactions at LHC in FLUKA and scored the neutrinos. They have been forwarded to me by Annarita. All the files have been download from her link to my EOS folders. They can be accessed at "**/eos/user/a/aiulian/sim\_snd/Generate\_Genieinput/NeutrinoFiles**"

Information provided by LHC Fluka simulation team:

* x=y=z=0 is IP1. p-p collisions are sampled around IP1 taking into account the longitudinal spread. A half crossing angle of 150 urad in the vertical plane is implemented (upwards). This was taken into account, together with the scoring plane longitudinal coordinate z = 48386 cm, to determine the famous R distance using the neutrino absolute coordinates x and y.
* x dir cosine is the component of the neutrino direction (represented by a vector of length 1) along the x axis.
* Particles carry the generation number 1 if they are direct products of p-p collisions. The generation number is increased by any further event (such as a particle decay: the products of the decay of a p-p collision product get the generation number 2).
* The statistical weight is different from 1 (and has then to be considered) in case of former charged pion/kaon decay.
* tau (anti)neutrinos were scored at 22.18 m and then projected to z = 483.86 cm
* for normalization purposes, the provided tau (anti)neutrino population comes from 385,720,000 p-p collisions, the electron (anti)neutrino population comes from 56,000,000 p-p collisions.

### Generation of neutrino interactions in GENIE

The first step to launch the simulation is to set the GENIE environment:

`export GALGCONF=yourpath/config/`

This environment extends the energy range to the LHC one.

{% hint style="info" %}
The update to GENIE3 by Simona leads to a different configuration folder. Therefore, it is important that the configuration folder is set correctly. It should be the same as the defaulg config folder with this GENIE version, with the only modifications in CommonDecay.xml and CommonParam.xml files, which can be accessed at /eos/user/a/aiulian/sim\_snd/Generate\_Genieinput/Genie3\_0\_6config/

(need to access to lxplus with CERN credentials)
{% endhint %}

I usually have a bash script with this command already configured for my location (lxplus, local machine).

If the cross section splines are not present, they need to be produced. This must also done if the target material changes. Currently for the TP we use "W 95%: Ni 3%: Cu 2%". 1000741840\[0.95],1000280580\[0.03],1000290630\[0.02]. I have labelled this configuration "tungstenTP"in the code. The command to produce the splines is :

```bash
python /afs/cern.ch/work/a/aiuliano/public/SNDLHCBuild/sndsw/macro/makeSNDGenieEvents.py spline --nupdg "nupdg" -t "tungstenTP" -o "outputfolder"
```

Neutrino interactions are generated with the following command:

```bash
python /afs/cern.ch/work/a/aiuliano/public/SNDLHCBuild/sndsw/macro/makeSNDGenieEvents.py sim --nupdg "nupdg" -p "process" -n "nevents" -o "outputfolder"
```

where:

* nupdg is the neutrino pdgcode;
* process is the neutrino interaction process we want to generate (CCDIS, CharmCCDIS, NCDIS...)
* nevents is the number of neutrino interactions to generate
* outputfolder is the path of the output folder where the produced files will be stored

We need to provide in the code as input the cross section file and neutrino flux file. (flux sections are stored as histograms, with the same notation used in FairShip).

This produces the GENIE simulation file, which is used as input for Geant4 (GenieGenerator) simulation.

### New: extraction of interacting neutrinos from FLUKA simulations

In SHiP and during preparation of SND@LHC first proposals, we propagated the neutrinos from the collision point, by looping over GENIE neutrino interactions and generating a random phi angle (theta was generated according to the p-pt distribution). However, due to the high distance covered by particles before arriving to SND@LHC detector from the collision points (480 m), the production point of neutrinos cannot be neglected (we cannot assume them to come from the origin).

Therefore, we now use the positions directly from the FLUKA simulations. First, among all the neutrinos produced and stored in the FLUKA simulation file, we need to select the subsample of interacting neutrinos. The neutrinos most probable to interact are selected via **hit or miss Monte Carlo**, according to the ratio between neutrino energy distribution in GENIE (i.e. interacting neutrinos) over the distribution from FLUKA (i.e. produced neutrinos).&#x20;

This step is launched with the following command, by providing as input the GENIE and FLUKA .root simulation files. It will update the GENIE file, adding to it the TTree of interacting neutrinos:

```bash
python $SNDSW/shiplhc/extract_interacting_neutrinos.py --geniepath geniesimfile --flukapath flukasimfile
```

### Geant4 simulation

Geant4 simulation is performed to propagate the particles produced from the neutrino interaction in the SND@LHC detector geometry.

The generation is called from the GenieGenerator, exactly the same used in FairShip (See FairShip simulation section for more details). We just need to call the simulation script and provide the right input file:

```bash
python $SNDSW/shiplhc/run_simSND.py --Genie -f inputfile -n 10000 -o outputfolder
```

* input file is the file from the Genie simulation
* \-n sets the number of events to generate
* \-o sets the output folder where to store simulation results

Then the simualtion and geometry files are produced, same as FairShip simulations produced by GenieGenerator.

## Hit in the emulsion

EmulsionDetPoints are the emulsion films. They are made active by setting

c.EmulsionDet.PassiveOption = 0

in **shipLHC\_geom\_config.py** file (in geometry folder).

{% hint style="info" %}
It appears that simulation the emulsion film as a mixture (two emulsion layers and a plastic base) makes the energy loss too small (10^-8 GeV) and many hits are lost near the interaction vertex due to having exactly energy loss 0.

Simulating the two emulsion layers separately (even if only one is made active) with the plastic base increases the average energy loss to (10^-5 GeV), thus solving the issue at the vertex.
{% endhint %}

## Extra study: Genie simulations in acceptance

Due to close timelines for the TPs, the GENIE simulations used by Martina for background studies have not been propagated to sndsw. To ensure that only the component of the spectrum within acceptance has been considered, the flux has been selected from Francesco's files to be within detector acceptance, with the following file:

{% embed url="https://github.com/antonioiuliano2/macros-snd/blob/master/nu_simulations/Acceptance1DSpectra.C" %}

The selection is x within \[-47.6, -8] cm  and y within \[ 15.5, 55.1] cm (neutrino fluxes at the scoring plane, . An output file neutrinos\_SNDacceptance.root has been produced. This file is used for all the GENIE simulations, in the different processes and neutrino flavours, required for the TP of SND@LHC.
