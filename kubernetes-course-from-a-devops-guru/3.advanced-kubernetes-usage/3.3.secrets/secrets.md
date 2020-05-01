# Secrets

- Secrets contain small amounts of data
- Secrets can be delivered to a pod in the form of:
    - A file placed on a volume at runtime containing the secret data (useful for certificates)
    - An environment variable referenced by the Pod and inserted at runtime into the environment by the kubelete running the Pod - just like any other environment varible


## How secrets are stored

- Kubernetes provides separation for secrets, it does not provide strong encryption
- Secrets are typically Base64 encoded strings stored separately from configuration and inject at runtime
- You can encode it manually or use Kubernetes' tools to do it for you

## How secrets are structured

- Secrets are key:value pairs, both the key and the value are arbitrary strings

## A Note on Base64

- Base64 encoding is a method that stores binary data in a "safe" string by translating it using a "radix-64" representation
    - Helps store small pieces of text or non-text data that would otherwise be difficult to handle in systems or data structures that expect text
    - Small files, images, or even text can be Base64 encoded - the resulting string is typically "safe" in the sense it does not contain characters many popular systems/protocols such as HTTP or the like do not allow as parameters

- The string "Kubernetes" is encoded in Base64 as "S3ViZXJuZXRlcw=="

## Creating a secret

- You guessed it: kubectl will help you here!
- Creating a secret using kubectl
    - From a file (creates a secret named generic db-user-pass with a username from username.txt file and password from the password.txt file)

    ``` kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt  ```

    - From a literal on the command line

    ``` kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD ```  

## A Practical Example

- Let's remedy a weak-point in our WorPress & MySQL stack: a hard-code plain-text password
- Let's replace the plain-text value in the deployment with a "secret" in both the MySQL deployment & the WordPress deployment

1. We'll create the secret using kubectl command line

``` kubectl create secret generic mysql-pass --from-literal=password=AMuchBetterWayToStoreAPassword ```  

``` kubectl get secret ```

2. We'll apply the modified deployments for both MySQL and WordPress that have the new environment variable declaration referring to our secret for the value instead of including it inline

``` kubectl apply -f ./mysql-deployment.yaml ```

``` kubectl apply -f ./wordpress-deployment.yaml ```


3. You can verify the environment variable setted into the secret

``` kubectl describe deployment wordpress ```

4. Clean your cluster 

``` kubectl delete -f ./wordpress-deployment.yaml ```

``` kubectl delete -f ./mysql-deployment.yaml ```