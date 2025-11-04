---
description: Always done, never ending
---

# Basic workflow

I will list here all the procedure starting from LASSO processing:

Connect to scanner@nusrv9.na.infn.it Path for analysis: /ship/CHARM2018 for SHIP-CHARM \~/foot/2019\_GSI/ for FOOT

## New folder

Create a folder like this /ship/CHARM2018/CH1R1/b000001&#x20;

## Add a plate

* mkdir p001
* Create a symbolic link to the data folder (actual path depends from scanned brick): 'ln -s /mnt/data/../.../P01/tracks.raw.root 1.0.0.0.raw.root'
* Check raws with `check_raw.C` default script and thickness.C from /ship/CHARM2018/macros/thickness.C

## Create a ScanSet object

'EdbScanSet' is an object which contains all the identificatives of the plates where you want to work, along with affine transformations currently known between the plates. We need to create an EdbScanSet object **BEFORE EVERY OPERATION** of libScan applications (emlink, emalign, emtra)

We can create an EdbScanSet with the command:

`makescanset -set=1.0.0.0 -dzbase=175 -dz=-1300 -from-plate=29 -to_plate=1`

It could be useful to save the default configuration in a scanset.txt file, customized with the plates to be analized. Note: it is normal that dz is negative and dzbase is positive. dzbase is the thickness of the plastic base, dz is the distance between plates. Using negative dzbase fails the linking

Global coordinates are defined taking one plate as reference, then applying the affine transformations to the others, with respect to the reference one. By default it is the last plate, if needed it can be changed by adding a '-refplate' option.

Warning: Using -reset to reset parameters after an ill-done alignment resets also shrinkage parameters! This has no consequences after linking, but we lose information about how was the shrinkage (we can always find it in the report though). I usually prefer to manually insert the identity transformation in the AFF folder.

Automatic bash script to create folder and the symbolic link to data folder:

`source createlink.sh`

Other useful option for makescanset:

* **-refplate nplate**: choose referecence plate the set will built according to. By default, the first or last plate;
* **-updateaff "aff"**: update set with affine transformation "aff", which must be manually written (for example, -updateaff 1 0 0 1 20 30);
* **-updatesetaff refset:** update set with affine transformation from reference set "refset". It calls EdbScanSet::TransformBrick( EdbScanset \&ss)&#x20;

## Linking

Linking is made to obtain basetracks from microtracks. Basetracks are chosen according to a chi-square minimization, using coordinates, angles and cluster number as input (reference FEDRA 2006).\
Linking is done with the command:

`emlink -set=1.0.0.0 -new`

