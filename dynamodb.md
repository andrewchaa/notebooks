# dynamodb

## Scan

* TableName: table name
* IndexName: index name. Use GSI for sorting purpose

```text
let params = {
  TableName: registrationsTable,
  IndexName: 'warrantyYear-index'
}

dynamodb.scan(params, (err, data) => {
  if (err) {
    res.statusCode = 500;
    res.json({ error: 'Could not load items: ' + err });
  } else {
    res.json(data.Items);
  }
})
```

### Query

Any query requires partition key. If you want to query by a different attribute, create GSI, Global Secondary Index.



