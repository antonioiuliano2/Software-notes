---
description: Emulsion reconstruction software
---

# FEDRA

## Download and installation:

1. svn co [http://emulsion.na.infn.it/svn/fedra/trunk](http://emulsion.na.infn.it/svn/fedra/trunk) fedra
2. cd fedra
3. &#x20;source install.sh

## Configuration

4\. source setup\_new.sh

5\. add a line to $HOME/.rootrc with the following:

```
Unix.*.Root.MacroPath:      .:$(FEDRA_ROOT)/macros
```

&#x20;6\. cp src/\*/\*.pcm lib&#x20;

7\. cp src/\*/\*/\*.pcm lib

8\. cp macros/rootlogon\_root6x.C macros/rootlogon.C

9\. cp fedra/macros\_root6/\*.C fedra/macros

## Additional info

For more detail, check the wiki page:

[http://operaweb.lngs.infn.it/operawiki/index.php/How\_to\_Install\_FEDRA](http://operaweb.lngs.infn.it/operawiki/index.php/How\_to\_Install\_FEDRA)
