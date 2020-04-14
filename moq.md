# Moq

## Callback

### Async Callback

When you mock async method and use callback, make sure you set up return as well, so that the async method can return the initiated task.

```csharp
Business business = null; 
_api.BusinessRepositoryMock.Setup(r => r.UpdateBusiness(It.IsAny<Business>()))
    .Callback<Business>(r => business = r)
    .Returns(Task.CompletedTask);

var response = await _client.PatchAsync($"api/v1/businesses/{id}", CreateJsonContent(request));

Assert.Equal(HttpStatusCode.NoContent, response.StatusCode);
Assert.Equal("Updated Name", business.Name);
```

### Multiple parameters

```csharp
FpsMetadataDataModel metadataDataModel = null;
_cosmosContainer.Setup(x => x.UpsertItemAsync(It.IsAny<MetadataDataModel>(),
    new PartitionKey(@event.CashTransactionId.ToString()),
    null, 
    default(CancellationToken))
    )
    .ReturnsAsync((ItemResponse<FpsMetadataDataModel>)null)
    .Callback<MetadataDataModel, PartitionKey?, ItemRequestOptions, CancellationToken>(
        (dataModel, partitionKey, y, z) => metadataDataModel = dataModel);
```



