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



