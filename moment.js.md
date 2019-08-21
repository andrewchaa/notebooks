# moment.js

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

