# javascript

## Array

#### Merge / flatten an array of arrays

```javascript
// use concat
var arrays = [["$6"], ["$12"], ["$25"], ["$25"], ["$18"], ["$22"], ["$10"]];
var merged1 = [].concat.apply([], arrays);

// use flat()
const merged2 = arrays.flat();
```

#### Extract property values as array from objects array

```typescript
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name);
```

## Object

#### Transpose a javascript object into a key/value array

```javascript
var data = { firstName: 'John', lastName: 'Doe', email: 'john.doe@gmail.com' }
var output = Object.entries(data).map(([key, value]) => ({key,value}));
```

#### Set object property value dynamically

```javascript
update: (state, action) => {
  state.registrations.map((r, i) => {
    if (r.registrationId === action.payload.registrationId) {
     Object.entries(action.payload).map(([key, value]) => {
       console.log(key + ': ' + value)
       r[key] = value
     })
    }
  })
}
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

### String

#### to lowercase

```typescript
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name)
    .map((name: string) => name.toLowerCase())
    .sort();
```

