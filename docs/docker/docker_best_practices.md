# Docker Best Practices
 
This article provides you with important information about DO's and DON'Ts regarding Docker.

## Before we start

There are excellent guides about Docker best practices available on the internet (see [References](#references) below).
You should read them at least once.

If you know some more best practices, you are welcome to share them via this documentation.

So here's my opinionated short list of best practices.

## Try to keep your docker images small

Well folks, even these days size really matters: 
building a smaller image can reduce upload times and download times of an image significantly which will result in faster builds and faster deployments. 
Besides, in most public Docker registries or private Docker registries in enterprises you will have to pay for the 
storage you consume with your Docker images.

Check out sections 
[How to keep your images small](https://docs.docker.com/develop/dev-best-practices/#how-to-keep-your-images-small),
[Use multistage builds](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-multi-stage-builds), 
[Minimize the number of layers](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#minimize-the-number-of-layers)
and 
[Build the smallest image possible](https://cloud.google.com/solutions/best-practices-for-building-containers#build-the-smallest-image-possible)
for recommendations.

## Never run more than one application inside your docker container

It's a common mistake to treat containers as virtual machines that can run many different things simultaneously. Although
your container my work that way, you will create a strong dependency among all processes which run inside your container:
if the container dies or is stopped, all processes will terminate as well. Prefer to package each application as separate
docker images instead.

Here's an example for a simple scenario: a full-stack application consisting of an Angular frontend plus a Java backend 
plus a PostgreSQL database will result in three separate images during build time or three containers during runtime.

Checkout sections
[Package a single app per container](https://cloud.google.com/solutions/best-practices-for-building-containers#package_a_single_app_per_container)
and
[Decouple applications](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#decouple-applications)
for further information.

## Always use a non-root user to run software inside your docker containers
 
In general, it's never a good idea to run software with root user privileges: if an attacker hijacks an application
which is running in a root user context, he can use these root user privileges to attack the system which hosts the application.
So always add a non-root user to your images and use the USER statement in your Dockerfile to switch to this non-root user.
Any software running in a Docker container based on this Docker image will be executed within the context of the 
non-root user.

!!! danger "Most container runtimes will refuse to start privileged containers"
    Enforcing usage of non-root users is actually the default configuration of most container runtimes like OpenShift.
    Although Kubernetes does not prevent privileged containers from starting by default, it is configured to do so in most enterprise
    scenarios.

Checkout sections
[USER in Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user) ,
[Avoid privileged containers](https://cloud.google.com/solutions/best-practices-for-operating-containers#avoid_privileged_containers)
and 
[Avoid running as root](https://cloud.google.com/solutions/best-practices-for-operating-containers#avoid_running_as_root)
for instructions.

## Add proper Labels to your Docker Image

Docker supports labeling of images. These labels become part of the binary image and can be retrieved through image inspection.
Labels are the perfect choice to attach meaningful metadata to your image. Each image should come with the following set of labels:

* __maintainer__: name or email address of the user or team that built this image
* __version__: semantic version number of this image if not specified in a tag
* __documentation__: URL of a public accessible web page providing documentation of this image
* __repo-url__: URL of the git repo which contains the source code of this image
* __branch__: git branch from which this image was built
* __commit-hash__: git commit hash of the source code which was used to build this image
* __product/project__: name of the product or project that owns this image

Checkout sections
[Docker object labels](https://docs.docker.com/config/labels-custom-metadata/) 
for instructions and recommendations.

## Tag your docker images properly

Tag are the only means to identify a specific version of a docker image. Thus, you definitely should consider using 
semantic versioning when tagging docker images.
   
Checkout sections
[Properly tag your images](https://cloud.google.com/solutions/best-practices-for-building-containers#properly_tag_your_images)
for further information.

## Avoid using tag `latest`

Although using the tag `latest` in your Dockerfiles or in your references to docker images is very convenient, it may lead
to nasty problems: `latest` always refers to the latest version of a docker image. Since the latest version of an image
will change during time, you introduce unwanted side effects into your docker build which may result in unstable or 
unreproducible build results.

That being said you should always:

* use an explicit version of an image in the `FROM` clause of your Dockerfile
* tag your images with an explicit version before pushing them
* specify a specific version of an image when executing `docker run`
* refer to a specific version of an image in your `docker-compose.yaml` files or Kubernetes deployment manifests 

 
## Always scan your docker images for security vulnerabilities

Docker images contain snapshots of system libraries plus all packages and binaries that you added yourself during build time.
These libraries, packages and binaries may contain security vulnerabilities which can be exploited by an attacker. 
Fortunately, there are tools like [Clair](https://github.com/quay/clair) which can perform a static analysis of images
detecting known security vulnerabilities. Most commercial or enterprise-grade docker registries include an image scanner
but in most cases there is no common process what do to when a vulnerability is found.

You should stick to the following recommendations:

* At least scan all images in your docker registry for vulnerabilities on a regular base and notify the image owners about findings.
* Configure your docker registry to scan images when they are pushed to the registry: if a vulnerability is detected, the push fails.
* Scan your images in your build pipeline before you push them: if a vulnerability is detected, the build fails.
* Configure your container runtime environment to scan images before they are started as containers: in case of vulnerabilities, the deployment fails.
   
Checkout sections
[Use vulnerability scanning in Container Registry](https://cloud.google.com/solutions/best-practices-for-building-containers#use-vulnerability-scanning)
for further information.


## References

| Link | Description |
| --- | --- |
[Docker development best practices](https://docs.docker.com/develop/dev-best-practices/) | Official Docker documentation 
[Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) | Official Docker documentation about Dockerfiles in particular 
[Intro Guide to Dockerfile Best Practices](https://www.docker.com/blog/intro-guide-to-dockerfile-best-practices/) | Blog on best practices by a Docker employee
[Best practices for building containers](https://cloud.google.com/solutions/best-practices-for-building-containers) | Googles view on best practices when building Docker images
[Best practices for operating containers](https://cloud.google.com/solutions/best-practices-for-operating-containers) | Googles view on best practices when running Docker containers