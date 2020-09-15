---
toc: true
layout: post
description: Introduction to concept of kubernetes and its key terms
categories: [Kubernetes]
title: Kubernetes 101
---

## 1.0 What is Kubernetes

Kubernetes is an open-source platform/tool created by Google. It is written in GO-Lang. So currently Kubernetes is an open-source project under Apache 2.0 license. Sometimes in the industry, Kubernetes is also known as “K8s”. With Kubernetes, you can run any Linux container across private, public, and hybrid cloud environments. Kubernetes provides some edge functions, such as Loadbalancer, Service discovery, and Roled Based Access Control(RBAC).

Basically, kubernetes is a software that allows us to deploy, manage and scale applications. The applications will be packed in containers and kubernetes groups them into units. It allows us to span our application over thousands of servers while looking like one single unit.

- Key Features of Kubernetes
  - Horizontal Scaling
  - Auto Scaling
  - Health check & Self-healing
  - Load Balancer
  - Service Discovery
  - Automated rollbacks & rollouts
  - Canary Deployment

---

## 2.0 Why we need Kubernetes

First, we need to familier with few terms:

**Monolithic Applications:** Years ago, most software applications were big monoliths, running either as a single process or as a small number of processes spread across a handful of servers. These legacy systems are still widespread today. They have slow release cycles and are updated relatively infrequently. At the end of every release cycle, developers pack- age up the whole system and hand it over to the ops team, who then deploys and monitors it. In case of hardware failures, the ops team manually migrates it to the remaining healthy servers. So these components that are all tightly coupled together and have to be developed, deployed, and managed as one entity, because they all run as a single OS process.

- Problems with Monolithic Applications: Change of one part of the application require a redeployment of the whole application. Requires powerful servers, Uses Vertical Scaling ( which is Very Expensive ) and If one components creates problem, the whole application becomes unscalable.

**Microservices Applications:** Today, these big monolithic legacy applications are slowly being broken down into smaller, independently running components called microservices. Each microservice runs as an independent process and communicates with other microservices through simple, well defined interfaces ( APIs ). Microservices communicate through synchronous protocols such as HTTP, over which they usually expose RESTful (REpresentational State Transfer) APIs, or through asyn- chronous protocols such as AMQP (Advanced Message Queueing Protocol).

Microservices are decoupled from each other, they can be developed, deployed, updated,
and scaled individually. This enables you to change components quickly and as often as
necessary to keep up with today’s rapidly changing business requirements.

- Problems with Microservices Based Applications: The bigger numbers of deployable components and increasingly larger datacenters, it becomes increasingly difficult to configure, manage, and keep the whole system running smoothly. It’s much harder to figure out where to put each of those components to achieve high resource utilization and thereby keep the hardware costs down and the one of biggest problem with scaling microservices is multiple apps that are running on same host mey have conflicting dependencies. One solution of this is applications could run in the exact same environment during development and in production so they have the exact same operating system, libraries, system configuration, networking environment, and everything else but this will not solve the conflicting, versions of libraries or different environment requirements to solve this issue we have great technology called containers. ( You are also provide Dedicated VM to particular service but it is not a ideal solution because it increases hardware cost and management)

Doing all this manually is hard work. We need automation, which includes automatic scheduling of those components to our servers, automatic configuration, supervision, and failure-handling. This is where Kubernetes comes in.

