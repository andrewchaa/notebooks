# javascript

## Types

### Array

#### Merge / flatten an array of arrays

```javascript
// use concat
var arrays = [["$6"], ["$12"], ["$25"], ["$25"], ["$18"], ["$22"], ["$10"]];
var merged1 = [].concat.apply([], arrays);

// use flat()
const merged2 = arrays.flat();
```

### Number

#### Format number

```javascript
const formatNumber = (number) => {
  const formatter = new Intl.NumberFormat('en-UK', {
    style: 'currency',
    currency: 'EUR',
  });

  return formatter.format(number);
} 
```

