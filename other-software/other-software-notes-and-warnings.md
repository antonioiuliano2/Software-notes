---
description: DON'T PANIC
---

# ROOT stuff

## ROOT 6:23/01 changing branch name

The new (still experimental ) version of ROOT seems to have the brilliant idea to change the name of a TClonesArray branch ending with a dot, removing the dot.

Example: in previous ROOT version I had

t.

t..e

t..t

Now i have:

t

t.e

t.t

Checked with a modified version of tree0.C example.

Asking ROOT team



## ROOT I/O

### TFile reading online and local

It seems that TFile is more powerful than I knew. It allows to read both local files and files from a server, https and xROOTD

### Checking a file while is being written

First, we need to save the file periodically. You can use once at the time or within your loop&#x20;

```
mytree->SetAutoSave(nbytes) #saves every nbytes periodically
mytree->AutoSave() #saves now 
```

If we try to open the file now, we get warning message, and try to recover, but we can access the files. It is naturally safer to not rely on ROOT recovery system, but use the recommended procedure to access the TTree while being written ([https://root-forum.cern.ch/t/errors-in-remote-client-reading-of-ttree-from-tfile-via-xrootd-while-simultaneously-updating-ttree-on-server/43479](https://root-forum.cern.ch/t/errors-in-remote-client-reading-of-ttree-from-tfile-via-xrootd-while-simultaneously-updating-ttree-on-server/43479)), also found in the TDirectory reference. Writing process calls:

```
#writing process
inputtree.AutoSave()
r.gDirectory.Saveself()

#reading process
inputfile = r.TFile.Open("testfile.root","READ") #done only first time
inputfile.ReadKeys() #everytime you want an update
inputtree = r.gDirectory.Get("testtree")
```

From ROOT forum:

Recreate the TFile object does everything you need (except you also need to delete the previous one) but also does more than you really need. In other word, doing multiple TFile::Open is simpler but a bit slower.

Cheers,\
Philippe.

## Transparency

Volumes are made transparent with SetFillColorAlpha(color, alpha).

However, to actually see transparent volumes in the canvas, you need to set&#x20;

`OpenGL.CanvasPreferGL = 1`

in the .rootrc file or in your HOME path (or, if you prefer, in _`$ROOTSYS/etc/system.rootrc)`_



