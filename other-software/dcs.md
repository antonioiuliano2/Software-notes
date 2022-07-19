---
description: More I/O business
---

# DCS

## Motivation

Temperature and humidity sensors provide their data through JSON strings via MQTT protocol to the server. There are two messages, "Alert" and "Monitoring"

At the same time, the servers can set thresholds and send in general messages with the board to JSON.

Thus, naturally the DCS server needs to communicate with json, through c++ software.

The repository is [https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem](https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem)

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

C++/Python Library to connect to the MQTT server. Our server can be accessed from my aiulian lxplus account with [https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem/-/blob/master/python/testsndmqtt\_connection.py](https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem/-/blob/master/python/testsndmqtt\_connection.py), by modifying the on\_message function

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

## Online monitoring

### Storage of sensor data:

Online monitoring is performed via tmux screens on lxplus.

Important: writing permissions last 1 day. Once a day (twice, for safety), you should reconnect with kinit and your CERN credentials to the same lxplus you used with tmux (example. lxplus761);

To launch a new monitoring session:

* create a tmux session with tmux new -s dcsmonitorgraphs;
* enter sndsw environment;
* go to detectorcontrolsystem/python/ folder;
* Launch python sndmqtt\_startconnection.py
* Detach with ctrl-b d

### Drawing graphs:

Graphs are drawn with ROOT and stored in our webserver folder, accessible from snd-lhc-monitoring.web.cern.ch/dcsgraphsonline.html. To do them:

* create a tmux session with tmux new -s startdrawingdcsgraphs;
* enter sndsw environment;
* go to detectorcontrolsystem/cplusplus\_scripts/ folder;
* Launch source startdrawingmonitorgraphs.sh filename;

Replace filename with the latest monitoring file, for example dcs\_output\_20220719.root

### Updating web index

Last, not forget to add a new link to the index, otherwise people cannot find the link to the new graph. Modify /eos/experiment/sndlhc/www/dcsgraphsonline.html accordingly.

If you do not know how to write the file, please check [https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem/-/blob/master/dcsgraphsonline.html](https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem/-/blob/master/dcsgraphsonline.html) and [https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem/-/blob/master/python/getlinks.py](https://gitlab.cern.ch/sndlhc-daq/detectorcontrolsystem/-/blob/master/python/getlinks.py)

