# Create edge image

Log into your server at 9090 port using the root credentials, go to **Image Builder** and create a new blueprint:

![](imgs/cockpit-create-blueprint-01.png)

Put a name and a description:

![](imgs/cockpit-create-blueprint-02.png)

Search for the following packages and add them:

* **fuse-overlayfs**
* **setroubleshoot-server**
* **slirp4netns**

![](imgs/cockpit-create-blueprint-03.png)

To save the image modifications push the **Commit** button:

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

