---
description: List of selections
---

# Selections for Matching with Pixel from Christopher Betancourt

## Track selection

Tracks belong from "good vertices", according to BDT file provided by Valerio. Good vertices are identified by \*\* (1 - bdtvalue)/ntracks < 0.05\*\*

1\. 2 <= Nseg <= 28 (no fully penetrating tracks)&#x20;

2\. -150 mrad < tx,ty < 150 mrad&#x20;

3\. Track must have a segment in the last emulsion layer&#x20;

4\. Additionally, the initial angles are shifted (to center the beam at 0) by tx = tx0 - 4.64 mrad, ty = ty0 + 2.15 mrad

### Script to check number of input tracks

[https://github.com/antonioiuliano2/macros-ship/blob/master/analisi\_charmdata/matchedtracks.C](https://github.com/antonioiuliano2/macros-ship/blob/master/analisi\_charmdata/matchedtracks.C)

