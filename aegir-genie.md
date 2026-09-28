---
description: Interface between GENIE and the new Aegir interface
---

# Aegir-genie

## Input flux

The input flux uses a custom provider, which can be seen in the official repository. [https://github.com/ShipSoft/aegir-genie/tree/main](https://github.com/ShipSoft/aegir-genie/tree/main)\
\
It can also support the gsimple format for quick tests, and has a demo flux to play around.

### Launch simulations

For now, I am simply following the instructions from the README, so I will simply run the command&#x20;

```
pixi run ./build/gevgen_ship -f nu_flux.root -g ship_geometry.db \
    -x gxspl-ship.xml \
    -n 1000 -o genie_events 
```

Then the output file is converted as usual to rootracker format.

remember to set the tune, if you are using a different GENIE tune, with **--genie** format.

