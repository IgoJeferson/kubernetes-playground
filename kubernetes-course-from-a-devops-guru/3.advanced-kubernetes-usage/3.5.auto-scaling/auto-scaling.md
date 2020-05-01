# Auto Scaling

## What is the horizontal pod autoscaler?

- The Horizontal Pod Autoscaler (HPA) is a Kubernetes facility that adjusts the number of replicas of a Pod to match observed average CPU utilization to a target specified by the user
- There are a variety of configurable options - quite a few - the key takeaway is to know that HPA will create new Pods (or remove Pods) from a replica to maintain average CPU utilization across all Pods to a level specified when you create your HPA - subject to conditions you specify


## Creating an HPA

- Kubectl autoscale provides the needed functions to create an HPA

- To create an autoscaler on our "wordpress" deployment that targets CPU utilization to an average of 50% per POD, within the parameters of a minimum of 1 pod and up to a maximum of 10 Pods

``` $ kubectl autoscale deployment wordpress --cpu-percent=50 --min=1 --max=10 ```

## Practical Example

- Artificially limit our Wordpress Pod so we can stress it easily

``` $ kubectl apply -f ./mysql-deployment.yaml ```
``` $ kubectl apply -f ./wordpress-deployment.yaml ```

- Add an HPA to enable auto-scaling on our Wordpress installation to keep CPU at 50% average (or lower), with a minimum of 1 Pod and max of 5 Pods

``` $ kubectl autoscale deployment wordpress --cpu-percent=50 --min=1 --max=5 ```

- Simulate load using an infinite HTTP request loop from a worker Pod on our cluster to Wordpress to spike our CPU and see how HPA resposts

* In a separate terminal window/tab *

``` $ kubectl run -i --tty load-generator --image=busybox --generator=run-pod/v1 /bin/sh```

``` # while true ; do wget -q -O- http://wordpress.default.svc.cluster.local; done ```


- Check the HPA status (immediately and after about a minute)

``` $ kubectl get hpa ```

