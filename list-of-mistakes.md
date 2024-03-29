---
description: Errare è umano, perseverare è diabolico
---

# List of mistakes

There are scientific errors, and there are mistakes/blunders. We can only evaluate the first, but we of course need to avoid the latter. However, there is no perfection, and I cannot live in paranoia forever. Instead, I have to admit that I make mistakes, yes sometime stupid mistakes, and I should list them. In this way, I will be reminded to avoid them.

But first, remember the two most important rules:

1. **DO NOT PANIC**
2. **HAVE FUN**&#x20;

## General programming mistakes

### Using = instead of == for logic comparison

This angers me a lot, and I still make it too often. Both python and C++ require a logic condition to have the form:

```
if (value==2){do something;}
```

**two equals symbols, NOT one**. Otherwise, it becomes an assignment! It makes all the codes go wrong and unpredictable.

Modern compilers (ROOT6, Python) immediately notice this issue and either warn you (ROOT) or stop you altogether (Python). However, at this point I should not rely on the compiler to notice this stupid, but very bad, blunder.&#x20;

### Not resetting event counters or containers at the start of the loop

Often, I use counters which are related to each events: the number of hits in a given detector for each event. I may also store the hits in a container, for example an RVec, a TObjArray or a std::vector/python list.&#x20;

It stands to reason that the counter **must be resetted** to 0 at the start of the loop, and containers cleared (usually Clear() function or something similar). A good way to remind myself to be done and to create a section of the code, identified by some comments, which does that. A class should have its own dedicated function to this aim.

{% hint style="info" %}
Always check that you are using the correct function to empty your container. Some classes, such as TObjArray, effectively uses Clear() to delete all elements from within. Other classes, such as TEventList, use Clear() only for the title, instead you need to call Reset() to empty it!
{% endhint %}

### Copy and conversion errors

This is difficult to spot, so it is recommended to always check that the output of your copy and conversion is what you expect.&#x20;

A common mistake is between "float" and "double" numbers, since FEDRA uses float, while FairShip uses a double. At best, you reduce the precision from a double to a float (it is usually higher than our experimental precision, so the effect is minimal), but at worst you make the output **total jibberish.** This often happens to ROOT TTrees, when converting between ROOT and Python: someone with a very weird sense of humour decided to call the decimal values in Python "float", even if they have the precision of C "double" and not C "float" numbers.&#x20;

Luckily, numpy helps us, so always try to use the builtin types **np.float32** (for C float) and **np.float64** (for C double) when you need to interface your data between a ROOT TTree and a Pandas dataframe (for most case, it is safer to use the automatic utilities, like RDataframe, root\_numpy, etc. Sometimes, unfortunately, I cannot, since I need to handle weird customized objects like the FairShip or FEDRA ones). Please check the  following list of numpy types (on a side note, int is actually C long, you should use **np.intc**, but for integer numbers the problem usually does not present):

