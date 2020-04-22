# Cosmos DB

## Provision The database and container with Terraform

```text
resource "azurerm_cosmosdb_account" "metadata-cosmos-account" {
  name                = "${var.organisation}-${var.system}-${var.environment}-metadata-${var.location}"
  location            = "${azurerm_resource_group.metadata_rg.location}"
  resource_group_name = "${azurerm_resource_group.metadata_rg.name}"
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
    location          = "${azurerm_resource_group.metadata_rg.location}"
    failover_priority = 0
  }

  ip_range_filter = local.firewall_ip_range_filter
}

resource "azurerm_cosmosdb_sql_database" "metadata-cosmos-database" {
  name                = "metadata"
  resource_group_name = "${azurerm_resource_group.metadata_rg.name}"
  account_name        = "${azurerm_cosmosdb_account.metadata-cosmos-account.name}"
}

resource "azurerm_cosmosdb_sql_container" "metadata-cosmos-container" {
  name                = "metadata"
  resource_group_name = "${azurerm_resource_group.metadata_rg.name}"
  account_name        = "${azurerm_cosmosdb_account.metadata-cosmos-account.name}"
  database_name       = "${azurerm_cosmosdb_sql_database.metadata-cosmos-database.name}"
  partition_key_path  = "/TransactionId"
}

output "cosmos_db_endpoint" {
  value = "${azurerm_cosmosdb_account.metadata-cosmos-account.endpoint}"
}

output "cosmos_db_primary_master_key" {
  value     = "${azurerm_cosmosdb_account.metadata-cosmos-account.primary_master_key}"
  sensitive = true
}
```

## Operations

### Upsert document

```csharp
Services.AddSingleton<ICosmosDbRepository>(s =>
    {
        var options = s.GetService<IOptions<CosmosDbOptions>>();
        var client = new CosmosClient(options.Value.ConnectionString);
        return new CosmosDbRepository(client, "clients", "eventsLog");
    });
    
public async Task Insert(EventLog eventLog)
{
    var eventLogData = new 
    {
        id = eventLog.EventId,
        eventLog.EntityId,
        eventLog.DateTimeStamp,
        eventLog.EventName,
        eventLog.EventNameSequence,
        eventLog.Event
    };
    await _container.UpsertItemAsync(eventLogData, new PartitionKey(eventLogData.ClientId.ToString()));
}
```

## Performance

### Capacity calculator

[https://cosmos.azure.com/capacitycalculator/](https://cosmos.azure.com/capacitycalculator/)

![](.gitbook/assets/image%20%283%29.png)

### The size of an item

```text
x-ms-resource-usage: documentSize=1;documentsSize=1009;documentsCount=230;collectionSize=1069;

1 KB for the document
1009 KB for the all documents
1069 KB for all documents + metadata
```

### Indexing

Opt-out policy to selectively exclude some property paths

```javascript
{
  "indexingMode": "consistent",
  "includedPaths": [
    {
      "path": "/*"
    }
   ],
   "excludedPaths": [
     {
       "path": "/path/to/single/excluded/property/?"
     },
     {
       "path": "/path/to/root/of/multiple/excluded/properties/*"
     }
   ]
}
```

Opt-in polic to selectively include some property paths

```javascript
{
  "indexingMode": "consistent",
  "includedPaths": [
    {
      "path": "/path/to/included/property/?"
    },
    {
      "path": "/path/to/root/of/multiple/included/properties/*"
    }
  ],
  "excludedPaths": [
    {
      "path": "/*"
    }
  ]
}
```

No Indexing

```javascript
{
  "indexingMode": "none",
  "automatic": false,
  "includedPaths": [],
  "excludedPaths": []
}    
```

## Queries

```text
SELECT * FROM c 
 WHERE c.TransactionId = 'b848c6cc-9da5-4792-b554-34c80426a610'
```

Note: Queries are only for selection. You can't delete a document by query.

### 

## Security

### Firewall and virtual networks

Sometimes, the firewall doesn't allow your local machine to connect to it. Update firewall settings. The exception message will tell you which IP it's blocking. If you use VPN, your ip address will be different from your public IP as the traffic routes through your corporate VPN

