# Decay search

The decay search "DS" is performed on vertex and track files processed by FEDRA. Usually these files are called "vertextree\_test.root" and "linked\_tracks.root".

### Definitions.h&#x20;

Description: Header file where track and vertex variables and trees are declared.

## DsCuts.h&#x20;

Description: Datacard where cuts for the DS are defined (some cuts depends on dataset and should be set here)

## vtx\_data\_study.C&#x20;

Description: Code for the DS which requires in input the following files:&#x20;

1\) Definitions.h&#x20;

2\) DsCuts.h&#x20;

3\) vertextree\_test.root&#x20;

4\) linked\_tracks.root

&#x20;5\) vtx\_BDT\_data\_evaluated.root (see README\_BDT.txt)

&#x20;6\) vtx\_BDT\_data\_evaluated2nd.root (see README\_BDT.txt)

It provides in output the following files:

1\) log\_info\_ds.txt (preliminary info before the DS)

2\) log\_ds\_search.txt (report of the decay search)

3\) ds\_data\_result.root (output tree with the DS results)

Script name: selection\_decaysearch\_sim.C (see next section)

&#x20;Description: The script provides additional selections to the DS tree. It requires as input file "ds\_data\_result.root" and provides in output the file "annotated\_data\_result.root"

## Usage:

DS run

`root -l .L vtx_data_study.C myrun() (type 2 for the decay search)`

new selections

`root -l selection_decaysearch_sim.C`



## Silver and Golden events

During our first study of CHARM1-RUN6 candidates, we called "silver events" the events with at least one secondary vertex (dsvtx.vtx2\_vid.size>=1), while "golden events" require at least two vertices

