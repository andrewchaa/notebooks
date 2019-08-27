---
description: a predictable state container for JavaScript apps.
---

# Redux

## What is it?

Redux is basically a global state object that’s the single source of truth in an application. It takes advantage of a React feature, _context_.

Context is a React API that creates global variables that can be accessed anywhere in the application, as long as the component receiving the context is a child of the component that created it. Normally you’d have to do this by passing props down each level of the component structure. With context, you don’t need to use props. You can use the context anywhere in the app and access it without passing it down to each level.

```javascript
const ThemeContext = React.createContext() // new context variable 


class Parent extends Component {
  state = { themeValue: 'light' }  // state variable for themeValue
  toggleThemeValue = () => {       // simple function that toggles theme value
    const value = this.state.themeValue === 'dark' ? 'light' : 'dark' 
    this.setState({ themeValue: value })
  }
  render() {
    return (
      <ThemeContext.Provider       // Provides the context to child components. Anything wrapperd
        value={{                   // in a Provider is avaiable to children of a component in a Consumer
          themeValue: this.state.themeValue,
          toggleThemeValue: this.toggleThemeValue
        }}
      >
        <View style={styles.container}>
          <Text>Hello World</Text>
        </View>
        <Child1 />
      </ThemeContext.Provider>
    );
  }
}

const Child1 = () => <Child2 /> // Stateless function that returns a component, demonstrating
                                // that you aren't passing props between Parent adn Child2
const Child2 = () => ( // Stateless function that returns a component wrapped in a ThemeContext.Consumer
  <ThemeContext.Consumer>
    {(val) => (
        <View style={[styles.container, 
                      val.themeValue === 'dark' && 
                     { backgroundColor: 'black' }]}>
          <Text style={styles.text}>Hello from Component2</Text>
          <Text style={styles.text} 
                onPress={val.toggleThemeValue}>
              Toggle Theme Value
          </Text>
        </View>
      )}
  </ThemeContext.Consumer>
)

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  text: {
    fontSize: 22,
    color: '#666'
  }
})
```

## To use redux

1. Install redux and react-redux
2. Create [redux reducers](redux.md#redux-reducers)
3. Add [provider](redux.md#provider-and-store) and create store
4. Access data using the [connect function](redux.md#connect-function)
5. [Add actions](redux.md#adding-actions)
6. Delete items from a redux store in a reducer

#### Redux reducers

A reducer is a function that returns an object. Reducers can be thought of as data stores. Each store contains a piece of data. A root reducer will combine all reducers for the store.

```javascript
import { ADD_REGISTRATION, INITIALIZE_REGISTRATIONS } from '../actions/action';

const initialState = {
  registrations: []
}

const registrationReducer = ( state = initialState, action ) => {
  switch(action.type) {
    case ADD_REGISTRATION:
      return {
        registrations: [
          ...state.registrations,
          action.registration
        ]
      }
    default: 
      return state
  }
}

export default registrationReducer;
```

#### Provider and store

A provider is a parent component that passes data to all child components. In Redux, It passes the global state/store to the rest of the application

```javascript
import { Provider } from 'react-redux';
import { createStore } from 'redux';

const store = createStore(rootReducer);

class App extends React.Component {
  render() {
      return (
        <Provider store={store}>
          <AppContainer screenProps={{...this.props}} />
        </Provider>
      );
  }
  
```

#### connect function

Access Redux store by using connect function 

```text
connect(func1)(func2)
```

* func1: a function that gives the global Redux state object
* func2: a function where you pass the child component

```javascript
const mapStateToProps = (state) => {
  return { 
    registrations: state.registrations.list,
    loading: state.registrations.loading,
    filterText: state.registrations.filterText
  };
}

const mapDispatchToProps = {
  fetchListComplete:  registrations.actions.fetchListComplete,
  fetchListStart:     registrations.actions.fetchListStart,
  filter:             registrations.actions.filter
};

export default connect(mapStateToProps, mapDispatchToProps)(RegistrationList);
```

#### Adding actions

Actions are functions that return objects that send data to the store and update reducers. They are the only way to change the store.

When you dispatch an action, it's sent to all reducers as the 2nd argument. Reducers will check the action's type and update what the reducer returns accordingly 

```javascript
function fetchBooks() {
  return {
    type: 'FETCH_BOOKS'
  }
}

function addBook(book) {
  return {
    type: 'Add_BOOK',
    book: book
  }
}
```

#### 

#### Updating / Inserting data into store

```javascript
import { connect } from 'react-redux';
import { addRegistration } from '../actions/action';

this.props.dispatchAddRegistration(this.state)
const mapDispatchToProps = {
  dispatchAddRegistration: (registration) => addRegistration(registration),
}

const mapStateToProps = (state) => ({
  books: state.registrationReducer.registrations
})

export default connect(mapStateToProps, mapDispatchToProps)(BoilerRegistrationForm);
```

## Redux Start Kit

#### createSlice

It returns an object that looks like this:

```javascript
{
    reducer: (state, action) => newState,
    actions: {
        addTodo: (payload) => ({type: "todos/addTodo", payload}),
        toggleTodo: (payload) => ({type: "todos/toggleTodo", payload})
    }
}
```

So, use the action functions to generate action type.

```javascript
yield put(registrations.actions.populateList(list));
// is the same as
yield put({type: 'registrations/populateList', payload: list});
```

