# lodash

```
# install lodash
npm i --save lodash
```

#### GroupBy 

monthly

```
import _ from 'lodash';

const dates = action.payload.map(({installationDate}) => installationDate);
state.totalCount = dates.length;
state.monthlyCounts = _.countBy(dates, (d) => (moment(d, 'DD/MM/YYYY').format('YYYY-MM-01')));
```

