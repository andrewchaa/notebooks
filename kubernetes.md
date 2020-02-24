# Kubernetes

## Azure Container Registry

### Install Azure CLI 2.0

{% embed url="https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest" %}

```text
az login
```

to login to azure.

```text
az account show --output table
az acr --help
az acr create --help
```

### Create a container registry <a id="create-a-container-registry"></a>

Select **Create a resource** &gt; **Containers** &gt; **Container Registry**.

![Creating a container registry in the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/media/container-registry-get-started-portal/qs-portal-01.png)

Enter values for **Registry name** and **Resource group**. The registry name must be unique within Azure, and contain 5-50 alphanumeric characters. 

In case of Registry name, make sure you put all in lowercase to avoid authentication issue when log in, due to case-sensitivity.

![Create container registry in the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/media/container-registry-get-started-portal/qs-portal-03.png)

For details on available service tiers, see [Container registry SKUs](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus).

When the **Deployment succeeded** message appears, select the container registry in the portal.

![Container registry Overview in the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/media/container-registry-get-started-portal/qs-portal-05.png)

Take note of the value of the **Login server**. You use this value in the following steps while working with your registry with the Azure CLI and Docker.

### Log in to registry <a id="log-in-to-registry"></a>

Before pushing and pulling container images, you must log in to the ACR instance. Open a command shell in your operating system, and use the [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#az-acr-login) command in the Azure CLI.Azure CLICopy

```text
az acr login --name <acrName>
```

The command returns `Login Succeeded` once completed.

### Push image to registry <a id="push-image-to-registry"></a>

To push an image to an Azure Container registry, you must first have an image. If you don't yet have any local container images, run the following [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) command to pull an existing image from Docker Hub. For this example, pull the `hello-world` image.Copy

```text
docker pull hello-world
```

Before you can push an image to your registry, you must tag it with the fully qualified name of your ACR login server. The login server name is in the format _&lt;registry-name&gt;.azurecr.io_ \(all lowercase\), for example, _mycontainerregistry007.azurecr.io_.

Tag the image using the [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) command. Replace `<acrLoginServer>` with the login server name of your ACR instance.Copy

```text
docker image ls -a
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Finally, use [docker push](https://docs.docker.com/engine/reference/commandline/push/) to push the image to the ACR instance. Replace `<acrLoginServer>` with the login server name of your ACR instance. This example creates the **hello-world** repository, containing the `hello-world:v1` image.Copy

```text
docker push <acrLoginServer>/hello-world:v1
```

After pushing the image to your container registry, remove the `hello-world:v1` image from your local Docker environment. \(Note that this [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command does not remove the image from the **hello-world** repository in your Azure container registry.\)Copy

```text
docker rmi <acrLoginServer>/hello-world:v1
```

### List container images <a id="list-container-images"></a>

To list the images in your registry, navigate to your registry in the portal and select **Repositories**, then select the repository you created with `docker push`.

In this example, we select the **hello-world** repository, and we can see the `v1`-tagged image under **TAGS**.

![List container images in the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/media/container-registry-get-started-portal/qs-portal-09.png)

### Run image from registry <a id="run-image-from-registry"></a>

Now, you can pull and run the `hello-world:v1` container image from your container registry by using [docker run](https://docs.docker.com/engine/reference/commandline/run/):Copy

```text
docker run <acrLoginServer>/hello-world:v1  
```

Example output:Copy

```text
Unable to find image 'mycontainerregistry007.azurecr.io/hello-world:v1' locally
v1: Pulling from hello-world
Digest: sha256:662dd8e65ef7ccf13f417962c2f77567d3b132f12c95909de6c85ac3c326a345
Status: Downloaded newer image for mycontainerregistry007.azurecr.io/hello-world:v1

Hello from Docker!
This message shows that your installation appears to be working correctly.

[...]
```

## Authenticate with Azure Container Registry from Azure Kubernetes Service

### Create a new AKS cluster with ACR integration <a id="create-a-new-aks-cluster-with-acr-integration"></a>

You can set up AKS and ACR integration during the initial creation of your AKS cluster. To allow an AKS cluster to interact with ACR, an Azure Active Directory **service principal** is used. The following CLI command allows you to authorize an existing ACR in your subscription and configures the appropriate **ACRPull** role for the service principal. Supply valid values for your parameters below.Azure CLICopy

```text
# set this to the name of your Azure Container Registry.  It must be globally unique
MYACR=myContainerRegistry

# Run the following line to create an Azure Container Registry if you do not already have one
az acr create -n $MYACR -g myContainerRegistryResourceGroup --sku basic

# Create an AKS cluster with ACR integration
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr $MYACR

```

Alternatively, you can specify the ACR name using an ACR resource ID, which has has the following format:

/subscriptions/&lt;subscription-id&gt;/resourceGroups/&lt;resource-group-name&gt;/providers/Microsoft.ContainerRegistry/registries/&lt;name&gt;Azure CLICopy

```text
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr /subscriptions/<subscription-id>/resourceGroups/myContainerRegistryResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry
```

This step may take several minutes to complete.

### Configure ACR integration for existing AKS clusters <a id="configure-acr-integration-for-existing-aks-clusters"></a>

Integrate an existing ACR with existing AKS clusters by supplying valid values for **acr-name** or **acr-resource-id** as below.Azure CLICopy

```text
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

You can also remove the integration between an ACR and an AKS cluster with the followingAzure CLICopy

```text
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```

### Working with ACR & AKS <a id="working-with-acr--aks"></a>

#### Import an image into your ACR <a id="import-an-image-into-your-acr"></a>

Import an image from docker hub into your ACR by running the following:Azure CLICopy

```text
az acr import  -n <myContainerRegistry> --source docker.io/library/nginx:latest --image nginx:v1
```

#### Deploy the sample image from ACR to AKS <a id="deploy-the-sample-image-from-acr-to-aks"></a>

Ensure you have the proper AKS credentials

```bash
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Create a file called **acr-nginx.yaml** that contains the following:Copy

```text
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <replace this image property with you acr login server, image and tag>
        ports:
        - containerPort: 80
```

Next, run this deployment in your AKS cluster:Copy

```text
kubectl apply -f acr-nginx.yaml
```

You can monitor the deployment by running:Copy

```text
kubectl get pods
```

You should have two running pods.

```text
NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

## Helm

### Resources

* helm chart: [https://github.com/helm/charts/tree/master/stable/sonatype-nexus](https://github.com/helm/charts/tree/master/stable/sonatype-nexus) 
* kubernetes service: [https://kubernetes.io/docs/concepts/services-networking/service/](https://kubernetes.io/docs/concepts/services-networking/service/) 
* router: [https://docs.traefik.io/](https://docs.traefik.io/) 
* kube browse: [http://127.0.0.1:8001/](http://127.0.0.1:8001/) 
* permission to system service account: kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard 
* installing helm: choco install kubernetes-helm

#### Load Balancer 

* public load balancer: provide outbound connections for virtual machines \(VMs\) inside your virtual network 
* internal \(private\) load balancer: where private IPs are needed at the frontend only

```bash
# browse dashboard 
az aks browse --resource-group experimental --name clearbank-packagemanager

# to get the current context and delete the old context 
kubectl config current-context 
kubectl config delete-context myAKSCluster 
az aks show --resource-group experimental --name clearbank-packagemanager

# to create the context 
az aks get-credentials --name clearbank-packagemanager --resource-group experimental

# to give permission to dashboard role 
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

# installing nexus 
helm repo list
helm search repo stable

# To install sonatype-nexus 
helm install stable/sonatype-nexus --generate-name 
helm install -f .\sonatype-values.yaml stable/sonatype-nexus --generate-name 
helm upgrade sonatype-nexus-1582554190 -f .\sonatype-values.yaml stable/sonatype-nexus

# to list 
helm list
```



