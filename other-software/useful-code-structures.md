---
description: Commonly searched for
---

# Useful code structures

I will report here both the C++ and the python equivalent, when different

## ROOT

### Quickly converting numpy arrays to ROOT TTrees

When working with a python numpy script, often we want to store the numbers in a ROOT TTree for easy sharing. Before, it was done with external libraries, such as uproot, rootpy, etc. 

Now ROOT itself supports an user-friendly conversion, by passing through a RDataframe:

`energydata = {"Erec": Ehad_rec, "Etrue": y_test, "HitsTreeEntry": entrylist}`

`energydf = ROOT.RDF.MakeNumpyDataFrame(energydata) energydf.Snapshot("energytree","energyfile.root")`

### Plotting numpy arrays in ROOT histograms

Another common conversion problem is plotting scipy variables into ROOT TH1, TGraph, etc. usually for fitting, or sharing issues. 

The most straightforward way to do this is to use the **root\_numpy** library, which implements general functions, to fill histograms, profile histograms, and graphs. See for example 

{% embed url="http://scikit-hep.org/root\_numpy/reference/generated/root\_numpy.fill\_hist.htm" %}

Note: for more than 1D plots, variables need to have the proper dimension. Remember how to increase dimension in python:

```text
x = x[:, np.newsize]
y = y[:, np.newsize]
xy = np.concatenate([x,y], axis=1)
root_numpy.fill_hist(h,xy)
```

### Formatting variables to string

Commonly used for nice printouts. We pass variables, with information about decimals and scientific notations. %d for integer, %.2f for floats with 2 decimals, %.2e etc

* Python works in a fantastic way with `format` : here you can just write your string and a format, like `'testing with {:.2f}'.format(a)`. Now also directly with fstrings \(but more strange to use\)
* C++ ROOT has a `TString::Format` which works similarly,  but with % symbols, like TString::Format\("testing with %.2f",a\). Remember that a TString needs to be re-converted to chars with Data\(\), while for a std::string it is data\(\)

### Numerical containers

Used to perform easily selections and operations:

* RVec containers are ROOT equivalent of numpy collections: `ROOT::VecOps::RVec v = {2,2,3}`  and can be used like v\[v&lt;2\]
* RDataFrame as equivalent of pandas dataframe

### TTree Indexing

It is often useful to sort a TTree according to a particular branch, instead of using the default entries. Luckily, there is a builtin function to do that:  **BuildIndex**

mytree-&gt;BuildIndex\("mybranch"\) creates an index according to mybranch, which identifies in a unique way an entry \(if that is not a case, a minor second branch can be added\). To the get the entry, then use mytree-&gt;GetEntryWithIndex\("mybranch"\)

### TEfficiency

The recommended way to produce a plot with the acceptances from the ratio of two histograms. Instead of the more intuitive TH1::Divide, this class automatically computes the errors in the expected binomial way. It is built with using:

```text
TEfficiency *peff = new TEfficiency(&hpassed, &hall)
```

ROOT reference guide recommends to check consistency between histograms when using this class. All details can be found there \(I want to have this page as clear as possible, just as a reminder of class names to look them in the ROOT reference\).

## BASH

### Loops

Bash uses a list loop, like for example:



```text
for VARIABLE in file1 file2 file3
do
	command1 on $VARIABLE
	command2
	commandN
done
```

a numerical loop can be done with seq, which uses the syntax FIRST INCREMENT LAST \(see man seq\):



```text
for alpha in $(seq 0.02 0.01 0.08)
do
 echo $alpha
done
```



