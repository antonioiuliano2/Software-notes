---
description: To avoid googling the same command over and over
---

# Ubuntu useful commands

## Changing default command

I often need, during installations and builds, to change the gcc, g++,python versions. To do that, there is the update-alternatives command

```
$ update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
```

Then we can choose it with configure

```
update-alternatives --config python
```

