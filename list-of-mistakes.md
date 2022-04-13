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

##











