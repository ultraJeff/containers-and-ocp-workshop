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

## Workshop within a Workshop

## Understand Container Orchestration

### Multi-Container Workloads: The classic two-tiered, wordpress application

The goal of this exercise is to build a containerized two tier application in an OpenShift cluster. This application will help you learn about clustered containers and distributed systems. It will teach you about Kubernetes and how it operates with the principles of "defined state" and "actual state" - it constantly monitors the environment and attempts to make the actual state match the defined state.

In Kubernetes/OpenShift, applications are defined with either JSON or YAML files - either file format can be imported or exported, even converting between the two. In this lab, we will use YAML files.

Essentially, the application definition files are a collection of software defined objects in Kubernetes. The objects in the file are imported and become defined state for the application. Important objects include:

- Pods: Collections of one or more containers
- ReplicationControllers: Ensure that the correct number of pods are running.
- Services: Internal object which represents the port/daemon/program running.
- Routes: Expose services to the external world.
- PeristentVolumeClaims: Represents the request for storage and how much. Typically defined in the application by the developer or architect.
- PersistentVolume: Represents the actual storage. Typically defined by sysadmins or automation.

Developers and architects can group these objects/resources in a single file to make sharing and deployment of the entire application easier. These definitions can be stored in version control systems just like code. Let's inspect some of the Kubernetes resource definitions we will use for this lab.

First, create a new OpenShift project:

```
oc new-project myproject

```

Now look at the objects/resources that define the application:

```
cat ~/labs/wordpress-demo/wordpress-objects.yaml
```

Notice there are two Services defined - one for MySQL and one for Wordpress. This allows these two Services to scale independently. The front end can scale with web traffic demand. The MySQL service could also be made to scale with a technology like Galera, but would require special care. You may also notice that even though there are two Services, there is only a single Route in this definition. That's because Services are internal to the Kubernetes cluster, while Routes expose the service externally. We only want to expose our Web Server externally, not our database.

Containers are ephemeral which means their storage is deleted and recreated every time they restart. MySQL tables and the Apacheweb root need storage because we don't want our website data deleted every time a container restarts. To do this, we will define Persistent Volumes in Kubernetes. We will create four persistent volumes - two that have 1GB of storage and two that will have 2GB of storage. In this lab environment, these persistent volumes will reside on the local all-in-one installation, but in a production environment they could reside on shared storage and be accessed from any node in the Kubernetes/OpenShift cluster. In a production environment, they could also be dynamically provisioned with the appropriate resiliency (Gluster, Ceph, AWS EBS Volumes, NFS, iSCSI, others):

```
cat ~/labs/wordpress-demo/persistent-volumes.yaml
```

This host has been initialized with some already available PVs:

```
oc get pv
```

Notice that the persistent volumes are unbound. They are available and waiting, but will not be utilized until you define an application which consumes storage. This is inline with the way Kubernetes constantly tries to drive the actual state toward the defined state. Currently, there is no definition to consume this storage.


Now, let's instantiate our two tier web application with a single command. This single command can be thought of as an API call which tells Kubernetes the desired state or defined state which it will use as a guide:

```
oc create -f ~/labs/wordpress-demo/wordpress-objects.yaml
```

Look at the status of the application. The two pods that make up this application will remain in a "pending" state - why? Kubernetes will connect the Persistent Volume Claims with the available Persistent Volumes, pull images, and schedule the pods to a node. Keep running these commands until the pods start:

```
oc describe pod wordpress-
```

```
oc describe pod mysql-
```

Now, the persistent volume claims for the application will become Bound and satisfy the storage requirements. Kubernetes/OpenShift will now start to converge the defined state and the actual state:

```
oc get pvc
```

Also, take a look at the Persistent Volumes again:

```
oc get pv
```

You may notice the wordpress pod enter a state called CrashLoopBackOff. This is a natural state in Kubernetes/OpenShift which helps satisfy dependencies. The wordpress pod will not start until the mysql pod is up and running. This makes sense, because wordpress can't run until it has a database and a connection to it. Similar to email retries, Kubernetes will back off and attempt to restart the pod again after a short time. Kubernetes will try several times, extending the time between tries until eventually the dependency is satisfied, or it enters an Error state. Luckily, once the mysql pod comes up, wordpress will come up successfully. Here are some useful commands to watch the state:

Show all pods. You may run this command several times waiting for the actual state to converge with the defined state:

```
oc get pods
```

View the events for a pod. You may run these commands several times waiting for the actual state to converge with the defined state:

```
oc describe pod mysql-
```

```
oc describe pod wordpress-
```

Once the pods are scheduled and running, view the terminal output for each pod:

