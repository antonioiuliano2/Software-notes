---
description: Checking if everyhing is OK
---

# Data quality plots

Data quality monitoring is a fundamental step of every analysis. It becomes even more important when there are a lot of data to be processed in a short time, thus people do not have a lot of time to inspect all the data features.

In this case, having some reference plots helps a lot, providing a **baseline** on what should be expected. However, critical eye and common sense are always required to evaluate what you are looking at.

Reference plots are from SND@LHC Wall Commissioning. Numbering goes from top to bottom, left to right (i.e. 1 is top left, 2 is top right, 3 is below 1, 4 is below 2 and so on...)

## Raw data quality

Raw data quality is monitored with 2 scripts:

* check\_raw.C (default macro of FEDRA)
* [thickness.C](https://github.com/antonioiuliano2/macros-snd/blob/master/FEDRA/thickness.C)

### Check raw

1D Plots are usually separated in different line colors:

* Blue line: down side of the emulsion plate;
* Red line: up side of the emulsion plate;
* Black line: total (sum of blue and red lines)

The script is launched as:

```bash
root -l tracks.raw.root check_raw.C
```

It gives you 3 canvases:

#### Plot raw\_z

1. space distribution of the scanned views. In the ideal case it should be flat and horizontal. In the practical case it can be a bit inclined but regular (due to stage nonplanarity). If one see non-regular effects it could be a signal for focusing problems.
2. the same as before presented as time dependence
3. Number of clusters vs Z. In ideal case should be nearly flat with the central valley. Often there are a lot more clusters on the external layers
4. Should be approximately base thickness. In reality in the last versions of SySal this plot becomes less significant because give almost fixed value.
5. and
6. Number of clusters vs frame id
7. and
8. Z0 - the microtracks starting point distribution. The difference between up and down ones can give the approximate estimation of the real base thickness.

![Example of raw\_z plot](../.gitbook/assets/check\_z.png)



#### Plot raw\_view

1. distribution of the microtracks inside one view (down side). In the ideal case it should be uniform. This plot is very important - if the variation is big it means that in pure zones of view the efficiency will be lost and reach zones the high background will be acquired. Sometimes one can see the CCD defects (high bins) on this plot. Them are very dangerous especially for intercalibration and must be removed by "recset -ccd". In principle them should be already removed by on-line. The view equalization of Sysal is usually far to be perfect - this is a reason of inappropriate signal rise near the angles.
2. same as 1 presented as 2-d color plot; down side
3. same as 1 for up side
4. same as 2 for up side
5. TX Microtracks angular distribution. Usually not so interesting. In case of CCD defects presence - there are sharp peaks near the 0
6. same as 5 for TY
7. bidimensional angular distribution for down side. If emulsion was exposed to high intensity beam, and the data quality is reasonably good, one can note the hints to the beam peaks on this plot
8. same as 7 for up side

![Example of raw view plot](../.gitbook/assets/check\_view.png)

#### Plot raw\_surf

1. microtracks density distribution on the scanned surface (down). It is important plot. Normally it should be uniform. Any structure is a potential problem
2. same as 1 for up side
3. microtracks/view distribution. Normally should be nearly Gaussian with not so big spread. The acceptable mean value may variate from 100 to 800 depending on the emulsion quality and scanning conditions. Normally for one set of emulsions scanned with the same settings the mean value should not variate. In the reality it almost newer happened and it is a big problem. If this number >500 the probability of the casual linking is high and this provocate high background on the level of basetracks.
4. clusters/view distribution.
5. pulse height distribution (number of clusters associated with microtrack). In principle should be the most important parameter for estimation of microtrack quality. Unfortunately having the huge background on the microtracks level and short dynamic diapason of this value its almost impossible to use this parameter for signal to bg separation on the level of single microtrack. For basetracks and long tracks this parameter (integrated) has much more sense.
6. microtrack volume: the sum of area of all clusters associated to the microtrack
7. this parameter should give the idea about RMS of clusters distribution around the microtrack. For this valid most of comments given for item 5.
8. same as 7 calculated by FEDRA tracking if it is applied

![Example of raw data surf](../.gitbook/assets/check\_surf.png)

### Thickness.C

This script is used to inspect the thickness of emulsion layers:

* 1 and 2 show the number of segments in bottom and top layers;
* 3 shows the thickness 1D distributions for bottom layer (blue), top layer (red) and plastic base(black);
* 4, 5 and 6 show the thickness maps for bottom layer (4), top layer (5) and plastic base (6):

![Example of thickness plot](../.gitbook/assets/thickness\_p003.png)

{% hint style="info" %}
In order to see eventual issues during scanning, it is **imperative** that the bin width match the view step: 770 micron x 565 micron. For OPERA emulsions, the number of bins used was 202 x 182. For SND@LHC, it is 300 x 350, but the width remains the same. Please remember it, it is important not to find white lines in the future.

Antonio
{% endhint %}

## Linking report

Linking is the reconstruction of particle trajectories in a single emulsion film (base-tracks) from the trajectories in the bottom and top emulsion layers (micro-tracks). At this step, the shrinkage correction is applied.

The linking report is produced automatically after a linking. A PDF contains the most recent linking reports, but any report can be accessed from the cp.root file, by drawing the **report** TCanvas. It contains the following plots:

* 1 and 2 are the xy distributions in shrinkage correction sample, 1 (bottom) and 2 (top);
* 3 and 4 are the theta angle distributions, 1 (bottom) and 2 (top);&#x20;
* 5 is the position residuals for the shrinkage correction sample;
* 6 is the angular residuals for the shrinkage correction sample;
* 7 and 8 are the shrinakge corrections for bottom (7) and top (8) layers. **Always check that there is a clear peak in these two distributions**
* 9 and 10 are the TX and TY residuals between bottom micro-tracks and base-tracks;
* 11 and 12 are the TX and TY residuals between top micro-tracks and base-tracks;
* 13 is the xy distribution of the produced couples;
* 14 is the 2D angular distribution of the produced couples;
* 15 is the chi squared distribution for the base-tracks fit. **It should have a clear peak before 1, then an asymmetric curve;**
* 16 is the same, for high W base-tracks. All comments for 15 apply here.



![Linking report example](../.gitbook/assets/linking\_report\_p003.png)

## Alignment report

It reports the alignment procedure between two plates. It starts with a coarse alignment, then it does a fine alignment on the results of the coarse one. Again, a PDF contains the most recent alignment reports, but any report can be accessed from the AFF/al.root file, by drawing the **report\_al** TCanvas. It contains the following plots:

* 1 and 2 are the patterns of the base-tracks in the single films;
* 3 is the Z distance between plates, vs the phi angle rotation, after coarse alignment. It is normal that z has not a clear peak, but **check that phi is well limited**;
* 4 is the position residuals after coarse alignment;
* 5 is the position residuals after fine alignment. **Check that there is a peak here**
* 6 is the angular residuals after fine alignment**. Check for a clear peak here**
* 7 is the theta density. The bins have not the same size!
* 8 and 9 contains the distributions of matched base-tracks after alignment. For high number of base-tracks, they are not produced to save memory.

&#x20;

![Example of alignment report](../.gitbook/assets/alignment\_report\_p004\_p003.png)
