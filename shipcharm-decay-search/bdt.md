# BDT

## TMVAClassification\_MC.C 

\(only for training and test of simulation data\) 

Description: This code provides several functions to create input files for the training and test of the BDTs; and a main function to perform the BDTs. List of functions:

 1\) prepareTMVAtree\(\) --&gt; It requires a vertex\_file with MC information \(mp\_motherID, mp\_eventID, charm\_daug, etc...\) The file in input is usually called "vtx\_MC\_analysis.root" It provides in output the file "tmva\_input\_vertices.root" that is used as input for the BDT on interaction vertices. 

2\) prepareTMVAtree2nd\(\) --&gt; It requires a vertex\_file with MC information \(mp\_motherID, mp\_eventID, charm\_daug, etc...\) and a file with the BDT values evaluated on interaction vertices. The file in input are usually called "vtx\_MC\_analysis.root" and "vtx\_BDT\_data\_evaluated.root" It provides in output the file "tmva\_input\_vertices2nd.root" that is used as input for the BDT on decay vertices. 

3\) prepareTMVAtreeDS\(\) --&gt; It requires a file where the decay search is already performed. The file in input is usually called "annotated\_data\_result.root". It provides in output the file "tmva\_input\_verticesDS.root" that is used as input for the decay search BDT. 

4\) TMVAClassification\_MC\(\) --&gt; The script allows to perform the BDT on interaction vertices, decay vertices and the BDT for the decay search. Different cases are possible: 

* Case 1: BDT on interaction vertices --&gt; Input files like "tmva\_input\_vertices.root". 
* Case 2: BDT on decay vertices --&gt; 2 Input files like "tmva\_input\_vertices2nd.root" for the signal \(charm vertices\) and the background \(the remaining part\). 
* Case 3: BDT for the decay search --&gt; Input files like "tmva\_input\_verticesDS.root". It provides in output the directories with the datasets with the weigths of the tested network and the "TMVA.root" to check the BDT results and distributions. 

## TMVAClassification.C 

\(only for data and simulation evaluation\) 

Description: This code provides several functions to create input files for the evaluation of the BDTs; List of functions: 

1\) prepareTMVAtree\(\) --&gt; It requires a vertex file usually called "vertextree\_test.root" It provides in output the file "tmva\_input\_vertices.root" that is used as input for the BDT evaluation of interaction vertices usually called "vtx\_BDT\_data\_evaluated.root".

 2\) prepareTMVAtree2nd\(\) --&gt; It requires a vertex\_file and a file with the BDT values evaluated on interaction vertices The file in input are usually called "vertextree\_test.root" and "vtx\_BDT\_data\_evaluated.root" It provides in output the file "tmva\_input\_vertices2nd.root" that is used as input for the BDT evaluation on decay vertices. 

3\) prepareTMVAtreeDS\(\) --&gt; It requires a file where the decay search is already performed. The file in input is usually called "annotated\_data\_result.root". It provides in output the file "tmva\_input\_verticesDS.root" that is used as input for the decay search BDT evaluation. 

## TMVAClassificationApplication.C

 Description: This code performs the evaluation of the BDT for the interaction vertices. 

It requires in input the file "tmva\_input\_vertices.root" and provides in output the file "vtx\_BDT\_data\_evaluated.root" 

## TMVAClassificationApplication2nd.C 

Description: This code performs the evaluation of the BDT for the decay vertices. It requires in input the file "tmva\_input\_vertices2nd.root" and provides in output the file "vtx\_BDT\_data\_evaluated2nd.root" 

## TMVAClassificationApplicationDS.C 

Description: This code performs the evaluation of the BDT for the decay search. It requires in input the file "tmva\_input\_verticesDS.root" and provides in output the file "vtx\_BDT\_data\_evaluatedDS.root"

 Usage: 

\# input files 

`root -l .L TMVAClassification_MC.C or (.L TMVAClassification.C)` 

`prepareTMVAtree()` 

`prepareTMVAtree2nd()` 

`prepareTMVAtreeDS()` 

\# BDTs training and test

 `root -l ./TMVAClassification_MC.C\(\"BDT,Likelihood\"\)` 

\# BDT evaluation

 `root -l TMVAClassificationApplication.C` 

`root -l TMVAClassificationApplication2nd.C` 

\# BDT results 

`root -l TMVA::TMVAGui("TMVA.root")`

