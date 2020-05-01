# Volumes

## Like disks, but with a bit more

- Volumes can be considered just a directory, with some data, which containers in a pod can access

- Kubernetes supports multiples types of volumes that take care of how that data is stored, persisted, and made available
    - Support for a variety of cloud providers' block store products
    - Support for SAN-type hardware, file systems, etc
    - Support for local volumes (for testing/minikube only! Not production)

- Certain types of Volumes can also provide sharing of files between Pods by being mounted to multiple Pods simultaneously

## Types of Volumes

Check -> https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes

## Using Volumes

- Pods can specify what volumes they need and where to mount them
    - Using the spec.volumes field (what volumes they need)
    - Using the spec.containers.volumeMounts field (where to mount them)
- Processes in the container then see a filesystem view of the data in that Volume
- Using Volumes lets us separate stateless portions of our application (the code) from statefull data
    - The infrastructure can be scaled, maintained, and live separately from the data it works on/with
    - Also may ease portability, backup, recovery, and other management tasks in well-architected systems

## Using PersistentVolumes

- PersistentVolumes are a Volumes designed to provide persistent disk-like functionality
- Using them involves:
    - Provisioning a PersistentVolume (akin to creating/installing a disk in a virtual machine or hardware server)
    - Establishing a PersistentVolumeClaim (it is a request for storage by a user/Pod)
- By examining available PersistentVolume and demands from PersistentVolumeClaims by running Pods, Kubernetes binds available volumes to Pods based on the options specified by the PersistentVolumeClaims to matching PersistentVolume (e.g. by name, storage size, sotrage class, etc asked for in the Claims)

## Process

1. Create the volumes

``` kubectl create -f local-volumes.yaml ```

``` kubectl get persistentvolumes ```

2. Deploy Mysql

``` kubectl apply -f mysql-deployment.yaml ```

3. Deploy Wordpress

``` kubectl apply -f wordpress-deployment.yaml ```

4. To validate and access the wordpress page

``` kubectl get services wordpress ```

``` minikube service wordpress --url ``` copy the url returned in this command into your preffered browser

5. Update the wordpress configuration create a post with a name like "This post should live on.", delete de deployment and recreate with step number 3, your post should be there.

``` kubectl delete deployment wordpress ```

6. Clean your cluster 

``` kubectl delete -f ./wordpress-deployment.yaml ```

``` kubectl delete -f ./mysql-deployment.yaml ```

``` kubectl delete -f ./local-volumes.yaml ```

``` minikube stop ```

``` minikube start ```