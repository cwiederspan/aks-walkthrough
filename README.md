# Azure AKS Walkthrough
A walkthrough for creating and managing an AKS cluster from a development and DevOps point-of-view.

## Intro
**Name:** Chris Wiederspan  
**Role:** Microsoft Azure App Dev Specialist  
**Email:** chris.wiederspan@microsoft.com

## Prerequisites
You will also need a full, working Azure subscription so you can create your own cluster to test on. We will also use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) as part of this excercise, so make sure you can access your subscription through both the [Azure Portal](https://portal.azure.com/) and with cloud shell.

```bash
git clone https://github.com/cwiederspan/aks-walkthrough.git
```

## AKS/Kubernetes Setup

### Task 1: Create the AKS Cluster

1. Create two variables that you will use when creating your cluster.
   ```bash
   CLUSTER_NAME="cdw-aks-20180816"  
   CLUSTER_LOCATION="WESTUS2"
   ```

2. Create a Azure Resource Group that will contain the AKS cluster.
   ```bash
   az group create --name $CLUSTER_NAME --location $CLUSTER_LOCATION
   ```

3. Create the AKS Cluster using the CLI.
   ```bash
   az aks create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --location $CLUSTER_LOCATION 
   --node-count 2 --node-vm-size Standard_DS2_v2 --dns-name-prefix $CLUSTER_NAME --generate-ssh-keys 
   --enable-addons http_application_routing --kubernetes-version 1.11.1 --no-wait
   ```
   > **NOTE:** This will take several minutes, so use the `--no-wait` so you can continue to use the shell while we are waiting.

4. Monitor the status of the cluster while waiting for it to finish provisioning.
   ```
   az aks list -o table
   ```

### Task 2: Manage AKS with Kubectl

After several minutes, when your AKS cluster has finished creating, we need to wire it up to the Kubernetes CLI - `kubectl`. Luckily, using cloud shell to do this is very easy.

1. Get AKS credentials for kubectl.
   ```bash
   az aks get-credentials -n $CLUSTER_NAME -g $CLUSTER_NAME
   ```

2. Confirm kubectl is connected and configured correctly.
   ```bash
   kubectl config view
   ```

### Task 3: Install Helm/Tiller

1. Because we setup our cluster using RBAC above, we need to create a service account and binding so that Helm (on our dev machine or cloud shell) can talk to Tiller (on the cluster).
   ```bash
   kubectl create -f utilities\helm-rbac.yaml
   ```

2. Now that we can connect to the cluster, let's initialize Helm in our cloud shell, and Tiller on the cluster.
   ```bash
   helm init --service-account tiller
   ```

### Task 4: Create a New Project

1. Use the cli to create a new .NET Core project (or use the existing content in the **src** folder).
   ```bash
   mkdir newsrc
   cd newsrc
   curl -o .gitignore https://www.gitignore.io/api/visualstudio%2Cvisualstudiocode
   dotnet new razor
   draft create --app aksdevopsdemo --pack csharp
   touch azure-pipelines.yaml
   code .
   ```






Before we go further, let's create a new namespace that we will use later on when we wire up our Dev Space.

`kubectl create namespace dev`

You can find out more about Dev Spaces [here](https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-netcore-visualstudio). They allow you to deploy and debug your project code to an AKS cluster right from Visual Studio or VS Code. This becomes invaluable when you need developing a microservice-based application that relies on dozens of microservice components that aren't practical to install on every developer's local machine.

Using the `dev` namespace that we setup above, setup your cluster to use Dev Spaces.

`az aks use-dev-spaces -g $CLUSTER_NAME -n $CLUSTER_NAME`  // TODO: Does this need to be run on local machine vs. cloud shell


*** Create AKS Project in Visual Studio ***

*** Deploy to AKS Using Helm ***

`draft init`



`helm repo add helm-demo-site https://github.com/cwiederspan/helm-demo-site`






kubectl get all --all-namespaces  
kubectl get nodes  
kubectl get namespaces  
kubectl get pods  
kubectl get services  