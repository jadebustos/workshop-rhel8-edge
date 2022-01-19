# Requirements

You will need the following:

* An ansible controller node to deploy the workshop.
* A virtualization software, this workshop assumes that you use KVM.
* A RHEL 8 Virtual Machine.

Clone this repository:

```console
# git clone git@github.com:jadebustos/workshop-rhel8-edge.git
```

## Hypervisor

A Fedora/RHEL/CentOS computer which with the capability to create Virtual Machines using KVM:

* RHEL 8 iso must be present in the hypervisor. You must configure [hypevisor.yaml](ansible/group_vars/hypevisor.yaml) with the iso path:

  ```yaml
  rhel_iso: '/opt/isos/rhel-8.5-x86_64-dvd.iso'
  ```
* SSH must be running and one user must be configured with passwordless sudo to be used with ansible.
* Edit the [inventory file](ansible/hosts) and under inventory group **hypervisor** replace **192.168.1.200** by your hypervisor's IP. Configure the **ansible_user** accordingly, as well.

## RHEL 8 server deployment

Deploy a RHEL 8 server and perform the following:

* Register the server and configure the **BaseOS** and **AppStream** repos.
* Configure root user to be used with ansible.
* Edit the [inventory file](ansible/hosts) and under inventory group **rhelserver** replace **192.168.1.222** by your server's IP.
* Configure the server and from your ansible controller node:

  ```console
  # git clone git@github.com:jadebustos/workshop-rhel8-edge.git
  # cd workshop-rhel8-edge/ansible
  # ansible-playbook -i hosts -l rhelserver prerequisites_server.yaml
  ```

This server will be used to:

* Create container images.
* Create the RHEL Edge Images.

## Hypervisor

A Fedora/RHEL/CentOS server which