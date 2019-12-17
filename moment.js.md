# moment.js

## Getting Started

```javascript
// installation
npm i moment

// now
moment() // now

// day
let day = moment('2019-08-23')
day = moment(1318781876406)
day = moment('17/12/2019', 'DD/MM/YYYY')


moment('20130208T080910') //Short date and time up to seconds

// utc
moment().utc()
```

## Format

```
var date = new Date();
```

```javascript
var formattedDate = moment(date).format('DD/MM/YYYY');
```

#### date + hour

```javascript
loginDateTimeStamp: 
  moment().utc().format('DD/MM/YYYY HH:mm:ss')
```

## Parse

The parser ignores non-alphanumeric characters, so both of the following will return the same thing

```javascript
moment("12-25-1995", "MM-DD-YYYY");
moment("12/25/1995", "MM-DD-YYYY");
```

## Add

```javascript
{moment(r.installationDate, 'DD/MM/YYYY')
  .add(7, 'year')
  .format('DD/MM/YYYY')}
```

### Query

```javascript
moment().isAfter(Moment|String|Number|Date|Array);
moment().isAfter(Moment|String|Number|Date|Array, String);
```



