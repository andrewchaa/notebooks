# javascript

## Types

### Array

#### Merge / flatten an array of arrays

```
// use concat
var arrays = [["$6"], ["$12"], ["$25"], ["$25"], ["$18"], ["$22"], ["$10"]];
var merged1 = [].concat.apply([], arrays);

// use flat()
const merged2 = arrays.flat();
```

#### Extract property values as array from objects array

```
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name);
```

#### Sort

```
action.payload.sort(
  (a,b) => moment(a.installationDate, 'DD/MM/YYYY').isBefore(moment(b.installationDate, 'DD/MM/YYYY')) 
    ? 1 
    : -1
  )
```

#### filter

```
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

```
var data = { firstName: 'John', lastName: 'Doe', email: 'john.doe@gmail.com' }
var output = Object.entries(data).map(([key, value]) => ({key,value}));
```

#### Set object property value dynamically

```
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

```
<FormLabelInput
  label="Address"
  value={registration.houseNoName}
  onChangeText={houseNoName => update({ ...registration, houseNoName: houseNoName})}
/>
```

#### property null check

```
return list.filter(l => 
      (l.firstName && l.firstName.includes(filterText)) ||
      (l.lastName && l.lastName.includes(filterText)) ||
      (l.city && l.city.includes(filterText)) ||
      (l.postCode && l.postCode.includes(filterText))
      )
```

#### Remove blank attributes

```
const registration = { ...action.payload.newItem } 
Object.entries(registration).map(([key, value]) => {
  if (!value) {
    delete registration[key]
  }
});
```

### Number

#### Format number

```
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

```
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name)
    .map((name: string) => name.toLowerCase())
    .sort();
```

#### includes\(\)

Determines whether one string may be found within another string, returning `true` or `false` as appropriate.

```
list.filter(l => l.firstName.includes(filteredText))
```

#### replace

put global option to replace all occurrences

```
const imageKey = registration.imageUri.replace(/\//gi, '\\')
```

## Decomposition

#### Nested decomposition

```
handleSave(event) {
  const { updateRegistration, match: { params: { registrationid }} } = this.props
  
  updateRegistration({ registrationid, 
    warrantyYear: this.state.warrantyYear,
    warrantyDate: this.state.warrantyDate
    })
}
```