```
oc logs $(oc get pods | grep mysql- | awk '{print $1}')
```

```
oc logs $(oc get pods | grep wordpress- | awk '{print $1}')
```

Finally, ensure that the web interface is up and running. The following curl/grep/awk command will show us the HTML returned form the webserver, letting us know that wordpress is up and running.

```
curl http://$(oc get svc | grep wpfrontend | awk '{print $3}')/wp-admin/install.php
```

Think about it, we just instantiated a fairly complex, multi-service application with a single command and pre-defined state in the YAML files. This is extremely powerful and can be used to construct complex applications with 10, 20, 30 or 100 services.

In this exercise you learned how to deploy a fully functional two tier application with a single command (oc create). As long as the cluster has persistent volumes available to satisify the application, an end user can do this on their laptop, in a development environment or in production data centers all over the world. All of the dependent code is packaged up and delivered in the container images - all of the data and configuration comes from the environment. Production instances will access production persistent volumes, development environments can be seeded with copies of production data, etc. It's easy to see why container orchestration is so powerful.

### Inspecting & Troubleshooting: Using the OpenShift web interface

In the last step, you created a multi-tier web application. Now, log into the OpenShift web interface which is a convenient place to monitor the state, and troubleshoot things when they go wrong. You can even get a debug terminal into a Pod to troubleshoot if it crashes. This can help you figure out why it crashed. This shouldn't happen in this lab, but as you build applications it surely will. Also, feel free to delete a pod and see what happens. Kubernetes will see that the defined state and actual state no longer match and will recreate it. This is useful when things are taking too long :-)

Access the OpenShift Web Console to login from the Web UI:
```
oc get routes console -n openshift-console -o jsonpath='{"https://"}{.spec.host}{"\n"}'
```
Copy the URL from the output of the above command and open it in your browser.

We'll deploy our app as the `admin` user. Use the following credentials:
* Username:

```
admin
```
* Password:
```
admin
```

Here are some useful locations to investigate what is happening. From `myproject` project, go to the "Events" section. It is useful early in the Pod creation process, when its being scheduled in the cluster and then when the container image is being pulled. Later in the process when a Pod is crashing because of something in the container image itself, its useful to watch the terminal output in the "Logs" section. It can also be useful to run commands live in a Terminal. Sometimes a Pod won't start, so it's useful poke around with using the "Debug in Terminal" section. Get a feel in the following areas of the interface.

First check out the MySQL Pod:

- Workloads -> Pods -> mysql-<id> -> Details
- Workloads -> Pods -> mysql-<id> -> Events
- Workloads -> Pods -> mysql-<id> -> Logs
- Workloads -> Pods -> mysql-<id> -> Terminal

Do the same for Wordpress:

- Workloads -> Pods -> wordpress-<id> -> Details
- Workloads -> Pods -> wordpress-<id> -> Events
- Workloads -> Pods -> wordpress-<id> -> Logs
- Workloads -> Pods -> wordpress-<id> -> Terminal

Once you've spent some time in the web interface, move on to the next lab.

### Cluster Performance: Scaling applications horizontally with containers

In this exercise, you will scale and load test a distributed application using a tool called Apache Bench. Apache Bench (ab command) is a tool for benchmarking HTTP servers. It is designed to give you an impression of how your current Apache installation perform. In particular ab shows you how many requests per second your Apache installation is capable of serving. The ab tool is especially useful with a Kubernetes cluster, where you can scale up web servers with a single command and provide more capacity to handle more requests per second.

Before we test, copy the URL address to connect to for several tests:

```
echo "http://$(oc get svc | grep wpfrontend | awk '{print $3}')/wp-admin/install.php"
```

From *Terminal 2*, start toolbox to install AB benchmarking tool from Apache Tools inside a container:

```
toolbox
```

Install AB:

```
dnf install -y httpd-tools
```

Copy the URL from the Terminal 1 into an environment variable so we can use it multiple times:

```
export SITE=<URL_COPIED>
```

Test with ab before we scale the application to get a base line. Take note of the "Time taken for tests" section:

```
ab -n10 -c 3 -k -H "Accept-Encoding: gzip, deflate" $SITE
```


Take note of the following sections in the output:

- Time taken for tests
- Requests per second
- Percentage of the requests served within a certain time (ms)


Go to the web interface. Scale the Wordpress Deployment up to three containers. Click the up arrow:

- Workloads -> Deployments -> wordpress -> Up Arrow


Test with ab again. How did this affect our benchmarking and why?

```
ab -n10 -c 3 -k -H "Accept-Encoding: gzip, deflate" $SITE
```

Come back in *Terminal 1*, now lets scale up more with command line instead of the web interface:

