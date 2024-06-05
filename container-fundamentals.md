# Containers and OpenShift Workshop

Welcome to this workshop on containers and OpenShift! Today, we will use a hosted OCP environment provided by your instructor, along with these instructions, as we journey from simple Linux containers to advanced container orchestration using Red Hat OpenShift. Let's begin!

- [Containers and OpenShift Workshop](#containers-and-openshift-workshop)
  - [Container Fundamentals](#container-fundamentals)
    - [1. Get Started with Containers](#1-get-started-with-containers)
      - [Part I: Introduction to Containers](#part-i-introduction-to-containers)
      - [Part II: Container Images](#part-ii-container-images)
      - [Part III: Container Registries](#part-iii-container-registries)
      - [Part IV: Container Hosts](#part-iv-container-hosts)
        - [Container Engine](#container-engine)
        - [Container Runtime](#container-runtime)
        - [Linux Kernel](#linux-kernel)
      - [Part V: Container Orchestration](#part-v-container-orchestration)
      - [Part VI: Closing](#part-vi-closing)
    - [2. Manage Your Containers](#2-manage-your-containers)
      - [Part I: Image Layers and Repositories](#part-i-image-layers-and-repositories)
      - [Part II: Image URLs](#part-ii-image-urls)
      - [Part III: Image Internals](#part-iii-image-internals)
    - [3. Understand Container Registries](#3-understand-container-registries)
      - [Part I: Understanding the Basics of Trust - Quality \& Provenance](#part-i-understanding-the-basics-of-trust---quality--provenance)
        - [Trusted Thing](#trusted-thing)
        - [Trusted Source](#trusted-source)
      - [Part II: Evaluating Trust - Images and Registry Servers](#part-ii-evaluating-trust---images-and-registry-servers)
        - [Evaluating Images](#evaluating-images)
          - [CentOS](#centos)
          - [Fedora](#fedora)
          - [Ubuntu](#ubuntu)
          - [Red Hat Enterprise Linux](#red-hat-enterprise-linux)
        - [Evaluating Registries](#evaluating-registries)
          - [Fedora Registry](#fedora-registry)
          - [DockerHub](#dockerhub)
          - [Bitnami](#bitnami)
          - [Red Hat Container Catalog](#red-hat-container-catalog)
        - [Summary](#summary)
      - [Part III: Analyzing Storage and Graph Drivers](#part-iii-analyzing-storage-and-graph-drivers)
    - [4. Run Container Images with Hosts](#4-run-container-images-with-hosts)
      - [Part I: Container Engines \& The Linux Kernel](#part-i-container-engines--the-linux-kernel)
      - [Part II: Step-by-Step Creation of a Container](#part-ii-step-by-step-creation-of-a-container)
        - [Pull/Expand/Mount Image](#pullexpandmount-image)
        - [Create Spec File](#create-spec-file)
        - [Call Runtime](#call-runtime)
      - [Part III: SELinux \& sVirt: Dynamically generated contexts to protect your containers](#part-iii-selinux--svirt-dynamically-generated-contexts-to-protect-your-containers)
      - [Part IV: Cgroups: Dynamically created with container instantiation](#part-iv-cgroups-dynamically-created-with-container-instantiation)
      - [Part V: SECCOMP: Limiting how a containerized process can interact with the kernel](#part-v-seccomp-limiting-how-a-containerized-process-can-interact-with-the-kernel)
    - [5. Understand Container Orchestration](#5-understand-container-orchestration)
      - [Part I: Multi-Container Workloads: The classic two-tiered, wordpress application](#part-i-multi-container-workloads-the-classic-two-tiered-wordpress-application)
      - [Part II: Inspecting \& Troubleshooting: Using the OpenShift web interface](#part-ii-inspecting--troubleshooting-using-the-openshift-web-interface)
      - [Part III: Cluster Performance: Scaling applications horizontally with containers](#part-iii-cluster-performance-scaling-applications-horizontally-with-containers)
      - [Part IV: Distributed Debugging: Troubleshooting in a distributed systems environment](#part-iv-distributed-debugging-troubleshooting-in-a-distributed-systems-environment)
    - [6. Architect a Better Environment](#6-architect-a-better-environment)
      - [Part I: Overview of The OCI Specifications](#part-i-overview-of-the-oci-specifications)
      - [Part II: The OCI Image Specification](#part-ii-the-oci-image-specification)
      - [Part III: The OCI Runtime Specification](#part-iii-the-oci-runtime-specification)
      - [Part IV: The OCI Runtime Reference Implementation](#part-iv-the-oci-runtime-reference-implementation)
        - [Setup](#setup)
        - [Experiments](#experiments)
    - [7. Red Hat Container Tools](#7-red-hat-container-tools)
      - [Part I: An Overview of Container Tools](#part-i-an-overview-of-container-tools)
      - [Part II: Using the Fast and Stable Streams](#part-ii-using-the-fast-and-stable-streams)
      - [Part III: Podman: Familiar Territory](#part-iii-podman-familiar-territory)
      - [Part IV: Buildah: Granularity \& Control](#part-iv-buildah-granularity--control)
        - [Prep Work](#prep-work)
        - [Basic Build](#basic-build)
        - [Using Tools Outside The Container](#using-tools-outside-the-container)
        - [External Build Time Mounts](#external-build-time-mounts)
        - [Cleanup](#cleanup)
        - [Conclusion](#conclusion)
      - [Part V: Skopeo: Moving \& Sharing](#part-v-skopeo-moving--sharing)
        - [Remotely Inspecting Images](#remotely-inspecting-images)
        - [Pulling Images](#pulling-images)
        - [Moving Between Container Storage (Podman \& Docker)](#moving-between-container-storage-podman--docker)
        - [Moving Between Container Registries](#moving-between-container-registries)
        - [Conclusion](#conclusion-1)
      - [Part VI: CRIU: Checkpointing and Restoring](#part-vi-criu-checkpointing-and-restoring)
        - [Conclusions](#conclusions)
      - [Part VII: Udica: Custom SELinux Policies](#part-vii-udica-custom-selinux-policies)
        - [Conclusions](#conclusions-1)
      - [Part VIII: OSCAP Podman: Trust but Verify](#part-viii-oscap-podman-trust-but-verify)
        - [Conclusion](#conclusion-2)
  - [Next Up](#next-up)


## Container Fundamentals

### 1. Get Started with Containers

#### Part I: Introduction to Containers

If you understand Linux, you probably already have 85% of the knowledge you need to understand containers. If you understand how processes, mounts, networks, shells and daemons work - commands like ps, mount, ip addr, bash, httpd and mysqld - then you just need to understand a few extra primitives to become an expert with containers. Remember that all of the things that you already know today still apply: from security and performance to storage and networking, containers are just a different way of packaging and delivering Linux applications. There are four basic primitives to learn to get you from Linux administrator to feeling comfortable with containers:

* [Container Images](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.dqlu6589ootw)
* [Container Registries](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.4cxnedx7tmvq)
* [Container Hosts](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.8tyd9p17othl)
* [Container Orchestration](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo66)

Once, you understand the basic four primitives, there are some advanced concepts that will be covered in future labs including:

* Container Standards: Understanding OCI, CRI, CNI, and more
* Container Tools Ecosystem - Podman, Buildah, Skopeo, cloud registries, etc
* Production Image Builds: Sharing and collaborating between technical specialists (performance, network, security, databases, etc)
* Intermediate Architecture: Production environments
* Advanced Architecture: Building in resilience
* Container History: Context for where we are at today

Covering all of this material is beyond the scope of any live training, but we will cover the basics, and students can move on to other labs not covered in the classroom. These labs are available online at http://learn.openshift.com/subsystems.

Now, let's start with the introductory lab, which covers these four basic primitives:

![New Primitives](./assets/subsystems-container-internals-lab-2-0-part-1/assets/01-new-primitives.png)

#### Part II: Container Images

Container images are really just tar files. Seriously, they are tar files, with an associated JSON file. Together we call these an Image Bundle. The on-disk format of this bundle is defined by the [OCI Image Specification](https://github.com/opencontainers/image-spec). All major container engines including Podman, Docker, RKT, CRI-O and containerd build and consume these bundles.

![Container Images](./assets/subsystems-container-internals-lab-2-0-part-1/assets/02-basic-container-image.png)

But let's dig into three concepts a little deeper:

`1.` **Portability:** Since the OCI standard governs the images specification, a container image can be created with Podman, pushed to almost any container registry, shared with the world, and consumed by almost any container engine including Docker, RKT, CRI-O, containerd and, of course, other Podman instances. Standardizing on this image format lets us build infrastructure like registry servers which can be used to store any container image, be it RHEL 6, RHEL 7, RHEL8, Fedora, or even Windows container images. The image format is the same no matter which operating system or binaries are in the container image. Notice that Podman can download a Fedora image, uncompress it, and store it in the local /var/lib/containers image storage even though this isn't a Fedora container host:

```
podman pull quay.io/fedora/fedora
```

`2.` **Compatibility**: This addresses the content inside the container image. No matter how hard you try, ARM binaries in a container image will not run on POWER container hosts. Containers do not offer compatibility guarantees; only virtualization can do that. This compatibility problem extends to processor architecture, and also versions of the operating system. Try running a RHEL 8 container image on a RHEL 4 container host -- that isn't going to work. However, as long as the operating systems are reasonably similar, the binaries in the container image will usually run. Note that executing basic commands with a Fedora image work even though this isn't a Fedora container host:

```
podman run -t quay.io/fedora/fedora cat /etc/redhat-release
```

`3.` **Supportability**: This is what vendors can support. This is about investing in testing, security, performance, and architecture as well as ensuring that images and binaries are built in a way that they run correctly on a given set of container hosts. For example, Red Hat supports RHEL 6, UBI 7, and UBI 8 container images on both RHEL 7 and RHEL 8 container hosts (CoreOS is built from RHEL bits). Red Hat cannot guarantee that every permutation of container image and host combination on the planet will work. It would expand the testing and analysis matrix resources at a non-linear growth rate. To demonstrate, run a Red Hat Universal Base Image (UBI) container on this container host. If this was a RHEL container host, this would be completely supported (sorry, only CentOS hosts available for this lab environment :-) so not supported, but you get the point):

```
podman run -t registry.access.redhat.com/ubi7/ubi cat /etc/redhat-release
```

Analyzing portability, compatibility, and supportability, we can deduce that a RHEL 7 image will work on RHEL 7 host perfectly. The code in both were designed, compiled, and tested together. The Product Security Team at Red Hat is analyzing CVEs for this combination, performance teams are testing RHEL 7 web servers, with a RHEL 7 kernel, etc, etc. The entire machine of software creation and testing does its work in this configuration with programs and kernels compiled, built and tested together. Matching versions of container images and hosts inherit all of this work:

![Matching Container Image and Host](./assets/subsystems-container-internals-lab-2-0-part-1/assets/02-rhel7-image-rhel7-host.png)

However, there are limits. Red Hat can't guarantee that RHEL 5, Fedora, and Alpine images will work like they were intended to on a RHEL 7 host. The container image standards guarantee that the container engine will be able to ingest the images, pulling them down and caching them locally. But, nobody can guarantee that the binaries in the container images will work correctly. Nobody can guarantee that there won't be strange CVEs that show up because of the version combinations (yeah, that's "a thing"), and of course, nobody can guarantee the performance of the binaries running on a kernel for which it wasn't compiled. That said, many times, these binaries will appear to just work.

![Mismatching Container Image and Host](./assets/subsystems-container-internals-lab-2-0-part-1/assets/02-container-image-host-mismatch.png)

This leads us to supportability as a concept separate from portability and compatibility. This is the ability to guarantee to some level that certain images will work on certain hosts. Red Hat can do this between selected major versions of RHEL for the same reason that we can do it with the [RHEL Application Compatibility Guide](https://access.redhat.com/articles/rhel-abi-compatibility). We take special precautions to compile our programs in a way that doesn't break compatibility, we analyze CVEs, and we test performance. A bare minimum of testing, security, and performance can go a long way in ensuring supportability between versions of Linux, but there are limits. One should not expect that container images from RHEL 9, 10, or 11 will run on RHEL 8 hosts.

![Container Image & Host Supportability](./assets/subsystems-container-internals-lab-2-0-part-1/assets/02-container-image-host-supportability.png)

Alright, now that we have sorted out the basics of container images, let's move on to registries...

#### Part III: Container Registries

Registries are really just fancy file servers that help users share container images with each other. The magic of containers is really the ability to find, run, build, share and collaborate with a new packaging format that groups applications and all of their dependencies together.

![Container Registry](./assets/subsystems-container-internals-lab-2-0-part-1/assets/03-basic-container-registry.png)

Container images make it easy for software builders to package software, as well as provide information about how to run it. Using metadata, software builders can communicate how users *can* and *should* run their software, while providing the flexibility to also build new things based on existing software.

Registry servers just make it easy to share this work with other users. Builders can push an image to a registry, allowing users and even automation like CI/CD systems to pull it down and use it thousands or millions of times. Some registries like the [Red Hat Container Catalog](https://access.redhat.com/containers/) offer images which are highly curated, well tested, and enterprise grade. Others, like [Quay.io](http://quay.io), are cloud-based registries that give individual users public and private spaces to push their own images and share them with others. Curated registries are good for partners who want to deliver solutions together (eg. Red Hat and CrunchyDB), while cloud-based registries are good for end users collaborating on work.

As an example which demonstrates the power of sharing with quay.io, let's pull a container image that was designed and built for this lab:

```
podman pull quay.io/fatherlinux/linux-container-internals-2-0-introduction
```

Now, run this simulated database:

```
podman run -d -p 3306:3306 quay.io/fatherlinux/linux-container-internals-2-0-introduction
```

Now, poll the simulated database with our very simple client, curl:

```
curl localhost:3306
```


Notice how easy these commands were. We didn't have to know very much about how to run it. All of the complex logic for how to run it was embedded in the image. Here's the build file, so that you can inspect the start logic (ENTRYPOINT). You might not fully understand the bash code there, but that's OK, that's part of why containers are useful:

```
# Version 1

# Pull from Red Hat Universal Base Image
FROM registry.access.redhat.com/ubi7/ubi-minimal

MAINTAINER Scott McCarty smccarty@redhat.com

# Update the image
RUN microdnf -y install nmap-ncat && \
    echo "Hi! I'm a database. Get in ma bellie!!!" > /srv/hello.txt

# Output
ENTRYPOINT bash -c 'while true; do /usr/bin/nc -l -p 3306 < /srv/hello.txt; done'
```

Realizing how easy it is to build and share using registry servers is the goal of this lab. You can embed the runtime logic into the container image using a build file, thereby communicating not just *what* to run, but also *how*. You can share the container image making it easier for others to use. You can also share the build file using something like GitHub to make it easy for others to build off of your work (open source for the win).

Now, let's move on to container hosts...

#### Part IV: Container Hosts

To understand the Container Host, we must analyze the layers that work together to create a container. They include:

* [Container Engine](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo3l)
* [Container Runtime](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo55)
* [Linux Kernel](https://lwn.net/Articles/780364/)

##### Container Engine
A container engine can loosely be described as any tool which provides an API or CLI for building or running containers. This started with Docker, but also includes Podman, Buildah, rkt, and CRI-O. A container engine accepts user inputs, pulls container images, creates some metadata describing how to run the container, then passes this information to a container Runtime.

##### Container Runtime
A container runtime is a small tool that expects to be handed two things - a directory often called a root filesystem (or rootfs), and some metadata called config.json (or spec file). The most common runtime [runc](https://github.com/opencontainers/runc) is the default for every container engine mentioned above. However, there are many innovative runtimes including katacontainers, gvisor, crun, and railcar.

##### Linux Kernel
The kernel is responsible for the last mile of container creation, as well as resource management during its running lifecycle. The container runtime talks to the kernel to create the new container with a special kernel function called clone(). The runtime also handles talking to the kernel to configure things like cgroups, SELinux, and SECCOMP (more on these later). The combination of kernel technologies invoked are defined by the container runtime, but there are very recent [efforts to standardize this in the kernel](https://lwn.net/Articles/780364/).


![Container Engine](./assets/subsystems-container-internals-lab-2-0-part-1/assets/04-simple-container-engine.png)


Containers are just regular Linux processes that were started as child processes of a container runtime instead of by a user running commands in a shell. All Linux processes live side by side, whether they are daemons, batch jobs or user commands - the container engine, container runtime, and containers (child processes of the container runtime) are no different. All of these processes make requests to the Linux kernel for protected resources like memory, RAM, TCP sockets, etc.

Execute a few commands with podman and notice the process IDs, and namespace IDs. Containers are just regular processes:

```
podman ps -ls
```

```
podman top -l huser user hpid pid %C etime tty time args
```

```
ps -ef | grep 3306
```

We will explore this deeper in later labs but, for now, commit this to memory, containers are simply Linux ...

#### Part V: Container Orchestration

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


Take a quick look at this YAML file but don't don't get too worried if you don't fully understand the YAML. There are plenty of great tutorials on Kubernetes, and most people learn it over iterations, and building new applications:

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

Now, let's pull data directly from the "database."  It's the same file as we would expect, but this time coming back over port 3306:

```
curl $(oc get svc cheese-pizza -ojsonpath='{.spec.clusterIP}'):3306
```

Take a moment to note that we could fire up 50 copies of this same application in Kubernetes with 49 more commands (in different projects). It's that easy.

#### Part VI: Closing

In this lab, we have covered container images, registries, hosts, and orchestration as four new primitives you need to learn on your container journey. If you are struggling to understand why you need containers, or why need to move to orchestration - or maybe you are struggling to explain it to your management or others in your team - maybe thinking about it in this context will help:

![Container Journey](./assets/subsystems-container-internals-lab-2-0-part-1/assets/06-journey.png)

It is a journey, and we are always happy to help. If you want help along the way, here are some people to follow on Twitter:

* Scott McCarty (Product Manager): [@fatherlinux](https://twitter.com/fatherlinux)
* Dan Walsh (Containers Team Lead): [@rhatdan](https://twitter.com/rhatdan)
* Mrunal Patel (Lead for CRI-O): [@mrunalp](https://twitter.com/mrunalp)
* Nalin Dahyabhai (Lead for Buildah): [@atnalind](https://twitter.com/atnalind)
* Tom Sweeney (Core Engineer): [@TSweeneyRedHat](https://twitter.com/TSweeneyRedHat)
* Valentin Rothberg (Core Engineer): [@vlntnrthbrg](https://twitter.com/vlntnrthbrg)
* William Henry (Core Engineer): [@ipbabble](https://twitter.com/ipbabble)
* Vincent Batts: (OCI Contributor): [@vbatts](https://twitter.com/vbatts)

### 2. Manage Your Containers

#### Part I: Image Layers and Repositories

The goal of this exercise is to understand the difference between base images and multi-layered images (repositories). Also, we'll try to understand the difference between an image layer and a repository.

Let's take a look at some base images. We will use the podman history command to inspect all of the layers in these repositories. Notice that these container images have no parent layers. These are base images, and they are designed to be built upon. First, let's look at the full ubi7 base image:

```
podman history registry.access.redhat.com/ubi7/ubi:latest
```

Now, let's take a look at the minimal base image, which is part of the Red Hat Universal Base Image (UBI) collection. Notice that it's quite a bit smaller:

```
podman history registry.access.redhat.com/ubi7/ubi-minimal:latest
```

Now, using a simple Dockerfile we created for you, build a multi-layered image:

```
podman build -t ubi7-change -f ~/labs/lab2-step1/Dockerfile
```

Do you see the newly created ubi7-change tag?

```
podman images
```

Can you see all of the layers that make up the new image/repository/tag? This command even shows a short summary of the commands run in each layer. This is very convenient for exploring how an image was made.

```
podman history ubi7-change
```

Notice that the first image ID (bottom) listed in the output matches the registry.access.redhat.com/ubi7/ubi image. Remember, it is important to build on a trusted base image from a trusted source (aka have provenance or maintain chain of custody). Container repositories are made up of layers, but we often refer to them simply as "container images" or containers. When architecting systems, we must be precise with our language, or we will cause confusion to our end users.

#### Part II: Image URLs

Now we are going to inspect the different parts of the URL that you pull. The most common command is something like this, where only the repository name is specified:

```
podman inspect ubi7/ubi
```

But, what's really going on? Well, similar to DNS, the podman command line is resolving the full URL and TAG of the repository on the registry server. The following command will give you the exact same results:

```
podman inspect registry.access.redhat.com/ubi7/ubi:latest
```

You can run any of the following commands, and you will get the exact same results as well:

```
podman inspect registry.access.redhat.com/ubi7/ubi:latest
```

```
podman inspect registry.access.redhat.com/ubi7/ubi
```

```
podman inspect ubi7/ubi:latest
```

```
podman inspect ubi7/ubi
```

Now, let's build another image, but give it a tag other than "latest":

```
podman build -t ubi7:test -f ~/labs/lab2-step1/Dockerfile
```

Now, notice there is another tag.

```
podman images
```

Now try the resolution trick again. What happened?

```
podman inspect ubi7
```

It failed, but why? Try again with a complete URL:

```
podman inspect ubi7:test
```

Notice that podman resolves container images similar to DNS resolution. Each container engine is different, and Docker will actually resolve some things podman doesn't because there is no standard on how image URIs are resolved. If you test long enough, you will find many other caveats to namespace, repository, and tag resolution. Generally, it's best to always use the full URI, specifying the server, namespace, repository, and tag. Remember this when building scripts. Containers seem deceptively easy, but you need to pay attention to details.


#### Part III: Image Internals

In this exercise, we will take a look at what's inside the container image. Java is particularly interesting because it uses glibc, even though most people don't realize it. We will use the ldd command to prove it, which shows you all of the libraries that a binary is linked against. When a binary is dynamically linked (libraries loaded when the binary starts), these libraries must be installed on the system, or the binary will not run. In this example, in particular, you can see that getting a JVM to run with the exact same behavior requires compiling and linking in the same way. Stated another way, all Java images are not created equal:

```
podman run -it registry.access.redhat.com/jboss-eap-7/eap70-openshift ldd -v -r /usr/lib/jvm/java-1.8.0-openjdk/jre/bin/java
```

Notice that dynamic scripting languages are also compiled and linked against system libraries:

```
podman run -it registry.access.redhat.com/ubi7/ubi ldd /usr/bin/python
```

Inspecting a common tool like "curl" demonstrates how many libraries are used from the operating system. First, start the RHEL Tools container. This is a special image that Red Hat releases with all of the tools necessary for troubleshooting in a containerized environment. It's rather large, but quite convenient:

```
podman run -it registry.access.redhat.com/rhel7/rhel-tools bash
```

Take a look at all of the libraries curl is linked against:

```
ldd /usr/bin/curl
```

Let's see what packages deliver those libraries. It seems as if it's both OpenSSL and the Network Security Services libraries. When there is a new CVE discovered in either OpenSSL or NSS, a new container image will need to be built to patch it:

```
rpm -qf /lib64/libssl.so.10
```

```
rpm -qf /lib64/libssl3.so
```

Exit the rhel-tools container:

```
exit
```


It's a similar story with Apache and most other daemons and utilities that rely on libraries for security or deep hardware integration:

```
podman run -it registry.access.redhat.com/rhscl/httpd-24-rhel7 bash
```

Inspect mod_ssl Apache module:

```
ldd /opt/rh/httpd24/root/usr/lib64/httpd/modules/mod_ssl.so
```

Once again, we find a library provided by OpenSSL:

```
rpm -qf /lib64/libcrypto.so.10
```

Exit the httpd24 container:

```
exit
```

What does this all mean? Well, it means you need to be ready to rebuild all of your container images any time there is a security vulnerability in one of the libraries inside it.

### 3. Understand Container Registries

#### Part I: Understanding the Basics of Trust - Quality & Provenance

The goal of this exercise is to understand the basics of trust when it comes to [Registry Servers](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.4cxnedx7tmvq) and [Repositories](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.20722ydfjdj8). This requires quality and provenance - this is just a fancy way of saying that:

1. You must download a trusted thing
2. You must download from a trusted source

Each of these is necesary, but neither alone is sufficient. This has been true since the days of downloading ISO images for Linux distros. Whether evaluating open source libraries or code, prebuilt packages (RPMs or Debs), or [Container Images](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.dqlu6589ootw), we must:

1. determine if we trust the image by evaluating the quality of the code, people, and organizations involved in the project. If it has enough history, investment, and actually works for us, we start to trust it.

2. determine if we trust the registry, by understanding the quality of its relationship with the trusted project - if we download something from the offical GitHub repo, we trust it more than from a fork by user Haxor5579. This is true with ISOs from mirror sites and with image repositories built by people who aren't affiliated with the underlying code or packages.

There are [plenty of examples](https://www.infoworld.com/article/3289790/application-security/deep-container-inspection-what-the-podman-hub-minor-virus-and-xcodeghost-breach-can-teach-about-con.html) where people ignore one of the above and get hacked.  In a previous lab, we learned how to break the URL down into registry server, namespace and repository.

##### Trusted Thing

From a security perspective, it's much better to remotely inspect and determine if we trust an image before we download it, expand it, and cache it in the local storage of our container engine. Everytime we download an image, and expose it to the graph driver in the container engine, we expose ourselves to potential attack. First, let's do a remote inspect with Skopeo (can't do that with docker because of the client/server nature):

```
skopeo inspect docker://registry.fedoraproject.org/fedora
```

Examine the JSON. There's really nothing in there that helps us determine if we trust this repository. It "says" it was created by the Fedora project ("vendor": "Fedora Project") but we have no idea if that is true. We have to move on to verifying that we trust the source, then we can determin if we trust the thing.

##### Trusted Source

There's a lot of talk about image signing, but the reality is, most people are not verifying container images with signatures. What they are actually doing is relying on SSL to determine that they trust the source, then inferring that they trust the container image. Lets use this knowledge to do a quick evaluation of the official Fedora registry:

```
curl -I https://registry.fedoraproject.org
```

Notice that the SSL certificate fails to pass muster. That's because the DigiCert root CA certificate is not in /etc/pki on this CentOS lab box. On RHEL and Fedora this certficate is distributed by default and the SSL certificate for registry.fedoraproject.org passes muster. So, for this lab, you have to trust me, I tested it :-) If you were on a Fedora or Red Hat Enterprise Linux box with the right keys, the output would have looked like this:

```shell
HTTP/2 200
date: Thu, 25 Apr 2019 17:50:25 GMT
server: Apache/2.4.39 (Fedora)
strict-transport-security: max-age=31536000; includeSubDomains; preload
x-frame-options: SAMEORIGIN
x-xss-protection: 1; mode=block
x-content-type-options: nosniff
referrer-policy: same-origin
last-modified: Thu, 25 Apr 2019 17:25:08 GMT
etag: "1d6ab-5875e1988dd3e"
accept-ranges: bytes
content-length: 120491
apptime: D=280
x-fedora-proxyserver: proxy10.phx2.fedoraproject.org
x-fedora-requestid: XMHzYeZ1J0RNEOvnRANX3QAAAAE
content-type: text/html
```

Even without the root CA certificate installed, we can discern that the certicate is valid and managed by Red Hat, which helps a bit:

```
curl 2>&1 -kvv https://registry.fedoraproject.org | grep subject
```

Think carefully about what we just did. Even visually validating the certificate gives us some minimal level of trust in this registry server. In a real world scenario, rememeber that it's the container engine's job to check these certificates. That means that Systems Administrators need to distribute the appropriate CA certificates in production. Now that we have inspected the certificate, we can safely pull the trusted repository (because we trust the Fedora project built it right) from the trusted registry server (because we know it is managed by Fedora/Red Hat):

```
podman pull registry.fedoraproject.org/fedora
```

Now, lets move on to evaluate some trickier repositories and registry servers...

#### Part II: Evaluating Trust - Images and Registry Servers

The goal of this exercise is to learn how to evaluate [Container Images](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.dqlu6589ootw) and [Registry Servers](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.4cxnedx7tmvq).

##### Evaluating Images

First, lets start what we already know, there is often a full functioning Linux distro inside a container image. That's because it's useful to leverage existing packages and the dependency tree already created for it. This is true whether running on bare metal, in a virtual machine, or in a container image. It's also important to consider the quality, frequency, and ease of consuming updates in the container image.

To analyze the quality, we are going to leverage existing tools - which is another advantage of consuming a container images based on a Linux distro. To demonstrate, let's examine images from four different Linux distros - CentOS, Fedora, Ubuntu, and Red Hat Enterprise Linux. Each will provide differing levels of information:

###### CentOS

```
podman run -it docker.io/centos:7.0.1406 yum updateinfo
```

CentOS does not provide Errata for package updates, so this command will not show any information. This makes it difficult to map CVEs to RPM packages. This, in turn, makes it difficult to update the packages which are affected by a CVE. Finally, this lack of information makes it difficult to score a container image for quality. A basic workaround is to just update everything, but even then, you are not 100% sure which CVEs you patched.

###### Fedora

```
podman run -it registry.fedoraproject.org/fedora dnf updateinfo
```

Fedora provides decent meta data about package updates, but does not map them to CVEs either. Results will vary on any given day, but the output will often look something like this:

```
Last metadata expiration check: 0:00:07 ago on Mon Oct  8 16:22:46 2018.
Updates Information Summary: available
    5 Security notice(s)
        1 Moderate Security notice(s)
        2 Low Security notice(s)
    5 Bugfix notice(s)
    2 Enhancement notice(s)
```

###### Ubuntu

```
podman run -it docker.io/ubuntu:trusty-20170330 /bin/bash -c "apt-get update && apt list --upgradable"
```

Ubuntu provides information at a similar quality to Fedora, but again does not map updates to CVEs easily. The results for this specific image should always be the same because we are purposefully pulling an old tag for demonstration purposes.

###### Red Hat Enterprise Linux
```
podman run -it registry.access.redhat.com/ubi7/ubi:7.6-73 yum updateinfo security
```

Regretfully, we do not have the active Red Hat subscriptions necessary to analyze the Red Hat Universal Base Image (UBI) on the command line, but the output should look like the following if ran on RHEL or in OpenShift:

```
RHSA-2019:0679 Important/Sec. libssh2-1.4.3-12.el7_6.2.x86_64
RHSA-2019:0710 Important/Sec. python-2.7.5-77.el7_6.x86_64
RHSA-2019:0710 Important/Sec. python-libs-2.7.5-77.el7_6.x86_64
```

Notice the RHSA-***:*** column - this indicates the Errata and it's level of importnace. This errata can be used to map the update to a particular CVE, giving you and your security team confidence that a container image is patched for any particular CVE. Even without a Red Hat subscription, we can analyze the quality of a Red Hat image by looking at the Red Hat Container Cataog and using the Contianer Health Index:

- Click: [Red Hat Enterprise Universal Base Image 7](https://access.redhat.com/containers/?architecture=#/registry.access.redhat.com/ubi7/ubi/images/7.6-73)

We should see something similar to:

![Evaluating Trust](https://raw.githubusercontent.com/openshift-instruqt/instruqt/master/assets/subsystems/container-internals-lab-2-0-part-3/02-evaluating-trust.png)


##### Evaluating Registries

Now, that we have taken a look at several container images, we are going to start to look at where they came from and how they were built - we are going to evaluate four registry servers - Fedora, podmanHub, Bitnami and the Red Hat Container Catalog:

###### Fedora Registry
- Click: [registry.fedoraproject.org](https://registry.fedoraproject.org/)

The Fedora registry provides a very basic experience. You know that it is operated by the Fedora project, so the security should be pretty similar to the ISOs you download. That said, there are no older versions of images, and there is really no stated policy about how often the images are patched, updated, or released.


###### DockerHub
- Click: [https://hub.docker.com/_/centos/](https://hub.docker.com/_/centos/)

DockerHub provides "official" images for a lot of different pieces of software including things like CentOS, Ubuntu, Wordpress, and PHP. That said, there really isn't standard definition for what "official" means. Each repository appears to have their own processes, rules, time lines, lifecycles, and testing. There really is no shared understanding what official images provide an end user. Users must evaluate each repository for themselves and determine whether they trust that it's connected to the upstream project in any meaningful way.

###### Bitnami
- Click: [https://bitnami.com/containers](https://bitnami.com/containers)

Similar to podmanHub, there is not a lot of information linking these repostories to the upstream projects in any meaningful way. There is not even a clear understanding of what tags are available, or should be used. Again, not policy information and users are pretty much left to sift through GitHub repositories to have any understanding of how they are built of if there is any lifecycle guarantees about versions. You are pretty much left to just trusting that Bitnami builds containers the way you want them...


###### Red Hat Container Catalog
- Click: [https://access.redhat.com/containers](https://access.redhat.com/containers/#/registry.access.redhat.com/ubi7/ubi)

The Red Hat Container catalog is setup in a completely different way than almost every other registry server. There is a tremendous amount of information about each respository. Poke around and notice how this particular image has a warning associated. For the point of this exercise, we are purposefully looking at an older image with known
 vulnerabilities. That's because container images age like cheese, not like wine. Trust is termporal and older container images age just like servers which are rarely or never patched.

Now take a look at the Container Health Index scoring for each tag that is available. Notice, that the newer the tag, the better the letter grade. The Red Hat Container Catalog and Container Health Index clearly show you that the newer images have a less vulnerabiliites and hence have a better letter grade. To fully understand the scoring criteria, check out [Knowledge Base Article](https://access.redhat.com/articles/2803031). This is a compeltely unique capability provided by the Red Hat Container Catalog because container image Errata are produced tying container images to CVEs.

##### Summary

Knowing what you know now:
- How would you analyze these container repositories to determine if you trust them?
- How would you rate your trust in these registries?
- Is brand enough? Tooling? Lifecycle? Quality?
- How would you analyze repositories and registries to meet the needs of your company?

These questions seem easy, but their really not. It really makes you revisit what it means to "trust" a container registry and repository...

#### Part III: Analyzing Storage and Graph Drivers

In this lab, we are going to focus on how [Container Enginers](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo3l) cache [Repositories](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.20722ydfjdj8) on the container host. There is a little known or understood fact - whenever you pull a container image, each layer is cached locally, mapped into a shared filesystem - typically overlay2 or devicemapper. This has a few implications. First, this means that caching a container image locally has historically been a root operation. Second, if you pull an image, or commit a new layer with a password in it, anybody on the system can see it, even if you never push it to a registry server.

Now, let's take a look at Podman container engine. It pulls OCI compliant, docker compatible images:

```
podman info  | grep -A4 graphRoot
```

First, you might be asking yourself, [what the heck is d_type?](https://docs.docker.com/storage/storagedriver/overlayfs-driver/). Long story short, it's filesystem option that must be supported for overlay2 to work properly as a backing store for container images and running containers.

Now, pull an image and verify that the files are just mapped right into the filesystem:

```
podman pull registry.access.redhat.com/ubi7/ubi
cat $(find /var/lib/containers/storage | grep /etc/redhat-release | tail -n 1)
```

With Podman, as well as most other container engines on the planet such as Docker, image layers are mapped one for one to some kind of storage, be it thinp snapshots with devicemapper, or directories with overlay2.

This has implications on how you move container images from one registry to another. First, you have to pull it and cache it locally. Then you have to tag it with the URL, Namespace, Repository and Tag that you want in the new regsitry. Finally, you have to push it. This is a convoluted mess, and in a later lab, we will investigate a tool called Skopeo that makes this much easier.

For now, you understand enough about registry servers, repositories, and how images are cached locally. Let's move on.

### 4. Run Container Images with Hosts

#### Part I: Container Engines & The Linux Kernel

If you just do a quick Google search, you will find tons of architectural drawings which depict things the wrong way or only tell part of the story. This leads the innocent viewer to come to the wrong conclusion about containers. One might suspect that even the makers of these drawings have the wrong conclusion about how containers work and hence propogate bad information. So, forget everything you think you know.

![Containers Are Linux](https://raw.githubusercontent.com/openshift-instruqt/instruqt/master/assets/subsystems/container-internals-lab-2-0-part-4/01-google-wrong.png)

How do people get it wrong? In two main ways:

First, most of the architectural drawings above show the podman daemon as a wide blue box stretched out over the [Container Host](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.8tyd9p17othl). The [Containers](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.j2uq93kgxe0e) are shown as if they are running on top of the podman daemon. This is incorrect - [containers don't run on podman](https://crunchtools.com/so-what-does-a-container-engine-really-do-anyway/). The podman engine is an example of a general purpose [Container Engine](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo3l). Humans talk to container engines and container engines talk to the kernel - the containers are actually created and run by the Linux kernel. Even when drawings do actually show the right architecture between the container engine and the kernel, they never show containers running side by side:


Second, when drawings show containers are Linux processes, they never show the container engine side by side. This leads people to never think about these two things together, hence users are left confused with only part of the story:

![Containers Are Linux](https://raw.githubusercontent.com/openshift-instruqt/instruqt/master/assets/subsystems/container-internals-lab-2-0-part-4/01-not-the-whole-story.png)

For this lab, let’s start from scratch. In the terminal, let's start with a simple experiment - start three containers which will all run the top command:


```
podman run -td registry.access.redhat.com/ubi7/ubi top
```

```
podman run -td registry.access.redhat.com/ubi7/ubi top
```

```
podman run -td registry.access.redhat.com/ubi7/ubi top
```

Now, let's inspect the process table of the underlying host:

```
ps -efZ | grep -v grep | grep " top"
```

Notice that we started each of the ``top`` commands in containers. We started three with podman, but they are still just a regular process which can be viewed with the trusty old ``ps`` command. That's because containerized processes are just [fancy Linux processes](http://sdtimes.com/guest-view-containers-really-just-fancy-files-fancy-processes/) with extra isolation from normal Linux processes. A simplified drawing should really look something like this:

![Containers Are Linux](https://raw.githubusercontent.com/openshift-instruqt/instruqt/master/assets/subsystems/container-internals-lab-2-0-part-4/01-single-node-toolchain.png)

In the kernel, there is no single data structure which represents what a container is. This has been debated back and forth for years - some people think there should be, others think there shouldn't. The current Linux kernel community philosophy is that the Linux kernel should provide a bunch of different technologies, ranging from experimental to very mature, enabling users to mix these technologies together in creative, new ways. And, that's exactly what a container engine (Docker, Podman, CRI-O, etc) does - it leverages kernel technologies to create, what we humans call, containers. The concept of a container is a user construct, not a kernel construct. This is a common pattern in Linux and Unix - this split between lower level (kernel) and higher level (userspace) technologies allows kernel developers to focus on enabling technologies, while users experiment with them and find out what works well.

![Containers Are Linux](https://raw.githubusercontent.com/openshift-instruqt/instruqt/master/assets/subsystems/container-internals-lab-2-0-part-4/01-kernel-engine.png)

The Linux kernel only has a single major data structure that tracks processes - the process id table. The ``ps`` command dumps the contents of this data structure. But, this is not the total definition of a container - the container engine tracks which kernel isolation technologies are used, and even what data volumes are mounted. This information can be thought of as metadata which provides a definition for what we humans call a container. We will dig deeper into the technical underpinnings, but for now, understand that containerized processes are regular Linux processes which are isolated using kernel technologies like namespaces, selinux and cgroups. This is sometimes described as "sand boxing" or "isolation" or an "illusion" of virtualization.

In the end though, containerized processes are just regular Linux processes. All processes live side by side, whether they are regular Linux processes, long lived daemons, batch jobs, interactive commands which you run manually, or containerized processes. All of these processes make requests to the Linux kernel for protected resources like memory, RAM, TCP sockets, etc. We will explore this deeper in later labs, but for now, commit this to memory...

#### Part II: Step-by-Step Creation of a Container

In this step, we are going to investigate the basic construction of a container. The general contruction of a container is similar with almost all OCI compliant container engines:

1. Pull/expand/mount image
2. Create OCI compliant spec file
3. Call runc with the spec file


##### Pull/Expand/Mount Image

Let's use podman to create a container from scratch. Podman makes it easy to break down each step of the container construction for learning purposes. First, let's pull the image, expand it, and create a new overlay filesystem layer as the read/write root filesystem for the container. To do this, we will use a specially constructed container image which lets us break down the steps instead of starting all at once:

```
podman create --name on-off-container -v /mnt:/mnt:Z quay.io/fatherlinux/on-off-container
```

After running the above command we have storage created. Notice under the STATUS column, the container is the "Created" state. This is not a running container, just the first step in creation has been executed:

```
podman ps -a
```

Try to look at the storage with the mount command (hint, you won't be able to find it):

```
mount | grep -v podman | grep merged
```

Hopefully you didn't look for too long because you can't see it with the mount command. That's because this storage has been "mounted" in what's called a mount namespace. You can only see the mount from inside the container. To see the mount from outside the container, podman has a cool feature called podman-mount. This command will return the path of a directory which you can poke around in:

```
podman mount on-off-container
```

The directory you get back is a system level mount point into the overlay filesystem which is used by the container. You can literally change anything in the container's filesystem now. Run a few commands to poke around:

```
mount | grep -v podman | grep merged
```

```
ls $(podman mount on-off-container)
```

```
touch $(podman mount on-off-container)/test
```

```
ls $(podman mount on-off-container)
```

See the test file there. You will see this file in our container later when we start it and create a shell inside of it. Let's move on to the spec file...

##### Create Spec File

At this point the container image has been cached locally and mounted, but we don't actually have a spec file for runc yet. Creating a spec file from hand is quite tedious because they are made up of complex JSON with a lot of different options (governed by the OCI runtime spec). Luckily for us, the container engine will create one for us. This exact same spec file can be used by any OCI compliant runtime can consume it (runc, crun, katacontainers, gvisor, etc). Let's run some experiments to show when it's created. First let's inspect the place where it should be:

```
cat /var/lib/containers/storage/overlay-containers/$(podman ps -l -q --no-trunc)/userdata/config.json|jq .
```

The above command errors out because the container engine hasn't created the config.json file yet. We will initiate the creation of this file by using podman combined with a specially constructed container image:

```
podman start on-off-container
```

Now, the config.json file has been created. Inspect it for a while. Notice that there are options in there that are strikingly similar to the command line options of podman. The spec file really highlights the API:

```
cat /var/lib/containers/storage/overlay-containers/$(podman ps -l -q --no-trunc)/userdata/config.json|jq .
```

Podman has not started a container, just created the config.json and immediately exited. Notice under the STATUS column, that the container is now in the Exited state:

```
podman ps -a
```

In the next step, we will create a running container with runc...

##### Call Runtime

Now that we have storage and a config.json, let's complete the circuit and create a containerized process with this config.json. We have constructed a container image, which only starts a process in the container if the /mnt/on file exists. Let's create the file, and start the container again:

```
touch /mnt/on
```

```
podman start on-off-container
```

When podman started the container this time, it fired up top. Notice under the STATUS column, that the container is now in the Up state::

```
podman ps -a
```

Now, let's fire up a shell inside of our running container:

```
podman exec -it on-off-container bash
```

Now, look for the test file we created before we started the container:

```
ls -alh
```

The file is there like we would expect. You have just created a container in three basic steps. Did you know and understand that all of this was happening every time you ran a podman or podman command? Now, clean up your work:

```
exit
```

```
podman kill on-off-container
podman rm on-off-container
```

#### Part III: SELinux & sVirt: Dynamically generated contexts to protect your containers

The goal of this exercise is to gain a basic understanding of SELinux/sVirt. Run the following commands:

```
podman run -dt registry.access.redhat.com/ubi7/ubi sleep 10
podman run -dt registry.access.redhat.com/ubi7/ubi sleep 10
sleep 3
ps -efZ | grep container_t | grep sleep
```


Example Output:

```
system_u:system_r:container_t:s0:c228,c810 root 18682 18669  0 03:30 pts/0 00:00:00 sleep 10
system_u:system_r:container_t:s0:c184,c827 root 18797 18785  0 03:30 pts/0 00:00:00 sleep 10
```

Notice that each container is labeled with a dynamically generated [Multi Level Security (MLS)](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/mls) label. In the example above, the first container has an MLS label of c228,c810 while the second has a label of c184,c827. Since each of these containers is started with a different MLS label, they are prevented from accessing each other's memory, files, etc.

SELinux doesn't just label the processes, it must also label the files accessed by the process. Make a directory for data, and inspect the SELinux label on the directory. Notice the type is set to "user_tmp_t" but there are no MLS labels set:

```
mkdir /tmp/selinux-test
```

```
ls -alhZ /tmp/selinux-test/
```


Example Output:

```
drwxr-xr-x. root root system_u:object_r:container_file_t:s0:c177,c734 .
drwxrwxrwt. root root system_u:object_r:tmp_t:s0       ..
```


Now, run the following command a few times and notice the MLS labels change every time. This is sVirt at work:

```
podman run -t -v /tmp/selinux-test:/tmp/selinux-test:Z registry.access.redhat.com/ubi7/ubi ls -alhZ /tmp/selinux-test
```


Finally, look at the MLS label set on the directory, it is always the same as the last container that was run. The :Z option auto-labels and bind mounts so that the container can access and change files on the mount point. This prevents any other process from accessing this data and is done transparently to the end user.

```
ls -alhZ /tmp/selinux-test/
```

#### Part IV: Cgroups: Dynamically created with container instantiation

The goal of this exercise is to gain a basic understanding of how containers prevent using each other's reserved resources. The Linux kernel has a feature called cgroups (abbreviated from control groups) which limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of processes. Normally, these control groups would be set up by a system administrator (with cgexec), or configured with systemd (systemd-run --slice), but with a container engine, this configuration is handled automatically.

To demonstrate, run two separate containerized sleep processes:

```
podman run -dt registry.access.redhat.com/ubi7/ubi sleep 10
podman run -dt registry.access.redhat.com/ubi7/ubi sleep 10
sleep 3
for i in $(podman ps | grep sleep | awk '{print $1}' | grep [0-9]); do find /sys/fs/cgroup/ | grep $i; done
```

Notice how each containerized process is put into its own cgroup by the container engine. This is quite convenient, similar to sVirt.

#### Part V: SECCOMP: Limiting how a containerized process can interact with the kernel

The goal of this exercise is to gain a basic understanding of SECCOMP. Think of a SECCOMP as a firewall which can be configured to block certain system calls.  While optional, and not configured by default, this can be a very powerful tool to block misbehaved containers. Take a look at this sample:

```
cat ~/labs/lab3-step5/chmod.json
```

Now, run a container with this profile and test if it works.

```
podman run -it --security-opt seccomp=/root/labs/lab3-step5/chmod.json registry.access.redhat.com/ubi7/ubi chmod 777 /etc/hosts
```

Notice how the chmod system call is blocked.

### 5. Understand Container Orchestration

#### Part I: Multi-Container Workloads: The classic two-tiered, wordpress application

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

#### Part II: Inspecting & Troubleshooting: Using the OpenShift web interface

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

#### Part III: Cluster Performance: Scaling applications horizontally with containers

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

#### Part IV: Distributed Debugging: Troubleshooting in a distributed systems environment

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

### 6. Architect a Better Environment

#### Part I: Overview of The OCI Specifications

The goal of this lab is to get a basic understanding of the three Open Containers Initiative (OCI) specificaitons that govern finding, running, building and sharing container - image, runtime, and distribution. At the highest level, containers are two things - files and processes - at rest and running. First, we will take a look at what makes up a [Container Repository](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.20722ydfjdj8) on disk, then we will look at what directives are defined to create a running [Container](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.j2uq93kgxe0e).

If you are interested in a slightly deeper understanding, take a few minutes to look at the OCI  work, it's all publicly available in GitHub repositories:

- [The Image Specification Overview](//github.com/opencontainers/image-spec/blob/master/spec.md#overview)
- [The Runtime Specification Abstract](//github.com/opencontainers/runtime-spec/blob/master/spec.md)
- [The Distributions Specification Use Cases](https://github.com/opencontainers/distribution-spec/blob/master/spec.md#use-cases)

Now, lets run some experiments to better understand these specifications.

#### Part II: The OCI Image Specification

First, lets take a quick look at the contents of a container repository once it's uncompressed. We will use a utility you may have seen before called Podman. The syntax is nearly identical to Docker. Create a working directory for our experiment, then make sure the fedora image is cached locally:

```
cd /root && mkdir fedora
cd fedora
podman pull fedora
```

Now, export the image to a tar, file and extract it:

```
podman save -o fedora.tar fedora
tar xvf fedora.tar
```

Finally, let's take a look at three important parts of the container repository - these are the three major pieces that can be found in a container repository when inspected:

1. Manifest - Metadata file which defines layers and config files to be used.
2. Config - Config file which is consumed by the container engine. This config file is combined with user input specified at start time, as well as defaults provided by the container engine to create the runtime Config.json. This file is then handed to the continer runtime (runc) which communicates with the Linux kernel to start the container.
3. Image Layers - tar files which are typically typically gzipped. They are merged together when the container is run to create a root file system which is mounted.

In the Manifest, you should see one or more Config and Layers entries:

```
cat manifest.json | jq
```

In the Config file, notice all of the meta data that looks strikingly similar to command line options in Docker & Podman:

```
cat $(cat manifest.json | awk -F 'Config' '{print $2}' | awk -F '["]' '{print $3}') | jq
```

Each Image Layer is just a tar file. When all of the necessary tar files are extracted into a single directory, they can be mounted into a container's mount namespace:

```
tar tvf $(cat manifest.json | awk -F 'Layers' '{print $2}' | awk -F '["]' '{print $3}')
```

The take away from inspecting the three major parts of a container repository is that they are really just the wittiest use of tarballs ever invented. Now, that we understand what is on disk, lets move onto the runtime.

#### Part III: The OCI Runtime Specification

This specification governs the format of the file that is passed to [Container Runtime](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo55). Every OCI compliant runtime will accept this file format, including runc, crun, Kata, gVisor, Railcar, etc. Typically, this file is constructed by a [Container Engine](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo3l) such as CRI-O, Podman, containerd or Docker. These files can be created manually, but it's a tedious process. Instead, we are going to do couple of experiments so that you can get a feel for this file without having to create one manually.

Before we begin our experiments, you need to have a basic understanding of the inputs that go into creating this spec file:

1. The [Container Image](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo55) comes with a config.json which provides some input. We inspected this file in the last section on the image specification. These inputs are a combination of things provided by the image builder (Example: CMD) as well as defaults specified by the build tool (Example: Architecture). The inputs specified at build time can be thought of as a way for the image builder to communicate with the image consumer about how the image should be run.

2. The container engine itself also provides some default inputs. Some of these can be configured in the configuration for the container engine (Example: SECCOMP profiles), some are dynamically generated by the container engine (Example: sVirt/SELinux contexts, or Bind Mounts - aka the copy on write layer which gets mounted in the container's namespace), while others are hardcoded into the container engine (Example: the default namespaces to utilize).

3. The command line options specified by the user of the container engine (or robot in Kubernetes' case) can override many of the defaults provided in the image or by the container engine. Some of these are simple things like bind mounts (Example: -v /data:/data) or more complex like security options (Example: --privileged which disables a lot of technologies in the kernel).

Now, let's start with some experiments. Being the reference implementation for the runtime specifiction, runc has the ability to create a very simple spec file. Let's create one and take a quick look at the fairly simple set of directives:

```
cd /root/fedora && runc spec
cat config.json | jq
```

The simple file created by runc is a good introduction, but to truly understand the breadth of what a container engine does, we need to look at a more complex example. Podman has the ability to create a container and generate a spec file without actually starting the container:

```
podman create --name fedora -t fedora bash
podman init fedora
```

The "podman init" command generates a config.json and we can take a look at it in /var/lib/containers:

```
cat $(find /var/lib/containers/ | grep  $(podman ps --no-trunc -q | tail -n 1)/userdata/config.json) | jq
```

Take a minute to browse through the json output. See if you can spot directives which come from the container image, the container engine, and the user.

Now that we have a basic understanding of the runtime spec file, lets move on to starting a container...

#### Part IV: The OCI Runtime Reference Implementation

The goal of this lab is to learn how to use the container runtime to communicate with the Linux kernel to start a container. You will build a simple set of metadata, and start a container. This will give you insight into what the [Container Engine](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo3l) is actually doing every time you run a command.

##### Setup

To get runc to start a new container we need two main things:

1. A filesystem to mount (often called a RootFS)

2. A config.json file

First, lets create (or steal) a RootFS, which is really nothing more than a Linux distribution extracted into a directory. Podman makes this ridiculously easy to to do. The following command will fire up a container, get the ID, mount it, then rsync the filesystem contents out of it into a directory:

```
rsync -av $(podman mount $(podman create fedora bash))/ /root/fedora/rootfs/
```

We have ourselves a RootFS directory to work with, check it out:

```
ls -alh /root/fedora/rootfs
```

Now that we have a RootFS, lets create a spec file and modify it:

```
rm -rf /root/fedora/config.json
runc spec -b /root/fedora/
sed -i 's/"terminal": true/"terminal": false/' /root/fedora/config.json
```

Now, we have ourselves a full "bundle" which is a collequial way of referring to the RootFS and Config together in one directory:

```
ls -alh /root/fedora
```

##### Experiments

First, lets create an empty container. This essentially creates the user space definition for the container, but no processes are spawned yet:

```
runc create -b /root/fedora/ fedora
```

List the created containers:

```
runc list
```

Now, lets execute a bash process in the container, so that we can see what's going on. Essentially, any number of processes can be exec'ed in the same namespace and they will all have access to the same PID table, Mount table, etc:

```
runc exec --tty fedora bash
```

It looks just like a normal container you would see with Podman, Docker or CRI-O inside of Kubernetes. That's because it is:

```
ls
```

Now, get out of the container:

```
exit
```

Delete it and verify that things are cleaned up. You may notice other containers running, that may be because other containers on the system are running in CRI-O, Podman, or Docker:

```
runc delete fedora
runc list
```

In summary, we have learned how to create containers with a terse little program called runc. This is the exact same program used by every major container engine on the planet. In production, you would never create containers like this, but it's useful to understand what is going on under the hood in CRI-O, Podman and Docker. When you run into new projects like Kata, gVisor, and others, you will now understand exactly how and where they fit in into the software stack.

### 7. Red Hat Container Tools

#### Part I: An Overview of Container Tools

In this lab we're going to cover a plethora of container tools available in Red Hat Enterprise Linux (RHEL), including Podman, Buildah, Skopeo, CRIU and Udica. Before we get into the specific tools, it's important to understand how these are tools are provided to the end user in the Red Hat ecosystem.

The RHEL kernel, systemd, and the container tools, centered around github.com/containers and github.com/cri-o/cri-o, serve as the foundation for both RHEL Server as well as RHEL CoreOS. RHEL Server is a flexible, general purpose operating system which can be customized for many different use cases. On the other hand, RHEL CoreOS is a minimal, purpose built operating system intended to be consumed within automated environments like OpenShift. This lab will specifically cover the tools available in RHEL Server, but much of what you learn is useful with CoreOS which is built from the same bits, but packaged specifically for OpenShift and Edge use cases.

Here's a quick overview of how to think about RHEL Server versus RHEL CoreOS:

1. General Purpose: User -> Podman -> RHEL Server
2. OpenShift: User -> Kubernetes API -> Kubelet -> CRI-O -> RHEL CoreOS

In a RHEL Server environment, the end user will create containers directly on the container host with Podman. In an OpenShift environment, the end user will create containers through the Kubernetes API - users generally do not interact directly with CRI-O on individual hosts in the cluster. Stated another way, Podman is the primary container interface in RHEL, while Kubernetes is the primary interface in OpenShift.

For the rest of this lab, we will focus on the container tools provided in RHEL Server. The launch of RHEL8 introduced the concept of [Application Streams](https://www.redhat.com/en/blog/introduction-appstreams-and-modules-red-hat-enterprise-linux), which provide users with access to the latest versions of software like Python, Ruby, and Podman. These Application Streams have different, and often shorter life cycles than RHEL (10+ years). Specifically, RHEL8 Server provides users with two types of Application Streams for Container tools:

1. Fast: Rolling stream which is updated with new versions of Podman and other tools up to every 12 weeks, and only supported until the next version is released. This stream is for users looking for the latest features in Podman.
2. Stable: Traditional streams released once per year, and supported for 24 months. Once released these streams do not update versions of Podman and other tools, and only provide security fixes. This stream is for users looking to put Podman into production depending on stability.

With either stream, the underlying RHEL kernel, systemd, and other packages are treated as a rolling stream. The only choice is is whether to use the fast stream or one of the stable streams. Since RHEL provides a very stable [ABI/API Policy](https://access.redhat.com/articles/rhel8-abi-compatibility) the vast majority of container users will not notice and should not be concerned with kernel, systemd, glibc, etc updates on the container host. If the users selects one of the stable streams, the API to Podman will remains stable and updated for security.

For a deeper dive, check out [RHEL 8 enables containers with the tools of software craftsmanship](https://www.redhat.com/en/blog/rhel-8-enables-containers-tools-software-craftsmanship-0). Now, let's move on to installing and using these different streams of software.

#### Part II: Using the Fast and Stable Streams

As mentioned in the previous step, there are two main types of streams. To view them, run the following command:

```
yum module list | grep container-tools
```

Notice there are two types:
1. container-tools:rhel8 - this is the fast moving stream, it's updated once every 12 weeks and generally fixes bugs by rolling to new versions
2. container-tools:1.0 - this was released with RHEL 8.0 and supported for 24 months, and receives bug fixes with back ports that keep the API and CLI interfaces stable
3. container-tools:2.0 - this was released with RHEL 8.2 and supported for 24 months, and receives bug fixes with back ports that keep the API and CLI interfaces stable

Now, let's pretend we are developer looking for access to the latest features in RHEL. Let's inspect the description of the fast moving stream. Notice that there are multiple versions of the rhel8 application stream. Every time a package is updated the entire group of packages is version controlled and tested together:

```
yum module info container-tools:rhel8
```

Now, let's install the fast moving container-tools:rhel8 Application Stream like this:

```
yum module install -y container-tools:rhel8
```

We should have a whole set of tools installed:

```
yum module list --installed
```

Look at the packages that were installed as part of this Application Stream:

```
yum module repoquery --installed container-tools
```

Look at the version of Podman that was installed. It should be fairly new, probably within a few months of what's latest upstream:

```
podman -v
```

Let's clean up the environment, and start from scratch:

```
yum module remove -y container-tools
yum module reset -y container-tools
```

OK, now let's pretend we are a systems administrator or SRE that wants a set of stable tools which are supported for 24 months. First, inspect the stable stream that was released in RHEL 8.0. Notice that there are several versions of this Application Stream. Every time a package is updated a new stream version is generated to snapshot the exact versions of each package together as a stream:

```
yum module info container-tools:1.0
```

Now, install it:

```
yum module install -y --allowerasing container-tools:1.0
```

Check the version of Podman again:

```
podman -v
```

Notice that it's an older version of Podman. This version only gets back ports and will never move beyond Podman 1.0.2. Note, there is no connection between the container-tools version number and the Podman version number. It is purely coincidence that these numbers coincide. The container-tools version number is an arbitrary number representing all of the tools tested together in the Application Stream. This includes, Podman, Buildah, Skopeo, CRIU, etc.

Now, let's go back to the latest version of the container-tools for the rest of this module:

```
yum module remove -y container-tools
yum module reset -y container-tools
yum module install -y container-tools:rhel8
```

Notice how easy it was to move between the stable streams and the fast moving stream. This is the power of modularity. Now, let's move on to using the actual tools.

#### Part III: Podman: Familiar Territory

The goal of this lab is to introduce you to Podman and some of the features that make it interesting. If you have ever used Docker, the basics should be pretty familiar. Lets start with some simple commands.

Pull an image:

```
podman pull ubi8
```

List locally cached images:

```
podman images
```

Start a container and run bash interactively in the local terminal. When ready, exit:

```
podman run -it ubi8 bash
```

```
exit
```

List running containers:

```
podman ps -a
```

Now, let's move on to some features that differentiates Podman from Docker. Specifically, let's cover the two most popular reasons - Podman runs with a daemon (daemonless) and without root (rootless). Podman does not have a daemon, it's an interactive command more like bash, and like bash it can be run as a regular user (aka. rootless).

The container host we are working with already has a user called RHEL, so let's switch over to it. Note, we set a couple of environment variables manually because we're using the switch user command. These would normally be set at login:

```
su - rhel
export XDG_RUNTIME_DIR=/home/rhel
```

Now, fire up a simple container in the background:

```
podman run -id ubi8 bash
```

Now, lets analyze a couple of interesting things that makes Podman different than Docker - it doesn't use a client server model, which is useful for wiring it into CI/CD systems, and other schedulers like Yarn:

Inspect the process tree on the system:

```
pstree -Slnc
```

You should see something similar to:

```
└─conmon─┬─{conmon}
         └─bash(ipc,mnt,net,pid,uts)
```

There's no Podman process, which might be confusing. Lets explain this a bit. What many people don't know is that containers disconnect from Podman after they are started. Podman keeps track of meta-data in ~/.local/share/containers (/var/lib/containers is only used for containers started by root) which tracks which containers are created, running, and stopped (killed). The meta-data that Podman tracks is what enables a "podman ps" command to work.

In the case of Podman, containers disconnect from their parent processes so that they don't die when Podman exit exits. In the case of Docker and CRI-O which are daemons, containers disconnect from the parent process so that they don't die when the daemon is restarted. For Podman and CRI-O, there is utility which runs before runc called conmon (Container Monitor). The conmon utility disconnects the container from the engine by doing forking twice (called a double fork). That means, the execution chain looks something like this with Podman:

`bash -> podman -> conmon -> conmon -> runc -> bash`

Or like this with CRI-O:

`systemd -> crio -> conmon -> conmon -> runc -> bash`

Or like this with Docker engine:

`systemd -> dockerd -> containerd -> docker-shim -> runc -> bash`

Conmon is a very small C program that monitors the standard in, standard error, and standard out of the containerized process. The conmon utility and docker-shim both serve the same purpose. When the first conmon finishes calling the second, it exits. This disconnects the second conmon and all of its child processes from the container engine. The second conmon then inherits init system (systemd) as its new parent process. This daemonless and simplified model which Podman uses can be quite useful when wiring it into other larger systems, like CI/CD, scripts, etc.

Podman doesn't require a daemon and it doesn't require root. These two features really set Podman apart from Docker. Even when you use the Docker CLI as a user, it connects to a daemon running as root, so the user always has the ability escalate a process to root and do whatever they want on the system. Worse, it bypasses sudo rules so it's not easy to track down who did it.

Now, let's move on to some other really interesting features. Rootless containers use a kernel feature called User Namespaces. This maps the one or more user IDs in the container to one or more user IDs outside of the container. This includes the root user ID in the container as well as any others which might be used by programs like Nginx or Apache.

Podman makes it super easy to see this mapping. Start an nginx container to see the user and group mapping in action:


```
podman run -id registry.access.redhat.com/rhscl/nginx-114-rhel7 nginx -g 'daemon off;'
```

Now, execute the Podman bash command:

```
podman top -l args huser hgroup hpid user group pid seccomp label
```

Notice that the host user, group and process ID  ''in'' the container all map to different and real IDs on the host system. The container thinks that nginx is running as the user ''default'' and the group ''root'' but really it's running as an arbitrary user and group. This user and group are selected from a range configured for the ''rhel'' user account on this system. This list can easily be inspected with the following commands:

```
cat /etc/subuid
```

You will see something similar to this:

```
rhel:165536:65536
```

The first number represents the starting user ID, and the second number represents the number of user IDs which can be used from the starting number. So, in this example, our RHEL user can use 65,535 user IDs starting with user ID 165536. The Podman bash command should show you that nginx is running in this range of UIDs.

The user ID mappings on your system might be different because shadow utilities (useradd, usderdel, usermod, groupadd, etc) automatically creates these mappings when a user is added. As a side note, if you've updated from an older version of RHEL, you might need to add entries to /etc/subuid and /etc/subgid manually.

OK, now stop all of the running containers. No more one liners like with Docker, it's just built in with Podman:

```
podman kill --all
```

Remove all of the actively defined containers. It should be noted that this might be described as deleting the copy-on-write layer, config.json (commonly referred to as the Config Bundle) as well as any state data (whether the container is defined, running, etc):

```
podman rm --all
```

We can even delete all of the locally cached images with a single command:

```
podman rmi --all
```

The above commands show how easy and elegant Podman is to use. Podman is like a Chef's knife. It can be used for pretty much anything that you used Docker for, but let's move on to Builah and show some advanced use cases when building container images.

#### Part IV: Buildah: Granularity & Control

The goal of this lab is to introduce you to Buildah and the flexibility it provides when you need to build container images your way. There are a lot of different use cases that just "feel natural" when building container images, but you often, you can't quite wire together and elegant solutions with the client server model of existing container engines. In comes Buildah. To get started, lets introduce some basic decisions you need to think through when building a new container image.

1. Image vs. Scratch: Do you want to start with an existing container image as the source for your new container image, or would you prefer to build completely from scratch? Source images are the most common route, but it can be nice to build from scratch if you have small, statically linked binaries.

2. Inside vs. Outside: Do you want to execute the commands to build the next container image layer inside the container, or would you prefer to use the tools on the host to build the image? This is completely new concept with Buildah, but with existing container engines, you always build from within the container. Building outside the container image can be useful when you want to build a smaller container image, or an image that will always be ran read only, and never built upon. Things like Java would normally be built in the container because they typically need a JVM running, but installing RPMs might happen from outside because you don't want the RPM database in the container.

3. External vs. Internal Data: Do you have everything you need to build the image from within the image? Or, do you need to access cached data outside of the build process? For example, It might be convenient to mount a large cached RPM cache inside the container during build, but you would never want to carry that around in the production image. The use cases for build time mounts range from SSH keys to Java build artifacts - for more ideas, see this [GitHub issue](https://github.com/moby/moby/issues/14080).

Alright, let's walk through some common scenarios with Buildah.

##### Prep Work
Just like Podman, Buildah can execute in rootless mode, but since you have tools on the container host interacting files in the container image, you need to make Buildah think it's running as root. Buildah comes with a cool sub-command called unshare which does just this. It puts our shell into a user namespace just like when you have a root shell in a container. The difference is, this shell has access to tools installed on the container host, instead of in the container image. Before we complete the rest of this lab, execute the "buildah unshare" command. Think of this as making yourself root, without actually making yourself root:

```
buildah unshare
```

Now, look at who your shell thinks you are:

```
whoami
```

It's looks like you are root, but you really aren't, but let's prove it:

```
touch /etc/shadow
```

The touch command fails because you're not actually root. Really, the touch command executed as an arbitrary user ID in your /etc/subuid range. Let that sink in. Linux containers are mind bending. OK, let's do something useful.

##### Basic Build

First declare what image you want to start with as a source. In this case, we will start with Red Hat Universal Base Image:

```
buildah from ubi8
```

This will create a "reference" to what Buildah calls a "working container" - think of them as a starting point to attach mounts and commands. Check it out here:

```
buildah containers
```

Now, we can mount the image source. In effect, this will trigger the graph driver to do its magic, pull the image layers together, add a working copy-on-write layer, and mount it so that we can access it just like any directory on the system:

```
buildah mount ubi8-working-container
```

Now, lets add a single file to the new container image layer. The Buildah mount command can be ran again to get access to the right directory:

```
echo "hello world" > $(buildah mount ubi8-working-container)/etc/hello.conf
```

Lets analyze what we just did. It's super simple, but kind of mind bending if you come from using other container engines. First, list the directory in the copy-on-write layer:

```
ls -alh $(buildah mount ubi8-working-container)/etc/
```

You should see hello.conf right there. Now, cat the file:

```
cat $(buildah mount ubi8-working-container)/etc/hello.conf
`````

You should see the text you expect. Now, lets commit this copy-on-write layer as a new image layer:

```
buildah commit ubi8-working-container ubi8-hello
```

Now, we can see the new image layer in our local cache. We can view it with either Podman or Buildah (or CRI-O for that matter, they all use the same image store):

```
buildah images
```

```
podman images
```

When we are done, we can clean up our environment quite nicely. The following command will delete references to "working containers" and completely remove their mounts:

```
buildah delete -a
```

But, we still have the new image layer just how we want it. This could be pushed to a registry server to be shared with others if we like:

```
buildah images
```

```
podman images
```

##### Using Tools Outside The Container

Create a new working container, mount the image, and get a working copy-on-write layer:

```
WORKING_MOUNT=$(buildah mount $(buildah from scratch))
echo $WORKING_MOUNT
```

Verify that there is nothing in the directory:

```
ls -alh $WORKING_MOUNT
```

Now, lets install some basic tools:

```
yum install --installroot $WORKING_MOUNT bash coreutils --releasever 8 --setopt install_weak_deps=false -y
yum clean all -y --installroot $WORKING_MOUNT --releasever 8
```

Verify that some files have been added:

```
ls -alh $WORKING_MOUNT
```

Now, commit the copy-on-write layer as a new container image layer:

```
buildah commit working-container minimal
```

Now, test the new image layer, by creating a container:

```
podman run -it minimal bash
```

```
exit
```

Clean things up for our next experiment:

```
buildah delete -a
```

We have just created a container image layer from scratch without ever installing RPM or YUM. This same pattern can be used to solve countless problems. Makefiles often have the option of specifying the output directory, etc. This can be used to build a C program without ever installing the C toolchain in a container image layer. This is best for production security where we don't want the build tools laying around in the container.

##### External Build Time Mounts

As a final example, lets use a build time mount to show how we can pull data in. This will represent some sort of cached data that we are using outside of the container. This could be a repository of Ansible Playbooks, or even Database test data:

```
mkdir ~/data
dd if=/dev/zero of=~/data/test.bin bs=1MB count=100
ls -alh ~/data/test.bin
```

Now, lets fire up a working container:

```
buildah from ubi8
buildah mount ubi8-working-container
```

To consume the data within the container, we use the buildah-run subcommand. Notice that it takes the -v option just like "run" in Podman. We also use the Z option to relabel the data for SELinux. The dd command simply represents consuming some smaller portion of the data during the build process:

```
buildah run -v ~/data:/data:Z ubi8-working-container dd if=/data/test.bin of=/etc/small-test.bin bs=100 count=2
```

Commit the new image layer and clean things up:

```
buildah commit ubi8-working-container ubi8-data
buildah delete -a
```

Test it and note that we only kept the pieces of the data that we wanted. This is just an example, but imagine using this with a Makefile cache, or Ansible playbooks, or even a copy of production database data which needs to be used to test the image build or do a schema upgrade, which must be accessed during the image build process. There are tons of places where you need to access data, only at build time, but don't want it during production deployment:

```
podman run -it ubi8-data ls -alh /etc/small-test.bin
```

##### Cleanup

Exit the user namespace:

```
exit
```

##### Conclusion

Now, you have a pretty good understanding of the cases where Buildah really shines. You can start from scratch, or use an existing image, use tools installed on the container host (not in the container image), and move data around as needed. This is a very flexible tool that should fit quite nicely in your tool belt. Buildah lets you script builds with any language you want, and build tiny images with only the bare minimum of utilities needed inside the image.

Now, lets move on to sharing containers with Skopeo...

#### Part V: Skopeo: Moving & Sharing

In this step, we are going to do a couple of simple exercises with Skopeo to give you a feel for what it can do. Skopeo doesn't need to interact with the local container storage (.local/share/containers), it can move directly between registries, between container engine storage, or even directories.

##### Remotely Inspecting Images

First, lets start with the use case that kicked off the Skopeo project. Sometimes, it's really convenient to inspect an image remotely before pulling it down to the local cache. This allows us to inspect the meta-data of the image and see if we really want to use it, without synchronizing it to the local image cache:

```
skopeo inspect docker://registry.fedoraproject.org/fedora
```

We can easily see the "Architecture" and "Os" meta-data which tells us a lot about the image. We can also see the labels, which are consumed by most container engines, and passed to the runtime to be constructed as environment variables. By comparison, here's how to see this meta-data in a running container:

```
podman run --name meta-data-container -id registry.fedoraproject.org/fedora bash
podman inspect meta-data-container
```

##### Pulling Images

Like, Podman, Skopeo can be used to pull images down into the local container storage:

```
skopeo copy docker://registry.fedoraproject.org/fedora containers-storage:fedora
```

But, it can also be used to pull them into a local directory:

```
skopeo copy docker://registry.fedoraproject.org/fedora dir:$HOME/fedora-skopeo
```

This has the advantage of not being mapped into our container storage. This can be convenient for security analysis:

```
ls -alh ~/fedora-skopeo
```

The Config and Image Layers are there, but remember we need to rely on a [Graph Driver](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.kvykojph407z) in a [Container Engine](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/#h.6yt1ex5wfo3l) to map them into a RootFS.

##### Moving Between Container Storage (Podman & Docker)

First, let's do a little hack to install Docker CE side by side with Podman on RHEL 8. Don't do this on a production system as this will overwrite the version of runc provided by Red Hat:

```
yes|sudo rpm -ivh --nodeps --force https://download.docker.com/linux/centos/8/x86_64/stable/Packages/containerd.io-1.3.7-3.1.el8.x86_64.rpm
sudo yum install -y docker-ce
```

Now, enable the Docker CE service:

```
sudo systemctl enable --now docker
```

Now that we have Docker and Podman installed side by side with the Docker daemon running, lets copy an image from Podman to Docker. Since we have the image stored locally in .local/share/containers, it's trivial to copy it to /var/lib/docker using the daemon:

```
skopeo copy containers-storage:registry.fedoraproject.org/fedora docker-daemon:registry.fedoraproject.org/fedora:latest
```

Verify that the repository is now in the Docker CE cache:

```
docker images | grep registry.fedoraproject.org
```

This can be useful when testing and getting comfortable with other OCI complaint tools like Podman, Buildah, and Skopeo. Sometimes, you aren't quite ready to let go of what you know so having them side by side can be useful. Remember though, this isn't supported because it replaces the runc provided by Red Hat.

##### Moving Between Container Registries

Finally, lets copy from one registry to another. I have set up a writeable repository under my username (fatherlinux) on quay.io. To do this, you have to use the credentials provided below. Notice, that we use the "--dest-creds" option to authenticate. We can also use the "--source-cred" option to pull from a registry which requires authentication. This tool is very flexible. Designed by engineers, for engineers.

```
skopeo copy docker://registry.fedoraproject.org/fedora docker://quay.io/fatherlinux/fedora --dest-creds fatherlinux+fedora:5R4YX2LHHVB682OX232TMFSBGFT350IV70SBLDKU46LAFIY6HEGN4OYGJ2SCD4HI
```

This command just synchronized the fedora repository from the Fedora Registry to Quay.io without ever caching it in the local container storage. Very cool right?

Finally, exit the ''rhel'' user because we need root for the next lab:

```
exit
```

##### Conclusion

You have a new tool in your tool belt for sharing and moving containers. Hopefully, you find other uses for Skopeo.

#### Part VI: CRIU: Checkpointing and Restoring

With some help from a program called CRIU, Podman can checkpoint and restore containers on the same host. This can be useful with workloads that have a long startup period or require a long time to warm up caches. For example, large memcached servers, database, or even Java workloads can take several minutes or even hours to reach maximum throughput performance. This is often referred to as cache warming.

If this doesn't quite make sense, let's talk about it in the context of container creation and deletion. Podman allow you to break the creation and deletion of containers down into very granular steps. Here's what the life cycle of a container looks like from start to finish:

1. podman pull - Pull the container image
2. podman create - Add tracking meta-data to /var/lib/containers or .local/share/containers
3. podman mount - Create a copy-on-write layer and mount the container image with a read/write layer above it
4. podman init - Create a config.json file
5. podman start - Run the workload by handing the config.json and root file system to runc
6. Workload runs either as a batch process, or as a daemon
7. podman kill - kills the process or processes in the container
8. podman rm - Unmount and delete the copy-on-write layer
9. podman rmi - remove the image /var/lib/containers or .local/share/containers

To understand CRIU, you need to understand step 6. When this step is executed, Podman sends a kill signal to the processes in the container. CRIU allows us to break this down even further like this:


1. podman pull - Pull the container image
2. podman create - Add tracking meta-data to /var/lib/containers or .local/share/containers
3. podman mount - Create a copy-on-write layer and mount the container image with a read/write layer above it
4. podman init - Create a config.json file
5. podman start - Run the workload by handing the config.json and root file system to runc
6. Workload runs either as a batch process, or as a daemon
7. podman checkpoint - Dump contents of memory to disk and kill processes
8. Workload process no longer running, memory contents are saved on disk
9. podman restore - Restore memory contents to new processes
10. Workload runs either as a batch process, or as a daemon
11. podman kill - kills the process or processes in the container
12. podman rm - Unmount and delete the copy-on-write layer
13. podman rmi - remove the image /var/lib/containers or .local/share/containers

So, in a nutshell, CRIU gives you more flexibility with containerized processes. Let's see it in action. First, start a simple container which generates incrementing numbers so that we can verify memory contents are really restored:

```
podman run -d --name looper ubi8 /bin/sh -c \
         'i=0; while true; do echo $i; i=$(expr $i + 1); sleep 1; done'
```

Now, verify that numbers are being generated. Run this a few times to see the numbers incrementing:

```
podman logs -l
```

Now, let's dump the contents of memory to disk, and kill the process:

```
podman container checkpoint -l
```

Verify that it's not running. Notice that that container is in the exited state. This means the copy-on-write layer for the container has not been deleted. Since we used the checkpoint sub-command, the contents of memory are also saved on disk:

```
podman ps -a
```

Verify that numbers are not being generated. Run this a few times to verify:

```
podman logs -l
```

Restore the container:

```
podman container restore -l
```

Verify the contents of memory and disk are being used and the numbers are incrementing again:

```
podman logs -l
```

We're all done, so clean up. This will kill the process, delete the contents of the copy-on-write layer, and remove all of the meta-data for all containers:

```
podman kill -a
```

##### Conclusions

Checkpointing and restoring containers is easy with CRIU and Podman. As part of the container-tools application streams, specific versions of Podman and CRIU are tested and verified to work together (not all versions of Podman and CRIU are guaranteed to work together). Now, let's move on to some more tools.

#### Part VII: Udica: Custom SELinux Policies

The default Containers SELinux policy does a really good job Podman in RHEL 8. Most containers "just work" but like any security tool, every now and then we need to make some customizations. Sometimes, especially as we expand the types of workloads that we run in containers, we bump into places where SELinux blocks us. Udica allows an administrator to customize the SELinux policy specifically for a workload for without being an SELinux expert.

For example, run the following container:

```
podman run --name home-test -v /home/:/home:ro -it ubi8 ls /home
```

The above command will fail because the default SELinux policy does not allow containers to mount /home as read only. We can verify that there are no allow rules which permit this command to be executed:

```
sesearch -A -s container_t -t home_root_t -c dir -p read
```

With Udica, we can quickly and easily configure SELinxux to allow us to mount /home as root. First, we have to extract the meta-data from our container:

```
podman inspect home-test > home-test.json
```

Now, Udica will analyze this data and create a custom SELinux policy for us:

```
udica -j home-test.json home_test
```

Use the SELinux tools to load the new policy:

```
semodule -i home_test.cil /usr/share/udica/templates/{base_container.cil,home_container.cil}
```

Now, run the same type of container again, but pass it a security option telling it to label the process to use our new custom policy, and it will execute without being blocked. First start the contaier:

```
podman run --name home-test-2 --security-opt label=type:home_test.process -v /home/:/home:ro -id ubi8 bash
```

Execute the ''ls'' command:

```
podman exec -it home-test-2 ls /home
```

You will notice that the process is running with the "home_test.process" SELinux context:

```
ps -efZ | grep home_test
```

We can also verify that there is a new rule in this policy to allow our container to mount /home read only:

```
sesearch -A -s home_test.process -t home_root_t -c dir -p read
```

##### Conclusions

It's always best to have SELinux enabled, especially with containers. It's so easy to create a custom SELinux policy with Udica, that you should never disable it. If you'd like to understand Udica a bit deeper, check out this great article, [Use udica to build SELinux policy for containers](https://fedoramagazine.org/use-udica-to-build-selinux-policy-for-containers/) by Lukas Vrabec. Now, let's move on to another tool.

#### Part VIII: OSCAP Podman: Trust but Verify

In this lab we're going to demonstrate using the [oscap-podman](https://github.com/OpenSCAP/openscap/blob/master/utils/oscap-podman) command to verify the security of the [Red Hat Universal Base Image](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image). Red Hat UBI is a rock solid, freely distributable container base image created by Red Hat and build on RHEL bits (see also: [Comparison of Container Images](http://crunchtools.com/comparison-linux-container-images/). UBI is rebuilt every six weeks or every time there is a Critical or Important [CVE](https://www.redhat.com/en/topics/security/what-is-cve)(Common Vulnerabilities and Exposures) patched and is the foundation for all containerized products at Red Hat. See also [UBI FAQ](https://developers.redhat.com/articles/ubi-faq).

While UBI is a great foundation, some of us are skeptical and like our vendors to show their work. Luckily, the Red Hat Product Security Team does just that using OVAL data. For those that have never heard of this, OVAL stands for The Open Vulnerability and Assessment Language project, which maintained by The MITRE Corporation (same group that does CVEs), an international, information security effort that promotes open and publicly available security content, and seeks to standardize the transfer of this information across the entire spectrum of security tools and services. Red Hat Product Security helps customers evaluate and manage risk by tracking and investigating all security issues affecting Red Hat customers and providing timely and concise patches and security advisories via the Red Hat Customer Portal creating and supporting OVAL patch definitions, providing a machine-readable versions of our security advisories. This allows OVAL-compatible tools to test for the presence of vulnerabilities for which Red Hat has released security updates that could be applied to the system.

You can see that data here: https://www.redhat.com/security/data/oval/v2/RHEL8/

Before we begin the lab, download the latest OVAL data from Red Hat Product Security:

```
wget https://www.redhat.com/security/data/oval/v2/RHEL8/rhel-8.oval.xml.bz2
```

Now, we're going to use the oscap-podman tool to consume the data and compare some versions of the Red Hat Universal Base Image which we have stored locally in the Podman cache. Container images age like cheese, not like wine. Older container images which haven't been rebuilt will build up CVEs over time. From the Red Hat Ecosystem Catalog, we can see that the [UBI 8.0-126 image](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e?tag=8.0-126&push_date=1560882955000) which was build June 11th, 2019 has a grade of F and at the time of this writing had 3 important security vulnerabilities.

Check it out here: [UBI 8.0-126 image](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e?tag=8.0-126&push_date=1560882955000)

Now, let's take a more detailed look, using the oscap-podman command:

```
oscap-podman registry.access.redhat.com/ubi8/ubi:8.0-126 oval eval --report ./assets/html/ubi-8.0-126-report.html rhel-8.oval.xml.bz2
```

This will create a report. We've already set up a web server (in a container) so that you can quickly look at the  *OSCAP Report 8.0-126* Tab.

Notice that there are many orange lines displaying the CVEs/RHSAs. If you built and ran an application on this very old container image, it would be exposed to these vulnerabilities. Now, let's scan the latest version of UBI provided by Red Hat:

```
oscap-podman registry.access.redhat.com/ubi8/ubi:latest oval eval --report ./assets/html/ubi-latest-report.html rhel-8.oval.xml.bz2
```

Look at the new report from thr *OSCAP Report latest* Tab.
##### Conclusion

In general you should almost never see any unapplied patches in this latest build of UBI. If you do, it's like a small window between the time that Red Hat Product Security has issued a CVE, and our product team has rebuilt and published the UBI image. The oscap-podman command allows you to verify this and is useful in "trust but verify" operations like CI/CD system tests. For more information and deeper dive on OSCAP-Podman, check out this great video by Brian Smith: [Scanning Containers for Vulnerabilities on RHEL 8.2 With OpenSCAP and Podman](https://www.youtube.com/watch?v=nQmIcK1vvYc)

## Next Up

Congratulations! You just completed the first module of today's workshop. Please continue to [Developing on OpenShift](./assets/developing-on-openshift-getting-started.md).