# moment.js

#### Getting Started

```
// install
npm i moment

moment() // now
const day = moment('2019-08-23')
const day = moment(1318781876406);
moment('20130208T080910') //Short date and time up to seconds

moment().utc() // utc
```

## Format

```
var date = new Date();
```

```
var formattedDate = moment(date).format('DD/MM/YYYY');
```

## Parse

The parser ignores non-alphanumeric characters, so both of the following will return the same thing

```
moment("12-25-1995", "MM-DD-YYYY");
moment("12/25/1995", "MM-DD-YYYY");
```

## Add

```
{moment(r.installationDate, 'DD/MM/YYYY')
  .add(7, 'year')
  .format('DD/MM/YYYY')}
```

