# javascript

## Array

### include

Check if an element is in the array

```javascript
  var paths = response.data
    .filter(x => !(['.gitbook', 'README.md', 'SUMMARY.md', 'drafts'].includes(x.path)))
    .map(x => x.path)
```

### Map

```typescript
const definitions = this.flattenManifest(manifest);
const names = definitions
    .map(({name}) => name);
```

### Join

```javascript
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```

### Merge / flatten an array of arrays

```javascript
// use concat
var arrays = [["$6"], ["$12"], ["$25"], ["$25"], ["$18"], ["$22"], ["$10"]];
var merged1 = [].concat.apply([], arrays);

// use flat()
const merged2 = arrays.flat();
```

### Remove the first element

```javascript
const arr = [1, 2, 3, 4]; 
const theRemovedElement = arr.shift(); // theRemovedElement == 1
console.log(arr); // [2, 3, 4]
```

### Sort

```javascript
action.payload.sort(
  (a,b) => moment(a.installationDate, 'DD/MM/YYYY').isBefore(moment(b.installationDate, 'DD/MM/YYYY')) 
    ? 1 
    : -1
  )
```

### filter

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

## FileSystem

### writeFileSync

```javascript
const content = await getContent(path)
const date = await getDate(path)
const frontMatter = await getFrontMatter(content.title, date, content.description)

fs.writeFileSync(`./posts/${path}`, `${frontMatter}\n${content.body}`)

```

## Node

#### Kill process to run npm

```javascript
pkill -f node
```

#### node-gym anomalies on mac

* refer to Catalina's troubleshooting page: [https://github.com/nodejs/node-gyp/blob/master/macOS\_Catalina.md\#The-acid-test](https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md#The-acid-test)
* Install standalone developer tool [https://developer.apple.com/download/more/?=command%20line%20tools](https://developer.apple.com/download/more/?=command%20line%20tools)

## Regular expressions

### Substitution

```javascript
  const body = lines.join('\n')
    .replace(
      /\.gitbook\/assets\/image%20%28([0-9]+)%29\.png/g, 
      'assets\/image$1\.png'
    )
```

## Timer

### Pause

```javascript
await sleep(1000)
const sleep = (ms) => {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}
```

## Types

## 

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

#### Remove blank attributes

```javascript
const registration = { ...action.payload.newItem } 
Object.entries(registration).map(([key, value]) => {
  if (!value) {
    delete registration[key]
  }
});
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

## String

### interpolation

```javascript
  return `
    ---
    title: ${title}
    `

```

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

#### replace

put global option to replace all occurrences

```javascript
const imageKey = registration.imageUri.replace(/\//gi, '\\')
```

## Decomposition

#### Nested decomposition

```javascript
handleSave(event) {
  const { updateRegistration, match: { params: { registrationid }} } = this.props
  
  updateRegistration({ registrationid, 
    warrantyYear: this.state.warrantyYear,
    warrantyDate: this.state.warrantyDate
    })
}
```