[https://numpy.org/doc/stable/user/basics.types.html](https://numpy.org/doc/stable/user/basics.types.html)

## Bad pandas

### Element in column condition

One of the Python neatest if condition is if an element is in a container. You literally just do

if (element in container)

However, there is a **dire warning** for pandas columns. **DO NOT** write:

if (element in df\["MCEvent"])

because it considers df\["MCEvent"] as a dictionary, and it will apply the condition to row numbers (a.k.a. the "keys" of the columns). **What do you want is:**

if (element in df\["MCEvent"].values)

## ROOT specific mistakes

### Wrong TRandom3 seed or initialization

I usually generate random numbers for my work. ROOT recommends the use of TRandom3 (**NOT** TRandom), and it is the one usually called when I simply use gRandom (but for important codes, I recommend to initalize a TRandom3 instance myself).

```
TRandom3 *myrandomgen = new TRandom3();
```

Also, check the seed. If I plan to use a code only once (or I want to repeat it in the same conditions everytime), I do not need to initialize the seed. It will be automatically set to 4357. However, if I want the code to **give different numbers everytime, I have to set the seed to 0**

```
myrandomgen->SetSeed(0);
```

In this case the seed is guaranteed to be unique in space and time, because it will be generated according to a TUUID object.

### Not weighting histograms or counters

Simulations often are given with some **weight**, representing the stastical contribution of each event for whatever reason (more dense material, produced with higher statistics, etc..). These weights **should be accounted for in my analysis**. It seems obvious, but for too much time I have totally ignored them.

Therefore, when I fill an histogram I should consider the weight:

```
my1Dhisto->Fill(myvar, weight);
```

and I should consider the weights in efficiency counters (I then divide to the total weight, instead of the total number of events). That way, the actual contribution of each event is considered.

Be careful with statistical errors for weighted efficiencies. The error is always given by the **actual number of simulated events** (If I used 1000 events in my simulation, the error in the efficiency is the same even if the events represent 10000, 1000000 or more physical events. Instead, an absolute error in the expected yield  scales with the weight, since I apply error propagation to a product with a constant).

### Integral() in an histogram with not enough range

We **always** need to check if histogram has underflow and overflow.

We must **never use Integral()** if an histogram has entries below or above range, otherwise only the ones in range are accounted for. Only bins in the bins range are considered.

### &#x20;RooFit chi squared :Not giving the function name

RooFit is a powerful tool for fits and p.d.f. computing, but it is a bit harder to use than standard ROOT fits. I have read that simpler toolkits cause people to make more mistakes. I disagree, the more something is counterintituive, the easier it becomes to make a blunder, due to a misunderstanding in the code structure.

In this case, in ROOT chi squared is automatically given when an histogram is fitted to a function (better use "L" option for fits to histograms):

```
histo->Fit(func,"L");
```

Instead in RooFit you need first to do the fit, then you need to draw the functions to a frame. Finally you retrieve the reduced chi squared  (i.e. **chi2/ndf**) from a frame:

```
myframe->chiSquare(pdfname, histname, nFitparam)
```

where nFitparam is the number of fitted parameters for the function. **I do not recommend to launch it in the way chiSquare(nFitparam).** It becomes easier to mess up, if multiple functions are drawn (i.e. the total distribution and the single components).

Just a final note: even for unbinned fits, the chiSquare is computed from the binned data in the plotted histograms.

## Hardware and Lab behaviour

### Respect the dark room

The emulsion dark room is the place where emulsions are prepared (before the data run) and developed (after the data run). It is scary to work there, and I really prefer to do it as less as possible, but when there is the need I am preparerd to go there. It stands to quite obvious reason that emulsions are fragile there, so **leave all light sources behind:** turn off the phone (better to leave it outside the room if possible), remove smart watches, torches, etc. Remember the time I scared my supervisor (luckily when all the emulsions were safe in another room). In general, adopt much extra cautionl in all times I go there

### Prisoner counting

Count along columns of x

### Oil must not go inside the miscroscope stage

Self-explaining. Scanning oil must go on the scanning stabe, but **not outside it**. If it goes inside the motors of the moving table....never happened before, but this is a warning I have been given, so I write it here

## FEDRA reconstruction

### Check quality plots

Never take for granted that linking and alignment worked. Even if they worked for 56 films, they may not work for film 57!

Check if residuals peaks are present, if chi square distributions are reasonable, etc. Opening many ROOT files is unfeasible, so I prepared png conversion tools for this purpose. Use them.

### MCS correction

Still no idea how the track reconstruction handles multiple scattering (it depends on the option, 0 or 2 should not use it). However, providing the right X0 value (3504 for tungsten, instead of 5810 for lead) sure helps.

### Mixing up makescansets

As our FEDRA reconstruction currently works, it reads the EdbScanSet stored in set.root files from the folder, to know which emulsion film numbers to process.

It is **bad** if you try running makescanset while another process is running, since it will continue running with the new file numbers provided in the scanset. It is **especially horrid** if it is a emlink process, which does not update files, but it **recreates** them, basically destroying your hard work for couples and there after (since, you need to redo couples again).

So, always check if processes are active in the same set, before launching makescanset. You simply need to launch **top**. To find where a process was launched from, please use pwdx  followed by process ID

Also, it is useful to **keep screen instances separate** for separate bricks. Launching many cd commands to different folder everytime leads to bad surprises. Let us keep sources of mistakes to the absolute minimum, please.

### Adding vertex trees and analyzing them

If you hadd vertex trees, you will have multiple tracks with the same TrackID (i.e., if you add 20 bricks, you will have 20 tracks with TrackID 0).

Naturally, this messes up the vertex analysis, which usually **requires refitting the vertices with the same VERTEX\_PAR.**

If TrackIDs are messed up, vertex->VX(), vertex->VY(), vertex->VZ(), among others, will be complete jibberish&#x20;

## FairShip/SNDSW

### Check Simulation Messages!

In general, always check if the output of the simulation has something unexepected. There may be something wrong in the simulation option, which could invalidate your data and months of analysis.

It is pretty difficult to list all possible problems, but some examples:

* Check if emulsions are active or not (default they are passive). Also, check if you need all tracks, or you can keep the cut at 100 MeV kinetic energy;
* Check for positions of generated volumes. I am TIRED of remaking simulations in a hurry because I did not change the GenieGen.SetPosition() default value again!
* Check for positions of generated tracks (StartX vs StartY). Checking positions of hits only **is not enough!!**
* Check for not used volumes. Geant4 does not like that you define volumes in TGeo, but you never use them in your geometry;
* Check field maps. Should they be present? Which ones? The nutaudet field map was present even months after we decided not to have a magnet in the neutrino detector anymore.
* Check conversion option into FEDRA (smearing, efficiency, background contamination, etc.)

In general, try to be as critical as possible for the simulation. Try to keep note of simulation time, files and conditions

## Data reproducibility

A fundamental part of the scientific method is being able to reproduce the plots you made.

It seems obvious, but people may (and they will) ask you about plots you made 10 years ago, and they will expect you to:\


* Be able to reproduce them;
* Be able to change them and know why they changed (a.ka. source code);

Of course, data are heavy, and many tests need to be discarded at some points, but **you must always keep and know about (in decreasing order of priority)**:\


* Thesis material;
* Paper material;
* Note material;
* Collaboration Meeting material;

Source code: GIT, GIT, GIT. Always keep repo synched, always put useful comment information;&#x20;

Plots: keep always them in ROOT format, PNG cannot answer questions (picture and schematics are of course an exception).

Heavy ROOT/CSV/Other data: write in your **private** notebook (and maybe in the latex folder) the paths and server locations.&#x20;

Keep test code in repository too, even if it must go in a separate branch. Test must become production at some point;

You need to be able to know about:

* Every **figure**;
* Every **table**;
* Every **measured value**

You may ask: is it obvious? You writed the note/paper/thesis? #Remindme10years

##











