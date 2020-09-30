---
description: Connecting to QUASAR machines
---

# PulseUI

## Download and setup \(Ubuntu 64 bit\)

First download the client package, then install it:

```
$ sudo dpkg -i pulse-5.3R7.x86_64.deb
```

{% hint style="info" %}
It may not find some packages, in that case please install them with

/usr/local/pulse/PulseClient\_x86\_64.sh install\_depending\_packages
{% endhint %}

Then if "pulse" does not work, set the alias manually in $HOME/.bashrc:

```bash
alias pulse = 'usr/local/pulse/pulseUI'
```



