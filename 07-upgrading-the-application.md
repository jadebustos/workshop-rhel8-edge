# Upgrading the application

In the RHEL 8 Server we created 2 versions for the containerized application:

```console
[root@rhel8edge ~]# podman images
REPOSITORY                           TAG         IMAGE ID      CREATED       SIZE
192.168.1.222:5000/httpd             v2          55704dd882b9  18 hours ago  424 MB
192.168.1.222:5000/httpd             v1          1a4c99e635e2  18 hours ago  424 MB
192.168.1.222:5000/httpd             prod        1a4c99e635e2  18 hours ago  424 MB
registry.access.redhat.com/ubi8/ubi  latest      fca12da1dc30  5 weeks ago   235 MB
docker.io/library/registry           2           b8604a3fe854  2 months ago  26.8 MB
[root@rhel8edge ~]#
```

We can check that **v1** and **prod** are the same image by their **IMAGE ID**. The **prod** version is the version which was included in the RHEL for Edge installation image that it is deployed using the boot ISO.

Now we are going to upgrade to the **v2** version.

## How the upgrade process works

Two systemd units were defined for the **core** user:

* **/var/home/core/.config/systemd/user/podman-auto-update.timer**
* **/var/home/core/.config/systemd/user/podman-auto-update.service**

![APPv2](imgs/appv2.png)
