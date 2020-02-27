# Setting Up High Availability

## Key HA Design Considerations

* How many masters (an odd number is critical)
* Where the master are located (across different AWS availability zones)

## Let's create a high availability cluster

1. kops create
2. Look at the nodes to see multiple masters
3. Deploy Wordpress & MySQL withou volumes


* Just a reference to our S3 Bucket created in our last activity

``` export KOPS_STATE_STORE=s3://igo-jeferson-basic-k8s-and-kops-demo-bucket ```

* kops create command, the main difference between our last version is that we specify 3 different zones for our worker and master nodes, and informe the paramenter node code with value 3.

``` kops create cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --zones us-east-1a,us-east-1b,us-east-2b --node-count 3 --master-zones us-east-1a,us-east-1b,us-east-2b ```

* Obs: If you don't have a default public key, create one with ssh-keygen command, and inform kops:

``` kops create secret --name igo-jeferson-basic-k8s-and-kops-demo.k8s.local sshpublickey admin -i ~/.ssh/id_rsa_igojeferson.pub ```

* Update the cluster, now referencing the secret ssh key and using --yes to create all resources in AWS.

``` kops update cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --yes ```

** Now, we can create our services:

``` kubectl apply -f ./mysql-deployment.yaml ```

``` kubectl apply -f ./wordpress -deployment.yaml ```


* *After finish your tests, remember to delete the cluster using kops, to ensure all resources are released and you are not charged.*

 ``` kops delete cluster igo-jeferson-basic-k8s-and-kops-demo.k8s.local --yes ```