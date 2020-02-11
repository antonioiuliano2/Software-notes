# Info Valerio Vtx Identification

CREATED BY V.GENTILE 2020/10/02

Script name: vtx\_MC\_analysis.C Description: Code for the identification of vertices reconstructed by FEDRA using a MC simulation. The code provides most probable event and mother ID for each vertex, the number of electrons, primaries and charm daughters. A flag, charm\_daug tells about the presence of a charm daughter in a vertex.

List of variables: 1\) mp\_eventID --&gt; Most probable montecarlo id of the vertex 2\) mp\_motherID --&gt; Most probable montecarlo mother id of the vertex 2\) mp\_pdgID --&gt; Most probable pdg id of the vertex 3\) charm\_daug --&gt; If 0 no charm daughter tracks in the vertex, if 1 a least one charm daughter in the vertex 4\) n\_electrons --&gt; Number of electrons in the vertex \(even if the montecarlo id is different from mp\_eventID\) 5\) n\_primaries --&gt; Number of primary daughter tracks \(mother id of the track is -1\) 6\) n\_charmdaug --&gt; Number of charm daughter tracks \(mother id of the track is 1 or 2\) 7\) n\_events --&gt; Number of montecarlo events linked to the vertex \(ideal case == 1\) 8\) n\_mothers --&gt; Number of mother IDs linked to the vertex \(ideal case == 1\)

Updated to gitbookCREATED BY V.GENTILE 2020/10/02

Script name: vtx\_MC\_analysis.C Description: Code for the identification of vertices reconstructed by FEDRA using a MC simulation. The code provides most probable event and mother ID for each vertex, the number of electrons, primaries and charm daughters. A flag, charm\_daug tells about the presence of a charm daughter in a vertex.

List of variables: 1\) mp\_eventID --&gt; Most probable montecarlo id of the vertex 2\) mp\_motherID --&gt; Most probable montecarlo mother id of the vertex 2\) mp\_pdgID --&gt; Most probable pdg id of the vertex 3\) charm\_daug --&gt; If 0 no charm daughter tracks in the vertex, if 1 a least one charm daughter in the vertex 4\) n\_electrons --&gt; Number of electrons in the vertex \(even if the montecarlo id is different from mp\_eventID\) 5\) n\_primaries --&gt; Number of primary daughter tracks \(mother id of the track is -1\) 6\) n\_charmdaug --&gt; Number of charm daughter tracks \(mother id of the track is 1 or 2\) 7\) n\_events --&gt; Number of montecarlo events linked to the vertex \(ideal case == 1\) 8\) n\_mothers --&gt; Number of mother IDs linked to the vertex \(ideal case == 1\)

Updated to gitbook

