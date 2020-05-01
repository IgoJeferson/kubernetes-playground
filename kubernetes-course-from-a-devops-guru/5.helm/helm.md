# Helm Fundamentals

- What is Helm?
- Helm Installation guides for all operation systems
- First Helm Deployment
- Helm charts and first chart creation
- Using Helm template calls
- Helm values files
- Common Helm commands

## What is Helm?

* Kubernetes can become incredibly complex with all the objects you need to manage
* Helm provides a simple way to package and configure exacly what you need by combining everything into one easy deployment
* Helm fills the need to be able to quickly and reliably provision container applications throught easy installation, update, and removal
* With Helm and its 'charts', you can define,run, and upgrade unique and complex Kubernetes apps
* Tha package manager helps developers streamline the installation and management of kubernetes applications
* Helm Charts are deployment packages all bundled up

### How does Helm Work?

* The package manager is comprised of two parts:
    * The tool itself (the 'helm')
    * The server (the 'tiller') which runs inside Kubernetes clusters
    * When you input the Helm install command, a Tiller server receives the request and installs the appropriate package

### Helm Review

* Helm provides a simple way to programmatically package everything into one simple deployment
* With Helm and its 'charts', developers can quickly define, run, and upgrade even really complex Kubernetes apps
* Helm offers a huge repository of both officional and unofficial charts on GitHub
* Charts can be stored on a disk or just pull them from remote chart repositories as needed