# Containers and OpenShift Workshop

Welcome to this workshop on containers and OpenShift! Today, we will use a hosted OCP environment provided by your instructor, along with these instructions, as we journey from simple Linux containers to advanced container orchestration using Red Hat OpenShift.

Let's begin by logging into the environment provided by your instructors and going to OpenShift Dev Spaces.

![Web Console Login](./assets/developing-on-openshift-getting-started/assets/web-console-login.png)

|NOTE:|
|----|
|You might see the following warning notification due to using an untrusted security certificate.
![Security warning](./assets/developing-on-openshift-getting-started/assets/security_warning.png)
If you do get the warning, click the **Advanced** button to complete the process necessary to grant permission to the browser to access the OpenShift Web Console.|

Now click the menu button in the OpenShift console header and select **OpenShift Dev Spaces**

![Dev Spaces Menu Item](./assets/images/ocp-to-dev-spaces.png)

From here, create a new blank workspace and open the terminal

![Dev Spaces Blank Workspace](./assets/images/empty-workspace.png)

This will take a minute or two to load since this is the first time you are opening this workspace

![Dev Spaces Loading](./assets/images/starting-empty-workspace.png)

Once your workspace loads, choose *Trust the Authors*

![Dev Spaces Trust the Authors](./assets/images/trust-the-authors.png)

And finally, open the terminal by clicking the hamburger menu icon on the top left

![Dev Spaces Open the Terminal](./assets/images/terminal-2.png)

Now, you are ready to begin the first workshop module. Continue with [Container Fundamentals](/container-fundamentals.md) now.
