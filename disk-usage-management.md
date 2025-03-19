---
description: Always out of quota
---

# Disk usage management

## Intro

I guess it should be by default by now, but worth reminding:

**WHEN IN DOUBT, DO NOT DELETE**

Only delete files you are sure you will never need again.

Of course, I would never be 100% sure, but useful hints are:



* Test production (prova, oldsims, etc.), never used in actual work;
* Wrong productions (after they have been replaced with a fixed one);
* Old simulations of previous experiment (and again, only the test ones, not the ones actually used for data/MC comparisons)

Never delete actual data (even if may be wrong, deleted data can never be replaced).

### Software for disk management

**du** is the default linux command: use du \* | sort -h to have a quick idea of what is heavy

**baobab: user** friendly, GUI-based. Shows folder occupancy with a pie-chart;

**ncdu**: similar to baobab, but Terminal-based. Useful for ssh-accessed servers (lxplus, nusrv1, etc.)

Both baobab and ncdu allow both to browse and delete files. Again, think before deleting anything **you would regret deleting**

In general, never be over 90% quota occupancy. You may need space for large production in a blink of an eye.
