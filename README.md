# Kubernetes Playground

 Files and deployments used in studies of kubernetes + docker + minikube

## Prerequisitos

* Start with a Linux or Mac OS X machine with a hypervisor installed


## Kubernetes and Minikube Setup

### Installing kubectl

* Download the latest package for your OS**

** Linux: 
 
``` curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl ```
 
** MacOS: 
 
``` curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/darwin/amd64/kubectl ```
 
** Windows: 
 
``` curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/windows/amd64/kubectl.exe ```

#### Kubernetes Config and Test
 
* Make the binary executable (macOS and Linux only): 
```chmod +x ./kubectl```

* Add the binary to your PATH (macOS and Linux only): 
```sudo mv ./kubectl /usr/local/bin/kubectl```

* Testing kubectl: ```kubectl version```
 
### Installing minikube

* Download the latest package for your OS**

** Linux: 
 
``` curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.23.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/ ```
 
** macOS: 
 
```curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.23.0/minikube-darwin-amd64 && chmod +x ```

** Windows: 

Use browser download link from releases [link](https://kubernetes.io/docs/tasks/tools/install-minikube/)


#### Minikube Config and Test

* Add the binary to your PATH (macOS and Linux only):
 ```minikube && sudo mv minikube /usr/local/bin/```

* Start minikube: ```minikube start```


## First app using Kubernetes and minikube

*  Deploy a sample Kubernetes "deployment" to your local minikbue
```kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080```

* Expose this deployment to an external network
```kubectl expose deployment hello-minikube  --type=NodePort```

* List the "pods" of this deployment
```kubectl get pod```

* Access the sample service
```curl $(minikube service hello-minikube --url)```

* Delete the deployment
```kubectl delete deployment hello-minikube```

* Stop minikube
```minikube stop```
