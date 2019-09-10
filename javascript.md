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

#### Extract property values as array from objects array

```typescript
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name);
```

#### Sort

```javascript
action.payload.sort(
  (a,b) => moment(a.installationDate, 'DD/MM/YYYY').isBefore(moment(b.installationDate, 'DD/MM/YYYY')) 
    ? 1 
    : -1
  )
```

#### filter

```javascript
var numbers = [1, 3, 6, 8, 11];

var lucky = numbers.filter(function(number) {
  return number > 7;
});

export const filteredListSelector = createSelector(
  ["registrations.list", "registrations.filterText"],
  (list, filterText) => {
    if (!filterText) 
      return list;

    return list.filter(l => 
      (l.firstName && l.firstName.includes(filterText)) ||
      (l.lastName && l.lastName.includes(filterText)) ||
      (l.city && l.city.includes(filterText)) ||
      (l.postCode && l.postCode.includes(filterText))
      )
  }
);
```

### Object

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

#### return an object, updating a single property

```javascript
<FormLabelInput
  label="Address"
  value={registration.houseNoName}
  onChangeText={houseNoName => update({ ...registration, houseNoName: houseNoName})}
/>
```

#### property null check

```javascript
return list.filter(l => 
      (l.firstName && l.firstName.includes(filterText)) ||
      (l.lastName && l.lastName.includes(filterText)) ||
      (l.city && l.city.includes(filterText)) ||
      (l.postCode && l.postCode.includes(filterText))
      )
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

#### lowercase

```typescript
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name)
    .map((name: string) => name.toLowerCase())
    .sort();
```

#### includes\(\)

Determines whether one string may be found within another string, returning `true` or `false` as appropriate.

```text
list.filter(l => l.firstName.includes(filteredText))
```

