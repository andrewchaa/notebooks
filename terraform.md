# Terraform

## Getting Started

1. Configure provider
2. Create a resource group
3. Create a virtual network within the resource group

```text
# Configure the Azure Provider
provider "azurerm" {
  # whilst the `version` attribute is optional, we recommend pinning to a given version of the Provider
  version = "=1.36.0"
}

# Create a resource group
resource "azurerm_resource_group" "test" {
  name     = "production"
  location = "West US"
}

# Create a virtual network within the resource group
resource "azurerm_virtual_network" "test" {
  name                = "production-network"
  resource_group_name = "${azurerm_resource_group.test.name}"
  location            = "${azurerm_resource_group.test.location}"
  address_space       = ["10.0.0.0/16"]
}
```

#### template\_body

```text
resource "azurerm_template_deployment" "customers_api" {
  name                = "api_template"
  resource_group_name = "${var.api_management_rg_name}"

  template_body = <<DEPLOY
  ${file("${path.module}/api.json")}
DEPLOY

  parameters = {
    apimInstanceName = "${var.api_management_name}"
    backendName      = "${var.backend_name}"
  }

  deployment_mode = "Incremental"
}
```

## CLI commands

```text
terraform fmt
```

Scans the currenty directory for configuration files and write them to a canonical format and style

## Creating Resources

### API Management

```bash
resource "azurerm_api_management_api" "paymentgateway" {
  name                = "paymentgateway-api"
  api_management_name = var.api_management_name
  resource_group_name = var.api_management_resource_group_name
  revision            = 1
  version             = 1
  version_set_id      = module.apim_version_set.version_set_id
  display_name        = "PaymentGateway Api"
  path                = "paymentgateway"
  protocols           = ["https"]
}

resource "azurerm_api_management_api_policy" "paymentgateway" {
  api_name            = azurerm_api_management_api.paymentgateway.name
  api_management_name = var.api_management_name
  resource_group_name = var.api_management_resource_group_name
  xml_content         = <<XML
<policies>
  <inbound>
    <base />
    <set-backend-service backend-id="${var.backend_name}"
    sf-resolve-condition="@(context.LastError?.Reason == &quot;BackendConnectionFailure&quot;)"
    sf-service-instance-name="${local.service_fabric_instance_name}" />    
  </inbound>
</policies>
XML
}

data "azurerm_api_management_product" "paymentgateway" {
  product_id          = var.api_management_product
  api_management_name = var.api_management_name
  resource_group_name = var.api_management_resource_group_name
}

resource "azurerm_api_management_product_api" "paymentgateway" {
  api_name            = azurerm_api_management_api.paymentgateway.name
  product_id          = data.azurerm_api_management_product.paymentgateway.product_id
  api_management_name = var.api_management_name
  resource_group_name = var.api_management_resource_group_name
}
```

### CosmosDB

1. Create account
2. Create sql database
3. Create sql container

```text
resource "azurerm_cosmosdb_account" "cosmos-account" {
  name                = "${var.environment}-example-${var.location}"
  location            = "${azurerm_resource_group.example_rg.location}"
  resource_group_name = "${azurerm_resource_group.example_rg.name}"
  offer_type          = "Standard"
  kind                = "GlobalDocumentDB"
  enable_automatic_failover = false

  consistency_policy {
    consistency_level       = "Eventual"
  }

  is_virtual_network_filter_enabled = local.enable_virtual_network_filter

  dynamic "virtual_network_rule" {
    for_each = ["${data.azurerm_subnet.service_fabric.id}"]
      content {
        id = virtual_network_rule.value
      }
  }

  geo_location {
    location          = "${azurerm_resource_group.example_rg.location}"
    failover_priority = 0
  }

  ip_range_filter = local.firewall_ip_range_filter
}

resource "azurerm_cosmosdb_sql_database" "cosmos-database" {
  name                = "customers"
  resource_group_name = "${azurerm_resource_group.example_rg.name}"
  account_name        = "${azurerm_cosmosdb_account.cosmos-account.name}"
}

resource "azurerm_cosmosdb_sql_container" "cosmos-container" {
  name                = "events"
  resource_group_name = "${azurerm_resource_group.example_rg.name}"
  account_name        = "${azurerm_cosmosdb_account.cosmos-account.name}"
  database_name       = "${azurerm_cosmosdb_sql_database.cosmos-database.name}"
  partition_key_path  = "/Id"
}
```

