# Proton beam

## Efficiency for each spill

This study has been performed to normalize our results directly from the data, instead of  using the Monte Carlo has done until now. It basically amounts to check how many proton tracks are reconstructed for each spill.

The subsample of proton track is defined with the following selection:

" s\[0\].Plate\(\)&lt;3 && s\[0\].Theta\(\)&lt;0.1 && nseg &gt;=5 "

Spills then are separated according to the following lines:

{0, 15, 35, 55, 75, 95} //5 spills

{0,10,20,30,40,50,60,70,80,90} //10 spills

The number of found tracks for each spills is finally normalized to the number of protons from the SPS counter, stored in the ROOT file charm\_spills.root

## Adding proton track to the vertex

After vertex reconstruction, vertices with high number of tracks are considered for parent search: looking for a near long track \(about 3 micron distance\)

