---
description: ROOT and a Python in a unique environment
---

# Anaconda

## Installing Anaconda

Just download it from the official page and launch the bash commands, as writte in the installation guide there: [https://www.anaconda.com/](https://www.anaconda.com/)

Anaconda gives you a lot of usefuld default packages, miniconda will give you a minimal setup with the conda installer

## Creating a conda environment with ROOT

```
$ conda create --name my-root-env --channel conda-forge-python=3 root
```

This will install ROOT and other default packages from Anaconda.

Then enter your environment with:



```
$ conda activate my-root-env
```

Installed environments can be listed with:

```text
$ conda env list
```

If you want to return to conda base environment, just launch

```
$ conda deactivate
```

### Adding packages

Just use:



```
$ conda install packagename
```





