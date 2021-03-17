---
description: Detailed infromation can be found on website or directly in fedra code
---

# Frequently used objects

## Reference coordinates

### Set: EdbScanSet

Location in fedra: /fedra/src/libEbase/EdbScanSet.h

* To access absolute Z coordinate: GetPlate\(iplate\)-&gt;Z\(\)
* Relative DZ between plates: GetDZP2P\(plate 1, plate 0\)
* Relative affine matrix between plates: GetAffP2P\(plate1, plate0\)



## Tracking and Vertexing

### Segment: EdbSegP

Location in fedra: /fedra/src/libEbase/EdbSegP.h Class where base tracks are stored. Segment ID can be accessed with ID\(\)

* To access kinematics: X\(\),Y\(\),Z\(\),TX\(\),TY\(\) \(SX\(\),SY\(\),SZ\(\),STX\(\) and STY\(\) for sigmas\) 
* Spherical angle coordinates: Phi\(\) and Theta\(\) \(actually Theta\(\) gives TanTheta\(\), due to historical bug\) 
* Emulsion information: Plate\(\), DZ\(\), DZem\(\), W\(\) \(the latter weigth w\(\) returns the number of clusters\) 
* Fit information \(from linking\): Chi2\(\), Prob\(\) 
* MC information \(if available\): MCEvt\(\), MCTrack\(\). Set with   SetMC\( int mEvt, int mTrack \)



### Pattern: EdbPattern

Location in fedra: /fedra/src/libEdr/EdbPattern.h

Container of segments, usually one for plate. After affine transformations are applied, they can be built together in a single brick \(EdbPVRec instance, usually\). The brick is then used for volume tracks or shower reconstruction. Please check the function TrackSetBT\(\) at /fedra/src/libScan/EdbScanTracking.cxx to check the details of the procedure.

An important note: when patterns are added in TrackAssembler::AddPattern\(\), a covariance matrix is computed: this allow to perform operations and to know track chi squared even if segments errors are not known \(i.e. simulation\). The segment covariance matrix is computed assuming angular degradation, according to "Sigma0" and "Degrad" parameters provided in track.rootrc. See also SetErrors\(\) method

\*\*\*\*

\*\*\*\*

### **Track: EdbTrackP**

Location in fedra: /fedra/src/libEdr/EdbPattern.h

Class for volume track, inherits from EdbSegP. ID\(\) returns ID of Track.

In general, segments are the data after linking, fitted segmented are the reconstructed positions of the track in the plates after track fit

* To access segments, GetSegmentFirst\(\), GetSegmentLast\(\) and GetSegment\(int i\). N\(\) returns number of segments associated to this track 
* Similar methods are used to access fitted segment. Usually are written the same, with an F after Segment \(example. GetSegmentFFirst\(\)\)
* Npl\(\) is the number of plates expected. Different from N\(\), which the number of plates where the segment has been found.

Important note: linked\_tracks.root files do NOT contain EdbTrackP objects, but EdbSegP objects, and track information are stored in separated branches. Probably for memory saving.

File sequence \(in src/\*\):

appl/emrec/emtra.cpp -&gt; libScan/EdbScanTracking.cxx \(also look for EdbTrackAssembler\)

#### Accessing tracks

See **src/libEIO/EdbDataSet.cxx** for an example on how to access the linked\_tracks.root file \(ReadTracksTree\). This function is used for vertexing and track display \(macro/check\_vertex.C, modified in my macro charm\_vertexing in my Git repository, to add info to output vertex tree\).

If necessary, tracks can be splitted with **EdbTrackFitter::SplitTrack\(EdbTrackP &t, EdbTrackP&t1, int ipoint\)**

### Vertex: EdbVertex

Location in fedra: /fedra/src/libEdr/EdbVertex.h Class for vertex reconstruction.

* Vertex Position: X\(\), Y\(\), Z\(\) . Also present VX\(\), VY\(\) and VZ\(\), which seem to return the same. The latter are the ones suggested in the Fedra Wiki to obtain vertex position;
* To obtain information of vertex fit: chi2\(\), prob\(\), ndf\(\); 
* To access associated tracks: N\(\) gives the number of tracks. GetTrack\(int i\) gives the i-th track; 
* Impact\(i\) returns distance of i-th track from the vertex; 
* MaxAperture\(\) returns the maximum angular distance between two tracks associated to this vertex \(as in tan\(theta\)\); 
* GetVTa\(i\) returns VTA object for i-th track; 
* Flag\(\) returns a flag describing vertex: 0 only begin tracks, 1 both begin and end present, 2 only end track present, -10 all tracks associated to other, higher-ranked, vertices

EdbVTA is the object returned by GetVTA\(i\). Declared also in EdbVertex.h. It describes Vertex-track association:

* Zpos\(\) returns 0 for end track, 1 for begin track; 
* Imp\(\) returns impact parameter; 
* Dist\(\) returns distance from vertex to nearest track point; 
* GetTrack\(\) and GetVertex\(\) return track and vertex objects from the association; 

Vertex fit probabilities and parameters defined in src/libVt++/vt++/libVertex.C

### EdbVertexComb

Location in fedra: /fedra/src/libEdr/EdbVertexComb.h Prepare track combinations to study 'anomalous OPERA event in Bari'. Still check for info

### EdbPVRec

Location in FEDRA: /fedra/src/libEdr/EdbPVRec.h

This is the main utility for storing data and pass them to reconstruction. The instance is usually called 'ali' and it contains reconstructed tracks and vertices, but it can also contain base-tracks \(couples\) if needed.

To recognize the various plates, a set of patterns needs to be defined. It can be filled with a set of tracks \(see Init\(\) method of check\_vertex for an example\), and then the pattern can be filled with any data, and also drawn with EdbEDA.

