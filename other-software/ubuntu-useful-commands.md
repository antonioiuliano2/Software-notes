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

Note: A trailing slash on a source path means "copy the contents of this directory". Without a trailing slash it means "copy the directory".

```text
rsync --progress -avhe ssh /usr/local/  XXX.XXX.XXX.XXX:/BackUp/usr/local/
```

## Mounting directory

Add source path and target path within **/etc/fstab**

Create an empty folder in target path. Then, from root launch 

```text
mount -a
```

## Changing permissions

Usually, you can add all permissions of a folder and subfolders to everyone by launching

```text
chmod -R ugo+rwx /path/to/folder
```

"r" is reading permissions, "x" is execution and "w" is writing \(i.e. changing files or creating new ones\).

"u" is the users who owns the files, "g" is his group and "o" are all the other users. For example, to remove writing permission to all users outside the file owner's group:

```text
chmod -R o-w /path/to/folder/
```



