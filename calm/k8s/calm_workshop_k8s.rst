**********
Kubernetes
**********

Overview
********

Kubernetes (also known as K8s) is a vendor-agnostic cluster and container management tool, created by Google and later donated to the Cloud Native Computing Foundation in 2014.  The idea behind Kubernetes was to build a system designed to run enterprise-class, cloud-enabled. and web-scalable IT workloads. It provides a “platform for automating deployment, scaling, and operations of application containers across clusters of hosts”.  Above all, it lowers the cost of cloud computing expenses and simplifies operations and architecture.

.. figure:: http://s3.nutanixworkshops.com/calm/k8s/image2.png

Kubernetes and the Need for Containers
**************************************

Before explaining what Kubernetes does, lets explain what containers are, how they differ from VMs, and why people are using those.

**Virtial Machine**

A virtual machine app creates a virtualized environment—called, simply enough, a *virtual machine* that behaves like a separate computer system, complete with virtual hardware devices. The VM runs as a process on your current operating system. 

*Source: How-to Geek*

**Containers:**

A container is a mini-virtual machine. It is small, as it does not have device drivers and all the other 
components of a regular virtual machine. Docker is by far the most popular container and it is written in Linux. 
Microsoft also has added containers to Windows as well, because they have become so popular.

The best way to illustrate why this is useful and important is to give an example.

Suppose you want to install the nginx web server on a Linux server. You have several ways to do that. 
First, you could install it directly on the physical server’s OS. But most people use virtual machines now, 
so you would probably install it there.

But setting up a virtual machine requires some administrative effort and cost as well. And machines 
will be underutilized if you just dedicate it for just one task, which is how people typically use VMs. 
It would be better to load that one machine up with nginx, messaging software, a DNS server, etc.

The people who invented containers thought through these issues and reasoned that since nginx or any 
other application just needs some bare minimum operating system to run, then why not make a stripped down 
version of an OS, put nginx inside, and run that. Then you have a self-contained, machine-agnostic unit 
that can be installed anywhere.

Containers are so popular in todays modern datacenter, they threaten to make VMs obsolete...

**Docker Hub**

But making the container small is not the only advantage. The container can be deployed just like a VM 
template, meaning an application that is ready to go that requires little or no configuration.

There are thousands of preconfigured Docker images at the Dockerhub public repository. There, people have 
taken the time to assemble opensource software configurations that might take someone else hours or days to 
put together. People benefit from that because they can install nginx or even far more complicated items simply 
by downloading them from there.

**Example:** this one line command will down, install, and start Apache Spark with Jupyter notebooks (iPython):

.. code-block:: bash

   $ docker run -d -p 8888:8888 jupyter/all-spark-notebook

As you can see it is running on port 8888. So you could install something else on another port or even install a 
second instance of Spark and Jupyter.

Container Proliferation - The Need for Orchestration
****************************************************

There's an inherent problem with containers, just like there is with virtual machines. That is the need to keep track of 
them. When public cloud companies bill you for CPU time or storage then you need to make sure you do not have any orphaned 
machines spinning out there doing nothing. Plus there is the need to automatically spin up more when a machine needs more 
memory, CPU, or storage, as well as shut them down when the load lightens.

Orchestration tackles these problems. This is where Kubernetes comes in.

Kubernetes Architecture
***********************

Google built Kubernetes and has been using it for 10 years. The fact it's been used to run Google’s massive systems 
for over decade is one of its key selling points. In 2014 Google pushed Kubernetes into open source.

Kubernetes is a cluster and container management tool. It lets you deploy containers to clusters, meaning a network
of virtual machines, and works with different containers, not just Docker.

.. figure:: http://s3.nutanixworkshops.com/calm/k8s/image1.png
*Image source: wikipedia*


**Kubernetes terminologies:**

- Pods– Pods are a collection of one or more containers. It acts as a Kubernetes’ core unit of management. Pods set the logical boundary for containers sharing the same context and resources.
- Labels– Labels are arbitrary tags that can be placed on the above work units to mark them as a part of a group. These can then be selected for management purposes and action targeting.
- Services– Services is a unit that acts as a basic load balancer and ambassador for other containers. A service groups together logical collections of pods that perform the same function to give an impression of single entity.
- Replication Controller– A more complex version of pod is known as replicated pod. These are handled a type of work unit known as a replication controller. Replication controllers make sure that a specific number of pod replicas are running 

**Kubernetes Basics**

The basic idea of Kubernetes is to further abstract machines, storage, and networks away from their physical implementation.
So it is a single interface to deploy containers to all kinds of clouds, virtual machines, and physical machines.

Here are a few of **Kubernetes** concepts to help understand what it does.

**Master component**

The master node is the one that is responsible for the management of Kubernetes cluster. This is the main entry point of all administrative tasks. The master node, also known as the control plane, is the one that is managing the worker nodes, where the actual services are running.

A master node is made of following components:

- API Server
   API Server is the main management point of the entire cluster, as it allows a user to configure many of Kubernetes’   workloads and organizational units. The API server is also the entry points for all the REST commands used to control the cluster. That means several different tools and libraries can easily communicate with it.