![Microservices](https://s3.ap-south-1.amazonaws.com/akash.r/Devops_Notes_screenshots/Kubernetes/MicroServices.png)

Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. For example, Kubernetes can easily manage a canary deployment for your system.

Before used Kubernetes, you need to prepare your infrastructure to deploy a new microservice. I believe it cost you a few days or weeks. Without Kubernetes, large teams would have to manually script the deployment workflows. With Kubernetes, you don’t need to create your deployment script manually and it will reduce the amount of time and resources spent on DevOps.

### 2.1 What kubernetes provides

- Service discovery and load balancing: Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- Storage orchestration: Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
Automated rollouts and rollbacks You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- Automatic bin packing: You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- Self-healing: Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- Secret and configuration management: Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.

### 2.2 What kubernetes not provides

Kubernetes is a lot of good things, I hope to have been clear exposing all Kubernetes benefits. The main problem from Kubernetes newbie is that they discover it is not a PaaS (Platform as a Service) system like they suppose. Kubernetes is a lot of things but not an “all included” service. It is great and reduces the amount of work, especially on the sysadmin side, but doesn’t offer you any apart from the infrastructure.

Said that most of the things you are looking for in a fully-managed system are there: simplified deployments, scaling, load balancing, logging, and monitoring. Usually, you get a standard configuration from your hosting but you can theoretically customize it if you really need it.

- It does not limit the types of applications supported. Everything is written into the container so every container application, no matter on technology, can be run. The counterpart is that you still have to define the container by hand.
- It doesn’t offer an automated deployment. You just have to push to a docker repository your built images, no more. This is quite easy if you already work in a process with Continuous Integration, Delivery, and Deployment (CI/CD), but consider that without it will be quite tricky.
- It does not provide any application-level services, just infrastructure. If you need a database, you have to buy a service or run it into a dedicated container. Of course, taking charges of backups and so on.
- Most of the interactions with the system are by a command line that wraps API. That’s very good because it allows automating each setting. Commands syntax is very simple but, if you are looking to a system that is managed fully by a UI you are looking to the worst place.

---

## 3.0 How does Kubernetes work

The system is composed of a master node and any number of worker nodes. When the developer submits a list of apps to the master, Kubernetes deploys them to the cluster of worker nodes. What node a component lands on doesn’t (and shouldn’t) matter—neither to the developer nor to the system administrator.

![Kubernetes Master](https://s3.ap-south-1.amazonaws.com/akash.r/Devops_Notes_screenshots/Kubernetes/Kubernetes_Master.png)

Kubernetes will run your containerized app somewhere in the cluster, provide information to its components on how to find each other, and keep all of them running.
Because your application doesn’t care which node it’s running on, Kubernetes can
relocate the app at any time, and by mixing and matching apps, achieve far better
resource utilization than is possible with manual scheduling.

### 3.1 Architecture of Kubernetes Cluster

We’ve seen a bird’s-eye view of Kubernetes’ architecture. Now let’s take a closer look at what a Kubernetes cluster is composed of. At the hardware level, a Kubernetes cluster is composed of many nodes, which can be split into two types:

- The master node, which hosts the Kubernetes Control Plane that controls and manages the whole Kubernetes system
- Worker nodes that run the actual applications you deploy

![Kubernetes Architecture](https://s3.ap-south-1.amazonaws.com/akash.r/Devops_Notes_screenshots/Kubernetes/Kubernetes_arch1.png)

#### 3.1.1 Master ( Control Plane )

The Control Plane is what controls the cluster and makes it function. It consists of
multiple components that can run on a single master node or be split across multiple
nodes and replicated to ensure high availability. These components are

- **Kubernetes API Server**: The application that serves Kubernetes functionality through a RESTful interface and stores the state of the cluster ( which admin and other control plane communicated with ).

- **Scheduler**: Scheduler watches API server for new Pod requests. It communicates with Nodes to create new pods and to assign work to nodes while allocating resources or imposing constraints.

- **Controller Manager**: Component on the master that runs controllers. Includes Node controller, Endpoint Controller, Namespace Controller, etc. ( Performs cluster-level functions, such as replicating components, keeping track of worker nodes, handling node failures, and so on )

  - Node Controller: Responsible for noticing and responding when nodes go down.
  - Replication Controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.
  - Endpoints Controller: Populates the Endpoints object (that is, it joins Services and Pods).
  - Service Account and Token Controllers: Create default accounts and API access tokens for new namespaces.
  - Cloud-Controller-Manager: Cloud-controller-manager runs controllers that interact with the underlying cloud providers. The cloud-controller-manager binary is an alpha feature introduced in Kubernetes release 1.6. Cloud-controller-manager runs cloud-provider-specific controller loops only. You must disable these controller loops in the Kube-controller-manager. You can disable the controller loops by setting the --cloud-provider flag to external when starting the Kube-controller-manager.
  - Node Controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding.
  - Route Controller: For setting up routes in the underlying cloud infrastructure.
  - Service Controller: For creating, updating, and deleting cloud provider load balancers.
  - Volume Controller: For creating, attaching, and mounting volumes, and interacting with the cloud provider to orchestrate volumes.

- **etcd**: A reliable distributed data store that persistently stores the cluster
configuration.

The components of the Control Plane hold and control the state of the cluster, but
they don’t run your applications. This is done by the (worker) nodes.

#### 3.1.2 Slave ( Worker Nodes )

The worker nodes are the machines that run your containerized applications. The
task of running, monitoring, and providing services to your applications is done by
the following components:

- Docker, rkt, or another container runtime, which runs your containers

- **Kubelet**: Kubectl registering the nodes with the cluster, watches for work assignments from the scheduler, instantiate new Pods, report back to the master.
Container Engine: Responsible for managing containers, image pulling, stopping the container, starting the container, destroying the container, etc.

- **Kube Proxy**: Responsible for forwarding app user requests to the right pod (load-balances network traffic between application components).

- The sequence of deployment: `DevOps -> API Server -> Scheduler -> Cluster ->Nodes -> Kubelet -> Container Engine -> Spawn Container in Pod`

- The sequence of App user request: `App user -> Kube proxy -> Pod -> Container(Your app is run here)`

### 3.2 The Six Layers of K8s

![Kubernetes Abstrction](https://s3.ap-south-1.amazonaws.com/akash.r/Devops_Notes_screenshots/Kubernetes/Kubernetes_Abstraction.png)

Deployments create and manage ReplicaSets, which create and manage Pods, which run on Nodes, which have a container runtime, which run the app code you put in your Docker image.

The levels shaded blue are higher-level K8s abstractions. The green levels represent Nodes and Node subprocess that you should be aware of, but may not touch.

#### 3.2.1 Deployments

Although pods are the basic unit of computation in Kubernetes, they are not typically directly launched on a cluster. Instead, pods are usually managed by one more layer of abstraction: the deployment. A deployment’s primary purpose is to declare how many replicas of a pod should be running at a time. When a deployment is added to the cluster, it will automatically spin up the requested number of pods, and then monitor them. If a pod dies, the deployment will automatically re-create it. Using a deployment, you don’t have to deal with pods manually. You can just declare the desired state of the system, and it will be managed for you automatically.

#### 3.2.2 ReplicaSet

The Deployment creates a ReplicaSet that will ensure your app has the desired number of Pods. ReplicaSets will create and scale Pods based on the triggers you specify in your Deployment. Replication Controllers perform the same function as ReplicaSets, but Replication Controllers are old school. ReplicaSets are the smart way to manage replicated Pods in 2019.

#### 3.2.3 Pods

All containers will run in a pod. Pods abstract the network and storage away from the underlying containers. Your app will run here. Each pod has one unique IP address assigned which means one pod can communicate with each other like a traditional container in a docker environment. Each container inside the pod can reach all other pods into the virtual network, but cannot deep to the other container on other pods. That’s important to guarantee the pod abstraction: nobody has to know how the Pods are composed internally. Moreover, the IP assigned is volatile so you must use always the service name (resolved to the right IP directly). Pods are used as the unit of replication in Kubernetes. If your application becomes too popular and a single pod instance can’t carry the load, Kubernetes can be configured to deploy new replicas of your pod to the cluster as necessary. Even when not under heavy load, it is standard to have multiple copies of a pod running at any time in a production system to allow load balancing and failure resistance. Pods handle Volumes, Secrets, and configuration for containers. Pods are ephemeral. They are intended to be restarted automatically when they die.

Note: Worker Node is already explained in above section

### 3.3 Key Terms used with Kubernetes

#### 3.3.1 Services

The name “service” in informatics science is overused. In Kubernetes scope think to a service like something you want to serve. A Kubernetes service involves a set of pods and may offer a complex feature or just expose a single Pod with a single container. So you can have a service that provides a CMS feature, with database and web server inside, or two different services, one for the database and one for the webserver. That’s up to you. Basically it is abstraction layer on top of a set of ephemeral pods (think of this as the ‘face’ of a set of pods)

#### 3.3.2 Ingress

By default, Kubernetes provides isolation between pods and the outside world. If you want to communicate with a service running in a pod, you have to open up a channel for communication. This is referred to as ingress. To do this there is an ingress controller that does something similar to a load balancer. It virtually forwards traffic from outside to the services. During this step, based on the ingress implementation you have chosen, you can add HTTPS encryption, route traffic based on the hostname or URL segments. The only service can be linked to the ingress controller, not Pods. There are multiple ways to add ingress to your cluster. The most common ways are by adding either an Ingress controller, or a LoadBalancer.

#### 3.3.3 Volume

By default Pod storage is volatile. This is important to know because at the first restart you will lose everything. Kubernetes volumes allow mapping some part of the hard drive of containers to a safe place. That space can be shared between containers. The mount point can be any part of the container, but a volume cannot be mount into another one.

**PersistentVolumes and PersistentVolumeClaims**: To help abstract away infrastructure specifics, K8s developed PersistentVolumes and PersistentVolumeClaims. Unfortunately the names are a bit misleading, because vanilla Volumes can have persistent storage, as well. PersisententVolumes (PV) and PersisentVolumeClaims (PVC) add complexity compared to using Volumes alone. However, PVs are useful for managing storage resources for large projects. With PVs, a K8s user still ends up using a Volume, but two steps are required first.

1. A PersistentVolume is provisioned by a Cluster Administrator (or it’s provisioned dynamically).
2. An individual Cluster user who needs storage for a Pod creates PersistentVolumeClaim manifest. It specifies how much and what type of storage they need. K8s then finds and reserves the storage needed.

The user then creates a Pod with a Volume that uses the PVC. PersistentVolumes have lifecycles independent of any Pod. In fact, the Pod doesn’t even know about the PV, just the PVC. PVCs consume PV resources, analogously to how Pods consume Node resources.

#### 3.3.4 Namespaces

Think to namespace like the feature that makes Kubernetes multitenant. The namespace is the tenant level. Each namespace can partitioning resources to isolate services, ingress, and many other things. This feature is good to have a strong separation between application, delegate safely to different teams, and have separated environments in a single infrastructure. It is a virtual cluster on top of an underlying physical cluster

#### 3.3.5 Labels and Selectors

Labels are key-value pairs used to tag objects. Objects are the items that you create in a Kubernetes cluster (pods, deployments, replica sets, services, volumes etc). Selectors are used to collect objects based on tags.

#### 3.3.6 StatefulSets

As we know, a ReplicaSet creates and manages Pods. If a Pod shuts down because a Node fails, a ReplicaSet can automatically replace the Pod on another Node. You should generally create a ReplicaSet through a Deployment rather than creating it directly, because it’s easier to update your app with a Deployment.

![States](https://s3.ap-south-1.amazonaws.com/akash.r/Devops_Notes_screenshots/Kubernetes/Kubernetes_States.png)

Sometimes your app will need to keep information about its state. You can think of state as the current status of your user’s interaction with your app. So in a video game it’s all the unique aspects of the user’s character at a point in time. What do you do when your app has state you need to keep track of? Use a StatefulSet.

Like a ReplicaSet, a StatefulSet manages deployment and scaling of a group of Pods based on a container spec. Unlike a Deployment, a StatefulSet’s Pods are not interchangeable. Each Pod has a unique, persistent identifier that the controller maintains over any rescheduling. StatefulSets for good for persistent, stateful backends like databases. The state information for the Pod is held in a Volume associated with the StatefulSet.

#### 3.3.7 DaemonSets

DaemonSets are for continuous process. They run one Pod per Node. Each new Node added to the cluster automatically gets a Pod started by the DaemonSet. DaemonSets are useful for ongoing background tasks such as monitoring and log collection.
StatefulSets and DaemonSets are not controlled by a Deployment. Although they are at the same level of abstraction as a ReplicaSet, there is not a higher level of abstraction for them in the current API.

#### 3.3.8 ETCD

It stores the configuration information which can be used by each of the nodes in the cluster. It is a high availability key-value store that can be distributed among multiple nodes. It is accessible only by Kubernetes API server as it may have some sensitive information. It is a distributed key-value store which is accessible to all.
ETCD is a distributed reliable key-value store used by Kubernetes to store all data used to manage the cluster. Think of it this way, when you have multiple nodes and multiple masters in your cluster, etcd stores all that information on all the nodes in the cluster in a distributed manner. ETCD is responsible for implementing locks within the cluster to ensure there are no conflicts between the Masters.

#### 3.3.9 Cluster IP

Only has a Virtual IP (also called Cluster IP). This service can be used within a cluster only Acts like a traffic router to your Pods inside the cluster. Each port exposed by a pod will need a service if you want a client to talk to it via that port. By default, the service port is the same as the port exposed by the Pod. Different methods to access this service: From any node inside the cluster: `<cluster IP>:<service port>` From the node where a replica of the Pod is running: `<pod IP>:<pod port>`.

#### 3.3.10 Node Port

For this service, a physical port on the node is mapped to the service port and the service connects to the Pod via the pod port. Remember, this will also have a Virtual IP (i.e. cluster IP). Different methods to access this service: From outside the cluster, if any node in the cluster has a public IP: `<node public IP>:<node port>` From any node inside the cluster: `<cluster IP>:<service port>` From the node where a replica of the Pod is running: `<pod IP>:<pod port>`.

#### 3.3.11 Load Balancer

Giving public access to a node in your cluster is not a recommended method, When you want to give public access to a service, create a load balancer service (not getting into ingress at this stage). For each service that you want to give public access, you need to create a load balancer service, This type of service will generally work only in a cloud environment. Remember, this will also have a Virtual IP (i.e. cluster IP) This service will have a node port too Different methods to access this service: From outside the cluster: `<load balancer dns>:<service port>` From outside the cluster, if any node in the cluster has a public IP: `<node public IP>:<node port>` From any node inside the cluster: `<cluster IP>:<service port>` From the node where a replica of the Pod is running: `<pod IP>:<pod port>`.

#### 3.3.12 External Name

This maps a service to endpoints completely outside of the cluster.

---
