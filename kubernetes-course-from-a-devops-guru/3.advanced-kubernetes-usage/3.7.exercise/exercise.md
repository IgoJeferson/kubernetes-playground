# On Your Own Exercise

* Set up a deployment of the "Ghost" open-source publishing platform that maintains its data across restarts of the container (or minikube)

* For extra credt & challenge, set up the deployment to scale when CPU usage is high in the deployment


## A Possible Solution

* Setup horizontal pod auto-scaler to scale when CPU hits 50%
``` kubectl autoscale deployment ghost --cpu-percent=50 --min=1 --max=5 ```