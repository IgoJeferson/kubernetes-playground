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

## Setting up a Kubernetes Cluster On AWS

* Amazon offers a managed Kubernetes service called Elastic Kubernetes Service
    * Managed master redundancy/High Availability
    * Highly automated setup
* We'll use "kops", a suite of software provided by kubernetes mainteiners to to a bit more manual work to set up our own cluster

Steps:

1. Download and install "kops" 

https://github.com/kubernetes/kops

2. Download, configure, and install the "aws" command line tool suite

https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

* After the download, use ``` aws configure ``` to configure your access key

3. Create an AWS S3 bucket as a "state store"

``` aws s3api create-bucket --bucket igo-jeferson-basic-k8s-and-kops-demo-bucket --region us-east-1 --create-bucket-configuration LocationConstraint=EU ```

4. Create the cluster using kops

* Just a reference to our S3 Bucket for kop use

``` export KOPS_STATE_STORE=s3://igo-jeferson-basic-k8s-and-kops-demo-bucket ```

* Kops create command, without the parameter '--yes', because we don't wan't provide the resources yet.

``` kops create cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --zones us-east-1a ```

* Obs: If you don't have a default public key, create one with ssh-keygen command, and inform kops:

``` kops create secret --name igo-jeferson-basic-k8s-and-kops-demo.k8s.local sshpublickey admin -i ~/.ssh/id_rsa_igojeferson.pub ```

* Update the cluster, now referencing the secret ssh key and using --yes to create all resources in AWS.

``` kops update cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --yes ```


Wait until all the resources become ready, this process can take almost 5 minutes, and follow the suggestions commands, like this:

 * validate cluster: ``` kops validate cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local ```
 * list nodes:  ``` kubectl get nodes --show-labels  ```
 * ssh to the master:  
 ``` ssh -i ~/.ssh/id_rsa admin@ec2-3-91-53-125.compute-1.amazonaws.com  ```
 * the admin user is specific to Debian. If not using Debian please use the appropriate user based on your OS. *(To access the master you will need to take the public IP in the aws console EC2 configuration)
 * read about installing addons at: https://github.com/kubernetes/kops/blob/master/docs/operations/addons.md.


* To check the master items: 
``` kubectl get deployments -n kube-system ```

* *After finish your tests, remember to delete the cluster using kops, to ensure all resources are released and you are not charged.*

 ``` kops delete cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --yes ```