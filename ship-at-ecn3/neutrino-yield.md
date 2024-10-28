---
description: What do we expect to look for
---

# Neutrino yield

Same script nu\_yield.C as in the SHiP original study.

First study, mass 1 ton, size 40 x 40 square cm

Then, moved to mass 4.5 ton, size 40 x 80 square cm

The distance is considered with respect to the start of the main SHiP target, **ship\_geo.target.z0**

with respect to the middle of SND target along z

## Options under study

### Check target configuration

The main parameters of this study are:

* Transverse size (dx, dy);
* Material density;
* Total mass (which gives you volume from density and then, **assuming a box shape**, gives you dz);
* Distance from proton target (see above)

Transverse size and distance are required for generate\_neutrinos(), which propagates the fluxes to neutrino detectors and cuts away the ones outside acceptances;

Transverse size, mass and atomic number (material), are needed for nu\_yield(), which computes the number of expected interactions for the different neutrino channels

### Checking different neutrino sources

What are the sources?

3 different files:

* no charm production, option 0;
* only charm production, option 1;
* with charm production, option 2 (ideally, should be the sum of the above);

If no option is provided, the total one (option 2) is used.

The code workflow is:

* Create a link to /eos/experiment/ship/data/Mbias/background-prod-2018/pythia8\_Geant4\_1.0\_withCharm\_nu.root and to /eos/user/a/aiuliano/public/sims\_FairShip/GenieEvents\_SHIP/SplinesTungstenSHIP/nu\_xsec\_TungstenSHIP.root
* Create an empty folder, called plots\_2;
* Launch generate\_neutrinos() or generate\_neutrinos(2);
* Launch nu\_yield() or nu\_yield(2);
* Normalize neutrino rates, initially 5e+13, to 2e+20 pot per target. From ECN3 onwards normalized per year, **4e+19 protons on target**. SpreadSheet can help with that, but **always check what you paste**
* Backup the output to EOS if needed.

Note: recently (2024) we added also the splines for Iron, due to the studies for implementation of AdvSND target in SHiP@ECN3

To do: check also the 10 GeV file, set links automatically in script



### With respect to detector size

With the same mass, what changes with different transverse sizes?

Creating a folder for each configuration

### With respect to detector angular position

Changing TX offset, telling us center position

Again, a folder for each configuration

Created a separate nu\_yield\_angles.C to check different angles

## Neutrino Simulation

Since GENIE changed, I would need to redo the splines.

For now, still using old splines, and regenerating neutrino interactions with old GENIE.

After SHiP approval, modernization should be performed (See Oliver discussion)
