---
description: >-
  Specific notes for our configuration, keep track how what I did wrong and
  right
---

# FEDRA reconstruction and csv conversion

## Intro

Always keep track of parameters. Always check if parameters change between bricks/walls/tests/other.&#x20;

[https://github.com/antonioiuliano2/macros-snd/blob/master/run1analysis/recoparameters/firstlink.rootrc](https://github.com/antonioiuliano2/macros-snd/blob/master/run1analysis/recoparameters/firstlink.rootrc)

## Reconstruction procedure

### Making scanset

`makescanset -set=21.0.0.0 -from_plate=60 -to_plate=1 -dz=-1315 -suff=cp.root`

``

### Linking notes for SND@LHC

Due to high track density, trying to perform unique linking leads to failure.

Therefore, two linking prodecure is used:

1. Find shrinkage correction, requiring Theta > 0.05. Also, in a small central region, 2.5 cm x 2.5 cm. Shrinkage and angular corrections are computed
2. Use shrinkage correction from previous computation. Shrinkage and angular correction computations are disabled (DoCorrectShrinkage and DoCorrectAngles are set to 0). All surface, all angles

Note: even if the region is the same, it appears that there are more couples in firstlinking. However, checking the "good couples" (eCHI2P<2.4&\&s.eW>20&\&eN1<=1&\&eN2<=1&\&s1.eFlag>=0&\&s2.eFlag>=0), numbers become compatible, as tested with background check films linking.

Also, global linking in nusrv1 required some ad-hoc changes:

* DoubleFilterOut with cell 6, instead of 1 (not recommended in normal operation);
* Increase maximum TTree size (safer change)

However, testing in RUN0 standard linking did not lead to appreciable changes in track quality (b000431 test).



### Track and vertex reconstruction

`emtra -set=21.0.0.0 -new -v=2`

{% hint style="info" %}
Remember to change the x and y ranges in track.rootrc according to the target unit we are considering.
{% endhint %}

A file \*.trk.root will be created. Link it to linked\_tracks.root

`ln -s b000021.0.0.trk.root linked_tracks.root`

Then we are ready for actual vertex reconstruction:

`root -l vertexing.C`

A file vertextree.root will be created

## Csv conversion

`python csvconversion.py`

Two files will be created:

* brick21.csv, containing all the information about the couples
* brick21vertices.csv, containing the vertices and tracks ID from recosntruction

## Summary of all bash scripts commands to do

Bash scripts should be found in macro\_snd GitHub, copy them into your main conversion folder. Check that paths are set properly.

The main idea is that the previous operations should be performed for all bricks, via .sh commands:

* Create folders b0000{11..14, 21..24,31..34,41..44,51..54}/p001...p060
* copy 2FEDRA.rootrc and check the settings;
* source preparebricks.sh
* source doreco.sh
* source csvconversion.sh
* source addvertexinfo.sh
* hadd vertextree\_allbricks.root \*/vertextrree.root&#x20;





