---
description: More I/O business
---

# JSON

## Motivation

Temperature and humidity sensors provide their data through JSON strings via MQTT protocol to the server. There are two messages, "Alert" and "Monitoring"

At the same time, the servers can set thresholds and send in general messages with the board to JSON.

Thus, naturally the DCS server needs to communicate with json, through c++ software

## JSONCPP

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
