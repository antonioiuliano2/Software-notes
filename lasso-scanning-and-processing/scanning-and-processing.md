---
description: Acquiring data, the first step of all analysis
---

# Scanning and processing

LASSO is the software currently used to reconstruct the tracks, from the grains left in emulsions layers. This software takes into account both **scanning** and **processing.**

## Scanning emulsions

Scanning is automatic, but the following preparatory steps need to be performed before every scan:

1. Place the emulsion on the scanning stage, removing air bubbles by tenderly pressing on them and moving on the vacuum pipes;
2. Open the vacuum with the dedicated valve (**Important!** Always check that vacuum is on before leaving the lab)
3. Change the plate number in LASSO scanning option. (**Important!** if old plate number is left, previous scan will be overwritten!)
4. Take the zero both in xy coordinates (bottom-left border, as seen on the screen) and in z (center, down until you can go down anymore). Thickness is changed with PagUp and PagDown keys;
5. Set grey level to about 190 (user Insert and Delete keys).
6. Measure thickness levels -> Glass, Bottom, Plastic Base and Top layers (check when you start not to see the grains anymore). Set the measured thickness levels in pargeom.
7. Check that the parameters are correctly inserted and start the scans.
8. Upload the ELOG at webpage nusrv9.na.infn.it:8080

{% hint style="info" %}
Most of these identifications (apart from the bold warnings), are not iron-clad rules, but they are mostly good working habits we have dediced to follow in the current scans. Other scanning runs may follow a different workflow (i.e. they might not reset the z and/or measure the thicknesss each time). Always ask experts and people currently work in that scan for details.
{% endhint %}

## Fundamental parameters

Before preparing the scanning of a new brick, it should be checked that the **number of images** collected is enough to cover the emulsion thickness \* dzgap, leaving at least 10-20 micrometers of tolerance. Otherwise the emulsion thickness cannot be accurately measured by the microscope.

Naturally, another fundamental parameter is the **scanning area** which is defined by setting the intervals \[xmin,xmax] and \[ymin,ymax].

### Thresholds in par\_corr

There are three thresholds in par\_corr, related to the number of clusters required to accept the view:

* **surf\_thres\_min** and surf\_thres\_max are related to the profile of clusters along z. surf\_thres\_min is for zmin (left side), **surf\_thres\_max** is for zmax (right side). They should be no more than half the number of clusters at that edge. They can be set to the same value, but naturally it should match the previous condition for both edges. Each of surf\_thres\_min and surf\_thres\_max has two numbers, for bottom and top emulsion layers.
* **min\_clust** is the parameter used by LASSO to know if a view is good enough to follow the emulsion. It can be set to the lowest between surf\_thres\_min and surf\_thres\_max.&#x20;

There is a **threshold** in par\_corr for the number of clusters to be required to accept the view. surf\_threshold\_top should be equal to surf\_threshold\_bottom and to overall threshold. During the scan, it might be useful to check the scanning log to see if the number of clusters is larger than this threshold in most of the emulsion layers, but not in the borders. Otherwise, it would be necessary to set a different threshold. This is particularly necessary when starting the scan of a new brick.

## Processing

Processing emulsion data allows to reconstruct the tracks in the single emulsion layers (commonly labeled as `micro-tracks.` Processing is also automatic, and it is done in dedicated computers, that can be accessed remotely. What basically needs to be set is:

* Name of folder with plate to be processed (usually P#Nplate);
* &#x20;Thicknesses of emulsion layers;

Processing can be done with GPU or CPU. Using GPU is faster, but it should be avoided when scanning large angles (larger than 0.78 rad), otherwise it might lose efficiency. Check the angle parameters each time, and take into accounts that parameters from GPU and CPU are defined in different fields.

Parameters used for DESY19:

trk\_lim\_len = 2. 12.5

trk\_lim\_theta = 0. 1.

(for future scans, remember to clearly **ask desired angular region**.

Other important parameter is the **threshold, with must be the same** used during the scan. Remember to check that.

## GPU and CPU Processing

Select which one (trackercpu or trackergpu), by commenting the other in the cfg file.

Remember: GPU is not reliable over 0.78

track\_min\_density 16&#x20;

track\_min\_link\_density 15 (only used in GPU, no sense in CPU)

These values usually give inefficiencies at low angles, min\_density is best set to 0

For GPU it is sensed to have trk\_min\_link\_dens <= trk\_min\_track\_dens, otherwise the setting value will always be the one from trk\_min\_link\_dens.

GPU works only with view\_cell\_size = 5. 5. 5.», other values are not good

