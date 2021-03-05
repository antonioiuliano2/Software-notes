---
description: When scanning is not enough
---

# Add-ons

## Activation of additional Pads

To activate an additional pad to the LASSO software routine, you can:

* Click on the status bar \(i.e. bottom bar with the info messages\), 
* Activate the additional pads you wish to use;
* Restart LASSO;

## Used Add-ons

### Manual check

This is used to check the grains from which a track was reconstructed, in top and bottom layers of emulsion. The main challenge is to actually **find** these grains. Indeed, you have the track coordinates, which are provided to the software, but of course they are not that accurate, and you have to spend quite effort to go at the rigth position. Anyway, what you usually have to do is:

* Obtain track coordinates \(in the **original** reference system, not the global one of the scanset\);
* Pass input txt file with track information;
* Activate the manual check interface. The plate will go to the estimated position, then you will see an image with the patterns for bot and top interface, coloured differently.
* Have fun looking for that track

### Fog Plot

This add-on is used to check the fog level of an emulsion. It is a good estimator of the background quality within the data. What you need to do is:

* Perform a normal scan \(even a small area is enough\);
* Modify the processing configuration to include fog measurement \(see Fog Plot manual for details\);
* Start processing: it will produce a fog.txt file;
* Read the fog.txt file with the LASSO Fog Plot add-on;

{% hint style="info" %}
Be careful about paths in the configuration file. I once put as path "fog.txt", without any path information: it was saved in the LASSO folder! This is not ideal
{% endhint %}

The window will produce three plots, describing the grain density, in top and bottom layer of emulsion. The three plots represent single cluster grain density, multiple cluster grain density, and all cluster grain density. The unit is grains/\(micron^3\). I was told that a density larger than 10 grains/\(micron^3\) is bad for reconstruction. There is also an option to draw "color" plots, representing in arbitrary units how much "dark" the grains are.

