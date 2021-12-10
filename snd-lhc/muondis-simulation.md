# MuonDIS simulation

Simulation of Deep Inelastic Scattering muons in the detector.&#x20;

The muon here is **present two times.** In order to simulate veto hits, as written in shipgen/MuDISGenerator.cxx:

* in order to have a simulation of possible veto detector hits, let Geant4 follow the muon backward. Due to dE/dx, as soon as the muon travers thick material, this approximation will become bad. a proper implementation however would be to have this better integrated in Geant4, follow the muon, call DIS event, continue.

