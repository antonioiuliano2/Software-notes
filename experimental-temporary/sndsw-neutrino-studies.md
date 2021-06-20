# SNDSW neutrino studies



### New: extraction of interacting neutrinos from FLUKA simulations

In SHiP and during preparation of SND@LHC first proposals, we propagated the neutrinos from the collision point, by looping over GENIE neutrino interactions and generating a random phi angle \(theta was generated according to the p-pt distribution\). However, due to the high distance covered by particles before arriving to SND@LHC detector from the collision points \(480 m\), the production point of neutrinos cannot be neglected \(we cannot assume them to come from the origin\).

Therefore, we now use the positions directly from the FLUKA simulations. First, among all the neutrinos produced and stored in the FLUKA simulation file, we need to select the subsample of interacting neutrinos. The neutrinos most probable to interact are selected via **hit or miss Monte Carlo**, according to the ratio between neutrino energy distribution in GENIE \(i.e. interacting neutrinos\) over the distribution from FLUKA \(i.e. produced neutrinos\). 

This step is launched with the following command, by providing as input the GENIE and FLUKA .root simulation files. It will update the GENIE file, adding to it the TTree of interacting neutrinos:

```bash
python $SNDSW/macro/extract_interacting_neutrinos.py --geniepath geniesimfile --flukapath flukasimfile
```

