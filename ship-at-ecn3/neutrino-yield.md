---
description: What do we expect to look for
---

# Neutrino yield

Same script nu\_yield.C as in the SHiP original study.

First study, mass 1 ton, size 40 x 40 square cm

Then, moved to mass 4.5 ton, size 40 x 80 square cm

## Options under study

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
* Normalize neutrino rates, initially 5e+13, to 2e+20 pot per target.&#x20;
* Backup the output to EOS if needed.

To do: check also the 10 GeV file, set links automatically in script



### With respect to detector size

With the same mass, what changes with different transverse sizes?

Creating a folder for each configuration

### With respect to detector angular position

Changing TX offset, telling us center position

Again, a folder for each configuration

Created a separate nu\_yield\_angles.C to check different angles

