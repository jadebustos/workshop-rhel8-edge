# Operating System upgrade

We have just seen how to upgrade a containerized application, but the rest of the server was not upgraded.

It is important to upgrade the Operating System due to bug fixes, security vulnerabilities but in the Edge where the devices are not as protected as they could be in the datacenter the upgrade process is quite important.

Other peculiarity in the Edge is that we can have a lot of devices, hundreds, thousands, millions, ... and if the upgrade fails it will not be possible for a technician to get the device to fix it. Obviously physically but remotely as well, if the upgrade failed it is possible that the connectivity with the device is lost.

## Greenboot

RHEL for Edge simplifies Operating System upgrade processes and automates the rollback when the upgrade fails.

Custom healthchecks are used to determine if nodes are properly working, this functionality is known as **Greenboot**.

RHEL for Edge is built using [rpm-ostree](https://coreos.github.io/rpm-ostree/) which makes RHEL for Edge an immutable Operating System. That makes that updates and rollback are atomic and eases the rollback process.

Let's see how Greenboot is configured:

```console
[root@rheledge ~]# tree /etc/greenboot/
/etc/greenboot/
├── check
│   ├── required.d
│   │   ├── 00_required_scripts_start.sh
│   │   └── 01_check_upgrade.sh
│   └── wanted.d
│       └── 00_wanted_scripts_start.sh
├── current.txt
├── green.d
├── orig.txt
└── red.d

5 directories, 5 files
[root@rheledge ~]# 
```

* **/etc/greenboot/check/required.d** all scripts within this directory must be successfully executed to consider the upgrade as successful. If not a rollback will be performed.
* **/etc/greenboot/check/wanted.d** scripts within this directory should be successfully executed to consider the upgrade as successful. If not a rollback will not be performed.
* **/etc/greenboot/green.d** scripts within this directory will be executed as part of a successful upgrade.
* **/etc/greenboot/red.d** scripts within this directory will be executed as part of a failed upgrade (when there is a rollback).