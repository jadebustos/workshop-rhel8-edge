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
* Edit the [group_vars/hypervisor.yaml](ansible/group_vars/hypervisor.yaml) file to configure the network for the RHEL for Edge server that will be deployed in the hypervisor using the RHEL for Edge image you have just created:
  ```yaml
  rheledge_hostname: 'rheledge.acme.es'
  rheledge_ip: '192.168.1.134'
  rheledge_netmask: '255.255.255.0'
  rheledge_gw: '192.168.1.1'
  rheledge_dns: '8.8.8.8'
  ```
* On your ansible controller node execute:
  ```console
  $ ansible-playbook -i hosts -l hypervisor prerequisites_hypervisor.yaml
  ```

  This playbook will deploy a virtual machine using the RHEL for Edge image we have created.

> ![IMPORTANT](icons/important-icon.png) If you get the following error **xorriso : FAILURE : Image size XXXXXXXXs exceeds free space on media YYYYYYYYs** check the free space in the **/tmp** filesystem. You will need to have at least twice of the RHEL 8 iso size free.

> ![TIP](icons/tip-icon.png) If we want to speed up the deployment we can connect to the virtual machine console and select the **Install Red Hat Enterprise Linux 8.5**. If not before deploying the server the iso will be checked and this will take more time.
>
> ![BOOT](imgs/rheledgeboot.png)

Once the deployment has finished you can use ssh to connect to the RHEL for Edge server using the ip you configured and the user **core** with password **edge**.