---
title: "Create Redux Toolkit in React Hook"
date: 2022-08-10
draft: false
categories: 
- React
tags:
- React
- Redux
cover:
    image: "cover.jpg"
    relative: true
---

## What is Redux?

Redux has several important concepts: store, action, and reducer. 

Redux is used to manage the state of JavaScript apps. It is used to manage the state uniformly, containing the "global" state in a single store, and the state of the entire application will be stored in the store in the form of an object tree.

This state is read-only, through pure reducer functions look at those actions and return an immutably updated state, the only way to change the state is to change the state in the store through dispatch actions.

## Why we need Redux?

In react, data flows in one direction. If we change the state of the data of the component, we need to pass it to the parent element, and then notify other components of the state of the data from the parent component.
But after the unified management of our state through Redux, we only need to change the state of the store, and then directly get the data from the store when we need it.

## Create react App

```
npx create-react-app my-app
cd my-app
```

## Install

Let's install Redux Core, the React bindings, and Redux Toolkit. Redux Toolkit is the officially recommended way to write Redux logic. It simplifies most tasks with Redux, prevents common mistakes, and provides two very useful functions, `configureStore()` and `createSlice()`, and makes writing Redux easier. which we will mention below.

```
npm install redux react-redux @reduxjs/toolkit
```

## Pre-work

Add a components folder under the src folder, and create our components. Here, create `Home.js` and `Login.js` as examples.


```
import React from 'react'

function Home() {
  return (
    <div>
        <h1>
            Home Page
        </h1>
        <p>UserName:</p>
        <p>UserEmail:</p>
    </div>
  )
}

export default Home;
```

```
import React from "react";

function Login() {
  return (
    <div>
      <button>
        Login
      </button>
    </div>
  );
}

export default Login;
```

Add component Home and component Login to `App.js`, and you should be able to see our components on the browser when done.

```
import './App.css';
import Home from './components/Home';
import Login from './components/Login';

function App() {
  return (
    <div className="App">
      <Home />
      <Login />
    </div>
  );
}

export default App;
```

## Construction Process

### store

In `index.js`, the entry point of the app, we first create a store through `configureStore()` provided by the toolkit, and merge multiple slices in the reducer. We will create the Slice in the next step.
Then use `<Provider>` to make the store available to all components in the APP, instead of passing the store in all components.

```
import { configureStore } from '@reduxjs/toolkit';
import { Provider } from 'react-redux';

export const store = configureStore({
  reducer: {

  },
});

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

reportWebVitals();
```

### reducer

Create another features folder in the src folder, in which you can create files to store different slices. Here, create a `user.js`, use the createSlice provided by Toolkit to generate a Slice, and provide the name, initialState and reducers of the Slice. Through createSlice, you can define state, action, and reducers at the same time.

```
import { createSlice } from "@reduxjs/toolkit";

const initialState =  { userName:"", userEmail:"" }
const userSlice = createSlice({
  name:"user",
  initialState:{ value: initialState },
  reducers:{}
})
```

Then we want to change the data state of component Home after clicking the login button in component Login. So define the login function in reducers to specify how the state should change when the action is sent to the store. 

The first parameter is the previous state, and the second parameter is the object of the action. The object of the action contains the payload and type respectively. Through the payload of the action, we can get the value we want to update.

```
import { createSlice } from "@reduxjs/toolkit";

const initialState =  { userName:"", userEmail:"" }
export const userSlice = createSlice({
  name:"user",
  initialState:{ value: initialState },
  reducers:{
    login: (state, action) => {
    state.value = action.payload
    },
  }
})

export default userSlice.reducer;
```

Export `userSlice.reducer` of `user.js` and import it in `index.js`, This way we can merge multiple slices in the reducer of the store, as follows:

```
import { configureStore } from '@reduxjs/toolkit';
import { Provider } from 'react-redux';
import userReducer from './features/user';

export const store = configureStore({
  reducer: {
    user: userReducer,
  },
});

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

reportWebVitals();
```


Because we want to be able to change the data state of component Home after clicking the login button in component Login, we need to use the useSelector hook in `Home.js` to get the state value from the store first,  so that the user's data can be presented on the website.

```
import React from 'react'
import { useSelector } from 'react-redux'

function Home() {
  const user = useSelector((state) => state.user.value)
  return (
    <div>
        <h1>
            Home Page
        </h1>
        <p>UserName: **{user.UserName}**</p>
        <p>UserEmail: **{user.UserEmail}**</p>
    </div>
  )
}

export default Home;
```


We can try to change the initialState set in `user.js`, the data on the website should show the initialState we changed. After the test, we can change the initialState back to an empty string.

```
import { createSlice } from "@reduxjs/toolkit";

const initialState =  { userName:"test", userEmail:"test@gmail.com" }
export const userSlice = createSlice({
  name:"user",
  initialState:{ value: initialState },
  reducers:{
      login: (state, action) => {
          state.value = action.payload
      },
  }
})

export default userSlice.reducer;
```

### Action

In order to change the data state of component Home after clicking the login button in component Login, we can export the login function of `user.js` through `userSlice.actions` provided by toolkit.

```
import { createSlice } from "@reduxjs/toolkit";

const initialState =  { userName:"", userEmail:"" }
export const userSlice = createSlice({
  name:"user",
  initialState:{ value: initialState },
  reducers:{
      login: (state, action) => {
          state.value = action.payload
      },
  }
})

export const { login } = userSlice.actions;
export default userSlice.reducer;
```

In `Login.js`, we import the login we just exported, and use the useDispatch hook, which will return the dispatch function of the store to let us dispatch the action. So when we onClick the button, we can call `dispatch(login())` in the callback and put the data to be updated like `dispatch(login({data}))`, let the login function of `user.js` get the data we want to update through the action payload, and then we can update the screen data when we click the button.

```
import React from "react";
import { useDispatch } from "react-redux";
import { login } from "../features/user";

function Login() {
  const dispatch = useDispatch();
  return (
    <div>
      <button
        onClick={() => {
          dispatch(
            login({ userName: "anila", userEmail: "anilahsu@gmail.com" })
          );
        }}
      >
        Login
      </button>
    </div>
  );
}

export default Login;
```

## Conclusion

This is a simple example of using redux in react, which helps us better understand the working principle of redux. We can also use redux in react directly using the template provided by the official. Here is the official website of [redux](https://redux.js.org/) and [react-redux](https://react-redux.js.org/). Hope this article helps you.