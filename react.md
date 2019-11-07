# React

## Resources

* create-react-app: [https://github.com/facebook/create-react-app](https://github.com/facebook/create-react-app)

## Getting Started

#### Creating an App

```bash
npx create-react-app my-appcd my-appnpm start
```

### Deploying App

#### Increase memory if npm install fails on build pipeline

I use azure devops build pipeline to publich .NET core app with react. Its ubuntu hosted agent doesn't seem to cope well with npm install, often failing with the message "**Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory**". You can increase the memory size

```javascript
"scripts": {  ...  "build": "react-scripts --max_old_space_size=4096 build",  ...},
```

#### importing packages

```javascript
import React, { Component } from 'react';import * as d3 from 'd3';import * as d3Graphviz from 'd3-graphviz';
```

#### Rendering Options from Array

```typescript
<select className="form-control"  onChange={this.selectApp} defaultValue={''} >  <option value="">All apps</option>  {    JsonToDotConverter.getAppNames()      .map((name: string) => <option key={name}>{name}</option> )   }</select>
```

Make sure you put unique key value so that react can draw the node effectively

## Routing

#### Install router

```bash
npm i --save react-router-dom
```

#### Define routes

```javascript
import { BrowserRouter as Router, Route } from "react-router-dom";import Registration from './pages/Registration'  render() {    return (      <Router>        <Route path="/" exact component={Registration} />      </Router>    )  }
```

#### Link with parameter

```markup
<Router>  <Route path="/" exact component={Registration} />  <Route path="/warranty/:registrationId" exact component={Warranty} /></Router><Link to={`/warranty/${r.registrationId}`}>{r.postCode}</Link>
```

#### Multiple parameters

```javascript
<Route path="/warranty/:userId/:registrationId"  exact component={Warranty} /><Link to={`/warranty/${r.userId}/${r.registrationId}`}>  {r.postCode}</Link>
```

#### Retrieve parameter

```javascript
const { updateRegistration, match: { params: { registrationid }} } = this.propsupdateRegistration({ registrationid,   warrantyYear: this.state.warrantyYear,  warrantyDate: this.state.warrantyDate  })
```

## Basics

#### Binding this to class function

```javascript
constructor(props) {  super(props)  this.handleWarrantyChange = this.handleWarrantyChange.bind(this)handleWarrantyChange(event) {  const year = parseInt(event.target.value)  this.setState({     warrantyYear : year,    warrantyDate : moment().add(year, 'year').format('DD/MM/YYYY')   })}
```

#### Show / Hide

```javascript
loading() {  return (    <HashLoader      sizeUnit={"px"}      size={50}      color={'#123abc'}      loading={true}    />  )}showForm() {  const { registrations } = this.props  const { firstName, lastName, postCode, installationDate, serialNumber, model } = registrations[0]  return (    <form>      <div className="form-row">        <div className="form-group col-md-4">          <label>First name</label>          <input type="text" className="form-control" readOnly value={firstName} />{(registrations.length === 0)   ? this.loading()   : this.showForm()}
```



