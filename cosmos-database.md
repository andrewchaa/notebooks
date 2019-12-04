# Cosmos Database

## Create a database with terraform

```text
resource "azurerm_cosmosdb_account" "sample-account" {
  name                = "${var.organisation}-${var.system}-${var.environment}-${var.location}"
  location            = "${azurerm_resource_group.sample.location}"
  resource_group_name = "${azurerm_resource_group.sample.name}"
  offer_type          = "Standard"
  kind                = "GlobalDocumentDB"
  enable_automatic_failover = false

  consistency_policy {
    consistency_level       = "Eventual"
  }

  dynamic "virtual_network_rule" {
    for_each = ["${data.azurerm_subnet.service_fabric.id}"]
      content {
        id = virtual_network_rule.value
      }
  }

  geo_location {
    location          = "${azurerm_resource_group.sample.location}"
    failover_priority = 0
  }

  ip_range_filter = local.firewall_ip_range_filter
}

resource "azurerm_cosmosdb_sql_database" "cosmos-database" {
  name                = "customers"
  resource_group_name = "${azurerm_resource_group.sample.name}"
  account_name        = "${azurerm_cosmosdb_account.sample-account.name}"
}

resource "azurerm_cosmosdb_sql_container" "cosmos-container" {
  name                = "eventsLog"
  resource_group_name = "${azurerm_resource_group.sample.name}"
  account_name        = "${azurerm_cosmosdb_account.sample-account.name}"
  database_name       = "${azurerm_cosmosdb_sql_database.cosmos-database.name}"
  partition_key_path  = "/PartitionId"
}
```



