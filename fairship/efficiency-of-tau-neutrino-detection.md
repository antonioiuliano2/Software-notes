---
description: Performance of Scattering and Neutrino Detector from simulation
---

# Efficiency of tau neutrino detection

## Introduction

This section is for notes regarding the neutrino efficiencies, computed from:

{% embed url="https://github.com/antonioiuliano2/macros-ship/blob/master/nu_ship_simulations/nutau_event.C" %}

This code estimates both efficiency of tau neutrino CCDIS detection (if tausim is true), and rejection of the background from numu CharmCCDIS interactions (if tausim is false). Both  tau neutrinos and antineutrinos are included.&#x20;

Events are only considered for this code if they happen within target acceptance. See the function NeutrinoVertexCoordinates() and the cut at dx less than 40.5 and dy less than 40.5;

## Description of efficiencies

All the efficiencies are defined with the weighted events, with take into account the mean density of the neutrino path within SND detector. However, the efficiency error considers the actual number of events, not weighted.

### Geometrical efficiency

This defines a fiducial area where interactions can be searched for. The function is GeometricalEfficiency(TVector3 Vn, double offsetxy, int Nminplates, int \&InteractionWall);

A border of 1 mm at each transverse side is excluded, and the vertex should be located at least 4 emulsion plates before the end of the brick, included the emulsion at the end of the brick.

### Primary vertex location

This selection defines the visibility of the primary neutrino vertex (the interaction vertex).

Tracks are considered visible at the primary vertex if they have momentum larger than 1 GeV and tangent of the angle less than 1.&#x20;

At least one track, other than the tau lepton, needs to be visible.

### Decay search

This selection efficiency, and all the other ones after this, are separated according to the decay channel of the tau lepton (1mu, 1e, 1h, 3h). The decay channel is identified by the function DecayChannel(vector \&daughters,TTreeReaderArray \&tracks, int nnue, int nnumu, int ievent)

The function is TauDecay(int trackID, int tauid, const ShipMCTrack \&track, TVector3 Vn, double tautx, double tauty, double& taudecaylength)

Tau decay daughters are accepted as visible if:

* Decay length is larger than 4 mm;
* Kink angle is larger than 0.02 rad;
* Impact Parameter is less than 10 micron;
* Momentum is larger than 0.1 GeV;
* Tangent of angle is less than 1;

All these contions are required for at least one track in order for the decay to be accepted, for all decay topologies (a 3h can be accepted even if only 1 hadron is accepted).

### Charge identification

This efficiency is not used for the tau->1e decay channel.

A loop is performed on both Downstream Trackers (DT) and Compact Emulsion Spectrometer (CES) hits. The CES hits must come from the same wall of the neutrino interaction. Sagitta is then computed from the position of 3 hits (only the first three stations are used).

A smearing is applied on both DT and CES hits, with a spatial resolution of 100 micron for DTs and 1.5 micron for CES. Since the magnetic field is directed along x, the sagitta around x is used to comput the sagitta resolution, which has been found to be 0.0172 cm for DT and 0.0002 cm for CES. The event is accepted if the magnitude of the sagitta around y is larger than three times the resolution.

### Muon identification&#x20;

Of course, this efficiency is defined only for the tau->1mu decay channel.

A loop in ShipRPC points is performed, storing the occupancy of hits as clusters of radius of 2 cm around the muon hit. The event is accepted if at least 4 stations have isolated clusters (i.e. only the muon cluster is found).

## Charm background

As told before, the charm background rejection is computed with the same script of the tau neutrino, with of course some slight differences:

The script looks for the pdg of charmed particles, instead of the tau one:

signalpdgs = {411, 431, 4122, 421, 4132, 4232, 4332, 441};

Events are not separated for decay channels, and they are all treated as the tau->1mu channel. However, the muon which is looked for at the identification step is the primary muon, not the muon from charm decay. Indeed, detecting the primary muon is proof enough that is background, and not signal (assuming the muon to be connected to the primary vertex).

### Phi angle

Recently, phi angle between tracks has been added to provide further separation between tau neutrino signal and background from muon neutrino with charm production, where the primary muon was not detected. The phi angle between the tau/charmed hadron and the hadron system allows to separate the two components, since primary lepton and hadron system are back to back.

&#x20;To exclude the track from the muon in the background case, the track with the largest phi angle must be excluded
