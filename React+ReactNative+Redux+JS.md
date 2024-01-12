# React
Ref: [React docs](https://react.dev/reference/react)
## Components

### Making a component
```js
//Button.js / Button.tsx

const Button () => {
    return (
        <button>Hello World!</button>
    );
}

export default Button;
```

### Using a component
```js
//App.js / App.tsx

import Button from "./components/Button";

const App = () => {
    return (
        <Button /> //<Button></Button> also works
    );
}
```

### Fragments
Used to return multiple components
```js
//App.js / App.tsx

import Button from "./components/Button";

const App = () => {
    return (
        //Fragment
        <> 
            <Button />
            <Button />
        </>
    );
}
```

### Props
Allow parent to pass in data to child

Javascript:
```js
//Button.js 

const Button ({color="primary", children, onClick}) => {
    //3 props:
    //color: has default value "primary"
    //children: special prop referring to wrapped content
    //onClick: a function

    return (
        <button className={"btn btn-"+color} onClick={onClick}>{children}</button> //display dynamically in {}
    );
}

export default Button;
```

Typescript:
```ts
// Button.tsx
import {ReactNode} from "react";

interface Props {
    //? means optional prop
    color?: 'primary' | 'secondary' | 'danger'; //enumerated type with 3 values
    children?: ReactNode; //special prop referring to wrapped content
    onClick: () => void; //a prop with a function type
}

const Button ({color='primary', children, onClick}: Props) => {
    //color has default value 'primary'
    
    return (
        <button className={"btn btn-"+color} onClick={onClick}>{children}</button>
    );
}

export default Button;
```

Usage:
```js
//App.js / App.tsx
    ...
    
    return (
        <Button color="secondary" onClick={/*some function*/}>Hello World!</Button>
    );

    ...

```

### Conditional Rendering
Render a component based on the value of a variable
```js
//App.js / App.tsx

    ...

    const showButton = false;

    return (
        //renders the button only if showButton is true
        {showButton && <Button ...></Button> }
    );

    ...

```

## Hooks

### `useState()`
Makes a variable and its corresponding updater 

```js
//Button.js
import {useState} from 'react'

const Button = () => {
    //makes variable `value` and updater function `setValue`
    //0 is the default value
    const [value, setValue] = useState(0); 

    return( 
        //component will automatically rerender when the state changes
        <button onClick={()=>{setValue(99)}}>{value}</button>
    );
}
```

### `useEffect()`
Specify an action to be run when a dependancy changes
```js
import {useEffect} from 'react'

const Button = (someState) => {
    useEffect(()=>{
        //this code will run when `someState` variable changes

        /* do something */

    }, [someState])//<-- add dependencies here

    return (...);
}
```

Other ways:
```js
useEffect(()=>{
    //this code will run ONLY on the first render of the component

    /* do something */

}, []) //<-- empty array
```

```js
useEffect(()=>{
    //this code will run on EVERY render of the component

    /* do something */

}) //<-- no array
```

#### Clean up
Return a function to run:
- right before the next scheduled effect
- when the component unmounts (when it is removed from DOM)
```js
useEffect(() => {
    let timer = setTimeout(() => {
        setCount((count) => count + 1);
    }, 1000);

  return () => clearTimeout(timer)
  }, []);
```

### `useContext()`
Subscribes to a `context`, allowing you to get state deep inside a child component

```js
//Child component
function Button() {
    //returns the `value` attribute of ThemeContext
    const theme = useContext(ThemeContext);
    ...
}
```

```js
//Parent component
function MyPage() {
  return (
    <ThemeContext.Provider value="dark">
        //some nested components including buttons
        ...
    </ThemeContext.Provider>
  );
}
```
Can combine with state

Note: `useContext()` searches at the closest context provider ***above*** the usage of `useContext()`

#### Specifying a fallback value
Specify a default context value using `createContext()`

```js
const ThemeContext = createContext('light');

function MyPage() {
  return (
    <ThemeContext.Provider value="dark">
        //some nested components including buttons
        //these buttons will use the value `dark`
        ...
    </ThemeContext.Provider>

    //This button will use the default value `light`
    <Button />
  );
}
```

### `useMemo()`
Allows you to cache results of expensive calculations
```js

function TodoList({ todos, tab }) {
  //expensive calculation will only run when `todos` or `tab` change 
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab), //callback of expensive calculation
    [todos, tab] //dependencies that indicate when to recalculate
  );
  ...
}
```

### `useRef()`
Create a variable that doesn't trigger a re-render (unlike `useState()`)

```js
//default value is 0
const intervalRef = useRef(0);
...

//reading the ref using .current
const intervalId = intervalRef.current;
```

Only write/read refs in **event handlers or effects**

#### Manipulating the DOM
```js
function MyComponent() {
  //set the default value to null
  const inputRef = useRef(null);
  ...

  //reference the DOM node using .current
  function handleClick() {
    inputRef.current.focus();
  }
  ...

  //use ref attribute on JSX component
  return <input ref={inputRef} />;
}

```

# React Native

Ref: [React Native docs](https://reactnative.dev/)
## Basics
For grouping elements, use `<View></View>` (instead of `div`)

For text, use `<Text></Text>`

For creating a scrollable container, use `<ScrollView></ScrollView>`

For creating lists, use `<FlatList></FlatList>`
- Requires two props `data` and `renderItem`

e.g.
```js
<FlatList
    data={[
        {key: 'Devin'},
        {key: 'Dan'},
        {key: 'Dominic'},
        {key: 'Jackson'},
        {key: 'James'},
        {key: 'Joel'},
        {key: 'John'},
        {key: 'Jillian'},
        {key: 'Jimmy'},
        {key: 'Julie'},
    ]}
    renderItem={
        ({item}) => <Text>{item.key}</Text>
        }
/>
```

## Styling

Making a stylesheet
```js
//this is NOT CSS, but will be mapped to respective CSS attributes
const styles = StyleSheet.create({
  container: {
    marginTop: 50,
  },
  bigBlue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});
```
Using a stylesheet
```js
//LotsOfStyles component
const LotsOfStyles = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.red}>just red</Text>
      <Text style={styles.bigBlue}>just bigBlue</Text>
      <Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>
      //styles override all styles to the left
    </View>
    
  );
};
```

## Navigation

Ref: [React Navigation docs](https://reactnavigation.org/docs/getting-started)

To use navigation, wrap app with `NavigationContainer`
```js
function App() {
  return (
    <NavigationContainer>
        ...
    </NavigationContainer>
  );
}
```


### Stack Navigation
A type of navigation where screens are laid on each other in a 'stack'

#### Using a Stack Navigator
```js
const Stack = createNativeStackNavigator();

function App() {
    
  return (
    <NavigationContainer> 
        ...
            //specify the initial screen with initialRouteName prop
            <Stack.Navigator initialRouteName="Home">
                //Each screen has a `name` and `component` prop:
                //`name` is a string label
                //`component` is a React component (the screen component) corresponding to that screen 
                <Stack.Screen name="Home" component={HomeScreen} />
                <Stack.Screen name="Details" component={DetailsScreen} />
        
            </Stack.Navigator>
        ...
    </NavigationContainer>
  );
}
```

#### Specifying options
```js
...
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: 'Overview' }} //'Overview' will be displayed as the title
/>
...
```

#### Passing props to screens
Best to use React Context and wrap navigator with a context provider

#### Navigating between screens

```js
//Every screen component gets a `navigation` prop
function HomeScreen({ navigation }) {
  return (
      ...

      <Button
        title="Go to Details"
        //use navigation.navigate() to navigate to a screen
        onPress={() => navigation.navigate('Details')}
      />
      ...
  );
}
```

```js
//use navigation.push() to push the screen onto stack 
//slighly different than navigation.navigate()
<Button
    title="Go to Details... again"
    onPress={() => navigation.push('Details')}
/>
```

```js
//use navigation.goBack() to go back to previous screen
<Button 
    title="Go back" 
    onPress={() => navigation.goBack()}
/>
```

```js
//use navigation.popToTop() to go back to very first screen 
<Button 
    title="Go back to first screen" 
    onPress={() => navigation.popToTop()}
/>
```

### Passing data to routes

#### Sending params
```js
//Use .navigate with a second argument for params
navigation.navigate('Details', {
    itemId: 86,
    otherParam: 'anything you want here',
    ...
});
```

#### Getting params
```js
//In target route: use route.params
function DetailsScreen({ route, navigation }) {

    const { itemId, otherParam } = route.params;

    ...
}
```

#### Initial params
```js
<Stack.Screen
  name="Details"
  component={DetailsScreen}
  initialParams={{ itemId: 42 }}
/>
```

#### Updating params
```js
navigation.setParams({
  itemId: 6,
});
```

#### Passing params to nested navigators
```js
navigation.navigate('Account', {
  screen: 'Settings',
  params: { user: 'jane' },
});
```

### Tab Navigator
```js
const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
        <Tab.Navigator>
            <Tab.Screen name="Home" component={HomeScreen} />
            <Tab.Screen name="Settings" component={SettingsScreen} />
        </Tab.Navigator>
    </NavigationContainer>
  );
}
```

### Drawer Navigator
```js
const Drawer = createDrawerNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Drawer.Navigator initialRouteName="Home">
        <Drawer.Screen name="Home" component={HomeScreen} />
        <Drawer.Screen name="Notifications" component={NotificationsScreen} />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}
```

#### Opening/Closing/Toggling the drawer
```js
navigation.openDrawer();
navigation.closeDrawer();
navigation.toggleDrawer();
```

# Redux

## Terminology

### Actions
An object containing a `type` and `payload`

Analogous to an "event"
```js
//action
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}

//action creator
const addTodo = text => {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```

### Reducers
A function that takes in a current `state` and an `action` and returns a new `state`. 

Defines how the state changes based on the action

Analogous to an "event handler"

Note: Reducers **do not** modify the current state. They create a new state based on the old state.

```js
//initial state
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```

### Store
An object that contains the current application state
```js
import { configureStore } from '@reduxjs/toolkit'

//when using configureStore, pass in the reducer
const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

### Dispatch
Used to update the store by "dispatching" an action
```js
//dispatch an action
store.dispatch({ type: 'counter/increment' })

//the store is updated
console.log(store.getState())
// {value: 1}
```

### Selectors
Functions that extract specific pieces of info from the state
```js
//selector
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 1
```

## Dataflow
When something happens in the app:
1. The UI **dispatches** an **action**
2. The **store** runs the **reducers** and the state is updated
3. The UI is updated based on the new state 

## Hooks

### `useSelector()`
Specifies a selector
```js
const counter = useSelector((state) => state.counter)
```

### `useDispatch()`
Returns the dispatch function
```js
const dispatch = useDispatch()
```


# JavaScript

## Asynchronous JS

### Promises
An object returned by asynchronous functions
- A promise is **pending** if it has been created but hasn't succeeded or failed yet
- A promise is **fulfilled** if the async function succeeded. The `then()` handler is called.
- A promise is **rejected** if the async function failed. The `catch()` handler is called.
- A promise is **settled** if it is either fulfilled or rejected

#### Using Promises
```js
//fetch is an async function that returns a promise
//we call fetch and fetchPromise is a Promise
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

//define what to do when the promise settles
//then() returns another Promise so we can chain then()
fetchPromise
  .then((response) => response.json()) 
  .then((data) => { //can chain then() to perfom actions sequentially
    console.log(data[0].name);
  })
  .catch((error) => { //catch any of the above rejected promises 
    console.error('Could not get products')
  })
```

#### Combining Promises
`Promise.all([p1, p2...])` makes a new Promise that is fulfilled only when **all** promises passed in are fulfilled. If one promise is rejected, `Promise.all()` is rejected.
```js
const promise1 = fetch(...);
const promise2 = fetch(...);
const promise3 = fetch(...);

Promise.all([promise1, promise2, promise3])
    .then(...) //will only run when promise1, promise2, and promise3 are fulfilled
    .catch(...) //will run when any of the promises are rejected
```

`Promise.any([p1, p2, ...])` makes a new Promise that is fullfiled when **any** promises passed in are fulfilled. If all promises are rejected, `Promise.any()` is rejected.
```js
const promise1 = fetch(...);
const promise2 = fetch(...);
const promise3 = fetch(...);

Promise.any([promise1, promise2, promise3])
    .then(...) //will only run when any of the promises are fulfilled
    .catch(...) //will run when all promises are rejected
```

#### Creating Promises
```js
function querySomething() {
    /*
    run the query
    */
    
    //Pass in a function taking two arguments `resolve` and `reject` that correspondingly called 
    return new Promise((resolve, reject)=>{
        if(/* query successful */){
            resolve(/*query data*/)
        }else{
            reject('Error fetching data')
        }
    })
}
//We can use querySomething() like fetch(); it will return a promise
```

### `async` and `await`

```js
async function querySomething(){
    try {
        //code will wait until fetch is completed
        //data will be the response of fetch, NOT a promise
        const data = await fetch(...)
        return data;
    } catch (error) {
        console.error("Could not get data");
    }
}
//Like above, we can use querySomething() like fetch()
```