### Import ARM \(Azure Resource Manager\) Template

#### cosmos.tf

```javascript

# cosmos.json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "accountName": {
      "type": "string",
      "metadata": {
        "description": "Cosmos DB account name, max length 44 characters"
      }
    },
    "databaseName": {
      "type": "string",
      "metadata": {
        "description": "The name for the SQL database"
      }
    },
    "containerName": {
      "type": "string",
      "metadata": {
        "description": "The name for the container"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases",
      "name": "[concat(parameters('accountName'), '/', parameters('databaseName'))]",
      "apiVersion": "2019-08-01",
      "properties": {
        "resource": {
          "id": "[parameters('databaseName')]"
        }
      }
    },
    {
      "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers",
      "name": "[concat(parameters('accountName'), '/', parameters('databaseName'), '/', parameters('containerName'))]",
      "apiVersion": "2019-08-01",
      "dependsOn": [
        "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases', parameters('accountName'), parameters('databaseName'))]"
      ],
      "properties": {
        "resource": {
          "id": "[parameters('containerName')]",
          "partitionKey": {
            "paths": [
              "/TransactionId"
            ],
            "kind": "Hash"
          },
          "indexingPolicy": {
            "indexingMode": "none"
          }
        }
      }
    }
  ]
}
```

### Set custom timeout

```bash
resource "aws_db_instance" "example" {
  # ...

  timeouts {
    create = "60m"
    delete = "2h"
  }
}
```

### Application Insight

```bash
data "azurerm_resource_group" "app-insights" {
  name     = "${var.organisation}-${var.system}-${var.environment}-app_insights-${var.location}"
}

resource "azurerm_application_insights" "app_insights" {
  name                = "${var.organisation}-${var.system}-${var.environment}-jpm_paymentgateway-${var.location}"
  location            = var.location
  resource_group_name = data.azurerm_resource_group.app-insights.name
  application_type    = "other"
  retention_in_days   = 90
  tags                = var.tags
}

output "jpm_paymentgateway_instrumentation_key" {
  value = "${azurerm_application_insights.app_insights.instrumentation_key}"
}
```

## Providers

A provider is responsible for understanding API interactions and exposing resources. 

Azure releated providers are

* Azure
* Azure Active Directory

```bash
provider "azurerm" {
  version = "=2.0.0"
  features {}
}

provider "azuread" {
  version = "=0.7.0"
}
```

## Variables

### Output Values

```text
output "cosmos_db_endpoint" {
  value = "${azurerm_cosmosdb_account.metadata-cosmos-account.endpoint}"
}

output "cosmos_db_primary_master_key" {
  value     = "${azurerm_cosmosdb_account.metadata-cosmos-account.primary_master_key}"
  sensitive = true
}
```

## Conditional Configuration

```bash
resource "azurerm_cosmosdb_account" "metadata" {
  enable_automatic_failover = var.environment == "prod"

  dynamic "geo_location" {
    for_each = var.environment == "prod" ? [local.primary_geo_location, local.failover_geo_location] : [local.primary_geo_location]
    content {
      location          = geo_location.value.location
      failover_priority = geo_location.value.failover_priority
    }
  }

# locals.tf
failover_geo_location = {
  location            = "ukwest"
  failover_priority   = 1
}

primary_geo_location  = {
  location            = azurerm_resource_group.metadata_rg.location
  failover_priority   = 0
}

```

## Running it locally

```bash
# list all the subscriptions
az account list --output table

# set the current subscription
az account set --subscription "team-xxxxxxx"

# comment out backend.tf

# plan
terraform plan
```

## Removing resources from state file

Sometimes,  you have to remove some resources from the state file. It's becasue there are too many changes and terraform can't resolve what to remove and what to create.

```bash
terraform state rm rm module.apim_version_set
```

Make sure you remove the top-leve resource. In my case, rm module.apim\_version\_set.azurerm\_api\_management\_api\_version\_set.api\_version\_set failed

## Errors

features error

```bash
provider "azurerm" {
  version         = ">= 2.0"
  features {}
}
```



