# Azure

## API Manager

#### To create api manager with Terraform

1. Create a [terraform file for APIM](azure.md#terraform-for-apim)
2. Update [azure api definition json](azure.md#azure-api-manager-definition-json)

#### Terraform for APIM

```bash
resource "azurerm_template_deployment" "cust_api" {
  name                = "cust_api_template"
  resource_group_name = "${var.api_management_rg_name}"

  template_body = <<DEPLOY
  ${file("${path.module}/cust.json")}
  DEPLOY

  parameters {
    apimInstanceName = "${var.api_management_name}"
    backendName      = "${var.backend_name}"
  }

  deployment_mode = "Incremental"
}
```

#### Azure API Manager Definition json

```javascript
{
  "type": "Microsoft.ApiManagement/service/apis/operations",
  "name": "[concat(parameters('apimInstanceName'), '/', variables('custApiName'), '/create-cust')]",
  "apiVersion": "2017-03-01",
  "properties": {
    "displayName": "Create Cust",
    "method": "POST",
    "urlTemplate": "/api/v1/inst/{id}/cust",
    "templateParameters": [
      {
        "name": "id",
        "type": "",
        "required": true,
        "values": []
      }
    ],
    "description": ""
  },
  "dependsOn": [
    "[resourceId('Microsoft.ApiManagement/service/apis', parameters('apimInstanceName'), variables('custApiName'))]"
  ]
},
```

## Service Fabric

#### Installing client admin certificate to connect to cluster manager

1. Go to key vault
2. Update Access policies if you don't have permission to list certificates
   1. Add Access Policy
   2. Select all
   3. Don't forget to save
3. Go to service fabric client admin
4. Click on the version.
5. Click on "Download in PFX/PEM format
6. Install it locally by double-clicking it

## Subscriptions

### Tenants, users, and subscriptions <a id="tenants-users-and-subscriptions"></a>

You might have some confusion over the difference between tenants, users, and subscriptions within Azure. 

* Tenant: the Azure Active Directory entity that encompasses a whole organization. This tenant has at least one _subscription_ and _user_. 
* User: an individual and is associated with only one tenant, the organization that they belong to. Users are those accounts that sign in to Azure to create, manage, and use resources. A user may have access to multiple _subscriptions_, which are the agreements with Microsoft to use cloud services, including Azure. Every resource is associated with a subscription.

### Change the active subscription <a id="change-the-active-subscription"></a>

To access the resources for a subscription, switch your active subscription or use the `--subscription` argument. Switching your subscription for all commands is done with [az account set](https://docs.microsoft.com/en-us/cli/azure/account#az-account-set).

To switch your active subscription:

1. Get a list of your subscriptions with the [az account list](https://docs.microsoft.com/en-us/cli/azure/account#az-account-list) command

   ```text
   az account list --output table
   ```

2. Use `az account set` with the subscription ID or name you want to switch to

   ```text
   az account set --subscription "My Demos"
   ```

To run only a single command with a different subscription, use the `--subscription` argument. This argument takes either a subscription ID or subscription name:Azure CLICopyTry It

```text
az vm create --subscription "My Demos" --resource-group MyGroup --name NewVM --image Ubuntu
```

