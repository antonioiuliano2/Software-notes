---
description: Data, and more data to process
---

# Lab operations

There are operations to do every day, and other once a week.

Scanning and shifter operations are not covered here

## Daily operations

First of all, check that all microscope have enough space to operate.

Convert previous processed plates into ROOT format

Perform linking. Only one film at the time, thank you:

`source linkingloop.sh 19 19`

**ALWAYS PUT BOTH NUMBERS, even if it is the same number**

**After there is no more linking process** perform alignment procedure. Two alignments can be performed in parallel, but they should be from different bricks (only one .set.root file can exist at the time)

`source doallalign.sh 19 13`

Be in close contact with shifters, to know what is happenning

## Weekly operations

At least once (better twice) a week, check quality of films:

raw data (thickness AND check\_raw.C);

linked reports;

alignment reports;

(Use convert bash script to convert into png for easier access. Original ps files can then be deleted to save space)

Fill the google document table with the quality report:

**Synchronize** data to EOS, with rsync (both raw and reconstructed data).

For raw data (from snd2na@nusrv1.na.infn.it):

* source rsync\_cern\_nomount.sh

For reconstructed data:

* cd /home/scanner/sndlhc/RUN1
* source rsync\_emureco\_nomount\_21.sh
* source rsync\_emureco\_nomount\_24.sh&#x20;



