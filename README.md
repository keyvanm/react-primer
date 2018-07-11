# react-primer
Step by step process and boilerplates to set up a React project

This tutorial assumes you have the latest version of node installed. If not you can download from here https://nodejs.org/en/download/.
# Set up basic project
## Set up boilerplate using create-react-app

Create a directory and cd into it

    $ mkdir myproject && cd myproject

Create a React boilerplate project using [create-react-app](https://github.com/facebook/create-react-app).

    $ npx create-react-app my-app
## Set up version control

In your project directory run

    $ git init

Commit the changes with the message “Initial commit”.

## Set up redux

Unless the project is super simple and doesn’t use external data sources such as APIs, you probably need redux.

Install [redux](https://github.com/reduxjs/redux) and [react-redux](https://github.com/reduxjs/react-redux)

    $ npm install --save redux react-redux

Create a directory called `reducers` under `src`

    $ mkdir src/reducers

Create a file called `index.js` under `reducers`

    $ touch src/reducers/index.js

Put the following lines in `src/reducers/index.js` to set up your reducers
```javascript
import { combineReducers } from 'redux';

export default combineReducers({
  default: (state, action) => ({})
});
```

Now go to `src/index.js` and add the following lines:
```javascript
import { createStore } from 'redux';
import { Provider } from 'react-redux';

import reducers from './reducers';  // Basically ./reducers/index.js;
```
In the line `ReactDOM.render(<App />, document.getElementById('root'));` you want to wrap the <App /> component with the following:
```javascript
<Provider store={createStore(reducers)}>
  <App />
</Provider>
```
In the end `index.js` probably looks something like this:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';

import reducers from './reducers';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(
  <Provider store={createStore(reducers)}>
    <App />
  </Provider>,
  document.getElementById('root')
);

registerServiceWorker();
```

Run the webapp using the command `npm start`. Check the console for any errors.

Commit the changes with the message “Redux set up”.

## React Router

Your project will most likely utilize routing. We use the package [react-router](https://github.com/ReactTraining/react-router) for this purpose.

First of all install react-router

    $ npm install --save react-router-dom

The add the following to `src/index.js`
```javascript
import { BrowserRouter } from 'react-router-dom';
```
And wrap `<App />` with `<BrowserRouter>`.
```javascript
<Provider store={createStore(reducers)}>
  <BrowserRouter>
    <App />
  </BrowserRouter>
</Provider>
```
Check for errors and commit with message “react-router set up”.

Please note that some tutorials might refer to a package called `react-router-redux` that since `react-router@4.0` is implemented into `react-router` itself and doesn't seem to be needed anymore. I have personally not ran into problems with `redux` and `react-router`. If any issues arise refer to https://reacttraining.com/react-router/web/guides/redux-integration.

### connected-react-router
https://github.com/supasate/connected-react-router
This puts the `history`, `push`, `location` and etc in redux for easier access, so you don't have to keep passing them around in the props.

https://reacttraining.com/react-router/web/guides/redux-integration `withRouter` wrapper takes care of this apparently.

## Set up axios

If you need to make HTTP request in your app (you most likely will) install [axios](https://github.com/axios/axios).

    $ npm install --save axios

Check for errors and commit with message “axios set up”.

A more robust option is to make API calls within Redux using [redux-api-middleware](#redux-api-middleware).

# Push to remote

Create a [GitHub](https://github.com/) repository for your project.
Copy the git address and add it as a remote to your repository.

    $ git remote add origin https://github.com/USER/PROJECT.git


 Push your repo to origin and set origin/master as upstream (track).

    $ git push -u origin master
# Deploy to heroku

If you want to deploy your react app to heroku

    $ heroku create myproject -b https://github.com/mars/create-react-app-buildpack.git

And deploy

    $ git push heroku master
# Redux
## Creating a reducer

This is a simple boilerplate for a reducer
```javascript
const INITIAL_STATE = {};

export default function(state = INITIAL_STATE, action) {
  switch (action.type) {
    case "SOME_ACTION":
      return { ...state, someKey: action.payload }
    default:
      return state;
  }
}
```
## Connecting a component
```javascript
import { connect } from 'react-redux';
...
export default connect()(Component);
```
## Dispatch an action
```javascript
this.props.dispatch({
  type: "ACTION",
  payload: data
})
```
## mapStateToProps
```javascript
(reduxState) => (objWhichWillBeInjectedIntoProps)
```
## middleware
If you have middlewares to setup
```javascript
...
import { createStore, applyMiddleware } from 'redux';
...
const createStoreWithMiddleware = applyMiddleware(...middlewares)(createStore);
...
<Provider store={createStoreWithMiddleware(reducers)}>
```

# Redux-thunk
https://github.com/reduxjs/redux-thunk

Why `redux-thunk`? https://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559

## TODO
- [ ] Write this section


# redux-api-middleware
https://github.com/agraboso/redux-api-middleware

## TODO
- [ ] Write this section


# Authentication to a django-rest-framework powered backend
- https://medium.com/netscape/full-stack-django-quick-start-with-jwt-auth-and-react-redux-part-i-37853685ab57
- https://medium.com/@viewflow/full-stack-django-quick-start-with-jwt-auth-and-react-redux-part-ii-be9cf6942957
