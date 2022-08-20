---
title: "Redux - Implement lacalstorage middleware with toolkit"
description: ""
date: 2022-08-20
draft: false
categories: 
- Life
tags:
- redux
cover:
    image: "cover.jpg"
    relative: true
---



Basically, we manage global state through `store`, `action`, `reducer` in redux. We can define our `action` and `reducer` first, `dispatch` `action` through synchronous operations, and then processed by the `reducer` to update the `store` so that components can access the `state` from the `store`.

 <!--more-->

But in practice, we may often need to deal with asynchronous behaviors, such as API requests, accessing data stored locally in the browser, etc. We hope that these behaviors or data can also be handed over to redux for unified management, rather than writing in hadling event or In `useEffect`, we make our components as simple as possible to improve the testability of the program.

### Middleware

So what can we do? 

Using redux's middleware can help handle these asynchronous behaviors, dispatch(action)→middleware→reducer, that is, using middleware between `dispatch`'s `action` and `reducer` to extend asynchronous behavior , such as the aforementioned API requests or log records. As for middleware, we can use the official `redux-thunk` or use `redux-saga` to implement our own middleware.

This article will implement a simple middleware in the redux toolkit. The redux toolkit's `configureStore()`, in addition to automatically combining `slice reducers`, also includes `thunk` by default, and automatically starts redux DevTools.

### **Create Middleware**

We first need to initialize middleware. Our middleware is called when each `action` is `dispatch`, that is to say, each `action` will go through the middleware. If it matches the function type, it will execute what we want to execute, otherwise it will do nothing. But in either case, `action` will be passed to the next middleware in the chain through `return next(action)`, and if there is no next middleware, it will be handed over to `reducer` for processing.

```
const thinkMiddleware = (store) => (next) => (action) => {
//
   return next(action);
}

```

### localstoage

Suppose we have a user.js component with login and logout buttons respectively, and define our action and reducer in userSlice as follows. When we login, we will dispatch the login result to the store in the component, and then dispatch the logout result to the store when we logout.

```jsx
import { createSlice } from "@reduxjs/toolkit";

const initialState = {login:false}
export const userSlice = createSlice({
    name:"user",
    initialState:{ value:initialState },
    reducers:{   
        login:(state, action) => {
            state.value = action.payload;
        },
        logout:(state, action) => {
            state.value = action.payload;
        },
    }
})

export const { login, logout } = userSlice.actions;
export default userSlice.reducer;
```
We want the browser update to remain logged in, so we store the login result in the localstorage of the browser when logging in, and clear the localstorage on the browser when logging out, because localstoage is a side effect, We will implement Middleware below to operate.

### Implementing localstoage **Middleware**

In addition to subscribing to callbacks in the store, we can also perform this operation in middleware, or even use other permanent repositories, in this example we will implement a middleware. Using the action creator function of the redux toolkit contains a useful function `match()`, we can use `match()` without looking at the type of the function, if `login.match(action)` is true, it will Execute the result of the login in our `localstorage`, and if `logout.match(action)` is true, the `localstorage` will be cleared.

```jsx
import { login, logout } from './features/user';

const userMiddleware = (store) => (next) => (action) => {
  if (login.match(action)) {
    localStorage.setItem('userState', JSON.stringify(action.payload));
  } else if (logout.match(action)) {
    localStorage.removeItem();
  }
  return next(action);
};
```

After that, we call the getDefaultMiddleware function in configureStore of redux toolkit, then we add the middleware we set by ourselves by default.

```jsx
import { configureStore } from "@reduxjs/toolkit";
import userReducer, { login, logout } from './features/user';

const store = configureStore({
  reducer:{
    user:userReducer,
  },
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(userMiddleware)
});
```

This completes the preparation of a simple middleware.