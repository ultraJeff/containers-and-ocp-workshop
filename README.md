# Containers and OpenShift Workshop

Welcome to this workshop on containers and OpenShift! Today, we will use a hosted OCP environment provided by your instructor, along with these instructions, as we journey from simple Linux containers to advanced container orchestration using Red Hat OpenShift. Let's begin!

## Working with Containers

### Introduction

#### Part I

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

![New Primitives](./subsystems-container-internals-lab-2-0-part-1/assets/01-new-primitives.png)

#### Part II

Container images are really just tar files. Seriously, they are tar files, with an associated JSON file. Together we call these an Image Bundle. The on-disk format of this bundle is defined by the [OCI Image Specification](https://github.com/opencontainers/image-spec). All major container engines including Podman, Docker, RKT, CRI-O and containerd build and consume these bundles.

![Container Images](./subsystems-container-internals-lab-2-0-part-1/assets/02-basic-container-image.png)

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

![Matching Container Image and Host](./subsystems-container-internals-lab-2-0-part-1/assets/02-rhel7-image-rhel7-host.png)

However, there are limits. Red Hat can't guarantee that RHEL 5, Fedora, and Alpine images will work like they were intended to on a RHEL 7 host. The container image standards guarantee that the container engine will be able to ingest the images, pulling them down and caching them locally. But, nobody can guarantee that the binaries in the container images will work correctly. Nobody can guarantee that there won't be strange CVEs that show up because of the version combinations (yeah, that's "a thing"), and of course, nobody can guarantee the performance of the binaries running on a kernel for which it wasn't compiled. That said, many times, these binaries will appear to just work.

![Mismatching Container Image and Host](./subsystems-container-internals-lab-2-0-part-1/assets/02-container-image-host-mismatch.png)

This leads us to supportability as a concept separate from portability and compatibility. This is the ability to guarantee to some level that certain images will work on certain hosts. Red Hat can do this between selected major versions of RHEL for the same reason that we can do it with the [RHEL Application Compatibility Guide](https://access.redhat.com/articles/rhel-abi-compatibility). We take special precautions to compile our programs in a way that doesn't break compatibility, we analyze CVEs, and we test performance. A bare minimum of testing, security, and performance can go a long way in ensuring supportability between versions of Linux, but there are limits. One should not expect that container images from RHEL 9, 10, or 11 will run on RHEL 8 hosts.

![Container Image & Host Supportability](./subsystems-container-internals-lab-2-0-part-1/assets/02-container-image-host-supportability.png)

Alright, now that we have sorted out the basics of container images, let's move on to registries...

#### Part III
#### Part IV
#### Part V
#### Part VI

### Images
#### Part I
#### Part II
#### Part III

### Trust
#### Part I
#### Part II
#### Part III

### Container Engine
#### Part I
#### Part II
#### Part III
#### Part IV
#### Part V

### Deploying

### Something Else? (Section 6)

### Review