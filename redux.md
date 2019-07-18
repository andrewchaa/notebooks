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

## Getting started

#### Create store and add Provider

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

#### Create reducers

```javascript
// registration reducer
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

// combine reducers
import { combineReducers } from 'redux';
import registrationReducer from './registrationReducer';

const rootReducer = combineReducers({
  registrationReducer
});

export default rootReducer;
```

#### Accessing data

Map state to props and get data from the storage from props

```javascript
render() {
  const { registrations } = this.props;
  <FlatList
    data={registrations}
    renderItem={this.renderItem}
    keyExtractor={({registrationId}, index) => registrationId}
  />

const mapStateToProps = (state) => ({ registrations: state.registrationReducer.registrations })
const mapDispatchToProps = { dispatchInitializeRegistrations: (registrations) => initializeRegistrations(registrations) }
export default connect(mapStateToProps, mapDispatchToProps)(BoilerRegistrationList);

```

