# Kubernetes

Kubernetes is a production-grade, open-source platform that orchestrates the placement (scheduling) and execution of application containers within and across computer clusters.

## Minikube

Can be considered as a developers testing ground, running Kubernetes locally within a VM.

## Cluster

Kubernetes coordinates a highly available cluster of computers that are connected to work as a single unit.

A K8s [**Cluster**](https://kubernetes.io/docs/tutorials/kubernetes-basics/cluster-intro/) consists of two types of resources:
* The **Master** coordinates the cluster
* **Nodes** are the workers that run applications

### Master

Responsible for managing the **Cluster**, coordinating all activies in the **Cluster** such as:
* Scheduling applications
* Maintaining applications' desired state
* Scaling applications
* Rolling out new updates

### Node

A VM or a physical computer that serves as a worker machine in a K8s **Cluster**.

**Node** requirements:
* Docker/rkt
* Kubelet

Note
* A production system should have a minimum of three **Nodes**.

#### Kubelet

Each **Node** requires this, it is an agent for managing the *Node* and communicating with the K8s **Master**.

![K8s Cluster](https://d33wubrfki0l68.cloudfront.net/99d9808dcbf2880a996ed50d308a186b5900cec9/40b94/docs/tutorials/kubernetes-basics/public/images/module_01_cluster.svg)

## Deployments

Prerequisites
* K8s **Cluster**
* Container image
* Number of replicas that you would like to run

A **Deployment** is responsible for creating and updating instances of your application on top of a **Cluster**.

The main component of a **Deployment**:
* K8s Deployment Controller

### K8s Deployment Controller

Once application instances have been created, the **Controller** continuously monitors those instances. An example of what a **Controller** might action could be if a **Node** hosting an application instance goes down or is deleted, the **Controller** replaces it.

*Note*
* This component provides a self-healing machanism to address machine failure or maintenance.

![K8S Deployment](https://d33wubrfki0l68.cloudfront.net/152c845f25df8e69dd24dd7b0836a289747e258a/4a1d2/docs/tutorials/kubernetes-basics/public/images/module_02_first_app.svg)

### What happens under the hood when creating a deployment?

Creating a deployment does the following:
* Searches for a suitable **Node** where an instance of the application can be run
* Schedules the application to run on that **Node**
* Configured the **Cluster** to reschedule the instance on a new **Node** when needed.

## Pods

Prerequisites:
* **Node**

A K8s **Pod** is a group of containers that are deployed together on the same host. Pods operate at one level higher than individual containers because it's very common to have a group of containers work together to produce an artifact or process a set of work. **Containers** in a **Pod** share an IP address and port space.

**Pods** are the atomic unit on the Kubernetes platform, when a **Deployment** is created it creates **Pods** with containers inside of them (as opposed to creating containers directly). It is important to note that **Pods** are considered ephemeral "cattle" and won't survive a machine failure and may be terminated for machine maintenance. 

Shared resources within a **Pod** include:
* Shared storage, as Volumes
* Networking, as a unique cluster IP address
* Information about to run each container

An example could be an application server **Pod** that contains three seperate containers:
* The app server itself
* A monitoring adapter
* A logging adapter

![K8s Pod](https://d33wubrfki0l68.cloudfront.net/fe03f68d8ede9815184852ca2a4fd30325e5d15a/98064/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)

## Services

Prerequisites
* **Cluster**
* **Deployment/Pods**

Is an abstraction layer which defines a logical set of **Pods** and enables the following for the **Pods**:
* External traffic exposure
* Load Balancing by routing traffic across a set of **Pods**
* Service Discovery
* **Pod** replication if one dies

**Services** enable a loose coupling between dependent **Pods**, and they are defined using YAML or JSON, like all K8s objects.

### How does K8s know which **Pods** to group together into a **Service**?

**Services** match a set of **Pods** using **[labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)** and **[selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)**. Labels are key/value pairs attached to object and can be used in any number of ways:
* Designate objects for development, test, production
* Embed version tags
* Classify an object using tags

![K8s Services](https://d33wubrfki0l68.cloudfront.net/b964c59cdc1979dd4e1904c25f43745564ef6bee/f3351/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)

## Scaling

Prerequisites
* **Cluster**
* **Deployment/Pods**

**Scaling** is accomplished by changing the number of replicas in a **Deployment**. When we scale out a **Deployment**; **Scaling** will ensure new **Pods** are created and scheduled to **Nodes** with available resources. **Services** have an integrated load-balancer taht will distribute network traffic to all **Pods** of an exposed **Deployment**.

**Note:** K8s also supports autoscaling however this documentation is still to come.

## CLI

Interacting with K8S is pretty straight forward by using `kubectl`. kubectl expects the following command syntax:
* `kubectl [command] [TYPE] [NAME] [flags]`

### Working with Clusters

View **Cluster** details

`kubectl cluster-info`

View nodes in the **Cluster**

`kubectl get nodes`

### Working with Deployments

Create a *Deployment*

`kubectl run {deployment name} --image={image location - either in URL form or path pased} --port={port number}`

List deployments

`kubectl get deployments`

Check the **Deployments** events log

`kubectl describe deployments/{insert deployment name}`

### Checking your application

Look for existing **Pods**

`kubectl get pods`

View what containers are inside that **Pod**.

`kubectl describe pods`

Retrieve logs for the container within the **Pod**. Anythin taht the application would normally send to `STDOUT` becomes logs for the container within the **Pod**. 

`kubectl logs {insert pod name / insert container name}`

**Note:** If we only have one container within a **Pod** we can use the **Pod** name instead of passing the **Container** name to the command.

### Execute a command within a **Container**

`kubectl exec {insert pod name / insert container name} {command}`

#### Example Command

Let's start a Bash session in the **Container**

`kubectl exec -ti {insert container name} bash`

### Working with Services

List current **Services** from our **Cluster**

`kubectl get services`

`kubectl get services {insert service name}`

Create a **Service** object that exposes a **Deployment**

`kubectl expose deployment {insert deployment name} --type={NodePort(Minikube)/LoadBalancer} --port {insert port number} --name={insert service name}`

### Working with Scaling

Scale a **Deployment**

`kubectl scale deployment/{insert deployment name} --replicas=4`

Check the deployments configuration, verifying the *DESIRED*, *CURRENT*, *UP-TO-DATE*, and *AVAILABLE* settings.

`kubectl get deployments`

Check the number of **Pods** once **Scaling** has occurred.

`kubectl get pods -o wide`