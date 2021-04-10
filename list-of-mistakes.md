---
description: 'Errare è umano, perseverare è diabolico'
---

# List of mistakes

There are scientific errors, and there are mistakes/blunders. We can only evaluate the first, but we of course need to avoid the latter. However, there is no perfection, and I cannot live in paranoia forever. Instead, I have to admit that I make mistakes, yes sometime stupid mistakes, and I should list them. In this way, I will be reminded to avoid them.

## General programming mistakes

### Using = instead of == for logic comparison

This angers me a lot, and I still make it too often. Both python and C++ require a logic condition to have the form:

```text
if (value==2){do something;}
```

**two equals symbols, NOT one**. Otherwise, it becomes an assignment! It makes all the codes go wrong and unpredictable.

Modern compilers \(ROOT6, Python\) immediately notice this issue and either warn you \(ROOT\) or stop you altogether \(Python\). However, at this point I should not rely on the compiler to notice this stupid, but very bad, blunder. 

### Not resetting event counters or containers at the start of the loop

Often, I use counters which are related to each events: the number of hits in a given detector for each event. I may also store the hits in a container, for example an RVec, a TObjArray or a std::vector/python list. 

It stands to reason that the counter **must be resetted** to 0 at the start of the loop, and containers cleared \(usually Clear\(\) function or something similar\). A good way to remind myself to be done and to create a section of the code, identified by some comments, which does that. A class should have its own dedicated function to this aim.

## ROOT specific mistakes

### Wrong TRandom3 seed or initialization

I usually generate random numbers for my work. ROOT recommends the use of TRandom3 \(**NOT** TRandom\), and it is the one usually called when I simply use gRandom \(but for important codes, I recommend to initalize a TRandom3 instance myself\).

```text
TRandom3 *myrandomgen = new TRandom3();
```

Also, check the seed. If I plan to use a code only once \(or I want to repeat it in the same conditions everytime\), I do not need to initialize the seed. It will be automatically set to 4357. However, if I want the code to **give different numbers everytime, I have to set the seed to 0**

```text
myrandomgen->SetSeed(0);
```

In this case the seed is guaranteed to be unique in space and time, because it will be generated according to a TUUID object.

### Not weighting histograms or counters

Simulations often are given with some **weight**, representing the stastical contribution of each event for whatever reason \(more dense material, produced with higher statistics, etc..\). These weights **should be accounted for in my analysis**. It seems obvious, but for too much time I have totally ignored them.

Therefore, when I fill an histogram I should consider the weight:

```text
my1Dhisto->Fill(myvar, weight);
```

and I should consider the weights in efficiency counters \(I then divide to the total weight, instead of the total number of events\). That way, the actual contribution of each event is considered.

Be careful with statistical errors for weighted efficiencies. The error is always given by the **actual number of simulated events** \(If I used 1000 events in my simulation, the error in the efficiency is the same even if the events represent 10000, 1000000 or more physical events. Instead, an absolute error in the expected yield  scales with the weight, since I apply error propagation to a product with a constant\).

### Not giving the function name for chi squared to RooFit

RooFit is a powerful tool for fits and p.d.f. computing, but it is a bit harder to use than standard ROOT fits. I have read that simpler toolkits cause people to make more mistakes. I disagree, the more something is counterintituive, the easier it becomes to make a blunder, due to a misunderstanding in the code structure.

In this case, in ROOT chi squared is automatically given when an histogram is fitted to a function \(better use "L" option for fits to histograms\):

```text
histo->Fit(func,"L");
```

Instead in RooFit you need first to do the fit, then you need to draw the functions to a frame. Finally you retrieve the reduced chi squared  \(i.e. **chi2/ndf**\) from a frame:

```text
myframe->chiSquare(pdfname, histname, nFitparam)
```

where nFitparam is the number of fitted parameters for the function. **I do not recommend to launch it in the way chiSquare\(nFitparam\).** It becomes easier to mess up, if multiple functions are drawn \(i.e. the total distribution and the single components\).

Just a final note: even for unbinned fits, the chiSquare is computed from the binned data in the plotted histograms.

