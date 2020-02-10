---
description: Never really got the hang of it
---

# Vertexing basics

There is a do\_vertex\(\) call, which uses some parameters:

namespace VERTEX\_PAR { float DZmax = 3000.; // maximum z-gap in the track-vertex group float ProbMinV = 0.0001; // minimum acceptable probability for chi2-distance between tracks float ImpMax = 15.; // maximal acceptable impact parameter [microns](for%20preliminary%20check) bool UseMom = false; // use or not track momentum for vertex calculations bool UseSegPar = true; // use only the nearest measured segments for vertex fit \(as Neuchatel\) int QualityMode= 0; // vertex quality estimation method \(0:=Prob/\(sigVX^2+sigVY^2\); 1:= inverse average track-vertex distance\) }

the do\_vertex simply loads these parameters in a EdbVertexRec object \(defined in the EdbVertex.h class\), which then starts finding the 2-tracks vertices, with gEVR-&gt;FindVertex\(\);

Then it will add tracks to these 2 tracks-vertices, with a ProbVertexN\(\) method.

Let's see what it does:

## FindVertex\(\)

First it looks for tracks starts and ends \(Note: EdbTrackP is defined in EdbPatternh\), with void EdbVertexRec::FillTracksStartEnd\(TIndexCell &starts, TIndexCell &ends \) this justs gets the start and the end of the track

Then the 'fun' part starts -&gt; LoopVertex\(\)! It will look for three types of vertex: begin begin, end begin and end end

It will check that z2 &gt; z1 \(sorting to avoid compare the same tracks twices\) and that z2 - z1 &lt;= DZMax Then it will use TrackP pointers to access the TrackP objects Ok, so far, so good

Do we have a vertex? Ask ProbVertex2\(tr1, tr2, zpos1, zpos2\)

If we have a vertex, this function will return the pointer to the vertex If not, it will return 0

Note: it requires a lot of time, why? Check if is ProbVertex or the combination before -&gt; use  clock\(\)

