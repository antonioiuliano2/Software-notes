# Microscope corrections

## Introductions

These corrections need to be computed after the installation of a new microscope, or when the support plate is changed (for example, OPERA-like or SND-like emulsion size).&#x20;

### List of corrections

1. **flimgen**
2. **cormtx** (This is a correction of uneven magnification distortion along the field of view)
3. **slope** (This is a correction of spherical distribution)
4. **flatview** (Correction allowing to transfer the spherical field of view into a flat one)
5. **grainvol** (Correction of grain volume/brightness)

## Pixel size determination and flimgen

Stretching the image from the camera on two monitor screens, select a point, run it from left to right - record x coordinate, run back - record x coordinate. Repeat at least 3 times, get average value, divide 1 by number of pixels: **Nx = 2336**

Repeat the procedure for y axis. Divide the average value by the number of pixels: **Ny = 1728**&#x20;

Enter the resulting values in&#x20;

&#x20;**Modules -> Processor -> img\_pixel\_size**

Make a test scan (**flimgen**) and convert into clusters

## Cormtx

Create two directories: **/bot** and **/top**

Launch copied files from previous directory

Select scan parameters x 90 y 70

Set thickness of bot (top) with a distance of **10-15 μm** from base.&#x20;

Make test scans, convert them into raw.root files

Send them to Y: folder (visibile by nusrv)

Insert Xpix and Ypix in **viewdist.rootrc**

On Linux server, copy directories to disk Y:

**viewdist -f=clust.raw.root**

obtain correction map

send the correction map back to LASSO

create folders bot\_1 and top\_1

Fill these 2 maps in img\_cor\_mat\_0 and img\_cor\_mat\_1. Both the same file, bot if you scan top, top if you scan top

Repeat the scan and conversion

N.B. When building following maps, remember to build them **on top of previous corrections**!

Example: (for top\_1)

```bash
viewdist -f clust_raw.root -add=/top_1/correction_matrix.root
```

Repeat until map converge

Obtained files need to be moved to **C:\LASSO\_x64**, renaming them in order to replace already existing files.

## Slope

It is only a fast scan

* launch **run.bat**
* launch scans
* launch **OpGrnProc.bat**
* launch **fit.C:**

```bash
root -l grains.raw.root fit.C
```

## Flatview and grainvol

They use the same scan:

* copy folders
* Scanning (measure glass, bottom, base and top, check light level, to have gray level about 190)
* **OpGrainProc.bat**
* **run\_flatview.bat**
* **run\_grain\_vol.bat** (from folder 5.grainvol)
* check plots with .C scripts
* copy **flatview\_0.vfm** and **flatview\_1.vfm** in **C:\LASSO\_x64**
* copy **grnvol\_mtx\_0.grv** and **grnvol\_mtx\_1.grv** in **C:\LASSO\_x64**

### gfind

copy fit values in **link\_corr**, **link\_cor\_0, link\_cor\_1**

****

{% hint style="info" %}
**Note:** the last corrections (the 5th, in particular), should be performed with a sampling emulsion film from the OPERA experiment, not one with SND@LHC beam.
{% endhint %}

****

****

## Angular offset

This correction cannot be applied before scanning, must be performed a posteriori.

Each microscopes has not an exactly planar stage. Therefore, an angular offset can be observed, depending on the singolar microscope (about 10 mrad in TX and TY).

The proposed solution to correct it is to analyze a plate scanned twice: first normally, second rotated of 180°.&#x20;
