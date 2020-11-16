---
description: 'Log everything, from everywhere'
---

# ELOG

Software used to log scanning. Main page:

{% embed url="https://elog.psi.ch/elog/index.html" %}

To check what each field means and see how to change configuration file, first **always** check the "Administrator's guide", "elogd.cfg" syntax. I should avoid do changes without checking the documentation first \(remember the toaster man page comic\).

On nusrv9, the elog configuration page is stored in 

/usr/local/elog/elogd.cfg

After changing it, launching startlog will refresh the ELOG.

