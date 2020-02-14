# Vtx Identification

### Script name: Definitions.h

Description: Header file where simulation and vertex variables and trees are declared.

The name of the simulation files needs to be set here.

### Script name: vtx\_MC\_analysis.C

#### Description:

* Code for the identification of vertices reconstructed by FEDRA using a MC simulation.
* The code provides most probable event and mother ID for each vertex, the number of electrons, primaries and charm daughters. A flag, charm\_daug tells about the presence of a charm daughter in a vertex.
* It requires as input file the vertex file usually called "vertextree\_test.root" and a simulation file.
* It provides in output the file called "vtx\_MC\_analysis.root" that is used as input for the BDTs studies \(see README\_BDT.txt\).

#### List of variables:

1. mp\_eventID --&gt; Most probable montecarlo id of the vertex
2. mp\_motherID --&gt; Most probable montecarlo mother id of the vertex
3. mp\_pdgID --&gt; Most probable pdg id of the vertex
4. charm\_daug --&gt; If 0 no charm daughter tracks in the vertex, if 1 a least one charm daughter in the vertex
5. n\_electrons --&gt; Number of electrons in the vertex \(even if the montecarlo id is different from mp\_eventID\)
6. n\_primaries --&gt; Number of primary daughter tracks \(mother id of the track is -1\)
7. n\_charmdaug --&gt; Number of charm daughter tracks \(mother id of the track is 1 or 2\)
8. n\_events --&gt; Number of montecarlo events linked to the vertex \(ideal case == 1\)
9. n\_mothers --&gt; Number of mother IDs linked to the vertex \(ideal case == 1\)

#### Usage:

* `root -l`
* `.L vtx_MC_analysis.C`
* `myrun()`