Please check [EdbScanProc](https://twiki.cern.ch/twiki/bin/edit/SndLHC/EdbScanProc?topicparent=SndLHC.FedraWorkflow;nowysiwyg=0) class for details about linking and alignment

#### Linking parameter file

```
fedra.link.AFID:                         1
fedra.link.PixelCorr:                    0 1. 1.
fedra.link.CheckUpDownOffset:            1
fedra.link.BinOK:                        6
fedra.link.NcorrMin:                     100
fedra.link.DoCorrectShrinkage:           1
fedra.link.read.InvertSides:             0
fedra.link.read.HeaderCut:               1
fedra.link.read.UseDensityAsW:           0
fedra.link.read.ICUT:                    -1     -500. 500.   -500.   500.    -1.   1.      -1.   1.       0.  50.
fedra.link.RemoveDoublets:               1    5. .1   1
fedra.link.DumpDoubletsTree:             0
fedra.link.shr.NsigmaEQ:                 7.5
fedra.link.shr.Shr0:                     0.7
fedra.link.shr.DShr:                     0.3
fedra.link.shr.ThetaLimits:              0.005  1.
fedra.link.DoCorrectAngles:              1
fedra.link.ang.Chi2max:                  1.5
fedra.link.DoFullLinking:                1
fedra.link.full.NsigmaEQ:                5.5
fedra.link.full.DR:                      30
fedra.link.full.DT:                      0.1
fedra.link.full.CHI2Pmax:                3
fedra.link.DoSaveCouples:                1
fedra.link.Sigma0:                       1 1 0.013 0.013
fedra.link.PulsRamp0:                    6 9
fedra.link.PulsRamp04:                   6 9
fedra.link.Degrad:                       5
fedra.link.LLfunction:                   0.256336-0.16489*x+2.11098*x*x
fedra.link.CPRankingAlg:                 0
emlink.outdir:                           ..
emlink.env:                              link.rootrc
emlink.EdbDebugLevel:                    1
```



A fundamental parameter of the linking is the shrinkage initial value. Check **Shr0** (default is 0.9) and the tolerance **DShr**.

To select a particular surface you can apply the selection in **HeaderCut**&#x20;

Also, if you change angular range, remember to change **ICUT** and **ThetaLimits**

After the linking, check the report b000001.0.0.0.link.pdf and verify that the shrinkage plots show some peaks.



#### From FEDRA TWiki

In this case for the piece of raw scanning data (for example 1cm\*1cm scanned for the one emulsion plate) will be found all couples of microtracks satisfying to the CHI2 criteria. It is assumed that in one file there is 1 tree containing the data for one emulsion plate. All data in the couples tree are in the local **Plate Reference System**. It means that the Z-coord is local (0-300 microns) and affine transformations calculated (later) during the alignment procedure do not applied (them are conserved in par-files). One entry to the tree correspond to 1 basetrack. For each basetrack **s** appropriate microtracks **s1** and **s2** are saved. Note that it is possible that one microtrack from the one side is in agreement with two or more microtracks on the other side, in this case more then 1 couple (basetracks) will be stored (see eN1,eN1tot variables). This method permit not to loose any information and to perform additional selections after linking.

#### Couples ranking

In the link.rootc file, the CPRankingAlg 0 will rank the couples **(EdbSegCouple)**, according to their **eCHI2P.** They are thus sorted, so for each microtrack we have a rank of the couples (eN1 and eN2).&#x20;

* eN1 equal to 1 means this is the best couple for microtrack 1 (bot)
* eN2 equal to 2 means this is the best couple for microtrack 2 (top)

In the end, the base-track usually used **(EdbSegP),** is the one corresponding to the best couple for both micro-tracks (this is the reason eN1==1 and eN2==1 are in both alignment and tracking selections)

#### Cell linking via HTCondor

In order to be able to handle density up to 10,000 tracks/mm2, we need to process the cell in parallel, with better memory management and accuracy of reconstruction.

This parallelization is performed with HTCondor.

The code can be found here:&#x20;

{% embed url="https://github.com/SND-LHC/emu_reco_analysis/blob/master/condor_scripts/condorlinking_second.sh" %}

{% embed url="https://github.com/SND-LHC/emu_reco_analysis/blob/master/condor_scripts/condorlinking_first.sh" %}

We also need to remember to copy the link.rootrc file **before** starting the HTCondor submission, since inserting the copy in the submission script created issues (it was done simultaneously by all scripts...)

After the process, there is a script to check if files have been correctly created

There is also an overlap, which is later removed by selecting only the couples within each cells. A final couples tree is finally created:

[https://github.com/SND-LHC/emu\_reco\_analysis/blob/master/linkingmap/merge\_couplestrees.C](https://github.com/SND-LHC/emu_reco_analysis/blob/master/linkingmap/merge_couplestrees.C)

## Alignment

It is one of the most delicate and important operations, because it allows to connect the different plates. Alignment is done with the command:

`emalign -set=1.0.0.0 -new`

In order to avoid too long waits, it is convenient to optimize the alignment parameters on a small area, then we can move to the whole scanned area (usually 3 iterations). Aligning parameters are to be inserted in 'align.rootrc'.

As with the linking, check the report b000001.0.0.0.align.pdf to verify the presence of peaks in xy residuals and angle. If alignment is bad, reset the affine transformations by adding a row in AFF/\*aff.par with the identity transformation (1.,0.,0.,1.,0.,0.) to reset the alignment iterations.

Fine alignment does not use DZ parameter, which can lead to absurd values! at the beginning.&#x20;

Note: thetadens plot by default has **no uniform binning!** Interpreation may be misleading&#x20;

### Alignment parameters

* **OffsetMax:** the maximal offset to be looked for
* **DZ:** the range +- dz will be scanned by coarce align
* **DPHI:** the range +- dphi will be scanned by coarce align
* **Sigma\[2]:** sigma of the bt useful for the fine alignment ie:(10,0.01) -> SigmaR is for position, SigmaT is for angles
* **DoFine:** Performs fine alignment after coarce alignment
* **readCPcut:** Selection of couples for alignment
* **SaveCouples:** Saves tree of aligned couples in al.root file

### Bash Scripts

Linking (alignment) may be required to be performed in multiple iterations for each plate (pair of plates). In this case, it is useful to write all the steps in bash files, like for example:

`source linkingloop.sh 10 9`

and

`source alignloop.sh 10 9`

These scripts activate the commands for linking and alignment.

### Affine parameter file structure

The alignment results are stored in an AFF.par file, for example

named 6.10.0.0.6.9.0.0.aff.par

```
ZLAYER 	 0 	 -1306.780151 0.000000 0.000000
AFFXY 	 0 	 1.00078 -0.00145571 0.0011822 1.00174 -430.349 1040.46
#(AFFXY LAYER a11 a12 a21 a22 b1 b2)
AFFTXTY 	 0 	 1 0 0 1 0 0
SHRINK 	 0 	 1.000000
```

### Global reports

To check the plots of the reports from the different steps, I have added a global check alignment script, to check quickly the alignment in all the plates from the same brick.

### Reproduce residuals

The alignment report is produced by EdbPlateAlignment::ProduceReport()

To reproduce the position residuals from the al.root file, there are also the couples stored there in a tree.

6.10.0.0.6.9.0.0.al.root

The positions of the couples are already **corrected** with the affine transformation matrix (of the previous step, docorrectcouples and rankcouples are by default false in alignment parameters):

* s1 is the plate downstream (plate 10, corrected to 9)
* s2 is the plate upstream (plate 9, not corrected)

Note that we usually set in the other order when tracking (plate 9 corrected to 10). Please not be mislead

To have them at the same position and plot the residuals, the difference to be computed is:

```
s2.eX - (s1.eX + dz * s1.eTX) , s2.eY - (s1.eY + dz * s1.eTY)
```

with **dz** the Z distance between  the two plates, computed from the alignment and stored in the aff.par file

{% hint style="info" %}
Update: it is more accurate to reproduce them as originally done by FEDRA. It uses layer corrections, applied to segments from the two plates (even if for s2 they are identical transformations). See procedure in  [https://github.com/antonioiuliano2/macros-snd/blob/master/FEDRA/alignmentresiduals.C](https://github.com/antonioiuliano2/macros-snd/blob/master/FEDRA/alignmentresiduals.C)
{% endhint %}



## Tracking

It will perform global tracking between the different plates. Tracking is done with the command:

`emtra -set=1.0.0.0 -new`

and the script `check_tr.C` can be used to check the efficiency and results. Parameters are in the 'track.rootrc' file

Please check the EdbScanTracking::TrackSetBT() function for details

### Main parameters:

* **readCPcut:** selection on segments to do the tracking;
* **nsegmin**: minimum number of segments to define a track;
* **ngapmax**: maximum number of gaps between tracks;
* **DZGapMax**: maximum dz between gaps of segments. Definition: Min(|z1 -z|, |z2-z|)
* **DRmax**: maximum distance between points, check both in x,y and R
* **DTmax:** maximum angular distance, same procedure as DRmax
* **Sigma0:** parameters at 0 angles (x y TX TY)
* **Degrad:** angular degradation of parameters: S = S0 _(1 + eDegra&#x64;_&#x41;ng)
* **DoRealign:** use volume tracks to improve alignment transformations and update set.root file at the end
*

{% hint style="info" %}
Segments with negative xy coordinates are not tracked. Need to inspect the source code for the reason. For now, beware of negative x or y coordinates.
{% endhint %}



## Vertexing

Example of basical vertex script is found in check\_vertex.C. It will search for a file named `linked_tracks.root` and it will reconstruct the vertices from there.&#x20;

For the charm analysis, I have sligthly modified it into a charm\_vertexing.C to:

* Save not only vertex information, but also data from associated tracks and segments in an output tree. This will allow to analyze the reconstructed vertices offline

After producing the file, I usually prepare a tree file with some additional distributions for Valerio. See script `manual_check_vertices.C` for details.

### VTX tree structure

Currently, the vertex tree contains the following branches.

Scalar branches (i.e. one number for vertex):

* **n:** vertex molteplicity (accessed with N());
* **vx,vy,vz:** coordinates of reconstructed vertex (accessed with VX(), VY(), VZ());
* **vCOV:** covariance matrix of vertex position (stored as TMatrixD)
* **meanvx,meanvy,meanvz:** mean point of starting tracks (accessed with X(), Y(),Z());
* **maxaperture;** maximum angle between tracks in the vertex (computed with MaxAperture());
* **probability**: probability of vertex from fit (accessed with V()->prob());
*   **flag:** a flag providing information about vertex topology (accessed with Flag()):

    * flag 0: neutral (tracks' starts attached only
    * flag 1: charge (tracks' ends\&starts attached
    * flag 2: back neutral (tracks' ends attached only

    The following flags are similar to the previous one, but they add the information that vertex has common tracks to at least another vertex (if LinkedVertexes() has been called)

    * flag 3: neutral, linked (has common tracks with other vertex, only if LinkedVertexes() has been called)
    * flag 4: charge, linked
    * flag 5: back neutral, linked
    * flag -10: **discarded vertices (all tracks were associated to higher rank vertices).**&#x20;



Array branches (i.e. track properties):

* **nseg\[itrk]:** number of found segments for each track (accessed with track->N());
* **npl\[itrk]:** number of expeceted segments for each track (accessed with track->Npl());
* **incoming\[itrk]:** tell us if track ends or start at vertex (accessed with GetVTa(itrk)->ZPos()). 1-track start, 0-track end connect to the vertex
* **nholes\[itrk];** number of holes for each track (accessed with track->N0());
* **maxgap\[itrk]**: maximum gap between segments of each track (accessed with track->CheckMaxGap());
* **impactparameter\[itrk]:** impact parameter of track with respect to vertex (accessed with vertex->GetVTa(itrk)->Imp()); (note, IP is positive, and computed as in libvt++/VtDistance/distance-> See the distance() function).

Arrays of FEDRA objects (as from linked\_tracks.root format)

* **t.:** TClonesArray of EdbSegP with tracks information;
* **s:** TClonesArray of EdbSegP with segments information for each track;
* **sf:**  TClonesArray of EdbSegP with fitted segments information for each track;

The following branches are to be used only in vertices from MC simulations, no sense in real data:

* **MCEventID\[itrk]:** true MCEventID of each track (accessed with track->MCEvt());
* **MCTrackID\[itrk]:** true MCTrackID of each track (accessed with track->MCTrack());
* **MCTrackPdgCode\[itrk]:** true MCTrack PdgCode of each track (accessed with track->Vid(0));
* **MCMotherID\[itrk]:** true MCMotherID of each track (accessed with track->Aid(0));&#x20;

{% hint style="info" %}
When accessing by terminal with Scan, t.Aid\[0] does not work properly (it returns 0 or wrong values for second track onwards, probably due to be an array of array branch). It is set properly when vertexfile is read with the ReadVertexTree function. Track->Aid(0) then works as expected!\
When doing Scan, please use MCMotherID instead
{% endhint %}

## ReadVertexTree

ReadVertexTree is used to rebuild the vertices from the information in the TTree, just as ReadTracksTree is done for volume tracks.

This was done to avoid saving all vertices as root objects, unfeasible for SHiP and SNDLHC vertices numbers (but still used in FOOT, for example).

Advantages:

* Easy to share and manipulate: ROOT TTrees
* Can be easily merged in bigger files, but..

Disadvantages:

* ... the merge messes up the IDs (vertexID, trackID...);
* The parameters of EdbVertexRec MUST be the same used in vertexing (and if they are not stored, values WILL be different -> adding them in branch?). If the values are the sames, however, VX, VY, VZ, IMP are exactly reproduced

