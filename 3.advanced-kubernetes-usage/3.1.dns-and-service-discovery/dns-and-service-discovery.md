# DNS in Kubernetes

- DNS, or the Domain Name Service, is a cornerstone service of the Internet
- It translates names into IP addresses
- Kubernetes has a build-in DNS service that is launched (and configured) automatically
- Kubernetes configures kubelets to tell individual containers to use the DNS service's IP to resolve
DNS names

# DNS as Service Discovery in Kubernetes

- Every Service in your Kubernetes cluster gets a DNS name
- Kubernetes has a specific & consistent nomenclature for deciding what this DNS name is

``` <my-service-name>.<my-namespace>.svc.cluster.local ```

General from of DNS Name for Non-headless Services

- But, you can use this well-known pattern to desing loosely-coupled services or at least set "sane-defaults"
    - One could use environment variables to set the DNS names of other services your app may need
    - Or you could expect services at set DNS names
    - Or, combine two above and expect services at well-known DNS names and allow environment variables
    to override ("convertion over configuration")

# A note on Namespaces

- Kubernetes allows you to define "namespaces" - they allow  you to separate your cluster into smaller
logical clusters
    - By default, everything you do is in the "default" namespace 
    - Kubernetes DNS names will include this namespace in the assigned DNS name
- Namespaces are helpful mostly for large clusters with many users across many teams & projects - using the default namespace is fine if this logical separation is not required.

# A Practical example

- Consider deploing WordPress & MySQL to a Kubernetes cluster
- WordPress & MySQL are both separate deployments
    - However, to function Wordpress needs MySQL
    - Specifically, it must know a TCP/IP hostname at which where to connect to MySQL
- Let's use DNS to create a WordPress deployment that expects MySQL to be running on the "wordpress-mysql"
hostname in the same namespace
    - We will alow this value to be overridden by environmental variable as a "best practice"
    - This follows the "convention over configuration" best practive


# MySQL

- We'll deploy MySQL in a deployment named "wordpress-mysql"
- Once deployed, it will run MySQL5.6 on port 3306 on the wordpress-mysql hostname

# WordPress

- We'll deplyo WordPress named as "wordpress"
- We'll configure MySQL to be available on the "wordpress-mysql" hostname and tell Wordpress to expect it there

# Process

1. Deploy Mysql

``` kubectl create -f mysql-deployment.yaml ```

2. Deploy Wordpress

``` kubectl create -f wordpress-deployment.yaml ```

3. To validate and access the wordpress page

``` kubectl get services wordpress ```
``` minikube service wordpress --url ``` copy the url returned in this command into your preffered browser

- (which Kubernetes apiVersion should I use?)[https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-apiversion-definition-guide.html]