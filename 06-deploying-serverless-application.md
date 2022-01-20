# Deploying a serverless application

An Operating System with no application is useless.

We are going to deploy a containerized application on top of the RHEL for Edge.

## Checking the RHEL for Edge server

When we deployed the RHEL for Edge we also deployed a containerized web application. This application is published in a non-secure registry in the RHEL 8 Server. If you connect to that server:

```console
[root@rhel8edge ~]# podman images
REPOSITORY                           TAG         IMAGE ID      CREATED       SIZE
192.168.1.222:5000/httpd             v2          0a12c9ecd326  28 hours ago  423 MB
192.168.1.222:5000/httpd             v1          78bfb437998c  28 hours ago  423 MB
192.168.1.222:5000/httpd             prod        78bfb437998c  28 hours ago  423 MB
<none>                               <none>      48d8ad8b2275  28 hours ago  423 MB
registry.access.redhat.com/ubi8/ubi  latest      fca12da1dc30  5 weeks ago   235 MB
docker.io/library/registry           2           b8604a3fe854  2 months ago  26.8 MB
[root@rhel8edge ~]#
```

When we executed the playbook to configure the RHEL 8 Server all this containers were created, the registre configured and the container published.

In the kickstart used to deploy the RHEL for Edge server a serverless container was configured to run in the Edge server.

You can connect to the server using ssh and check that there is no container running:

```console
[core@rheledge ~]$ podman container list
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
[core@rheledge ~]$ 
```

The container has been deployed as serverless, this means that the container is not started and will be started when someone request the service.

```console
```