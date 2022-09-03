---
title: "Redux - Using thank to handle Google login async logic"
description: ""
date: 2022-09-02
draft: false
categories: 
- redux
tags:
- react
- redux
- thunk
- google login
cover:
    image: "cover.jpg"
    relative: true
---


In Redux we often use middleware to handle asynchronous behavior, so that we can add different asynchronous logic to the store, and gradually we have developed various kinds of middleware.
 <!--more-->
In redux, we often use middleware to handle asynchronous behavior, so that we can add different asynchronous logic to the store. Gradually, various kinds of middleware have been developed, among which `redux-thunk` is officially recommended to help us handle AJAX requirements well.

### Firebase setup

First, login to the Firebase site and add the google login service, add our Firebase configuration and `GoogleAuthProvider` and `getAuth` to the project, and we can add a `firebase.js` to store the Firebase information separately.

### Firebase setup

We will first login to the Firebase website and add the google login service, copy our Firebase configuration and add it to our project along with GoogleAuthProvider and getAuth, here we can add a new `firebase.js` to write Firebase settings.

```jsx
import { initializeApp } from "firebase/app";
import { GoogleAuthProvider, getAuth } from "firebase/auth";
import { config } from "./firebaseConfig";

export function FirebaseAuth(){
	
 // Your web app's Firebase configuration
 const firebaseConfig = config();
  
    initializeApp(firebaseConfig);
    const provider = new GoogleAuthProvider();    

    const auth = getAuth();
    auth.languageCode = 'it';
   
    return [auth,provider]

}
```

### Designing React App

In our `index.js`, we add a `store` to store the user's login information in the global store.

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import reportWebVitals from "./reportWebVitals";
import { Provider } from "react-redux";
import { configureStore } from "@reduxjs/toolkit";
import App from "./App";
import userReducer from "./slices/userSlice";

const store = configureStore({
  reducer: {
    user: userReducer,
  },
});

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

reportWebVitals();
```

Also define our reducer in `userSlice.js`.

```jsx
import { createSlice } from "@reduxjs/toolkit";

const initialState = { login: false };
export const userSlice = createSlice({
  name: "user",
  initialState: { initialState },
  reducers: {
  //..
  },
});
```

In react, we can get the user's login information from the global store. In `App.js`, a simple content is designed so that when the user is logged in, the screen will show the `UserAvater` component, and conversely, when the user is logged out, the screen will show the `Login` component.

```jsx
export function App() {
  const loginInfo = useSelector((state) => state.user.loginInfo);
  if (loginInfo.login === true) {
    return(
      <UserAvatar />
    )
  } else if (loginInfo.login === false) {
    return (
      <Login />
    );
  }
}
```

In the `Login.js` component, we design a login with Google account button for the user to perform the login action.

Since the redux thunk middleware allows us to pass the thunk function directly to `store.dispatch`, we will write the thunk in `slice` and return the `thunk` function through the `thunk` action creators. When the user clicks the login button, the onClick Event Handler will dispatch the `thunk` function to execute the asynchronous logic we wrote in the `thunk`.

```jsx
import { userLogin } from "../slices/userSlice";
import { useDispatch } from "react-redux";

export function Lonin(){
const dispatch = useDispatch();
return(
		<div>
      <button
        onClick={() => {
          dispatch(userLogin());
        }}
      >
        login with Google account
      </button>
    </div>
)} 
```

Redux Toolkit provides a `createAsyncThunk` API to generate action creators and action types, and generate thunk to automatically assign those `pending` / `fulfilled `/ `rejected` actions. In `userSlice.js`, we create thunk action creators by `createAsyncThunk` to write async requests to the payload creator callback function. 

`createAsyncThunk` accepts a prefix string for generating action types and a payload creator. When we make an async call, the payload creator returns a `promise` with the result, and thunk action creator dispatches `pending` / `fulfilled` / `rejected` actions based on the content returned by `promise`.

```jsx
import { createSlice } from "@reduxjs/toolkit";
import { createAsyncThunk } from "@reduxjs/toolkit";
import { GoogleAuthProvider, signInWithPopup } from "firebase/auth";
import { FirebaseAuth } from "../firebase/firebase";

const [auth, provider] = FirebaseAuth();

const initialState = { loginInfo:{ login: false }, state:"",error:"" };
export const userSlice = createSlice({
  name: "user",
  initialState: { initialState },
  reducers: {
  //..
  },
});

export const userLogin = createAsyncThunk("user/userLoginData", async () => {
  const result = await signInWithPopup(auth, provider);
  const credential = GoogleAuthProvider.credentialFromResult(result);
  const token = credential.accessToken;
  const user = result.user;
  console.log("token:", token, "user:", user);
  const loginState = {
    login: true,
    name: user.displayName,
    email: user.email,
    photo: user.photoURL,
  };
  return loginState;
});

export default userSlice.reducer;
```

However, those extra action creators created in the thunk through `createAsyncThunk` that cannot be accessed and processed directly in the slice, we can add `extraReducers` to the `createSlice` to help us access each action dispatched in the `thunk`, so we define the individual reducers for these actions in the `extraReducers` via the `builder.addCase(actionCreator, reducer)` method.
- Handle
`pending` / `fulfilled` / `rejected` dispatched by our async thunks:
    -  When the request starts, we set `status` to `'loading'`.
    - If the request is successful, we set `status` to `'succeeded'` and add the fetched data to `loginInfo`.
    - If the request fails, we mark `status` as `'failed'` and save any error message to the status so we can display it.


```jsx
import { createSlice } from "@reduxjs/toolkit";
import { createAsyncThunk } from "@reduxjs/toolkit";
import { GoogleAuthProvider, signInWithPopup } from "firebase/auth";
import { FirebaseAuth } from "../firebase/firebase";

const [auth, provider] = FirebaseAuth();

const initialState = { loginInfo:{ login: false }, state:"",error:"" };
export const userSlice = createSlice({
  name: "user",
  initialState: { initialState },
  reducers: {
  //..
  },
  extraReducers(builder) {
    builder
      .addCase(userLogin.pending, (state, action) => {
        state.status = 'loading'
      })
      .addCase(userLogin.fulfilled, (state, action) => {
        state.status = 'succeeded'
        state.loginInfo = action.payload;
      })
      .addCase(userLogin.rejected, (state, action) => {
        state.status = 'failed'
        state.error = action.error.message
      })
  }
});

export const userLogin = createAsyncThunk("user/userLoginData", async () => {
  const result = await signInWithPopup(auth, provider);
  const credential = GoogleAuthProvider.credentialFromResult(result);
  const token = credential.accessToken;
  const user = result.user;
  console.log("token:", token, "user:", user);
  const loginState = {
    login: true,
    name: user.displayName,
    email: user.email,
    photo: user.photoURL,
  };
  return loginState;
});

export default userSlice.reducer;
```

In this way, we can listen to these action types in `createSlice` through `extraReducers` and update the status of our reducer according to these actions, so that when our UI needs to render UI content based on the results of async request, it can be determined and updated in the global store via `user.status` !