---
description: To avoid googling the same command over and over
---

# 😀 Linux useful commands

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

```
hadd outputfile file1 file2 ... fileN
```

which will automatically merge histograms and tree with the same name.

Alternatively, we can analyze them without merging them, with a TChain:

```
TChain chain ("treename");
chain.Add("file0.root"); //each file contains a TTree called treename
chain.Add("file1.root");
TTreeReader myreader(&chain);
```

To merge instead TXT or CSV files, simply use the linux built-in command **cat:**

```
cat file1.csv file2.csv > allfiles.csv
```

## Syncronizing

Without copying symbolic links:

Note: A trailing slash on a source path means "copy the contents of this directory". Without a trailing slash it means "copy the directory".

```
rsync --progress -avhe ssh /usr/local/  XXX.XXX.XXX.XXX:/BackUp/usr/local/
```

## Mounting directory

Add source path and target path within **/etc/fstab**

Create an empty folder in target path. Then, from root launch&#x20;

```
mount -a
```

## Changing permissions

Usually, you can add all permissions of a folder and subfolders to everyone by launching

```
chmod -R ugo+rwx /path/to/folder
```

"r" is reading permissions, "x" is execution and "w" is writing (i.e. changing files or creating new ones).

"u" is the users who owns the files, "g" is his group and "o" are all the other users. For example, to remove writing permission to all users outside the file owner's group:

```
chmod -R o-w /path/to/folder/
```

## Screening

Useful when connecting to ssh and launching long jobs, or not wanting to lose history of your session.

Use screen or tmux.

Create a tmux session with &#x20;

```
tmux new -s session_name
```

In lxplus, remember to renew permissions with kinit your@CERN.CH -l 24h00m

Detach tmux session with ctrl-b + d

recover your session with tmux a -t 0 (0 being session number)

#### Kerberos notes

To show all your active keys

klist -A&#x20;

To destroy all your active keys

kdestroy -A

#### Find from which folder a screen process is running

In some cases, such as FEDRA scanset reconstruction, the folder a process is running from contains vital information about the process (in this case, the brick currently being analyzed).

In this case, knowing a Process Id (PID, for example from **top**), you can find the current working directory of the process by launching (example for PID 47963:

```
pwdx 47963
```

It will return the working directory of the process

## Opening many files

If you try to have many files opened at once, you get error "Opened many files". You can solve it by ulimit:

```
ulimit -n 10000
```

## Providing output and error logs

Usually, when asking people for help with a bug, we need to provide output file and error file:



```
exec 1>>"$logfile" 2>>"$errorfile"
```



## Time conflicts between Windows and Linux in dual-boot

Ora solare/ora legale: Windows and Ubuntu report one or two hours differences with respect to each other: manually settting one breaks the other.

**Reason**: they use two different time systems, Linux by default uses Universal Time Clock (UTC) and Windows uses Real Time Clock(RTC).&#x20;

**Solution**: set them to use the same system, either UTC or RTC. I changed Linux to use RTC like Windows, following instructions here: [https://wiki.ubuntu-it.org/Installazione/PostInstallazione/ErroreOraWindows](https://wiki.ubuntu-it.org/Installazione/PostInstallazione/ErroreOraWindows)



## Find package directory

Often, you need to provide the paths to an installed package, usually for compilation. However, you installed it with apt-get install, and you have **no idea** where it may be hidden. You can ask directly the package manager where it is located:

```
dpkg -L mypackagename
```

In Python modules, this is done with the attribute&#x20;

```python
coolestpythonmoduleever.__file__
```
