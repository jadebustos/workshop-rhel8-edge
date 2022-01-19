# RHEL 8 server

You will need the following:

* An ansible controller node to deploy the workshop.
* A virtualization software, this workshop assumes that you use KVM.
* A RHEL 8 Virtual Machine.

Clone this repository:

```console
# git clone git@github.com:jadebustos/workshop-rhel8-edge.git
```

## RHEL 8 server deployment

Deploy a RHEL 8 server as a Virtual Machine and perform the following:

* Register the server and configure the **BaseOS** and **AppStream** repos.
* Configure root user to be used with ansible (public key authentication).
* Edit the [inventory file](ansible/hosts) and under inventory group **rhelserver** replace **192.168.1.222** by your RHEL 8 server's IP.
* Configure the RHEL 8 server executing from your ansible controller node:

  ```console
  $ git clone git@github.com:jadebustos/workshop-rhel8-edge.git
  $ cd workshop-rhel8-edge/ansible
  $ ansible-playbook -i hosts -l rhelserver prerequisites_server.yaml
  ```

This server will be used to:

* Create and publish container images.
* Create and publish the RHEL Edge Images.