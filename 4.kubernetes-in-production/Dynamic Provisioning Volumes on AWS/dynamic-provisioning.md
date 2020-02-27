# Dynamic Provisioning

* When an existing Volume does not match a Claim, k8s mayble able to (based on the provider type and configuration) to provision Volumes to fulfill the Claim

* Depends on underlying implementation's ability to create new Volumes (available space, permissions, etc)
    * Example: If underlying system is a cloud provider enough quota and access levels allow, k8s may be able to create new Volumes
    * Example: If underlying system is a SAN filesystem and there is no more free space, k8s would not be able to create new Volumes (it can't order and install more physical disks on its own, yet! ;) 

## Volumes on AWS

* Amazon Web Services (AWS) offers persistent disks via its Elastic Block Store (EBS)
* Kubernetes on AWS can use (and even dynamically provision) Volumes on EBS to match or meet PersistentVolumeClaims made by Pods

## Demo

* Create MySQL * WordPress deployments to include PersistentVolumeClaims, but we won't create the PersistentVolumes, so this will require Dynamic Provisioning

* Just a reference to our S3 Bucket created in our last activity

``` export KOPS_STATE_STORE=s3://igo-jeferson-basic-k8s-and-kops-demo-bucket ```

* kops create small cluster command

``` kops create cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --zones us-east-1a```

* Obs: If you don't have a default public key, create one with ssh-keygen command, and inform kops:

``` kops create secret --name igo-jeferson-basic-k8s-and-kops-demo.k8s.local sshpublickey admin -i ~/.ssh/id_rsa_igojeferson.pub ```

* Update the cluster, now referencing the secret ssh key and using --yes to create all resources in AWS.

``` kops update cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --yes ```

** Now, we can create our services:

``` kubectl apply -f ./mysql-deployment.yaml ```

``` kubectl apply -f ./wordpress -deployment.yaml ```


* *After finish your tests, remember to delete the cluster using kops, to ensure all resources are released and you are not charged.*

 ``` kops delete cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --yes ```

 * Now, we can create our services, withou our persistentVolume ifself:

``` kubectl apply -f ./mysql-deployment.yaml ```

``` kubectl apply -f ./wordpress -deployment.yaml ```

* And now, we can validate that our persistentVolume was created with EBS service

``` kubectl describe persistentvolumes ```