# Kibana

#### Query Language

```javascript
// andresponse:200 and extension:php// orAppName: Restaurant && Level: (Error or Warn)// multi value fieldstags: (success and info and security)
```

#### Logs Count Histogram

1. Add Metrics, Y-Axis: Count
2. Add Buckets, X-Axis: Aggregation of Date Histogram, Field: Timestamp, and Interval: Auto
3. Split Series, Sub Aggregation of Terms, Field: Level.keyword, and Order: by Count, Descending

![](.gitbook/assets/image%20%289%29.png)

