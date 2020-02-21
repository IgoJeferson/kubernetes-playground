# Mastering Kubernetes 

## Key Kubernetes Architecture Items

* Master
    * kube-apiserver
    * kube-controller-manager
    * kube-scheduler
* etcd

## Setting up a reliable system

1. Creating a reliable nodes that, together, will form an highly availably master implementation in following steps

2. Setting up a redundant, reliable data storage layer with clustered etcd

3. Starting replicated, load balaced API servers (kube-api)

4. Setting up master-elected kube-scheduler and kube-controller-manager daemons

5. Multiple worker nodes

## Steps

### Reliable nodes for our masters

* Separate, independent Linux machines that will run master processes
* Should run kubelet & monit

### A reliable data storage layer

* etcd is a system that stores key and value pairs that is the data in a kubernetes cluster
* It should run on every node that will be a master
* Consult the etcd documentation on the variety of options on how etcd can provide even deeper levels of redundancy if needed

### Replicated api servers

* kube-api should run on all nodes that will be a master
* kube-api should be behind a network load balancer, this will vary with your environment you are running

### Master Elected Components

* Now that items are set up on reliable nodes, we have the pieces in place, but they aren't actually running
* We need to ensure only one actor works on the data at a given time
* Each Scheduler and Controller Manager will be started with a "--leader-elect" flag that will use a lease-lock API between themselves to ensure only one instance of each is running at a given time

## To get  a look at Master ...

* We'll need something bigger than  minikube
* Let's set up a kubernetes cluster on AWS