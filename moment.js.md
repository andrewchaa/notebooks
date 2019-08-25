# moment.js

#### Getting Started

```javascript
// install
npm i moment

moment() // now
const day = moment('2019-08-23')
moment('20130208T080910') //Short date and time up to seconds

```

#### Format date string

```javascript
var date = new Date();
var formattedDate = moment(date).format('DD/MM/YYYY');
```

#### Parse string to date

The parser ignores non-alphanumeric characters, so both of the following will return the same thing

```javascript
moment("12-25-1995", "MM-DD-YYYY");
moment("12/25/1995", "MM-DD-YYYY");
```

