# Requirements

Clone this repository:

```console
# git clone git@github.com:jadebustos/workshop-rhel8-edge.git
```

## RHEL 8 server

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