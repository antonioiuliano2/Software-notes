---
description: I am tired of always googling them
---

# Latex tricks

## SI units

170 micron, etc. are always difficult to format. Luckily the package [siunitx](https://ctan.org/pkg/siunitx) does it for us:



```
\SI{1.55}{\micro\metre}
//the manual now recommends \qty, instead of \SI. It works similarly:
\qty{1.55}{\micro\metre}
```

