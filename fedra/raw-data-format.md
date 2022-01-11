---
description: Microtracks
---

# Raw data format

The raw.root files contain microtracks. Following information copied mostly from FEDRA wiki:



## Run file information

Inside the run file there are following objects:

* Run file header (EdbRunHeader)
* Views tree with the data
* Fiducial marks (now obsolete, used in OPERA)

One entry into the tree is one microscope view. All data inside the view are in local **View Reference System**. The information about the view position in respect to stage (plate) kept in the view header.

There are the following superbranches in the Views tree:

headersGeneral information about view position and contentsclustersarray of clusters: all clusters fond in the view are heresegmentsarray of segments: all segments found in the viewtracksnot used nowframesinformation about each frame and eventually image (image usually do not saved)

#### View Headers branch

Header is one/view. The header object of the type EdbViewHeader:

```
class EdbViewHeader : public TObject {

private:

  Int_t    eViewID;   // View ID in the Area
  Int_t    eAreaID;   // Area ID in Run (prediction id)

  Float_t  eXview;    // stage coord, top left corner
  Float_t  eYview;    // 

  Float_t  eZ1;    //
  Float_t  eZ2;    // emulsion surfaces in absolute stage coordinates
  Float_t  eZ3;    // measured once per view
  Float_t  eZ4;    //

  Int_t    eNframesTop;  // top    | number of frames in the view (0,1,2...16...)
  Int_t    eNframesBot;  // bottom | 

  Int_t   eTime;         // System time since last view saving in msec

  Int_t   eNclusters;    // number of clusters saved in the view
  Int_t   eNsegments;    // number of segments saved in the view

  EdbAffine2D   eAff;    // affine transformation for the view 
                         // (make sence for SySal-converted data only)

  TArrayF  *eZlevels;    //! z of each taken view (frame) (obsolete!)

  Int_t   eCol;          // the position of the view in the scanned area, measured in views, 
  Int_t   eRow;          // starting from the reference angle (typically up-left)

  Int_t   eStatus;       // View scanning status
  Int_t eEvent           //optional: the part of the tree may be associated with event. Setted via run->header->Flag(8)
  Int_t eTrack           //optional: some view group may be associated with track. Setted via run->header->Flag(9)
```



#### Clusters branch

All clusters of the view organized as one TClonesArray of the objects type EdbCluster:

```
class EdbCluster : public TObject, public EdbPoint3D {

public:

  Float_t    eX;       // cluster coordinates (in microns when converted from rwd)
  Float_t    eY;       //
  Float_t    eZ;       //
  Float_t    eArea;    // Cluster area: number of pixels
  Float_t    eVolume;  // Cluster volume - in case of greyscale pixels
  Int_t      eFrame;   // frame index
  Int_t      eSide;    // emulsion side index
  Int_t      eSegment; // segment id to be attached (-1 if no segment)
```

#### Segments branch

All segments of the view organized as one TClonesArray of the objects type EdbSegment.

Note: the segments here are what we call **micro-tracks**!

```
class EdbSeg3D : public TObject {
private:
  Float_t    eX0;         // |_coordinates of the segment's initial point
  Float_t    eY0;         // | in the SAME FOR ALL SEGMENTS stage coordinate system
  Float_t    eZ0;         // |
  Float_t    eTx;         // tanX: deltaX/deltaZ
  Float_t    eTy;         // tanY: deltaY/deltaZ
  Float_t    eDz;         // length of the segment along Z with sign

//______________________________________________________________________________
class EdbSegment : public EdbSeg3D {
private:
  Int_t      eSide;       // side of the segment location (0-up, 1-down)
  Int_t      ePuls;       // puls height (number of clusters) OR (sum of clust area)*1000+(number of clusters)
  Int_t      eID;         // segment identifier
  Float_t    eSigmaX;     // dispersion parameter of grains around track line
  Float_t    eSigmaY;     // dispersion parameter of grains around track line
```

When data converted from Sysal rwd the eSigmaX is filled with SIGMA provided by sysal, eSigmaY in this case dummy

#### Frames branch

All frames of the view organized as one TObjArray of the objects type EdbFrame

Note: the frames are the **images at different heights**, for each xy view there are multiple frames!

```
class EdbFrame : public TObject {

private:

  Int_t       eFrameID;           // frame identifier
  Float_t     eZframe;            // Z-coordinate of the frame
  Int_t       eNcl;               // total number of clusters found in the frame
  Int_t       eNpix;              // total number of nonzero pixels found in the frame
  EdbImage    *eImage;            // CCD image
```
