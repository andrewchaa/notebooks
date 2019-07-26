# React

## Resources

* create-react-app: [https://github.com/facebook/create-react-app](https://github.com/facebook/create-react-app)

## Getting Started

#### Creating an App

```bash
npx create-react-app my-app
cd my-app
npm start
```

### Basics

#### importing packages

```javascript
import React, { Component } from 'react';

import * as d3 from 'd3';
import * as d3Graphviz from 'd3-graphviz';
```

#### Rendering Options from Array

```typescript
<select className="form-control" 
 onChange={this.selectApp}
 defaultValue={''}
 >
  <option value="">All apps</option>
  {
    JsonToDotConverter.getAppNames()
      .map((name: string) => <option key={name}>{name}</option> ) 
  }
</select>
```

Make sure you put unique key value so that react can draw the node effectively

