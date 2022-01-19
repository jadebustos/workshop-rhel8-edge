# Workshop RHEL for the Edge

This workshop is based in [https://github.com/RedHatGov/RFESummit2021](https://github.com/RedHatGov/RFESummit2021).

This workshop is an introductory workshop to show how to use RHEL **Image Builder** feature to build, deploy and manage RHEL for Edge deployments.

> ![IMPORTANT](icons/important-icon.png) This workshop has been tested using Fedora 35 as hypervisor and ansible controller node.

> ![IMPORTANT](icons/important-icon.png) All the playbooks should run with no problems on Fedora 35, RHEL 8 and CentOS Stream 8. Maybe some minor changes will have to be done for repositories if CentOS Stream 8 is used. The key words here are **should** and **Maybe**. Remember that your definition of **minor** maybe it is not the same that mine :-P.

## Workshop

You will need the following:

* An ansible controller node to deploy the workshop. (Your laptop for instance)
* A hypervisor, this workshop assumes that you use KVM. (Your laptop for instance)
* A RHEL 8 Virtual Machine.

1. [Prepare the RHEL server](01-requirements-rhel-server.md)
2. [Create a RHEL for Edge image](02-create-image.md)
3. [How to upgrade a RHEL for Edge Image](03-image-upgrade.md)
4. [Prepare the hypervisor](04-requirements-hypervisor.md)