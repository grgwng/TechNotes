# React

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
