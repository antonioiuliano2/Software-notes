---
description: Input numeric values for reconstruction
---

# Reconstruction used values

## Tracking parameters:

| **Name** | **Value** | Comment |
| :--- | :--- | :--- |
| fedra.readCPcut  | eCHI2P13&&eN1==1&&eN2==1&&s1.eFlag&gt;=0&&s2.eFlag&gt;=0 | Only for data |
| fedra.track.TrZmap  | 2400 0 120000 2000 0 100000 30 |  |
| fedra.track.npass  | 1 |  |
| fedra.track.minPlate |  -999 |  |
| fedra.track.maxPlate |  999 |  |
| fedra.track.refPlate  | 999 |  |
| fedra.track.nsegmin  | 2 |  |
| fedra.track.ngapmax  | 3 \#4 in default options |  |
| fedra.track.DZGapMax |  5000 |  |
| fedra.track.DRmax  | 45 |  |
| fedra.track.DTmax  | 0.07 |  |
| fedra.track.Sigma0 |  5 5 0.003 0.003 | \#FEDRA defaults: 3 3 0.005 0.005 |
| fedra.track.PulsRamp0  | 15 20 |  |
| fedra.track.PulsRamp04  | 15 20 |  |
| fedra.track.Degrad  | 5 | \#FEDRA defaults: 4 |
| fedra.track.probmin  | 0.001 |  |
| fedra.track.momentum  | 2 |  |
| fedra.track.mass  | 0.14 |  |
| fedra.track.do\_use\_mcs  | 0 |  |
| fedra.track.RadX0  | 5810 |  |
| fedra.track.erase  | 0 |  |
| fedra.track.do\_misalign  | 0 |  |
| fedra.track.do\_realign | 0 |  |
| fedra.track.misalign\_offset: | 500 |  |
| fedra.track.do\_local\_corr  | 1 |  |
| fedra.track.do\_comb  | 0 |  |
| emtra.outdir | .. |  |
| emtra.env |  track.rootrc |  |
| emtra.EdbDebugLevel |  1 |  |

## Vertex reconstruction parameters

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Value</th>
      <th style="text-align:left">Comment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">DZmax</td>
      <td style="text-align:left">3000.</td>
      <td style="text-align:left">maximum z-gap in the track-vertex group</td>
    </tr>
    <tr>
      <td style="text-align:left">ProbMinV</td>
      <td style="text-align:left">0.01</td>
      <td style="text-align:left">minimum acceptable probability for chi2-distance between tracks</td>
    </tr>
    <tr>
      <td style="text-align:left">ImpMax</td>
      <td style="text-align:left">15.</td>
      <td style="text-align:left">
        <p></p>
        <p>maximal acceptable impact parameter [microns] (for preliminary check)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">UseMom</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">use or not track momentum for vertex calculations</td>
    </tr>
    <tr>
      <td style="text-align:left">UseSegPar</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">use only the nearest measured segments for vertex fit (as Neuchatel)</td>
    </tr>
    <tr>
      <td style="text-align:left">QualityMode</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">vertex quality estimation method (0:=Prob/(sigVX^2+sigVY^2); 1:= inverse
        average track-vertex distance) }</td>
    </tr>
  </tbody>
</table>