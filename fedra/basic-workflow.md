---
description: 'Always done, never ending'
---

# Basic workflow

I will list here all the procedure starting from LASSO processing:

Connect to scanner@nusrv9.na.infn.it Path for analysis: /ship/CHARM2018 for SHIP-CHARM ~/foot/2019\_GSI/ for FOOT

## New folder

Create a folder like this /ship/CHARM2018/CH1R1/b000001 

## Add a plate

* mkdir p001
* Create a symbolic link to the data folder \(actual path depends from scanned brick\): 'ln -s /mnt/data/../.../P01/tracks.raw.root 1.0.0.0.raw.root'
* Check raws with `check_raw.C` default script

## Create a ScanSet object

'EdbScanSet' is an object which contains all the identificatives of the plates where you want to work, along with affine transformations currently known between the plates. We need to create an EdbScanSet object **BEFORE EVERY OPERATION** of libScan applications \(emlink, emalign, emtra\)

We can create an EdbScanSet with the command:

`makescanset -set=1.0.0.0 -dzbase=175 -dz=-1300 -from-plate=29 -to_plate=1`

It could be useful to save the default configuration in a scanset.txt file, customized with the plates to be analized.

Global coordinates are defined taking one plate as reference, then applying the affine transformations to the others, with respect to the reference one. By default it is the last plate, if needed it can be changed by adding a '-refplate' option.

Warning: Using -reset to reset parameters after an ill-done alignment resets also shrinkage parameters! This has no consequences after linking, but we lose information about how was the shrinkage \(we can always find it in the report though\). I usually prefer to manually insert the identity transformation in the AFF folder.

Automatic bash script to create folder and the symbolic link to data folder:

`source createlink.sh`

## Linking

Linking is made to obtain basetracks from microtracks. Basetracks are chosen according to a chi-square minimization, using coordinates, angles and cluster number as input \(reference FEDRA 2006\).  
Linking is done with the command:

`emlink -set=1.0.0.0 -new`

A fundamental parameter of the linking is the shrinkage initial value. Check Shr0 \(default is 0.9\).

After the linking, check the report b000001.0.0.0.link.pdf and verify that the shrinkage plots show some peaks.

## Alignment

It is one of the most delicate and important operations, because it allows to connect the different plates. Alignment is done with the command:

`emalign -set=1.0.0.0 -new`

In order to avoid too long waits, it is convenient to optimize the alignment parameters on a small area, then we can move to the whole scanned area \(usually 3 iterations\). Aligning parameters are to be inserted in 'align.rootrc'.

As with the linking, check the report b000001.0.0.0.align.pdf to verify the presence of peaks in xy residuals and angle. If alignment is bad, reset the affine transformations by adding a row in AFF/\*aff.par with the identity transformation \(1.,0.,0.,1.,0.,0.\) to reset the alignment iterations. 

### Alignment parameters

* **OffsetMax:** the maximal offset to be looked for
* **DZ:** the range +- dz will be scanned by coarce align
* **DPHI:** the range +- dphi will be scanned by coarce align
* **Sigma\[2\]:** sigma of the bt useful for the fine alignment ie:\(10,0.01\) -&gt; SigmaR is for position, SigmaT is for angles
* **DoFine:** Performs fine alignment after coarce alignment
* **readCPcut:** Selection of couples for alignment
* **SaveCouples:** Saves tree of aligned couples in al.root file

### Bash Scripts

Linking \(alignment\) may be required to be performed in multiple iterations for each plate \(pair of plates\). In this case, it is useful to write all the steps in bash files, like for example:

`source linkingloop.sh 10 9`

and

`source alignloop.sh 10 9`

These scripts activate the commands for linking and alignment.

### Global reports

To check the plots of the reports from the different steps, I have added a global check alignment script, to check quickly the alignment in all the plates from the same brick.

## Tracking

It will perform global tracking between the different plates. Tracking is done with the command:

`emtra -set=1.0.0.0 -new`

and the script `check_tr.C` can be used to check the efficiency and results. Parameters are in the 'track.rootrc' file

### Main parameters:

* **readCPcut:** selection on segments to do the tracking;
* **nsegmin**: minimum number of segments to define a track;
* **ngapmax**: maximum number of gaps between tracks;
* **DZGapMax**: maximum dz between gaps of segments. Definition: Min\(\|z1 -z\|, \|z2-z\|\)
* **DRmax**: maximum distance between points, check both in x,y and R
* **DTmax:** maximum angular distance, same procedure as DRmax
* **Sigma0:** parameters at 0 angles \(x y TX TY\)
* **Degrad:** angular degradation of parameters: S = S0 _\(1 + eDegrad_Ang\)

## Vertexing

Example of basical vertex script is found in check\_vertex.C. It will search for a file named `linked_tracks.root` and it will reconstruct the vertices from there. 

For the charm analysis, I have sligthly modified it into a charm\_vertexing.C to:

* Save not only vertex information, but also data from associated tracks and segments in an output tree. This will allow to analyze the reconstructed vertices offline

After producing the file, I usually prepare a tree file with some additional distributions for Valerio. See script `manual_check_vertices.C` for details.

### VTX tree structure

Currently, the vertex tree contains the following branches.

Scalar branches \(i.e. one number for vertex\):

* **n:** vertex molteplicity \(accessed with N\(\)\);
* **vx,vy,vz:** coordinates of reconstructed vertex \(accessed with VX\(\), VY\(\), VZ\(\)\);
* **meanvx,meanvy,meanvz:** mean point of starting tracks \(accessed with X\(\), Y\(\),Z\(\)\);
* **maxaperture;** maximum angle between tracks in the vertex \(computed with MaxAperture\(\)\);
* **probability**: probability of vertex from fit \(accessed with V\(\)-&gt;prob\(\)\);
* **flag:** a flag providing information about vertex topology \(accessed with Flag\(\)\);

Array branches \(i.e. track properties\):

* **nseg\[itrk\]:** number of found segments for each track \(accessed with track-&gt;N\(\)\);
* **npl\[itrk\]:** number of expeceted segments for each track \(accessed with track-&gt;Npl\(\)\);
* **incoming\[itrk\]:** tell us if track ends or start at vertex \(accessed with GetVTa\(itrk\)-&gt;ZPos\(\)\). 0 if starts at vertex, 1 if end at vertex;
* **nholes\[itrk\];** number of holes for each track \(accessed with track-&gt;N0\(\)\);
* **maxgap\[itrk\]**: maximum gap between segments of each track \(accessed with track-&gt;CheckMaxGap\(\)\);
* **impactparameter\[itrk\]:** impact parameter of track with respect to vertex \(accessed with vertex-&gt;GetVTa\(itrk\)-&gt;Imp\(\)\);

Arrays of FEDRA objects \(as from linked\_tracks.root format\)

* **t.:** TClonesArray of EdbSegP with tracks information;
* **s:** TClonesArray of EdbSegP with segments information for each track;
* **sf:**  TClonesArray of EdbSegP with fitted segments information for each track;

The following branches are to be used only in vertices from MC simulations, no sense in real data:

* **MCEventID\[itrk\]:** true MCEventID of each track \(accessed with track-&gt;MCEvt\(\)\);
* **MCTrackID\[itrk\]:** true MCTrackID of each track \(accessed with track-&gt;MCTrack\(\)\);
* **MCTrackPdgCode\[itrk\]:** true MCTrack PdgCode of each track \(accessed with track-&gt;Vid\(0\)\);
* **MCMotherID\[itrk\]:** true MCMotherID of each track \(accessed with track-&gt;Aid\(0\)\); 



