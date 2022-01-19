# Creating the RHEL for edge image

## Using Cockpit to create the image

Log into your RHEL server at 9090 port using the root credentials, go to **Image Builder** and create a new blueprint:

![](imgs/cockpit-create-blueprint-01.png)

Put a name and a description:

![](imgs/cockpit-create-blueprint-02.png)

Search for the following packages and add them:

* **fuse-overlayfs**
* **setroubleshoot-server**
* **slirp4netns**

![](imgs/cockpit-create-blueprint-03.png)

To save the image modifications pushing the **Commit** button:

![](imgs/cockpit-create-blueprint-04.png)

The image has been saved:

![](imgs/cockpit-create-blueprint-05.png)

We have the blueprint for the image, now we can create the image to be deployed in several enviroments.

Clicking in **Create image** we will start to create the image:

![](imgs/cockpit-create-blueprint-06.png)

In the **Type** section we can select the type of the image we want to create, select **RHEL for Edge Commit (.tar)**:

![](imgs/cockpit-create-blueprint-07.png)

The image will start to create when pushing the **Create** button. We can check the state of the image creation in **Blueprints -> edgeserver -> Images**:

![](imgs/cockpit-create-blueprint-08.png)

It will take several minutes to create the image.

## Playing with the command line

Connect to the RHEL server using SSH.

To see the available blueprints:

```console
[root@rhel8edge ~]# composer-cli blueprints list
edgeserver
[root@rhel8edge ~]# 
```

> ![IMPORTANT](icons/important-icon.png) Blueprints can be created using **composer-cli**.

> ![HOMEWORK](icons/homework-icon.png) Investigate how to use **composer-cli** to create bluprints.

We can check what kind of images can be created:

```console
[root@rhel8edge ~]# composer-cli compose types
ami
ec2
ec2-ha
edge-commit
edge-container
edge-installer
edge-raw-image
edge-simplified-installer
image-installer
openstack
qcow2
tar
vhd
vmdk
[root@rhel8edge ~]#
```

Download the image:

```console
[root@rhel8edge ~]# mkdir edgeimages
[root@rhel8edge ~]# cd edgeimages
[root@rhel8edge edgeimages]# composer-cli compose image 4e9a0fe8-eca4-4de8-8604-21b5c83237c2
4e9a0fe8-eca4-4de8-8604-21b5c83237c2-commit.tar: 780.69 MB    
[root@rhel8edge edgeimages]# 
```

The image is a tar file so we can expand the image and check the metadata:

```console
[root@rhel8edge edgeimage]# tar xf *.tar
[root@rhel8edge edgeimage]# jq '.' compose.json 
{
  "ref": "rhel/8/x86_64/edge",
  "ostree-n-metadata-total": 10313,
  "ostree-n-metadata-written": 3746,
  "ostree-n-content-total": 30395,
  "ostree-n-content-written": 25915,
  "ostree-n-cache-hits": 0,
  "ostree-content-bytes-written": 1797710538,
  "ostree-commit": "073931ab9c68a19b9cf655cdf01ccd427d269d6b9d8d70dd5989d690d90daf5f",
  "ostree-content-checksum": "f47afe115ad3c793d9d3c0ab4f7f4b2472ed8be3c6a88139ea4fa9d832c98fd8",
  "ostree-version": "8.5",
  "ostree-timestamp": "2022-01-18T14:09:33Z",
  "rpm-ostree-inputhash": "13986da6d5b98ba469e5a1ffe51154172758c84bf9c6b350e33392018dc33535"
}
[root@rhel8edge edgeimage]#
```
**ref** can be used to get the rpm intalled in the image:

```console
[root@rhel8edge edgeimage]# rpm-ostree db list rhel/8/x86_64/edge --repo=repo
ostree commit: rhel/8/x86_64/edge (073931ab9c68a19b9cf655cdf01ccd427d269d6b9d8d70dd5989d690d90daf5f)
 ModemManager-1.10.8-4.el8.x86_64
 ModemManager-glib-1.10.8-4.el8.x86_64
 NetworkManager-1:1.32.10-4.el8.x86_64
 NetworkManager-libnm-1:1.32.10-4.el8.x86_64
 NetworkManager-wifi-1:1.32.10-4.el8.x86_64
 NetworkManager-wwan-1:1.32.10-4.el8.x86_64
 acl-2.2.53-1.el8.x86_64
 attr-2.4.48-3.el8.x86_64
 audit-3.0-0.17.20191104git1c2f876.el8.x86_64
 audit-libs-3.0-0.17.20191104git1c2f876.el8.x86_64
 basesystem-11-5.el8.noarch
...
```