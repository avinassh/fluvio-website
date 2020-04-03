---
title: Setup Fluvio Development Environment
menu: Dev Environment
weight: 100
---

Thank you for your interest on joining Fluvio Development Community! 

We look forward to working with you!

Fluvio leverages a **Key-Value (KV) store** to persist cluster object configurations. While the product is designed for a broad range of **KV** stores, the current version is integrated with **{{< target-blank title="Kubernetes" url="https://kubernetes.io" >}}** and **{{< target-blank title="etcd" url="https://etcd.io" >}}**.


## Install Kubernetes

To run Kubernetes on your local machine we recommend Minikube. Minikube runs a single-node Kubernetes cluster inside a Virtual Machine (VM) on your laptop. Follow the instructions on the Minikube website to get started:

* **{{< target-blank title="Minikube - Getting Started" url="https://minikube.sigs.k8s.io/docs/start" >}}**

### Check Minikube Cluster version

To check minikube has created the right version of the cluster. 
{{< fluvio >}}
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.3", GitCommit:"2d3c76f9091b6bec110a5e63777c332469e0cba2", GitTreeState:"clean", BuildDate:"2019-08-19T12:36:28Z", GoVersion:"go1.12.9", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.7", GitCommit:"4683545293d792934a7a7e12f2cc47d20b2dd01b", GitTreeState:"clean", BuildDate:"2019-06-06T01:39:30Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
{{< /fluvio >}}

Server version should be ```v1.13.x``` or greater.

### Install Minikube specific storage class

Fluvio needs Minikube storage driver to save messages for topic/partitions.  To install:
{{< fluvio >}}
$ kubectl apply -f ./k8-util/crd/config/minikube-storageclass-spu.yaml
{{< /fluvio >}}


### Setup Minikube Tunnel

To expose Minikube ports to your environment, we need to setup a [tunnel](https://minikube.sigs.k8s.io/docs/tasks/loadbalancer/) to your local network.  
Run the following script to setup the tunnel:

{{< fluvio >}}
$ sudo ./k8-util/minikube-tunnel.sh
{{< /fluvio >}}


## Clone Fluvio Repo

The Fluvio repository provides setup scripts to provision each infrastructure.

Open a terminal and clone the Fluvio repository from [github](https://github.com/infinyon/fluvio):

{{< fluvio >}}
$ git clone https://github.com/infinyon/fluvio.git
{{< /fluvio >}}  

Navigate to fluvio root directory:

{{< fluvio >}}
$ cd fluvio
{{< /fluvio >}}

The setup scripts are located in the __k8_util__ directory:

{{< fluvio >}}
$ ls
Cargo.lock		future-helper		metadata
Cargo.toml		future-helper-03	rust-toolchain
DEVELOPER.md	k8-client		    rustfmt.toml
LICENSE			k8-config		    sc-server
Makefile		k8-diff			    spu-server
README.md		k8-metadata		    storage
api			    k8-util			    test-helper
cli			    kf-protocol		    types
dev-tools		kf-service		    utils
future-aio		kf-socket
{{< /fluvio >}}

To continue setup, choose one of the following instructions:

## Setup Local Environment

...


## Compile Fluvio

...

## Deploy on your Local Cluster

...

## Run Hello Word

...



{{< links "Related Topics" >}}
* [Fluvio Architecture]({{< relref "../architecture/overview" >}})
* [Fluvio CLI]({{< relref "../cli/overview" >}})
{{< /links >}}
