---
description: Detailed infromation can be found on website or directly in fedra code
---

# Frequently used objects

## Reference coordinates

### Set: EdbScanSet

Location in fedra: /fedra/src/libEbase/EdbScanSet.h

* To access absolute Z coordinate: GetPlate(iplate)->Z()
* Relative DZ between plates: GetDZP2P(plate 1, plate 0)
* Relative affine matrix between plates: GetAffP2P(plate1, plate0)

### Conditions: EdbScanCond

This class contains precious information about data condition, and the assumed position and angular resolution are used to compute the **covariance matrix** of base-tracks, by using **Sigma0** e **Degrad** from track.rootrc in EdbScanCond:EdbScanCond::FillErrorsCov(). It is therefore done during tracking, couples in cp files have NOT Covariance Matrix at all (be careful of errors).

## Tracking and Vertexing

### Segment: EdbSegP

Location in fedra: /fedra/src/libEbase/EdbSegP.h Class where base tracks are stored. Segment ID can be accessed with ID()

* To access kinematics: X(),Y(),Z(),TX(),TY() (SX(),SY(),SZ(),STX() and STY() for sigmas)&#x20;
* Spherical angle coordinates: Phi() and Theta() (actually Theta() gives TanTheta(), due to historical bug)&#x20;
* Emulsion information: Plate(), DZ(), DZem(), W() (the latter weigth w() returns the number of clusters)&#x20;
* Fit information (from linking): Chi2(), Prob()&#x20;
* MC information (if available): MCEvt(), MCTrack(). Set with   SetMC( int mEvt, int mTrack )

#### Couples tree

For each basetrack **s** appropriate microtracks **s1** and **s2** are saved. Note that it is possible that one microtrack from the one side is in agreement with two or more microtracks on the other side, in this case more then 1 couple (basetracks) will be stored (see eN1,eN1tot variables). This method permit not to loose any information and to perform additional selections after linking.

There are the following superbranches in the tree:

* **cp** - "coupling" parameters
  * eCHI2P - Chi of the couple. This is the main agreement criteria
  * eN1tot - number of couples where participating this microtrack
  * eN1 - the rank of the couple for this microtrack, changing from 1 to eN1tot, 1 is the best
  * eN2, eN2tot - the same for the second microtrack
  * eID1, eID2, eCHI2 - do not important
* **s1** - the first microtrack of the couple (on the one side)
* **s2** - the second microtrack (other side)
* **s** - the basetrack



### Pattern: EdbPattern

Location in fedra: /fedra/src/libEdr/EdbPattern.h

Container of segments, usually one for plate. After affine transformations are applied, they can be built together in a single brick (EdbPVRec instance, usually). The brick is then used for volume tracks or shower reconstruction. Please check the function TrackSetBT() at /fedra/src/libScan/EdbScanTracking.cxx to check the details of the procedure.

An important note: when patterns are added in TrackAssembler::AddPattern(), a covariance matrix is computed: this allow to perform operations and to know track chi squared even if segments errors are not known (i.e. simulation). Chisquare of volume track is computed instead in EdbTrackP::FitTrackKFS.





### **Track: EdbTrackP**

Location in fedra: /fedra/src/libEdr/EdbPattern.h

Class for volume track, inherits from EdbSegP. ID() returns ID of Track.

In general, segments are the data after linking, fitted segmented are the reconstructed positions of the track in the plates after track fit

* To access segments, GetSegmentFirst(), GetSegmentLast() and GetSegment(int i). N() returns number of segments associated to this track&#x20;
* Similar methods are used to access fitted segment. Usually are written the same, with an F after Segment (example. GetSegmentFFirst())
* Npl() is the number of plates expected. Different from N(), which the number of plates where the segment has been found.

Important note: linked\_tracks.root files do NOT contain EdbTrackP objects, but EdbSegP objects, and track information are stored in separated branches. Probably for memory saving.

File sequence (in src/\*):

appl/emrec/emtra.cpp -> libScan/EdbScanTracking.cxx (also look for EdbTrackAssembler)

{% hint style="info" %}
To access track ID in a EdbTrackP, use yourtrack->Track() and not yourtrack->GetSegment(1)->Track(). The latter is affected by reset of the selection when a cut is applied in reading the input tree.
{% endhint %}

#### Accessing tracks

See **src/libEIO/EdbDataSet.cxx** for an example on how to access the linked\_tracks.root file (ReadTracksTree). This function is used for vertexing and track display (macro/check\_vertex.C, modified in my macro charm\_vertexing in my Git repository, to add info to output vertex tree).

If necessary, tracks can be splitted with **EdbTrackFitter::SplitTrack(EdbTrackP \&t, EdbTrackP\&t1, int ipoint)**

### Vertex: EdbVertex

Location in fedra: /fedra/src/libEdr/EdbVertex.h Class for vertex reconstruction.

* Vertex Position: X(), Y(), Z() . Also present VX(), VY() and VZ(), which seem to return the same. The latter are the ones suggested in the Fedra Wiki to obtain vertex position;
* To obtain information of vertex fit: chi2(), prob(), ndf();&#x20;
* To access associated tracks: N() gives the number of tracks. GetTrack(int i) gives the i-th track;&#x20;
* Impact(i) returns distance of i-th track from the vertex;&#x20;
* MaxAperture() returns the maximum angular distance between two tracks associated to this vertex (as in tan(theta));&#x20;
* GetVTa(i) returns VTA object for i-th track;&#x20;
* Flag() returns a flag describing vertex: 0 only begin tracks, 1 both begin and end present, 2 only end track present, -10 all tracks associated to other, higher-ranked, vertices

EdbVTA is the object returned by GetVTA(i). Declared also in EdbVertex.h. It describes Vertex-track association:

* Zpos() returns 0 for end track, 1 for begin track (stored in TTree in branch named "incoming");&#x20;
* Imp() returns impact parameter;&#x20;
* Dist() returns distance from vertex to nearest track point;&#x20;
* GetTrack() and GetVertex() return track and vertex objects from the association;&#x20;

Vertex fit probabilities and parameters defined in src/libVt++/vt++/libVertex.C

### EdbVertexComb

Location in fedra: /fedra/src/libEdr/EdbVertexComb.h Prepare track combinations to study 'anomalous OPERA event in Bari'. Still check for info

### EdbPVRec

Location in FEDRA: /fedra/src/libEdr/EdbPVRec.h

This is the main utility for storing data and pass them to reconstruction. The instance is usually called 'ali' and it contains reconstructed tracks and vertices, but it can also contain base-tracks (couples) if needed.

To recognize the various plates, a set of patterns needs to be defined. It can be filled with a set of tracks (see Init() method of check\_vertex for an example), and then the pattern can be filled with any data, and also drawn with EdbEDA.

### EdbLog

Print messages, of various gravity (verbose level set in gEdbDebugLevel)

To suppress them:

gEDBDEBUGLEVEL = 0;

