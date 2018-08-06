# Azure AKS Walkthrough
A walkthrough for creating and managing an AKS cluster from a development and DevOps point-of-view.

## Intro
**Name:** Chris Wiederspan  
**Role:** Microsoft Azure App Dev Specialist  
**Email:** chris.wiederspan@microsoft.com

## Prerequisites
You will also need a full, working Azure subscription so you can create your own cluster to test on. We will also use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) as part of this excercise, so make sure you can access your subscription through both the [Azure Portal](https://portal.azure.com/) and with cloud shell.

## AKS Cluster Setup
Open up Azure Cloud Shell and setup a couple variables that we will use when creating our cluster.

`CLUSTER_NAME="cdw-aks-20180804"`  
`CLUSTER_LOCATION="WESTUS2"`

Create a Resource Group that will house the AKS cluster.

`az group create --name $CLUSTER_NAME --location $CLUSTER_LOCATION`

Now, create the AKS Cluster itself. This will take several minutes, so use the `--no-wait` so we can continue to use the shell while we are waiting.

`az aks create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --location $CLUSTER_LOCATION --node-count 1 --dns-name-prefix $CLUSTER_NAME --generate-ssh-keys --enable-addons http_application_routing --kubernetes-version 1.10.6 --no-wait`

While our cluster is being created, we can check on the status.

`az aks list -o table`

After several minutes, when our AKS cluster has finished creating, we need to wire it up to the Kubernetes CLI - `kubectl`. Luckily, usint cloud shell to do this is very easy.

`az aks get-credentials -n $CLUSTER_NAME -g $CLUSTER_NAME`

You can confirm that kubectl is now configured correctly.

`kubectl config view`

Before we go further, let's create a new namespace that we will use later on when we wire up our Dev Space.

`kubectl create namespace dev`

You can find out more about Dev Spaces [here](https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-netcore-visualstudio). They allow you to deploy and debug your project code to an AKS cluster right from Visual Studio or VS Code. This becomes invaluable when you need developing a microservice-based application that relies on dozens of microservice components that aren't practical to install on every developer's local machine.

Using the `dev` namespace that we setup above, setup your cluster to use Dev Spaces.

`az aks use-dev-spaces -g $CLUSTER_NAME -n $CLUSTER_NAME  // TODO: Does this need to be run on local machine vs. cloud shell`


*** Create AKS Project in Visual Studio ***







kubectl get nodes
kubectl get namespaces
kubectl get pods
kubectl get services