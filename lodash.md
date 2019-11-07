# lodash

#### Getting Started

```text
# install lodash
npm i --save lodash
```

#### Check if an object is empty

```javascript
if (_.isEmpty(searchFilter)) 
  return list;
```

#### GroupBy 

monthly

```javascript
import _ from 'lodash';

const dates = action.payload.map(({installationDate}) => installationDate);
state.totalCount = dates.length;
state.monthlyCounts = _.countBy(dates, (d) => (moment(d, 'DD/MM/YYYY').format('YYYY-MM-01')));
```

