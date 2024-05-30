---
description: VM
---

# Openstack

I will refer here to CERN Virtual Machine Configuration.

Check instances from Compute

Basically follow these guidelines to access your virtual machine from lxplus

[https://clouddocs.web.cern.ch/using\_openstack/keypair\_options.html](https://clouddocs.web.cern.ch/using\_openstack/keypair\_options.html)&#x20;

[https://clouddocs.web.cern.ch/tutorial/create\_your\_openstack\_profile.html](https://clouddocs.web.cern.ch/tutorial/create\_your\_openstack\_profile.html)

Please remember to upload the right ssh key (in my case, **lxplus**), when creating your machine. **IT CANNOT CHANGED** afterwards, without re-creating the instance and the OS from the beginning.

Then you access simply from lxplus:

ssh root@ship-ubuntu2204

or simply

ssh ubuntu@ship-ubuntu2204

It will NOT ask for password, apart from your RSA password

