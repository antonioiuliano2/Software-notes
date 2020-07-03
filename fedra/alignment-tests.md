---
description: Experimental functions for alignment
---

# Alignment tests

## **emtrackan** 

Start from the long tracks. General idea:  

* do tracking with larger parameters \(emtra\) 
* use long tracks to calculate the local corrections, assuming that the tracks in mean are straight \(emtrackan\) 
* use found correction map for a new tracking \(emtra\) 
* repeat the iteration... 

Using both xy position and txty angular affine transformation on the same pattern does not work, since segments do not match anymore. It is necessary to apply the xy transformation \(and z one\) to one patter of a pair, then txty transformation to the other pattern. 

Instead, current standard alignment does not compute angular affine transformations, only position transformation

Two main functions:

* **DoGlobalV2**: provides global alignment with all affine transformations, seems to work well, but should be tested;
* **MakeCorrectionMaptest**: allows to perform local alignment on a map with the option "-divide NXM".

List of parameters for emtra:

* read\_cut: track selection;

\*NCPmin: number of minimum couples to find alignment



