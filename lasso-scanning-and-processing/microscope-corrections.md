# Microscope corrections

## Introductions

These corrections need to be computed after the installation of a new microscope, or when the support plate is changed (for example, OPERA-like or SND-like emulsion size).&#x20;

### List of corrections

1. flimgen
2. cormtx (This is a correction of uneven magnification distortion along the field of view)
3. slope (This is a correction of spherical distribution)
4. flatview (Correction allowing to transfer the spherical field of view into a flat one)
5. grainvol (Correction of grain volume/brightness)

## Pixel size determination and flimgen

Stretching the image from the camera on two monitor screens, select a point, run it from left to right - record x coordinate, run back - record x coordinate. Repeat at least 3 times, get average value, divide 1 by number of pixels: **Nx = 2336**

Repeat the procedure for y axis. Divide the average value by the number of pixels: **Ny = 1728**&#x20;

Enter the resulting values in&#x20;

&#x20;**Modules -> Processor -> img\_pixel\_size**

Make a test scan (**flimgen**) and convert into clusters

## Cormtx

Create two directories: /bot and /top

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