- etcd storage
   The etcd is a simple, lightweight, distributed key-value store that can be distributed across multiple nodes. The etcd storage was developed by the CoreOS team to be mainly used for shared configuration and service discovery.  Kubernetes uses etcd to store configuration data that can be used by each of the nodes in the cluster

- Scheduler
   The scheduler component configures pods and services onto the nodes. Moreover, the scheduler is also responsible for tracking resource utilization on each host to make sure that workloads are not scheduled in excess of the available resources.

- Controller-manager
   The controller manager service is a general service that is responsible for controllers that regulate the state of the cluster and perform routine tasks. The example of such a controller is the replication controller. As it ensures that the number of replicas defined for a service matches the number currently deployed on the cluster. The details of these operations are written to etcd, where the controller manager watches for changes through the API server.

**Node**

A node is a physical or virtual machine. It is not created by Kubernetes. You create those with a cloud operating system, 
like OpenStack or Amazon EC2, or manually install them. So you need to lay down your basic infrastructure before you use 
Kubernetes to deploy your apps. But from that point it can define virtual networks, storage, etc. For example, you could use 
OpenStack Neutron or Romana to define networks and push those out from Kubernetes.

Every single node has the services necessary to run **pods** and is managed by the **master components**. 

The services on a node include:

- Docker
   Docker is responsible for downloading the images and starting the containers. It runs on the encapsulated application containers in a lightweight operating environment. Each unit of work is implemented as series containers that must be deployed.

- kubelet
   kubelet gets the configuration of a pod from the API server and ensures that the described containers are up and running. This is the worker service that’s responsible for communicating with the master node. It is responsible for relaying information to and from the control plane services, as well as interacting with the etcd store to read configuration details or write new values.

- kube-proxy
   Kube-proxy runs on each node to deal with individual host sub-netting and ensure that the services are available to external parties. It serves as a network proxy and a load balancer for a service on a single worker node and manages the network routing for TCP and UDP packets.

**Pods**

A pod is a one or more containers that logically go together. Pods run on nodes. Pods run together as a logical unit. So 
they have the same shared content. They all share the same IP address, but can reach other Pods via localhost, as well as
share storage. Pods don't need to all run on the same machine as containers can span more than one machine. One node 
can run multiple pods.

Pods are cloud-aware. For example you could spin up two Nginx instances and assign them a public IP address on the Google 
Compute Engine (GCE). To do that you would start the Kubernetes cluster, configure the connection to GCE, and then type 
something like:

.. code-block:: bash

  $ kubectl expose deployment my-nginx –port=80 –type=LoadBalancer

**Deployment**

A set of pods is a deployment. A deployment ensures that a sufficient number of pods are running at one time to service 
the app and shuts down those pods that are not needed. It can do this by looking at, for example, CPU utilization.

**Vendor Agnostic**

Kubernetes works with many cloud and server products. And the list is always growing as so many companies are contributing 
to the open source project. Even though it was invented by Google, Google is not said to dominate it’s development.

To illustrate, the OpenStack process to create block storage is called Cinder. OpenStack orchestration is called Heat. You 
can use Heat with Kubernetes to manage storage with Cinder.

Kubernetes works with Amazon EC2, Azure Container Service, Rackspace, GCE, IBM Software, and other clouds. And it works with 
bare-metal (using something like CoreOS), Docker, and vSphere. And it works with libvirt and KVM, which are Linux machines 
turned into hypervisors (i.e, a platform to run virtual machines).

Use Cases
*********

So what platforms or ecosystems would you use Kubernetes on, for example, Amazon EC2, when it has its own tool for orchestration (CloudFormation)? 

With Kubernetes you can use the same orchestration tool and command-line interfaces for all your different systems. 
Amazon CloudFormation only works with EC2. So with Kubernetes you could push containers to the Amazon cloud, your in-house 
virtual and physical machines as well, and other clouds.

Benefits of Kubernetes
**********************

Kubernetes is designed in a way that provides scalability, availability, security, and portability. Reliability is another main benefit of Kubernetes and can be used to prevent failure from impacting the availability or performance of the application. Moreover, Kubernetes enables the users to respond efficiently to customers demand by scaling or rolling out new innovative features. It is designed in such a way that it offers freedom of choice when choosing operating systems, container runtimes, processor architectures, cloud platforms and PaaS. It also improves the cost of infrastructure by effectively dividing the workload across available resources. This shows that while other technologies are doing a commendable job at handling the cluster aspect, Kubernetes is providing a better management system.


Summary
*******

**What is Kubernetes?** It is an orchestration tool for containers. **What are containers?** They are small virtual machines that run ready-to-run applications on top of other virtual machines or any host OS. They greatly simplify deploying applications. They make sure machines are fully-utilized. 

All of this lowers the cost of cloud subscriptions, further abstracts the data center, and simplifies operations and architecture. To get started learning about it, the reader can install MiniKube to run it all on one machine and play around with it.

