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

## Prerequisites for the upgrade

Before proceeding to the Operating System upgrade we need to perform the following:

* If you have follow the workshop you will have two RHEL for Edge images.
* You need to get the **ostree-commit** value for the two images you have created. So you will have to modify the playbook [rhel_edge_upgrade.yaml](ansible/rhel_edge_upgrade.yaml) setting the **imageIDOrig** variable with the **ostree-commit** value for the first image an the **imageIDUpdated** variable with the value for the second image, the one which has the **strace** RPM package installed.
* Go to the RHEL 8 Server and perform the following steps to make the upgraded image available for the upgrade:
  ```console
  [root@rhel8edge edgeimage]# composer-cli compose list
  3096e960-2019-4575-ad4d-f3960026ec6b FINISHED edgeserver 0.0.2 edge-commit
  5218497e-1e47-4b78-864d-691280fc65f9 FINISHED edgeserver 0.0.1 edge-commit
  [root@rhel8edge edgeimage]# composer-cli compose image 3096e960-2019-4575-ad4d-f3960026ec6b
  ...
  [root@rhel8edge edgeimage]# rm -Rf /var/www/html/ostree/*
  [root@rhel8edge edgeimage]# tar xf 3096e960-2019-4575-ad4d-f3960026ec6b-commit.tar -C /var/www/html/ostree/ 
  [root@rhel8edge edgeimage]# 
  ```

## How the upgrade process and rollback has been implemented

The RHEL for Edge server is configured in such way that when files **/etc/greenboot/orig.txt** and **/etc/greenboot/current.txt** have the same content the upgrade will be considered as failed:

* **/etc/greenboot/check/required.d/01_check_upgrade.sh** check if file **/etc/greenboot/orig.txt** exists, if not the file is created with the **ostree-commit** for the running version. **/etc/greenboot/current.txt** is overwritten with the **ostree-commit** for the running version.

  > ![INFORMATION](icons/information-icon.png) to allow the upgrade the file **/etc/greenboot/orig.txt** needs to be deleted.

* **//usr/lib/systemd/system/rpm-ostreed-automatic.timer** systemd timer to periodically start the `rpm-ostreed-automatic.service` systemd  unit.
* **//usr/lib/systemd/system/rpm-ostreed-automatic.service** systemd unit which downloads the new upgrade for the Operating System.
* **/etc/systemd/system/applyupdate.timer** systemd timer to periodically start the `applyupdate.service` systemd unit.
* **/etc/systemd/system/applyupdate.service** systemd unit which applies the upgrade and reboots the RHEL for Edge server.

> ![INFORMATION](icons/information-icon.png) this is only for demo purposes to illustrate how upgrade/rollback are performed.. It is not inteded to be used in production.
