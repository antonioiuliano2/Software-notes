---
description: More I/O business
---

# DCS

## Motivation

Temperature and humidity sensors provide their data through JSON strings via MQTT protocol to the server. There are two messages, "Alert" and "Monitoring"

At the same time, the servers can set thresholds and send in general messages with the board to JSON.

Thus, naturally the DCS server needs to communicate with json, through c++ software

### JSONCPP

This C++ library allows both reading and writing json strings with c++ variables.

The official github repository: [https://github.com/open-source-parsers/jsoncpp](https://github.com/open-source-parsers/jsoncpp)

For the installation, there is already a Debian package, so I prefer to use this one:

sudo apt-get install libjsoncpp-dev

Only difference, the include. Instead of the default include

```cpp
#include "json/json.h"
```

we need this one:

```cpp
#include <jsoncpp/json/json.h>
```

Standard execution with ROOT then works

### Paho-mqtt

C++/Python Library to connect to the MQTT server. Our server can be accessed from my aiulian lxplus account with the [https://github.com/antonioiuliano2/macros-snd/blob/master/dcs\_monitoring/sndmqtt\_testconnection.py](https://github.com/antonioiuliano2/macros-snd/blob/master/dcs\_monitoring/sndmqtt\_testconnection.py), by modifying the on\_message function

## DCS  Presenter

The monitoring class, it stores the sensor data into a TTree. It is launched from lxplus via

```bash
python pyhon/sndmqtt_startconnection.py
```

and it will access the "monitoring channel", autosaving the TTree every 100 entries (about every 30 minutes). The saved TFile will have the date of creation, i.e. "dcs\_output\_20220421.root"

### Format of dcs tree info

The DCS Presenter store information into a TTree smstree, with the following variables:

* **monitortime:** TDatime, storing the time when the monitor message was stored;
* **coolingon:** A boolean, telling us if cooling system is active
* **temp\[5]**: An array of temperatures (in °C)
* **relhum\[5]:** An array of relative humidities (percentage);
* **smk\[3]:** An array of booleans, will tell us if a smoke sensor has been triggered

TDatime can be easily plotted, by filling a graph or histogram with the output of Convert() call, then the axis can be set as preferred:



```cpp
   mgr.GetXaxis()->SetTimeDisplay(1);
   mgr.GetXaxis()->SetTimeFormat("%Y-%m-%d %H:%M");
```
