# Vertexing improvement

## Introduction

this work has been performed by Giuliana, in order to:

* study passing-through oxygen tracks
* reattach tracks to vertices

the code missingtrksvtxvt.C reads linked\_tracks.root and vertices.root files, filling 1 mm squared cells with tracks selected according to their angle. The tracks are inserted in dedicated grids, then various loops are performed to associate them to the correct vertices.

The algorithm works well, but it appears to be too slow \(5 days required for the whole tracks file\) What could the possible reasons be?

