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

#### CosmosDB

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



