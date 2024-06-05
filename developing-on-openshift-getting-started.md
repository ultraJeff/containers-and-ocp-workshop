# Getting Started Developing on OpenShift

## Working with the Command Line Interface (CLI)

You can access the Red Hat OpenShift CLI with the command `oc`. Using the `oc` command lets you work with the entire OpenShift cluster and deploy new applications.

The CLI exposes the underlying Kubernetes orchestration system with the enhancements made by OpenShift. Users familiar with Kubernetes will be able to adapt to OpenShift quickly.

`oc` provides all of the functionality of the Kubernetes `kubectl` CLI tool. In addition, `oc` makes it easier to work with OpenShift. The CLI is ideal in situations where you are:

* Working directly with project source code
* Scripting OpenShift operations
* Cannot use the web console because of resource bandwidth restrictions

Let's start by logging in with `oc` for your first experience with this CLI.

## Logging in with the CLI
You log into OpenShift from the command line using the `oc login` command.

----

`Step 1:` Run the following command in the terminal window to the left:

```
oc login -u admin -p admin https://api.crc.testing:6443 --insecure-skip-tls-verify=true
```

Upon successful login, you'll get results similar to the following:

```
Login successful.

You have access to 64 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
```

Running the Linux [`whoami`](https://en.wikipedia.org/wiki/Whoami) command reports the current user and implicitly confirms that the login is successful.

----

`Step 2:` Run the following command:

```
oc whoami
```

You'll get the following results:

```
admin
```

## Congratulations!

 You've logged in to OpenShift using the `oc` command line tool.

----

This topic focuses on learning how to log into the Red Hat OpenShift web console and then create an application once logged in.

## Logging in with the web console

Your first task is to log into OpenShift from the web console.

----

`Step 1:`
Click the **Web Console** tab from the horizontal menu bar over the terminal to the left to open the OpenShift web console.

You will be presented with the OpenShift login screen. Use the following credentials to log in.
* **Username:** `admin`
* **Password:** `admin`

Log into the OpenShift web console, as shown in the figure below.

![Web Console Login](../assets/web-console-login.png)

|NOTE:|
|----|
|You might see the following warning notification due to using an untrusted security certificate.
![Security warning](../assets/security_warning.png)
If you do get the warning, click the **Advanced** button to complete the process necessary to grant permission to the browser to access the OpenShift Web Console.|

After logging into the web console, take a look a the menu on the left. Notice that you're in the **Administrator** perspective, as shown in the figure below:

![Admin Perspective](../assets/admin-perspective.png)

You need to change the perspective from **Administrator** to **Developer**.

----

`Step 2:` Select the **Developer** perspective from the dropdown on the left side menu in the OpenShift web console as shown in the figure below.

![Switch to Developer](../assets/change-to-developer.png)

Now that you're in the **Developer** perspective, let's take a moment to discuss the concept of a **project** in OpenShift.

## Understanding projects in OpenShift

OpenShift is often referred to as a container application platform in that it's a platform designed for the development and deployment of applications in [Linux containers](https://developers.redhat.com/topics/containers).

OpenShift has an organizational unit named **project**. You use a **project** to group resources in your application. The reason for organizing your application in a **project** is to enable controlled access and quotas for developers or teams.

You can think of a **project** as a visualization of the Kubernetes namespace based on the developer access controls.

Now, let's create a project.

## Creating a project

In this section you will create a project using the OpenShift web console.

----

`Step 3:` Click the **Web Console** tab from the horizontal menu bar over the terminal to the left to open the OpenShift Web Console.

Click the button labeled **+Add** on the menu bar on the left side of the Web Console. The **Add** web page appears.

----

`Step 4:` Click the link with the text **Create a project** as shown in the figure below:

![Create project](../assets/add_project.png)

You'll be presented with the **Add Project** dialog for declaring the project.

----

`Step 5:` Name the project `myproject` as shown in the figure below:

![Name project](../assets/config-project.png)

----

`Step 6:` Then click the button labeled `Create` as shown in the figure above.

You will be presented with the **Add** page as shown in the figure below.

![Select container](../assets/select-container-with-code.png)

----

`Step 7:` Scroll down the page and click the text block labeled `Container images` as shown in the figure above.


You'll be presented with the **Deploy Image** page as shown in the figure below.

![Deploy Image](../assets/deploy-container.png)

----

`Step 8:` Enter the text below in the text box labeled **Image name from external registry**

```
quay.io/openshiftroadshow/parksmap:1.3.0
```

Adding content to the text boxes in the rest of the form is optional.

----

`Step 9:` Scroll down the page and click the button labeled `Create` to continue.

After you create the application, you will be presented with the **Topology** view as shown in the figure below.

You will spend most of your time in the remainder of this tutorial in that perspective.

![Topology View with App](../assets/topology-view-with-app.png)

You are now ready to scale the application up and down.

## Congratulations!

 You deployed an application from a container image using the OpenShift web console.

----

In this step, you will learn how to scale your application up by creating multiple replicas of the pod that represents the application in the cluster. Red Hat OpenShift will make it look like all of the replicas are a single application. But, behind the scenes, OpenShift routes traffic among the replicas automatically.

The benefit of replicating a pod is that it increases the amount of Internet traffic the application can accommodate. The result is better overall performance.

## Scaling up the application

You will scale up the application up to 2 replicas.

----

`Step 1:` Click inside the circle for the `parksmap` application you just installed within the **Topology** view. The details window will appear in a side panel as shown in the figure below.

![Scaling Up 1](../assets/scaling-01.png)

----

`Step 2:` In the side panel, click the **Details** tab, and then click the up arrow next to the pod to increase the number of pods to 2. (The word [`pod` is a Kubernetes term](https://kubernetes.io/docs/concepts/workloads/pods/). In this case, a `pod` refers to an instance of the application.)

The figure below illustrates selecting the **Details** and increasing the number of pods to 2.

![Scaling Up 2](../assets/scaling-02.png)

You are now running two replicas of the application.

## Verifying the number of replicas

----

`Step 3:` To verify that we changed the number of replicas, click the **Resources** tab in the side panel. You will see a list of pods as shown in the figure below.

![View Pods](../assets/view-pods.jpg)

You can see that we now have 2 replicas. Now, let's take a look at OpenShift's self-healing feature. Self Healing ensures that the number of pods you declare for your application is always running.

# Understanding application "self healing"

Under the OpenShift application **Deployments** are constantly monitored to see that the desired number of pods are actually running. Therefore, if the actual state ever deviates from the desired state--for example, if a pod goes down--OpenShift will fix the situation.

Now you will kill one of the pods to see if OpenShift will bring it back to life.

----

`Step 4:` On the **Resources** tab where you viewed the list of pods after scaling to 2 replicas, click the name of one of the pods in the list as shown in the figure below.

![View pods](../assets/view-pods.jpg)

----

`Step 5:` In the top right corner of the page, there is an **Actions** drop down menu. Click it and select **Delete Pod** as shown in the figure below.

![Delete Pod](../assets//delete-pod.png)

After deleting the pod, you will be taken to a page listing pods. Notice that even though you deleted one pod, two pods are still running, as shown in the figure below.

![Replenish Pod](../assets/replenish-pod.jpg)

A replacement pod was created because OpenShift will always make sure that when a pod dies, it creates a new pod to fill its place.

# Scaling down an application

Before we continue, go ahead and scale your application down to a single instance.

----

`Step 6a:` Click the **Topology** item on the left side menu.

`Step 6b:` Click the `parksmap` circle.

`Step 6c:` Click the **Details** tab in the side panel.

`Step 6d:` Click the down arrow to the right of the pod circle to reset the number of pods to 1 as shown in the figure below.

![Reset pod count](../assets/reset-pod-count.jpg)

## Congratulations!

 You've learned how to use the OpenShift web console to scale the number of replica pods for an application up and down.

----

In this step you will explore **routes**.

In Red Hat OpenShift, the **service** resource provides the internal abstraction that binds access to an application to its logic, which is represented in an application's pods. Also, a **service** provides load-balancing capabilities within an OpenShift environment.

The binding of a **service** to an application's pods is internal to an OpenShift cluster. The way that external clients access applications running in OpenShift is through the OpenShift routing layer.

The formal name for the resource which represents the routing layer is called a **route**.

As mentioned earlier in the tutorial, When you create an application from a **container image**, OpenShift creates a **route** for the application automatically. Let's take a look at where the **route** is published. Then, let's access the application's website from a browser using its route URL.

## Viewing a route's URL


`Step 1:` To view the **route** for your application, make sure you are in the **Developer** perspective by selecting **Developer** from the dropdown list on the left side menu bar. Then from within **Topology** view, click the application circle as shown in the figure below.

![Select App](../assets/select-app.png)

The information page for the pods will slide out as a panel on the right-hand side of the page.

----

`Step 2:` Click the **Resource** tab in the side panel.

----

`Step 3:` Scroll down to the **Routes** section at the bottom of the web page as shown figure below.

![Select Route](../assets//select-route.png)

The route URL for your application is shown as a link.

----

`Step 4:` Click that link to load the application's web page in your browser.

## Accessing a Route's URL

You can also access an application's **route** URL directly within the application's circular graphic in the **Topology** page.

----

`Step 5:` Click the icon in the upper right of the application's circular graphic as shown in the figure below.

![Open from topology](../assets/access-url-from-topology.png)

Clicking the icon opens the application's web page in a browser as shown in th figure below.

![View map](../assets/view-map.png)

----

## Congratulations!

 You have now located the application's **route** URL in the OpenShift web console. Also, you've learned how to open the application in a web page from the **Resource** tab in the application detail, and also directly from the application's circular graphic in the **Topology** view.

This is the final step in this track.

## What's Next?

Congratulations on completing this lab. Keep learning about OpenShift:

* Visit the [Red Hat Developer learning page](https://developers.redhat.com/learn) for more labs and resources
* [Want to try a free, instant 30-day OpenShift cluster? Get started with the Developer Sandbox for Red Hat OpenShift](https://developers.redhat.com/developer-sandbox)
