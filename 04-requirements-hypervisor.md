# Hypervisor

We are going to configure now the hypervisor to deploy RHEL for the Edge in a virtual machine.

A Fedora/RHEL/CentOS computer which with the capability to create Virtual Machines using KVM is needed.

## Hypervisor configuration

Follow these stesp:

* RHEL 8 iso must be present in the hypervisor. You must configure [hypevisor.yaml](ansible/group_vars/hypevisor.yaml) with the iso path:

  ```yaml
  rhel_iso: '/opt/isos/rhel-8.5-x86_64-dvd.iso'
  ```
* SSH must be running and one user must be configured with passwordless sudo to be used by ansible.
* Edit the [inventory file](ansible/hosts) and under inventory group **hypervisor** replace **192.168.1.200** by your hypervisor's IP. Configure the **ansible_user** accordingly, as well.
* The **/tmp** filesystem must have at least twice of RHEL iso size free.
* On your ansible controller node execute:
  ```console
  $ ansible-playbook -i hosts -l hypervisor prerequisites_hypervisor.yaml
  ```