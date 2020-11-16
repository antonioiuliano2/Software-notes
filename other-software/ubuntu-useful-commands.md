---
description: To avoid googling the same command over and over
---

# Ubuntu useful commands

## Changing default command

I often need, during installations and builds, to change the gcc, g++,python versions. To do that, there is the update-alternatives command

```
$ update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
```

Then we can choose it with configure

```
update-alternatives --config python
```

## Analyzing multiple ROOT files

Usually simulations and data analysis process a lot of separate files with the same structure. To handle them, we can merge them in a single file with the user-friendly command hadd:

```text
hadd outputfile file1 file2 ... fileN
```

which will automatically merge histograms and tree with the same name.

Alternatively, we can analyze them without merging them, with a TChain:

```text
TChain chain ("treename");
chain.Add("file0.root"); //each file contains a TTree called treename
chain.Add("file1.root");
TTreeReader myreader(&chain);
```

To merge instead TXT or CSV files, simply use the linux built-in command **cat:**

```text
cat file1.csv file2.csv > allfiles.csv
```

## Syncronizing

Without copying symbolic links:

```text
rsync --progress -avhe ssh /usr/local/  XXX.XXX.XXX.XXX:/BackUp/usr/local/
```

