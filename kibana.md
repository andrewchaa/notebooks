# Kibana

#### Query Language

```javascript
// and
response:200 and extension:php

// or
AppName: Restaurant && Level: (Error or Warn)

// multi value fields
tags: (success and info and security)
```

#### Logs Count Histogram

1. Add Metrics, Y-Axis: Count
2. Add Buckets, X-Axis: Aggregation of Date Histogram, Field: Timestamp, and Interval: Auto
3. Split Series, Sub Aggregation of Terms, Field: Level.keyword, and Order: by Count, Descending

![](.gitbook/assets/image%20%2817%29.png)

