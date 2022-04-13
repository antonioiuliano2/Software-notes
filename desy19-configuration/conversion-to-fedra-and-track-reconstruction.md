# Conversion to FEDRA and track reconstruction



## Conversion to FEDRA

The code which currently does it is&#x20;

{% embed url="https://github.com/antonioiuliano2/macros-ship/blob/master/FEDRA/fromFairShip2Fedra.C" %}
fromFairShip2Fedra.C
{% endembed %}

We launch it with $MACROSDESY/FEDRA/fromFairShip2Fedra.C("simfile.root"). Parameters should be read from a .rootrc file. A directory b00000i (i=1,2...8) containing empty subdirectories p001...p057 should be prepared. At the end of the scripts, these directories will be filled with converted couples trees.

#### Default .rootrc parameters:

* **FairShip2Fedra.nevents** 10000 #how many events to convert
* **FairShip2Fedra.nbrick** 1 #it will set the brick numbers in the IDs&#x20;
* **FairShip2Fedra.nplates** 29&#x20;
* **FairShip2Fedra.ngrains** 70 #placeholder value, it cannot be 0 because it will not read the couples
* **FairShip2Fedra.minmomentum** 0.03 #cut on momentum&#x20;
* **FairShip2Fedra.dosmearing** 1 #applying smearing in angle
* **FairShip2Fedra.maxtheta 1** #maxangle
* **FairShip2Fedra.useefficiencymap** 0 #use angular efficiency map instead of fixed value
* **FairShip2Fedra.emuefficiency** 0.85 #fixed value of efficiency in plates
* **FairShip2Fedra.angres** 0.003 #angular resolution

Copy scanset.sh and track.rootrc files from $MACROSDESY/FEDRA/track.rootrc and $MACROSDESY/FEDRA/scanset.sh in the b00000i folder. Launch scanset.sh (check dz and number of plates!)

Then, Track reconstruction is performed with **emtra**, as usual by FEDRA. A text file is also produced, to be used as input for the shower reconstruction interface. See next section for details.

I have recently (9 July 2020) changed the cut from minimum kinetic energy to minimum momentum, to test a different cut with respect to the one used in SHiP-charm. Being here for the most part electrons, the difference should be minimal. However, I have always been told to set a momentum cut to be more similar with data, not a kinetic cut (just using the kinetic energy to be coherent with the FairShip cut). Therefore, it stands to reason to use a momentum cut here.

