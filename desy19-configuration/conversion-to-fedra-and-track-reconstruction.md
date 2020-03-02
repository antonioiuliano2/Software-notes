# Conversion to FEDRA and track reconstruction



## Conversion to FEDRA

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

Then, Track reconstruction is performed with **emtra**, as usual by FEDRA. A text file is also produced, to be used as input for the ShowRec shower reconstruction interface. See next section for details.



