---
description: List of selections
---

# Selections for Matching with Pixel from Chris

## Track selection

Tracks belong from "good vertices", according to BDT file provided by Valerio. Good vertices are identified by \*\* \(1 - bdtvalue\)/ntracks &lt; 0.05\*\*

1. 2 &lt;= Nseg &lt;= 28 \(no fully penetrating tracks\) 

2. -150 mrad &lt; tx,ty &lt; 150 mrad 

3. Track must have a segment in the last emulsion layer 

4. Additionally, the initial angles are shifted \(to center the beam at 0\) by tx = tx0 - 4.64 mrad, ty = ty0 + 2.15 mrad

### Script to check number of input tracks

[https://github.com/antonioiuliano2/macros-ship/blob/master/analisi\_charmdata/matchedtracks.C](https://github.com/antonioiuliano2/macros-ship/blob/master/analisi_charmdata/matchedtracks.C)



