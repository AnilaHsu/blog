---
title: "Redux - Write localStorage using middleware"
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

But in practice, we may often need to handle asynchronous behavior, such as API requests, handling browser localStorage data, logging, etc. We hope that these behaviors or data can also be unified and managed by redux, rather than written in the Handling event or `useEffect` of react, so that our react can be as simple and state more transparent and clear as possible.

### Middleware

So what can we do? 

We can use middleware between the `action` and `reducer` of the `dispatch` to extend the content we want to execute. In addition to using the official `redux-thunk` or using third-party `redux-saga` or a middleware written by someone else, we can also implement our own middleware.

This article will implement a simple middleware in the redux toolkit to write localStorage. In the `configureStore()` of the redux toolkit, besides automatically combining slice reducers, it also includes `thunk` by default and automatically starts the redux DevTools.


### **Create Middleware**

We first need to initialize middleware. Our middleware is called when each `action` is `dispatch`, that is to say, each `action` will go through the middleware. If it matches the function type, it will execute what we want to execute, otherwise it will do nothing. But in either case, `action` will be passed to the next middleware in the chain through `return next(action)`, and if there is no next middleware, it will be handed over to `reducer` for processing.

```
const thinkMiddleware = (store) => (next) => (action) => {
//
   return next(action);
}

```

### localStorage

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
We want the browser update to remain logged in, so we store the login result in the `localStorage` of the browser when logging in, and clear the `localStorage` on the browser when logging out, because `localStorage` is a side effect, We will implement Middleware below to operate.

### Implementing localStorage **Middleware**

In addition to subscribing to callbacks in the store, we can also perform this operation in middleware, or even use other permanent repositories, in this example we will implement a middleware. Using the action creator function of the redux toolkit contains a useful function `match()`, we can use `match()` without looking at the type of the function, if `login.match(action)` is true, it will Execute the result of the login in our `localStorage`, and if `logout.match(action)` is true, the `localStorage` will be cleared.

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

This completes the preparation of a simple middleware!