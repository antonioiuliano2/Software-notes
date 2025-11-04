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

### GPU and CPU Processing

Select which one (tracker\__cpu or tracker_\_gpu), by commenting the other in the cfg file.

Remember: GPU is not reliable over 0.78

track\_min\_density 16&#x20;

track\_min\_link\_density 15 (only used in GPU, no sense in CPU)

These values usually give inefficiencies at low angles, min\_density is best set to 0

For GPU it is sensed to have trk\_min\_link\_dens <= trk\_min\_track\_dens, otherwise the setting value will always be the one from trk\_min\_link\_dens.

GPU works only with view\_cell\_size = 5. 5. 5.», other values are not good

### Description of processing parameters

#### Gpu parameters

* trk\_source = grains (Do not change);
* trk\_linker = gpu (Do not change);
* trk\_lim\_len = 10. 45. : limiti in lunghezza raggi coppie. La differenza tra CPU e GPU è dovuta all'algoritmo;
* trk\__lim\__&#x74;heta = 0.0 0.78: angular limits for processing (in radiants);
* trk\__gr\__&#x65;rr = 0.1 0.1 0.6; Increase or decrease bin numbers&#x20;
* trk\_gr\_nsig =2.8; Used as resolution for histogram (related to parameter above);
* trk\_slh\_effLen = 5. ; Set bin numbers (better not to change);
* trk\_slh\_effLen2 = 20;  Set bin numbers (better not to change);
* trk\_min\_link\_dens = 10;
* trk\_min\_track\_dens = 20;
* trk\_mip\_track\_dens = 30.; Controllo di qualità confronto; Score with respect to Minimum Ionizing Particle. Quanti grani produce una MIP? 30 grani/100 micron
* trk\_min\_track\_len = 34.; Lunghezza misura di una microtraccia. Distanza fra primo e ultimo grano;
* trk\_cut\_track\_score = -1.2 1; Limiti sullo score;
* trk\_cref\_nsig = 4; esigma Xi tracks eTY {eNframes Top==0} Scor; angolo Theta

trk\_min\_link\_dens ha senso solo per GPU

Solo il linking è diverso fra GPU e CPU

Raggi in CPU, Cilindri in GPU



#### CPU parameters (commissioning)

* trk\_source = grains (Do not change);
* trk\_linker = blind (Do not change);
* trk\_lim\_len = 2. 12.5 : limiti in lunghezza raggi coppie. La differenza tra CPU e GPU è dovuta all'algoritmo;
* trk\__lim\__&#x74;heta = 0.0 1.0: angular limits for processing (in radiants). CPU can process even at angles larger than 0.78;
* trk\__gr\__&#x65;rr = 0.1 0.1 0.6; Increase or decrease bin numbers&#x20;
* trk\_gr\_nsig =2.8; Used as resolution for histogram (related to parameter above);
* trk\_slh\_effLen = 5. ; Set bin numbers (better not to change);
* trk\_slh\_effLen2 = 20;  Set bin numbers (better not to change);
* trk\_min\_track\_dens = 0;
* trk\_mip\_track\_dens = 30.; Controllo di qualità confronto; Score with respect to Minimum Ionizing Particle. Quanti grani produce una MIP?? 30 grani/100 micron
* trk\_min\_track\_len = 17.; Lunghezza misura di una microtraccia. Distanza fra primo e ultimo grano;
* trk\_cut\_track\_score = -1.2 1; Limiti sullo score;
* trk\_cref\_nsig = 4; esigma Xi tracks eTY {eNframes Top==0} Scor; angolo Theta

## Dispatcher processing

The Dispatcher is a recently added tool to LASSO to split the processing work between multiple modules, each analyzing a different module in parallel. The modules can belong to the same PC performing the scanning, or to another PC (like a micserver).

For example, here is an example with 4 modules belonging to the same PC performing the scan:

* go into localhost folder -> run **OpTraProc.bat** -> 4 module icons should appear
* from the parent folder, launch **OpDispatc.bat** -> 1 module icon should appear
* Launch PAVICOM.bat (is not need anymore to launch the camera first);
* Set Parameters and start scanning as usual

If needed, some of the processing parameters, such as the folder and the gfind ones, can be set from the GUI. Others, instead, must be set from the cfg (such as, correction paraemeters, for example). In that case:

* change tmpl-OpTraProc.cfg
* launch setup\_disp.bat

For example, it is possible to change the number of dispatched modules, as well as the number of threads per module

## ROOT conversion

