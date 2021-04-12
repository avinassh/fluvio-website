---
title: Installing Fluvio Locally
menu: Install Fluvio Locally
toc: true
weight: 40
---

Fluvio is built to run on [Kubernetes], an open-source system that automates the
deployment of containerized apps. If you haven't heard of Kubernetes or containers,
just think of them as tools that help run applications on many computers at once.
To get started with Fluvio locally, we'll need to install a version of Kubernetes 
called Minikube which is meant for testing out Kubernetes apps locally.

[Kubernetes]: https://kubernetes.io/

## Prerequisites

Before getting started, make sure you have the [Fluvio CLI] installed, as
we'll be using that to interact with your cluster and make sure everything
is working as expected.

[Fluvio CLI]: ../

{{< idea >}}

**Note**: If you installed Fluvio prior to version `0.7.4` and then upgraded,
you may need to run `fluvio install fluvio-run` in order to continue. If you can
run `fluvio run -h` and see a help menu, you're good to go 👍

{{</ idea >}}

## Required Packages

1) [Install Docker](#installing-docker)
2) [Install Minikube](#installing-minikube)
3) [Install Kubectl](#installing-kubectl)
4) [Install Helm](#installing-helm)

If you already have `docker`, `kubectl`, `helm`, and `minikube` set up, feel free
to scroll down to the [installing fluvio] section.

[installing fluvio]: #installing-fluvio

### Installing Docker

Docker is a container engine that Minikube can use to run Kubernetes apps. Go to
the [Docker downloads page] and choose the edition built for your OS. Follow all
of the installation steps, then verify that you're able to run `docker` without
using sudo:

```bash
$ docker run hello-world
```

~> **Note**: On Linux, make sure you run the [post-install steps] or this won't work without sudo

[Docker downloads page]: https://hub.docker.com/search?q=&type=edition&offering=community&sort=updated_at&order=desc
[post-install steps]: https://docs.docker.com/engine/install/linux-postinstall/

### Installing Minikube

Head on over to the [Minikube installation page] and follow the instructions to
download and install `minikube`. Make sure it is installed correctly by checking
the version:

-> **Note**: Make sure that the version you see is greater than or equal to the one below

```bash
$ minikube version
minikube version: v1.17.1
commit: 043bdca07e54ab6e4fc0457e3064048f34133d7e
```

To start your minikube cluster, run the following. This will vary depending on what
OS you use, so make sure you are following the instructions for your platform.

[Minikube installation page]: https://minikube.sigs.k8s.io/docs/start/

{{< tabs tabTotal="2" tabID="1" tabName1="Mac" tabName2="Linux">}}

{{< tab tabNum="1" >}}

In your Mac terminal:

```bash
minikube start --driver=hyperkit --kubernetes-version=1.19.6
```

-> On Mac, `hyperkit` is provided by Docker Desktop, so you still need to install docker

{{< /tab >}}

{{< tab tabNum="2" >}}

In your Linux terminal:

```bash
minikube start --driver=docker --kubernetes-version=1.19.6
```

{{< /tab >}}

{{< /tabs >}}


{{< caution >}}

**Note**: If you see an error with `❌  Exiting due to DRV_NOT_DETECTED`, [make sure you installed docker correctly]

[make sure you installed docker correctly]: ../fluvio-local-faq#minikube-start-unable-to-pick-a-default-driver

{{< /caution >}}

### Installing Kubectl

Minikube is nothing more than just a _mini_ Kubernetes. That means that we need to get
the Kubernetes tool `kubectl` in order to configure and interact with our mini cluster
just like you would with a regular cluster. Head on over to the [installing kubectl]
page to get that installed. Once you're done, you should be able to run the following
command to check that it's installed correctly.

[installing kubectl]: https://kubernetes.io/docs/tasks/tools/install-kubectl/

-> Remember, the versions you see may not match the versions shown here

```bash
$ kubectl version --short
Client Version: v1.20.1
Server Version: v1.19.6
```

{{<idea>}}

**Note**: Minikube can download the right version of `kubectl` for you, but you have to
access it by running `minikube kubectl -- <args>` rather than the typical `kubectl <args>`.
If you don't want to install another tool feel free to use it this way, but just remember
if we say to run something like `kubectl get pods`, you'll want to run
`minikube kubectl -- get pods`

{{</idea>}}

### Installing Helm

Helm is a package manager that makes it easy to install apps for Kubernetes. Fluvio is
packaged as a "Helm chart" (the name for a package in Helm), and the Fluvio CLI can
install that chart for you by calling the `helm` executable with all the right options.
In order to do this, you need to have `helm` installed.

You can install `helm` from the [helm releases page]. Once it's installed, make sure it
works by printing the version.

-> Once again, the version you see might not be the same!

```bash
$ helm version --short
v3.3.4+ga61ce56
```

[helm releases page]: https://github.com/helm/helm/releases

## Installing Fluvio

Now that we've got all of our tools installed, let's actually get Fluvio up and running!
If you run into any problems along the way, make sure to check out our [troubleshooting]
page to find a fix.

[troubleshooting]: ../fluvio-local-faq

First, we have to open up a "minikube tunnel", which allows programs on our local machine
to connect to programs running in minikube. This is how Fluvio commands communicate with
the local Fluvio cluster in minikube. To do this, we need to run the following command:

```bash
$ sudo true && (sudo nohup minikube tunnel >/tmp/tunnel.out 2>/tmp/tunnel.out &)
```

Once we have the minikube tunnel running, we can run the startup command.

```bash
$ fluvio cluster start
✅ ok: Kubernetes config is loadable
✅ ok: Supported helm version is installed
✅ ok: Load balancer is up
✅ ok: Fluvio system charts are installed
✅ ok: Previous fluvio installation not found
Waiting up to 60 seconds for Fluvio cluster version check...
Successfully installed Fluvio!
```

You can check that everything worked by listing out the cluster's SPUs:

```bash
$ fluvio cluster spu list
 ID  NAME    STATUS  TYPE       RACK  PUBLIC              PRIVATE
  0  main-0  Online  "managed"   -    10.98.127.175:9005  fluvio-spg-main-0.fluvio-spg-main:9006
```

## Hello, Fluvio!

Congratulations, you've successfully installed Fluvio on your local machine! 
Let's use the Fluvio CLI to play with some basic functionality.

The first thing we need to do is create a Fluvio topic. A Topic is like a
category where related events live together. We can create a new topic with
the following command:

```bash
$ fluvio topic create greetings
topic "greetings" created
```

Now that we have a topic, we can produce some messages! Use the following
command to send a message to the `greetings` topic:

```bash
$ echo "Hello, Fluvio" | fluvio produce greetings
Ok!
```

Finally, we can consume messages back from the topic

```bash
$ fluvio consume greetings -B -d
Hello, Fluvio
```

Way to go! You're well on your way to writing real-time distributed apps
with Fluvio! Next, check out our [Tutorials page](/tutorials) to see real-world examples
of Fluvio in action.

[Tutorials page]: /tutorials

#### Related Topics
----------------

- ["Hello World" in Node.js](/tutorials/node/hello-world/)
- ["Hello World" in Rust](/tutorials/rust/hello-world/)