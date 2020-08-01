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

### Development of alignment map with usual alignment

On Friday the 31, I was 'reminded' of the need of an alignment map procedure to be designed anew, so it is better to start taking notes here \(other that in my paper notebook\), for important steps.

#### Current alignment procedure

First, a list of all the files to checked for alignment: we use the command

$FEDRA\_ROOT/bin/emalign -set=1.0.0.0 -new -v=2

List of files called:

1. $FEDRA\_ROOT/src/appl/emrec/emalign.cpp \(options do\_set and do\_new\);
2. $FEDRA\_ROOT/src/libScan/EdbScanProc.cxx \(function AlignSetNewNopar\(\) and AlignNewNopar\(\)\); AlignSet reads the identifiers from the sets, actual alignment launched by AlignNewNoPar\(\)
3. $FEDRA\_ROOT/src/libAlignment/EdbPlateAlignment.cxx \(function Align\(\)\)

\(There are of course other classes involved, but these are the most important files to have in mind and to follow\).

Basic steps: 

* Get the patterns with couples from plates of the pair, if Affine transformation is present transform them
* Check if patterns are actual filled, than launch CoarseAl\(\) and FineAl\(\) \(if both coarse and fine alignment have been activated in align.rootrc, as by defaults\)
* RankCouples



