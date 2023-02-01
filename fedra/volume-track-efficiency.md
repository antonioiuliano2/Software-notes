# Volume-track efficiency

In order to accurately estimate the particle (muon) rates, it is necessary to correct them for the tracking efficiency.

check\_tr.C does estimate track efficiency, but it is approximate. It is done with (nseg-2)/(npl-2), which actually tells us the average base-track efficiency (i.e., the probability of a plate to have a segment of the track), and then it is plotted with the angle theta.

It does not take into account the possibility of bad films

## Measurement of plate efficiency

The procedure is written in the code:

{% embed url="https://github.com/antonioiuliano2/macros-snd/blob/master/FEDRA/efficiency_study.C" %}

Looping over long tracks (same tracks as used for rate measurement, but a different track set can also be used), plate efficiency is estimated as the ratio between:

* Found segment: simply, fill the histogram with the plate traversed by the volume track;
* Expected segment: we expect that long enough tracks will pass through all segments, so we loop from first to last and add 1.

The efficiency is then computed as the ratio between found and expected, you get a number for each plate

## Estimating volume track efficiency

After knowing the efficiency for each base-track, how to estimate the efficiency of a volume-track, that is, the probability of reconstructing a volume-track with at least k segments in n plates?

First approximation: the sum of all binomial distribution with k successes over n tests. This works pretty well, but it assumes all films have the same probability p. It may or not be true. Also, even if it is coded in ROOT as in TMath::BinomialI(), for large n (that is, many films, more than 12), it actually recommends to use the Incomplete Beta Function.

To take into account films with different efficiency, I have written a custom program here:&#x20;

{% embed url="https://github.com/antonioiuliano2/macros-snd/blob/master/FEDRA/combination_basetracks_efficiencies.C" %}

It basically loops over all the possible combinations of k segments in n films, and it takes as input a graph (saved with the previous program), to prepare an array of probabilities (efficiencies).

For every combination, probability is computed as the factor of efficiency of segments in the combination (found films) and 1 - efficiency of segments not in the combinations (missing segments).

Then the probabilty of all combinations with at least k segments in n films is summed together.