Conversion into ROOT data format is done with the FEDRA library. Command used: C:\LASSO\_x64\win32\EdbConv.exe -mode:OPERA\_MT .\tracks.obx\
\
It will use the FEDRA library file libEdb.dll from the folder of C:\LASSO\_x64\win32\\. So, if FEDRA is compiled again, file needs to be copied there.

## Common errors and issue:

### General

When the microscope is stuck, start by checking the lights. Is the camera light green and beeping? Check the end lanes (fine corsa) lights. &#x20;

Currently, resuming a stuck scanning is not supported.&#x20;

If microscope gets stuck, the only procedure to do is:

* Stop LASSO. If it does not stop, kill the PAVICOM GUI proess from task manager;
* Close (or kill) the camera icon;
* Copy the 4 log files in an error\_logs folder with the name of the stuck film;
* Restart stage for safety;
* Restart LASSO;
* Restart the scan. If a long time has passed (2 days), it is safer to remove the film and put it back, otherwise just check if there is oil;

### More combinations than expected

The tracks.obx and the converted tracks.raw.root are larger than expected, as in, more than 2 times larger than a similar film (same run, same brick position in the wall) on an another microscope.

This was solved by fixing dyafram orientation.

### Persistent pattern in screen

The patterns observed may be dots, or continuous lines.

You recognize them since they do not move with the emulsions, when you move the stage

They may be due to the camera, or the microscope lens.

You may be able study the pattern better by temporarily removing the "flatten" correction from the PAVICOM GUI, into "original" (but afterwards, **please put it back on**).

Please turn off both stage and microscope computers, then check camera and microscope connections. Contact expert for guidance.

### LASSO GUI issues

#### Cannot see microscope icon

LASSO cannot comunicate with the stage.

Please close LASSO and the camera, then reboot the stage from the stage computer.

If issue persists, check PAVGuide log.

#### Cannot set zero from coordinates correctly.

The coordinates are not set in PAVICOM itself. It communicates them to the stage controller, which sets them in its card.&#x20;

Try to reboot stage and LASSO. If the issue remains, it may be needed to contact an expert to test calibration and positions manually with the stage software.

#### At maximum light level, still unable to obtain gray level 190

The light is not powerful enough for the emulsion you are currently scanning (the fog and/or track density is too high).

The solution is to increase the camera exposure time. Close LASSO, then increase number in:

setFeatureValue=ExposureTime,INT,70

Then reboot LASSO

### PAVProcModule.log error message

#### Emulsion layer thickness check FAILED

The microscope could not measure the emulsion thickness for this layer.&#x20;

Check the number of clusters at different z positions.

If it happesn for every view, check emulsion quality and initial thickness values.

#### Many lines PopFoVHist timeout elapsed!

This tells us that the microscope is stuck. It will continue to timeout, leading to many identical lines.

If you see that the log is ending with this message repeated many times, please check the microscope.

Unfortunately, to have an idea on why the microscope is stuck, it is necessary to check the other logs.



### PAVCameraModule.log errors:

#### Cannot start Camera, Resources already in use

This means the camera was not closed correctly last time.

Please check the processes in Task Manager, and kill it.

If it does not work, the only solution is a computer reboot.

#### ERROR \*\*\*\*\*\*\*\*\*\*\*\*\*\* number of grabbed images is too different from expected: (68 != 72)

The camera could not acquire the expected images.

It crashed and it tried to reboot, it usually reboots itself correctly, but that view may be lost.

It may lead to a general crash of the camera and stop of the scanning

### OpTraProc.log error message

#### Warning: View skipped, too many clusters

View was skipped because there are too many clusters.&#x20;

Need to increase the limit in the cfg file, currently:\
\
clust\_lim=1100000

#### WARNING: View skipped - Too many grains (581211 > 524288)

View was skipped because there are too many grains.

This is an hard limit on the GPU processing algorithms, 2^{19} grains.

This limit cannot be increased, if scanning is needed, please increase cluster threshold for the scanning (two values for the two layers, they should be the same). Current values:

clz\_thres=500 500

### PAVGuide.log errors

#### Module (0x2) is not ready

Connection issues between stage and microscope PCs. Please, check the stage computer.

### Scanning halted due to PC Restart

Something happened that forced the PC to reboot.

To know what happened, you can use Windows Event Viewer

You can find  instructions here:

{% embed url="https://www.windowscentral.com/how-find-reason-pc-shutdown-no-reason-windows-10" %}

Basically, it amounts to check EventViewer - Windows Logs - System and Filter the Log according to the category

