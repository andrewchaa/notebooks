# moment.js

#### Getting Started

```javascript
// installnpm i momentmoment() // nowconst day = moment('2019-08-23')const day = moment(1318781876406);moment('20130208T080910') //Short date and time up to secondsmoment().utc() // utc
```

## Format

```
var date = new Date();
```

```javascript
var formattedDate = moment(date).format('DD/MM/YYYY');
```

## Parse

The parser ignores non-alphanumeric characters, so both of the following will return the same thing

```javascript
moment("12-25-1995", "MM-DD-YYYY");moment("12/25/1995", "MM-DD-YYYY");
```

## Add

```javascript
{moment(r.installationDate, 'DD/MM/YYYY')  .add(7, 'year')  .format('DD/MM/YYYY')}
```

