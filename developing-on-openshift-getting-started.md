# Getting Started Developing on OpenShift (Old)

- [Getting Started Developing on OpenShift (Old)](#getting-started-developing-on-openshift-old)
  - [Log into Openshift](#log-into-openshift)
  - [Container Orchestration Primer](#container-orchestration-primer)
  - [Deploying with an Image](#deploying-with-an-image)
    - [Logging in with the Web Console](#logging-in-with-the-web-console)
    - [Understanding Projects in OpenShift](#understanding-projects-in-openshift)
    - [Creating a Project](#creating-a-project)
      - [Congratulations!](#congratulations)
    - [Scaling Up the Application](#scaling-up-the-application)
    - [Verifying the Number of Replicas](#verifying-the-number-of-replicas)
    - [Understanding Application "Self-Healing"](#understanding-application-self-healing)
    - [Scaling Down an Application](#scaling-down-an-application)
      - [Congratulations!](#congratulations-1)
    - [Viewing a Route's URL](#viewing-a-routes-url)
    - [Accessing a Route's URL](#accessing-a-routes-url)
      - [Congratulations!](#congratulations-2)
  - [Deploying with a Helm Chart (Optional)](#deploying-with-a-helm-chart-optional)
    - [Creating a New Project](#creating-a-new-project)
    - [Add a Helm Repo](#add-a-helm-repo)
    - [Deploy a Helm Chart](#deploy-a-helm-chart)
    - [Verify the deployment from OpenShift Console](#verify-the-deployment-from-openshift-console)
  - [What's Next?](#whats-next)

Welcome to this workshop on containers and OpenShift! Today, we will use a hosted OCP environment provided by your instructors, along with these instructions, as we journey through advanced container orchestration using Red Hat OpenShift.

<!-- If you already have `oc` installed on your machine, feel free to use your own terminal for the CLI portion of this work -->

<!--
, otherwise you may use OpenShift Dev Spaces.

Let's begin by logging into the environment provided by your instructors and going to OpenShift Dev Spaces.

![Web Console Login](./assets/images/sign-in.png)

Get your OpenShift User ID and Password from the registration page:
![OpenShift Credentials](./assets/developing-on-openshift-getting-started/assets/openshift-user-password.png)

|NOTE:|
|----|
|You might see the following warning notification due to using an untrusted security certificate.
![Security warning](./assets/developing-on-openshift-getting-started/assets/security_warning.png)
If you do get the warning, click the **Advanced** button to complete the process necessary to grant permission to the browser to access the OpenShift Web Console.|

Now click the menu button in the OpenShift console header and select **OpenShift Dev Spaces**

![Dev Spaces Menu Item](./assets/images/ocp-to-dev-spaces.png)

From here, select the vote-ui workspace. This project will not be used during today's workshop, but provides us with a workspace.

![Dev Spaces Vote UI Workspace](./assets/images/vote-ui-dev-spaces.png)

This will take a minute or two to load since this is the first time you are opening this workspace

![Dev Spaces Loading](./assets/images/starting-empty-workspace.png)

Once your workspace loads, choose *Trust the Authors*

![Dev Spaces Trust the Authors](./assets/images/trust-the-authors.png)

And finally, open the terminal by clicking the hamburger menu icon on the top left

![Dev Spaces Open the Terminal](./assets/images/terminal-2.png)

-->
## Log into Openshift
Go back to your [workshop landing page](red.ht/bw-ocp-workshop) (you may need to login again using the same email address as before) and paste the `login command` into the bastion.


Now, you are ready to begin this workshop module!

## Container Orchestration Primer

Container Orchestration is the next logical progression after you become comfortable working with containers on a single host. With a single container host, containerized applications can be managed quite similarly to traditional applications, while gaining incremental efficiencies. With orchestration, there is a significant paradigm shift - developers and administrators alike need to think differently, making all changes to applications through an API.  Some people question the "complexity" of orchestration, but the benefits far outweigh the work of learning it. Today, Kubernetes is the clear winner when it comes to container orchestration, and with it, you gain:

* Application Definitions - YAML and JSON files can be passed between developers or from developers to operators to run fully-functioning, multi-container applications
* Easy Application Instances - Run many versions of the same application in different namespaces
* Multi-Node Scheduling - controllers built into Kubernetes manage 10 or 10,000 container hosts with no extra complexity
* Powerful API - Developers, Cluster Admins, and Automation alike can define application state, tenancy, and with OpenShift 4, even cluster node states
* Operational Automation - The [Kubernetes Operator Framework](https://www.redhat.com/en/topics/containers/what-is-a-kubernetes-operator#operator-framework) can be thought of as a robot systems administrator deployed side by side with applications managing mundane and complex tasks for the application (backups, restores, etc)
* Higher Level Frameworks - Once you adopt Kubernetes orchestration, you gain access to an innovative ecosystem of tools like Istio, Knative, and the previously mentioned Operator Framework

![Orchestration Node](./assets/subsystems-container-internals-lab-2-0-part-1/assets/05-simple-orchestration-node.png)


To demonstrate, all we need is bash, curl, and netcat which lets us pipe text across a TCP port. If you are familiar with basic bash scripting, this tiny lab teases apart the value of the orchestration, versus the application itself. This application doesn't do much, but it does demonstrate the power of a two-tier application running in containers with both a database and a web front end. In this lab, we use the same container image from before, but this time we embed the *how* to run logic in the Kubernetes YAML. Here's a simple representation of what our application does:

~~~~
User -> Web App (port 80) -> Database (port 3306)
~~~~


Take a quick look at this YAML file but don't don't get too worried if you don't fully understand the YAML. There are plenty of great tutorials on Kubernetes & OpenShift (like in the next chapter), and most people learn it over iterations, and building new applications:

```
curl https://raw.githubusercontent.com/fatherlinux/two-pizza-team/master/two-pizza-team-ubi.yaml
```

In the "database," we are opening a file and using netcat to ship it over port 3306. In the "web app", we are pulling in the data from port 3306, and shipping it back out over port 80 like a normal application would. The idea is to show a simple example of how powerful this is without having to learn other technology. We are able to fire this application up in an instant with a single *oc* command:

```
oc create -f https://raw.githubusercontent.com/fatherlinux/two-pizza-team/master/two-pizza-team-ubi.yaml
```

Wait for the cheese-pizza and pepperoni pizza pods to start:

```
for i in {1..5}; do oc get pods;sleep 3; done
```

Wait until all pods are are in status "RUNNING".

When the pods are done being created, pull some data from our newly created "web app".  Notice that we get back the contents of a file which resides on the the database server, not the web server:

```
curl $(oc get svc pepperoni-pizza -ojsonpath='{.spec.clusterIP}')
```

**Note:** The command in brackets above is simply getting the IP address of the web server.

Now, let's pull data directly from the "database." It's the same file as we would expect, but this time coming back over port 3306:

```
curl $(oc get svc cheese-pizza -ojsonpath='{.spec.clusterIP}'):3306
```

Take a moment to note that we could fire up 50 copies of this same application in Kubernetes with 49 more commands (in different projects). It's that easy.

## Deploying with an Image

<!--
### Working with the Command Line Interface (CLI)

You can access the Red Hat OpenShift CLI with the command `oc`. Using the `oc` command lets you work with the entire OpenShift cluster and deploy new applications.

The CLI exposes the underlying Kubernetes orchestration system with the enhancements made by OpenShift. Users familiar with Kubernetes will be able to adapt to OpenShift quickly.

`oc` provides all of the functionality of the Kubernetes `kubectl` CLI tool. In addition, `oc` makes it easier to work with OpenShift. The CLI is ideal in situations where you are:

* Working directly with project source code
* Scripting OpenShift operations
* Cannot use the web console because of resource bandwidth restrictions

Let's start by logging in with `oc` for your first experience with this CLI.

### Logging in with the CLI
You log into OpenShift from the command line using the `oc login` command.

----

`Step 1:` Run the following command in the terminal:

```
oc login -u <lab user id> -p <lab password> LAB_API_ENDPOINT --insecure-skip-tls-verify=true
```

Upon successful login, you'll get results similar to the following:

```
Logged into "https://api.cluster-c8wjq.sandbox2325.opentlc.com:6443" as "user1" using the token provided.

You have access to the following projects and can switch between them with 'oc project <projectname>':

  * argocd-user1
    user1-devspaces
    vote-app-ci-user1
    vote-app-dev-user1
    vote-app-prod-user1

Using project "argocd-user1".
```

Running the Linux [`whoami`](https://en.wikipedia.org/wiki/Whoami) command reports the current user and implicitly confirms that the login is successful.

----

`Step 2:` Run the following command:

```
oc whoami
```

You'll get the following results:

```
user1
```

#### Congratulations!

 You've logged in to OpenShift using the `oc` command line tool.

----
-->

This topic focuses on learning how to log into the Red Hat OpenShift web console and then create an application once logged in.

### Logging in with the Web Console

Your first task is to log into OpenShift from the web console. (Skip to Step 2 if you are already logged in from using OpenShift Dev Spaces.)

----

`Step 1:`
Go back to your [workshop landing page](red.ht/bw-ocp-workshop) (you may need to login again using the same email address as before) and paste the `console_url` into your browser.

You will be presented with the OpenShift login screen. Use the following credentials to log in.

* **Username:** `<lab user id>`
* **Password:** `<lab password>`

Log into the OpenShift web console, as shown in the figure below.

![Web Console Login](./assets/images/sign-in.png)

|NOTE:|
|----|
|You might see the following warning notification due to using an untrusted security certificate.
![Security warning](./assets/developing-on-openshift-getting-started/assets/security_warning.png)
If you do get the warning, click the **Advanced** button to complete the process necessary to grant permission to the browser to access the OpenShift Web Console.|

After logging into the web console, take a look a the menu on the left. Notice that you're in the **Administrator** perspective, as shown in the figure below:

![Admin Perspective](./assets/developing-on-openshift-getting-started/assets/admin-perspective.png)

You need to change the perspective from **Administrator** to **Developer**.

----

`Step 2:` Select the **Developer** perspective from the dropdown on the left side menu in the OpenShift web console as shown in the figure below.

![Switch to Developer](./assets/developing-on-openshift-getting-started/assets/change-to-developer.png)

Now that you're in the **Developer** perspective, let's take a moment to discuss the concept of a **project** in OpenShift.

### Understanding Projects in OpenShift

OpenShift is often referred to as a container application platform because it's a platform designed for the development and deployment of applications in [Linux containers](https://developers.redhat.com/topics/containers).

OpenShift has an organizational unit named **project**. You use a **project** to group resources in your application. The reason for organizing your application in a **project** is to enable controlled access and quotas for developers or teams.

You can think of a **project** as a visualization of the Kubernetes namespace based on the developer access controls.

Now, let's create a new project.

### Creating a Project

In this section you will create a project using the OpenShift web console.

----

`Step 3:` Click the button labeled **+Add** on the menu bar on the left side of the Web Console. The **Add** web page appears.

----

`Step 4:` Click the link with the text **create a Project** as shown in the figure below:

![Create project](./assets/developing-on-openshift-getting-started/assets/add_project.png)

You'll be presented with the **Create Project** dialog for declaring the project.

----

`Step 5:` Name the project `myproject-<YOUR_USER>` as shown in the figure below:

![Name project](./assets/images/create-project.png)

----

`Step 6:` Then click the button labeled `Create` as shown in the figure above.

You will be presented with the **Add** page as shown in the figure below.

![Select container](./assets/developing-on-openshift-getting-started/assets/select-container-with-code.png)

----

`Step 7:` Scroll down the page and click the text block labeled `Container images` as shown in the figure above.


You'll be presented with the **Deploy Image** page as shown in the figure below.

![Deploy Image](./assets/developing-on-openshift-getting-started/assets/deploy-container.png)

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

![Topology View with App](./assets/developing-on-openshift-getting-started/assets/topology-view-with-app.png)

You are now ready to scale the application up and down.

#### Congratulations!

 You deployed an application from a container image using the OpenShift web console.

----

In this step, you will learn how to scale your application up by creating multiple replicas of the pod that represents the application in the cluster. Red Hat OpenShift will make it look like all of the replicas are a single application. But, behind the scenes, OpenShift routes traffic among the replicas automatically.

The benefit of replicating a pod is that it increases the amount of internet traffic the application can accommodate. The result is better overall performance.

### Scaling Up the Application

You will scale up the application up to 2 replicas.

----

`Step 1:` Click inside the circle for the `parksmap` application you just installed within the **Topology** view. The details window will appear in a side panel as shown in the figure below.

![Scaling Up 1](./assets/developing-on-openshift-getting-started/assets/scaling-01.png)

----

`Step 2:` In the side panel, click the **Details** tab, and then click the up arrow next to the pod to increase the number of pods to 2. (The word [`pod` is a Kubernetes term](https://kubernetes.io/docs/concepts/workloads/pods/). In this case, a `pod` refers to an instance of the application.)

The figure below illustrates selecting the **Details** and increasing the number of pods to 2.

![Scaling Up 2](./assets/developing-on-openshift-getting-started/assets/scaling-02.png)

You are now running two replicas of the application.

### Verifying the Number of Replicas

----

`Step 3:` To verify that we changed the number of replicas, click the **Resources** tab in the side panel. You will see a list of pods as shown in the figure below.

![View Pods](./assets/developing-on-openshift-getting-started/assets/view-pods.jpg)

You can see that we now have 2 replicas. Now, let's take a look at OpenShift's self-healing feature. Self Healing ensures that the number of pods you declare for your application is always running.

### Understanding Application "Self-Healing"

Under the OpenShift application **Deployments** are constantly monitored to see that the desired number of pods are actually running. Therefore, if the actual state ever deviates from the desired state--for example, if a pod goes down--OpenShift will fix the situation.

Now you will kill one of the pods to see if OpenShift will bring it back to life.

----

`Step 4:` On the **Resources** tab where you viewed the list of pods after scaling to 2 replicas, click the name of one of the pods in the list as shown in the figure below.

![View pods](./assets/developing-on-openshift-getting-started/assets/view-pods.jpg)

----

`Step 5:` In the top right corner of the page, there is an **Actions** drop down menu. Click it and select **Delete Pod** as shown in the figure below.

![Delete Pod](./assets/developing-on-openshift-getting-started/assets//delete-pod.png)

After deleting the pod, you will be taken to a page listing pods. Notice that even though you deleted one pod, two pods are still running, as shown in the figure below.

![Replenish Pod](./assets/developing-on-openshift-getting-started/assets/replenish-pod.jpg)

A replacement pod was created because OpenShift will always make sure that when a pod dies, it creates a new pod to fill its place.

### Scaling Down an Application

Before we continue, go ahead and scale your application down to a single instance.

----

`Step 6a:` Click the **Topology** item on the left side menu.

`Step 6b:` Click the `parksmap` circle.

`Step 6c:` Click the **Details** tab in the side panel.

`Step 6d:` Click the down arrow to the right of the pod circle to reset the number of pods to 1 as shown in the figure below.

![Reset pod count](./assets/developing-on-openshift-getting-started/assets/reset-pod-count.jpg)

#### Congratulations!

 You've learned how to use the OpenShift web console to scale the number of replica pods for an application up and down.

----

### Viewing a Route's URL

In this next step you will explore **routes**.

In Red Hat OpenShift, the **service** resource provides the internal abstraction that binds access to an application to its logic, which is represented in an application's pods. Also, a **service** provides load-balancing capabilities within an OpenShift environment.

The binding of a **service** to an application's pods is internal to an OpenShift cluster. The way that external clients access applications running in OpenShift is through the OpenShift routing layer.

The formal name for the resource which represents the routing layer is called a **route**.

As mentioned earlier in the tutorial, when you create an application from a **container image**, OpenShift creates a **route** for the application automatically. Let's take a look at where the **route** is published. Then, let's access the application's website from a browser using its route URL.

`Step 1:` To view the **route** for your application, make sure you are in the **Developer** perspective by selecting **Developer** from the dropdown list on the left side menu bar. Then from within **Topology** view, click the application circle as shown in the figure below.

![Select App](./assets/developing-on-openshift-getting-started/assets/select-app.png)

The information page for the pods will slide out as a panel on the right-hand side of the page.

----

`Step 2:` Click the **Resource** tab in the side panel.

----

`Step 3:` Scroll down to the **Routes** section at the bottom of the web page as shown figure below.

![Select Route](./assets/developing-on-openshift-getting-started/assets//select-route.png)

The route URL for your application is shown as a link.

----

`Step 4:` Click that link to load the application's web page in your browser.

### Accessing a Route's URL

You can also access an application's **route** URL directly within the application's circular graphic in the **Topology** page.

----

`Step 5:` Click the icon in the upper right of the application's circular graphic as shown in the figure below.

![Open from topology](./assets/developing-on-openshift-getting-started/assets/access-url-from-topology.png)

Clicking the icon opens the application's web page in a browser as shown in th figure below.

![View map](./assets/developing-on-openshift-getting-started/assets/view-map.png)

----

#### Congratulations!

 You have now located the application's **route** URL in the OpenShift web console. Also, you've learned how to open the application in a web page from the **Resource** tab in the application detail, and also directly from the application's circular graphic in the **Topology** view.

This is the final step in **Deploying with an Image**.

## Deploying with a Helm Chart (Optional)

At the end of this section you will be able to:
- Install a Helm repository from the OpenShift Console
- Search, install and uninstall Helm Charts
- Review Helm Charts from OpenShift Console

<!-- ### Helm Command Line Interface (CLI)

In this scenario you will find the Helm CLI already installed for you, which can be  retrieved from the OpenShift Console (look for the >_ button in the OpenShift header.)

The CLI is the entry point for any interaction with the Helm 3 subsystem. In addition to that, OpenShift Developer Catalog, which is the central hub for all developer content, has support for Helm Charts in addition to Operator-backed services, Templates, etc.

When a user instructs the Helm CLI to install a Helm Chart, the information about the Helm Chart is fetched from the repository, rendered on the client and then applied to Kubernetes while a record of this installation is created within the namespace (which is known as a Release).

![Helm on OpenShift](https://raw.githubusercontent.com/openshift-instruqt/instruqt/master/assets/developing-on-openshift/helm/helm-diagram.png) -->


### Creating a New Project

Let's create a new OpenShift Project to have a namespace for our Helm Charts to work with.

```
oc new-project helm-<YOUR_USER>
```

### Add a Helm Repo
As discussed in the previous step, Helm Charts are available through repositories, and those can be pre-installed or installable by the user.

You can search for Helm Charts available in any public repositories through [Helm Hub](https://hub.helm.sh/).

For instance, searching Helm Charts for [NGINX](https://nginx.com) will give a list of available charts from multiple repositories. If we want to install it, we need to have such repositories configured.

By default the list of available repositories is empty. You can add a new one with the CLI or through the OpenShift Console. Let's add the Bitnami repository through the OpenShift Console now.

Start by going to "Helm" in the left hand menu of the OpenShift Console and then choosing the "Repositories" tab. Click the blue "Create" button in the top right corner of this page and choose "Repository" from the dropdown.

![Helm on OpenShift Console](./assets/images/helm-1.png)

Fill out the new Project Helm Chart Repository form with
* **Name:** bitnami
* **URL:** https://charts.bitnami.com/bitnami

![Helm on OpenShift Console](./assets/images/helm-2-create-bitnami.png)

And now you will be dropped back on the **+Add** page. From here, find "Helm Chart" and click here.

![Helm on OpenShift Console](./assets/images/helm-3.png)

Now check the Bitnami box to discover all of the Bitnami charts provided through this new Helm repository!

![Helm on OpenShift Console](./assets/images/helm-4.png)

<!-- ### Exercise: Explore CLI
Let's get started by using `helm` getting CLI version:

```
helm version
```

This should confirm we are using **Helm 3**.

As discussed in the previous step, Helm Charts are available through repositories, and those can be pre-installed or installable by the user.

You can search for Helm Charts available in any public repositories through [Helm Hub](https://hub.helm.sh/).

For instance, searching Helm Charts for [NGINX](https://nginx.com):

```
helm search hub nginx
```

This will give a list of available charts from multiple repositories. If we want to install it, we need to have such repositories configured.

By default the list of available repositories is empty. You can add a new one with the CLI. For NGINX, add Bitnami repository:

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Once added, verify it is present:

```
helm repo list
```

You can search for Helm Charts also inside repos, like in the one you just installed:

```
helm search repo bitnami/nginx
``` -->

### Deploy a Helm Chart

<!-- You can use `helm install` command to deploy your charts and start managing revisions. -->

To install the [NGINX Chart](https://hub.helm.sh/charts/bitnami/nginx), search for "nginx" in the search box on this page.

![Helm on OpenShift Console](./assets/images/helm-search-nginx.png)

After clicking on Nginx, click "Install Helm Chart"

![Helm on OpenShift Console](./assets/images/helm-5-install-nginx.png)

Now take all of the defaults and click "Install"

![Helm on OpenShift Console](./assets/images/helm-6-install-nginx-2.png)

<!-- ```
helm install my-nginx bitnami/nginx --set service.type=ClusterIP
```

This will install `nginx`, and for this example, we want to use `ClusterIP` Service type because we want to expose it afterwards through an OpenShift `Route`.

Check your Helm releases:

```
helm ls
``` -->

Now head back to Dev Spaces, and verify that all the pods are in Running state and Ready:

```
oc get pods
```

Now expose `nginx` service to access it over the internet via an OpenShift `Route`:

```
oc expose svc/nginx
```

Verify that route has been created:

```
oc get routes
```

You can click on the generated host to access the NGINX Pod provided by your just installed Helm Chart, or you can do it from OpenShift Console.

### Verify the deployment from OpenShift Console

To verify the creation of the resources generated by the Helm Chart, you can head out to the OpenShift web console.

Make sure the Developer perspective from the dropdown in the top left corner of the web console is selected as shown below:

<img src="https://katacoda.com/embed/openshift/courses/assets/middleware/pipelines/developer-view.png" width="800" />

Next, select the Project dropdown menu shown below and choose `helm` project you have been working with.

Next, click on the Topology tab on the left side of the web console if you don't see what's in the image below. Once in the Topology view, you can see the Deployment for `my-nginx` application and you can access it by clicking on the URL generated by the OpenShift Route:

<img src="./assets/images/nginx-topology.png" width="800" />

You'll notice the HR label and Helm icon below, this means that this application is managed by Helm, and you can overview Helm `Releases` for this app from left side menu, Helm section:

<img src="./assets/images/helm-release.png" width="800" />

Explore all `Resources` that are associated with a particular Helm `Release`, click on `nginx` Helm Release and then click on `Resouces` tab:

<img src="./assets/images/helm-resources.png" width="800" />

<!-- ### Uninstall and clean

Come back to Terminal clicking on Terminal tab.

Uninstall `my-nginx` release:

```
helm uninstall my-nginx
```

Delete previously created `route`:

```
oc delete route my-nginx

<!-- ## Create Your First Helm Chart

At the end of this section you will be able to:
- Create your own `Helm Chart`
- Understand `Helm Templates`
- Understand Helm integrations with `Kubernetes`


After having discovered `helm` CLI to install and manage Helm Charts, we can now create our first one from scratch. Before doing that, let's review the core concepts from [official documentation](https://helm.sh/docs/topics/charts/):

- A `Chart` is a Helm package. It contains all of the resource definitions necessary to run an application, tool, or service inside of a Kubernetes cluster.
- A `Repository` is the place where charts can be collected and shared.
- A `Release` is an instance of a chart running in a Kubernetes cluster-


Helm uses a packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources, and it organized as a collection of files inside of a directory. The directory name is the name of the chart.

## Creating a new Helm Chart

Navigate to `/root` directory:

```
cd /root
```

With `helm create` command you can create a chart directory along with the common files and directories used in a chart.

An Helm chart called `my-chart` has been already generated with the following command:

`helm create my-chart`


Inside `my-chart/` folder you will find the following files, you can also review them from the **Visual Editor** Tab.

```
cd my-chart
ls -la
```

* `Chart.yaml`: is a YAML file containing multiple fields describing the chart
* `values.yaml`:: is a YAML file containing default values for a chart, those may be overridden by users during helm install or helm upgrade.
* `templates/NOTES.txt`: text to be displayed to your users when they run helm install.
* `templates/deployment.yaml`: a basic manifest for creating a Kubernetes deployment
* `templates/service.yaml`: a basic manifest for creating a service endpoint for your deployment
* `templates/_helpers.tpl`: a place to put template helpers that you can re-use throughout the chart

This command generates a skeleton of your Helm Chart, and by default there is an NGINX image as example:


**1. Chart description**

Let's review our `Chart.yaml`. This contains `version` of the package and `appVersion` that we are managing, typically this can be refered to a container image tag.

**2. Fill chart with custom values**

In our example, we are working on a Helm Template `templates/deployment.yaml` describing a Kubernetes Deployment for our app, containing this structure for `spec.containers.image`:

`image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"`

> **Note:** *By default `appVersion` from `Chart.yaml` is used as image tag*

In `values.yaml` add `image.repository` variable to define the container image for our chart.

From the **Visual Editor** Tab, open `values.yaml` file and add this line after the `# TODO: image repository` comment:

```yaml
repository: bitnami/nginx
```

Now let's define which tag to use for this container image.
Add this line after the `# TODO: image tag"` comment:

```yaml
tag: latest
```


**3. Install**

Install our custom Helm Chart from local folder.

```
cd /root
helm install my-chart ./my-chart
```

This will install NGINX like in previous chapter, and we can follow installation like in previous chapter, either from Terminal or OpenShift Console:

```
oc get pods
```

<img src="https://katacoda.com/embed/openshift/courses/assets/developing-on-openshift/helm/my-chart-helm-chart.png" width="800" />

Review installed revision:

```
helm ls
```

In next chapter we will add an OpenShift Route as a Helm Template, like for `Service`, to be published in a new revision.

## Managing Helm Revisions in OpenShift

At the end of this section you will be able to:
- Manage multiple `Helm Revisions` for your Helm Chart
- `Upgrade` revisions for new changes
- Revert changes with `Rollback` of revisions

### Upgrade revisions

When we install a Helm Chart on OpenShift, we publish a release into the cluster that we can control in terms of upgrades and rollbacks.

To change something in any already published chart, we can use `helm upgrade` command with new parameters or code from our chart.

### Add OpenShift Route as Template

From the Visual Editor, create a new template for OpenShift Route in `templates` directory. From the `templates` directory, create a new file called `routes.yaml`. Copy the following content to that file:

```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: {{ include "my-chart.fullname" . }}
  labels:
    {{- include "my-chart.labels" . | nindent 4 }}
spec:
  port:
    targetPort: http
  to:
    kind: Service
    name: {{ include "my-chart.fullname" . }}
    weight: 100
  wildcardPolicy: None
```

Run `helm upgrade` to publish a new revision containing a `my-charm` Route:

```
helm upgrade my-chart ./my-chart
```

Verify new `Route` from Terminal:

```
oc get routes
```

Verify new `Revision`:

```
helm ls
```

Verify new `Route` and new `Revision` from Console:

<img src="https://katacoda.com/embed/openshift/courses/assets/developing-on-openshift/helm/my-chart-helm-chart-route.png" width="800" />

<img src="https://katacoda.com/embed/openshift/courses/assets/developing-on-openshift/helm/my-chart-new-revision.png" width="800" />


### Upgrade and Rollback

Let's update again our existing release overriding values in `values.yaml` changing `image.pullPolicy` from chart's default value `IfNotPresent` to `Always`, using same method we adopted previously for changing `service.type` with option `--set`:

```
helm upgrade my-chart ./my-chart --set image.pullPolicy=Always
```

Let's verify that our changes is reflected into resulting `Deployment`:

```
oc get deployment my-chart -o yaml | grep imagePullPolicy
```

Get current `Revision`:

```
helm ls
```

Now that our new release is published and verified, we can decide to rollback to previous version if we need to, and this is possible with `helm rollback` command.

It is also possible to dry-run the rollback with `--dry-run` option:

```
helm rollback my-chart 2 --dry-run
```

Rollback to starting revision:

```
helm rollback my-chart 2
```

Check pods:

```
oc get pods
```

Verify `imagePullPolicy` is rolled back to `Revision` 2 containing `IfNotPresent` Policy:

```
oc get deployment my-chart -o yaml | grep imagePullPolicy
```

### Uninstall

Uninstall will clean everything now, there's no further need to delete manually the `Route` like in first chapter, since the Helm Chart is now managing that resource:

```
helm uninstall my-chart
``` -->

## What's Next?

Congratulations on completing this lab. Keep learning about OpenShift:

* Visit the [Red Hat Developer learning page](https://developers.redhat.com/learn) for more labs and resources
* [Want to try a free, instant 30-day OpenShift cluster? Get started with the Developer Sandbox for Red Hat OpenShift](https://developers.redhat.com/developer-sandbox)
