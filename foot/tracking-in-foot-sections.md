# Tracking in FOOT sections

In FOOT, tracking is performed separately for each section

## Tracking in each section

Tracking parameters for each sections are stored in **track\_Sx.rootrc** . They need to be copied before launching track reconstruction, as written in **tracking\_stacks.sh**

We usually perform tracking with realignment:

* Before we launch tracking with alignment (in track.rootrc modify variable: fedra.track.do\_realign 1)&#x20;
* **Without launching makescanset again,** launch tracking standard (in track.rootrc modify variable: fedra.track.do\_realign 0)

We obtain track files b00000\*.\*.0.0.trk.root, one for each section (S1, S2,S3...S7)

## Merge trackings

* prepare scanset for whole brick: **BRICK.0.0.0**
* **root -l launch\_mergetrackings2.C**