```
oc scale --replicas=5 rc/wordpress
```


Come back to *Terminal 2* to test with ab. How did this affect our benchmarking and why?

```
ab -n10 -c 3 -k -H "Accept-Encoding: gzip, deflate" $SITE
```


In *Terminal 1*, scale the application back down to one pod:

```
oc scale --replicas=1 rc/wordpress
```

From *Terminal 2*, test with ab. How did this affect our benchmarking and why?

```
ab -n10 -c 3 -k -H "Accept-Encoding: gzip, deflate" $SITE
```

Counter intuitively, you may have seen slower response times when you scaled the pods up. This is probably because we are running Kubernetes on a single node. The requests are bing sent through a reverse proxy and distributed to multiple pods all running on the same server for this lab. If this were a real OpenShift cluster with 100s or 1000s of nodes, you may have seen scale out performance where response times went down.

Sometimes horizontal scaling can have counter intuitive effects. Sometimes great care must be taken with applications to get the performance characteristics that we need. The world of container orchestration opens up an entirely new kind of performance tuning and you will need new skills to tackle this challenge.

### Distributed Debugging: Troubleshooting in a distributed systems environment

The goal of this exercise is to understand the nature of a distributed systems environment with containers. Quickly and easily troubleshooting problems in containers requires distributed systems thinking. You have to think of things programatically. You can't just ssh into a server and understand the problem. You can execute commands in a single pod, but even that might prevent you from troubleshooting things like network, or database connection errors which are specific to only certain nodes. This can happen because of persnickety differences in locations of your compute nodes in a cloud environment or code that only fails in unforeseen ways at scale or under load.

We are going to simulate one of these problems by using a specially designed test application. In this exercise we will learn how to figure things out quickly and easily.

Inspect each of the files and try to understand them a bit:

```
cat ~/labs/goodbad/Build.yaml
```

```
cat ~/labs/goodbad/Run.yaml
```


Build the test application. **Wait** for the build to successfully complete. You can watch the log output in the OpenShift web interface.

```
oc create -f ~/labs/goodbad/Build.yaml
```


```
oc get builds
```

```
oc get pods
```

You can watch the logs like this. Keep running the following command until you see "Push successful" in the logs:

```
oc logs goodbad-1-build
```

When the above build completes, run the test application:

```
oc create -f ~/labs/goodbad/Run.yaml
```


Get the IP address for the goodbad service

```
oc get svc
```

Now test the cluster IP with curl. Use the cluster IP address so that the traffic is balanced among the active pods. You will notice some errors in your responses. You may also test with a browser. Some of the pods are different - how could this be? They should be identical because they were built from code right?

```
SVC_IP=$(oc get svc | grep goodbad | awk '{print $3}')
for i in {1..20}; do curl $SVC_IP; done
```

Example output:

```
ERROR
ERROR
Hello World
ERROR
```

Take a look at the code. A random number is generated in the entry point and written to a file in /var/www/html/goodbad.txt:

```
cat ~/labs/goodbad/index.php
```

```
cat ~/labs/goodbad/Dockerfile
```

Troubleshoot the problem in a programmatic way. Notice some pods have files which contain numbers that are lower than 7, this means the pod will return a bad response:

```
for i in $(oc get pods | grep goodbad | grep -v build | awk '{print $1}'); do oc exec -t $i -- cat /var/www/html/goodbad.txt; done
```

Continue to troubleshoot the problem by temporarily fixing the file

```
for i in $(oc get pods | grep goodbad | grep -v build | awk '{print $1}'); do oc exec -t $i -- sed -i -e s/[0-9]*/7/ /var/www/html/goodbad.txt; done
```

Write a quick test that verifies the logic of your fix

```
for i in {1..2000}; do curl $SVC_IP 2>&1; done | grep "Hello World" | wc -l
```

Scale up the nodes, and test again. Notice it's broken again because new pods have been added with the broken file

```
oc scale rc goodbad --replicas=10
```

```
for i in {1..2000}; do curl $SVC_IP 2>&1; done | grep "Hello World" | wc -l
```

Optional: As a final challenge, fix the problem permanently by fixing the logic so that the number is always above 7 and never causes the application to break. Rebuild, and redeploy the application. Hint: you have to get the images to redeploy with the newer versions (delete the rc) :-)

## What's Next?

Congratulations on completing this lab. Keep learning about OpenShift:

* Visit the [Red Hat Developer learning page](https://developers.redhat.com/learn) for more labs and resources
* [Want to try a free, instant 30-day OpenShift cluster? Get started with the Developer Sandbox for Red Hat OpenShift](https://developers.redhat.com/developer-sandbox)
