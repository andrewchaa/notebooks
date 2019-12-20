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

Enter values for **Registry name** and **Resource group**. The registry name must be unique within Azure, and contain 5-50 alphanumeric characters. For this quickstart create a new resource group in the `West US` location named `myResourceGroup`, and for **SKU**, select 'Basic'. Select **Create** to deploy the ACR instance.

![Create container registry in the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/media/container-registry-get-started-portal/qs-portal-03.png)

In this quickstart you create a _Basic_ registry, which is a cost-optimized option for developers learning about Azure Container Registry. For details on available service tiers, see [Container registry SKUs](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus).

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

### Clean up resources <a id="clean-up-resources"></a>

To clean up your resources, navigate to the **myResourceGroup** resource group in the portal. Once the resource group is loaded click on **Delete resource group** to remove the resource group, the container registry, and the container images stored there.

![Delete resource group in the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/media/container-registry-get-started-portal/qs-portal-08.png)

