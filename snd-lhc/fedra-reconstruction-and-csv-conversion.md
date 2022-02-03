# FEDRA reconstruction and csv conversion

## Reconstruction procedure

### Making scanset

`makescanset -set=21.0.0.0 -from_plate=60 -to_plate=1 -dz=-1315 -suff=cp.root`

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





