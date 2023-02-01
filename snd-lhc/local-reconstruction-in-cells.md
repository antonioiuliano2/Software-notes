---
description: Or simply local correction? I do not know anymore
---

# Local reconstruction in cells

There are two ways

## Map correction with volume tracks

Studied by Valeri, four plates at the times, correcting and computing

## Mapping together small cells

Cells can be processed in parallel with HTCondor. First sub and sh scripts have been written, but they mostlly use link.rootrc and creates hundreds of files! Not viable in the long terms.

## Overlapping cells

Cells must overlap (about 2.5 mm for 1 cm cell), at least during alignment and tracking!

Otherwise we lose a lot of tracks going from a cell to another!

Passing tracks can be actually used to match overlapping cells together, so the corrections go smootly from a cell to another.
