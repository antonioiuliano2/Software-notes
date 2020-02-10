---
description: 'Acquiring data, the first step of all analysis'
---

# Scanning and processing

LASSO is the software currently used to reconstruct the tracks, from the grains left in emulsions layers. This software takes into account both **scanning** and **processing.**

## Scanning emulsions

Scanning is automatic, but the following preparatory steps need to be performed before every scan:

1. Place the emulsion on the scanning stage, removing air bubbles by tenderly pressing on them and moving on the vacuum pipes;
2. Open the vacuum with the dedicated valve \(**Important!** Always check that vacuum is on before leaving the lab\)
3. Change the plate number in LASSO scanning option. \(**Important!** if old plate number is left, previous scan will be overwritten!\)
4. Take the zero both in xy coordinates \(bottom-left border, as seen on the screen\) and in z \(center, down until you can go down anymore\). Thickness is changed with PagUp and PagDown keys;
5. Set grey level to about 190 \(user Insert and Delete keys\).
6. Measure thickness levels -&gt; Glass, Bottom, Plastic Base and Top layers \(check when you start not to see the grains anymore\). Set the measured thickness levels in pargeom.
7. Check that the parameters are correctly inserted and start the scans.
8. Upload the ELOG at webpage nusrv9.na.infn.it:8080

{% hint style="info" %}
Most of these identifications \(apart from the bold warnings\), are not iron-clad rules, but they are mostly good working habits we have dediced to follow in the current scans. Other scanning runs may follow a different workflow \(i.e. they might not reset the z and/or measure the thicknesss each time\). Always ask experts and people currently work in that scan for details.
{% endhint %}

During the scan, it might be useful to check the scanning log to see if the number of clusters is larger than the threshold \(set in par geom\), in most of the emulsion layers, but not in the borders. Otherwise, it would be necessary to set a different threshold. This is particularly necessary when starting the scan of a new brick.

## Processing

Processing emulsion data allows to reconstruct the tracks in the single emulsion layers \(commonly labeled as `micro-tracks.` Processing is also automatic, and it is done in dedicated computers, that can be accessed remotely. What basically needs to be set is:

* Name of folder with plate to be processed \(usually P\#Nplate\);
*  Thicknesses of emulsion layers;

Processing can be done with GPU or CPU. Using GPU is faster, but it should be avoided when scanning large angles \(larger than 0.78 rad\), otherwise it might lose efficiency. Check the angle parameters each time, and take into accounts that parameters from GPU and CPU are defined in different fields.

Other important parameter is the threshold, with must be the same used during the scan. Remember to check that.

